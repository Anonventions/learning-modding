# 7. Custom Totem of Undying

> **Wiki home:** [Home](Home) | **Previous:** [Writing Recipes](6.-Writing-Recipes)

Creating a custom item that saves a player from death is one of the most common advanced modding goals. This page covers the vanilla internals, the legacy approach, and the clean modern implementation using the `death_protection` component.

---

## 7.1 How the Vanilla Totem Works

### Legacy internals (pre-1.21)

In older versions, the totem logic was **hardcoded into `LivingEntity`**. The `TotemOfUndyingItem` class itself was essentially empty — just a plain item registration. The actual resurrection logic lived in a private method called `tryUseTotem` inside `LivingEntity`, which was called every time an entity would die.

`tryUseTotem` did the following:
1. Iterated over the entity's main hand and offhand slots.
2. Checked if the `ItemStack` in either slot **exactly matched** `Items.TOTEM_OF_UNDYING`.
3. If matched: cancelled the death, reset health to 1.0, cleared all negative effects, applied hardcoded effects (Regeneration, Absorption, Fire Resistance), and decremented the stack.
4. Sent a byte-status packet to the client, triggering the screen overlay animation.

Because this check was **hardcoded to one specific item ID**, creating a custom totem required bypassing it entirely.

### Modern internals (1.21+)

Mojang extracted all of this logic into the `death_protection` data component. `LivingEntity` no longer checks for a specific item ID. Instead, it scans equipped items for **any item carrying the `death_protection` component**.

---

## 7.2 The Legacy Approach (Pre-1.21)

This section is relevant if you are maintaining a mod on 1.20.x or earlier.

### NeoForge / Forge — Event Bus

Subscribe to `LivingDeathEvent` or `LivingEntityUseTotemEvent` at high priority:

```java
@SubscribeEvent(priority = EventPriority.HIGH)
public void onDeath(LivingDeathEvent event) {
    LivingEntity entity = event.getEntity();

    // Check both hands
    for (InteractionHand hand : InteractionHand.values()) {
        ItemStack stack = entity.getItemInHand(hand);
        if (stack.is(MyItems.MY_TOTEM.get())) {
            event.setCanceled(true);

            // Reset health
            entity.setHealth(1.0f);

            // Clear negatives and apply custom effects
            entity.removeAllEffects();
            entity.addEffect(new MobEffectInstance(MobEffects.REGENERATION, 900, 1));
            entity.addEffect(new MobEffectInstance(MobEffects.ABSORPTION, 100, 1));
            entity.addEffect(new MobEffectInstance(MobEffects.FIRE_RESISTANCE, 800, 0));

            // Consume the item
            stack.shrink(1);

            // Trigger the client-side animation — requires a custom network packet
            // (see networking note below)
            break;
        }
    }
}
```

### The animation problem

The vanilla totem activation animation is triggered by a **server-to-client packet**. The vanilla packet only supports the default totem texture. To display a custom item texture in the HUD animation, you must:

1. Create a custom **network payload** class.
2. Register it with your loader's network channel.
3. Send it from the server when the totem triggers.
4. On the client, receive it and call:

```java
Minecraft.getInstance().gameRenderer.displayItemActivation(
    new ItemStack(MyItems.MY_TOTEM.get())
);
```

This requires maintaining separate client and server networking classes and strict logical side separation to avoid server crashes.

### Fabric — Mixin

```java
@Mixin(LivingEntity.class)
public abstract class LivingEntityMixin {

    @Inject(
        method = "checkTotemDeathProtection",
        at = @At("HEAD"),
        cancellable = true
    )
    private void onCheckTotem(DamageSource source, CallbackInfoReturnable<Boolean> cir) {
        LivingEntity self = (LivingEntity)(Object)this;

        for (InteractionHand hand : InteractionHand.values()) {
            ItemStack stack = self.getItemInHand(hand);
            if (stack.is(MyItems.MY_TOTEM)) {
                // Apply effects, consume item, trigger animation...
                cir.setReturnValue(true);
                return;
            }
        }
    }
}
```

---

## 7.3 The Modern Approach — `death_protection` Component (1.21+)

In 1.21+, all of the above complexity is **completely unnecessary** for standard resurrection behaviour.

### What the component provides automatically

When an item carries the `death_protection` component:

| Behaviour | Automatic? |
|---|---|
| Death cancellation (both hands) | ✅ |
| Health reset to 1.0 | ✅ |
| Item consumed on use | ✅ |
| Custom effects applied | ✅ (configured in the component) |
| Totem activation animation (with your item's texture) | ✅ |
| Activation sound | ✅ |
| `minecraft:totem_of_undying` advancement trigger | ✅ |

No events. No Mixins. No networking code.

### Implementation

```java
public static final Supplier<Item> MY_TOTEM = ITEMS.register("my_totem", () ->
    new Item(new Item.Properties()
        .stacksTo(1)
        .component(DataComponents.DEATH_PROTECTION, new DeathProtection(
            List.of(
                new MobEffectInstance(MobEffects.REGENERATION, 900, 1),
                new MobEffectInstance(MobEffects.ABSORPTION, 100, 1),
                new MobEffectInstance(MobEffects.FIRE_RESISTANCE, 800, 0)
            )
        ))
    )
);
```

### Customising the effects

The `DeathProtection` constructor takes a `List<MobEffectInstance>`. You can:

- Pass an **empty list** for a save with no effects:
  ```java
  new DeathProtection(List.of())
  ```
- Add any combination of vanilla or modded effects:
  ```java
  new DeathProtection(List.of(
      new MobEffectInstance(MobEffects.NIGHT_VISION, 200, 0),
      new MobEffectInstance(MyEffects.INVULNERABILITY.get(), 60, 0)
  ))
  ```
- Reference custom effects registered via your mod's `MobEffect` registry.

### Adding a recipe

Combine with a shaped recipe (see [Writing Recipes](6.-Writing-Recipes)) to make the totem obtainable in survival:

```json
{
  "type": "minecraft:crafting_shaped",
  "category": "misc",
  "pattern": [
    " G ",
    "GCG",
    "GGG"
  ],
  "key": {
    "G": "minecraft:gold_ingot",
    "C": "minecraft:nether_star"
  },
  "result": {
    "id": "mymod:my_totem",
    "count": 1
  }
}
```

### When you still need an event handler

The `death_protection` component covers the standard use case. You still need custom event logic if you require:

| Requirement | Needs event? |
|---|---|
| Conditional activation (e.g., only works in specific biome) | ✅ |
| Cooldown between uses | ✅ |
| Entirely custom animation sequence | ✅ |
| Triggering side effects (e.g., spawn particles, summon entities) | ✅ |
| Standard "save player, apply effects" with custom texture | ❌ — component handles this |

---

## Debugging in IntelliJ

- Set a **breakpoint** inside `DeathProtection` or the `LivingEntity` damage handling method to trace the activation sequence.
- Use the IntelliJ **debugger's Evaluate** panel (`Alt+F8`) to inspect the component on a live `ItemStack` during a debug session:
  ```java
  stack.get(DataComponents.DEATH_PROTECTION)
  ```
- Check the **Run** log for any registration errors if the component is not activating.

---

> **Wiki home:** [Home](Home) | **Previous:** [Writing Recipes](6.-Writing-Recipes)
