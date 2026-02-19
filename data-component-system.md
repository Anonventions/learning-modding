# 5. Data Component System

> **Wiki home:** [Home](Home) | **Previous:** [Porting to 1.21](4.-Porting-to-1.21) | **Next:** [Writing Recipes](6.-Writing-Recipes)

The most significant change in 1.21 is the complete replacement of unstructured NBT item data with a **strictly typed Data Component system**. This page explains what changed, why, and how to implement it.

---

## 5.1 Why NBT Was Removed from Items

Prior to 1.20.5, all custom data on an `ItemStack` was stored in a `CompoundTag` — an unstructured blob of key-value pairs accessible via `ItemStack.getTag()`.

### Problems with the old approach

- **No type safety.** A typo in a string key (`"chagres"` instead of `"charges"`) caused silent data loss — no compile error, no runtime error.
- **Performance cost.** The engine had to dynamically parse and decode the compound tag on every tick for every item.
- **No structural guarantees.** Any mod could read or accidentally overwrite any other mod's tag keys.

### The new approach

An `ItemStack` now holds a **map of strongly typed, independent components**. Each component is a registered Java object with a known type. The engine operates directly on the object — no dynamic decoding needed.

### Migration at a glance

```java
// ❌ Old NBT approach
itemStack.getOrCreateTag().putInt("charges", 5);
int charges = itemStack.getTag().getInt("charges");

// ✅ New component approach
itemStack.set(MyComponents.CHARGES, 5);
int charges = itemStack.getOrDefault(MyComponents.CHARGES, 0);
```

> 📝 NBT still exists in the game for **block entities**, **entities**, and **world data**. It has only been removed from `ItemStack`.

---

## 5.2 Removal of Hardcoded Base Classes

The component system was so comprehensive that Mojang used it to remove hardcoded item subclasses entirely.

### What was removed

| Old class (pre-1.21) | Replacement component |
|---|---|
| `SwordItem` | `DataComponents.WEAPON` + `DataComponents.ATTACK_DAMAGE` + `DataComponents.ATTACK_SPEED` |
| `DiggerItem` / `PickaxeItem` etc. | `DataComponents.TOOL` |
| `ArmorItem` | `DataComponents.ARMOR` |
| `ShieldItem` (base logic) | `DataComponents.BLOCKS_ATTACKS` |
| `DeferredSpawnEggItem` | Base `SpawnEggItem` (entities now guaranteed registered before items) |

### Creating a custom weapon (new approach)

```java
// ❌ Old way — extending SwordItem
public class MyBlade extends SwordItem {
    public MyBlade() {
        super(Tiers.IRON, 3, -2.4f, new Item.Properties());
    }
}

// ✅ New way — plain Item with components
public static final Supplier<Item> MY_BLADE = ITEMS.register("my_blade", () ->
    new Item(new Item.Properties()
        .component(DataComponents.ATTACK_DAMAGE, 7.0f)
        .component(DataComponents.ATTACK_SPEED, -2.4f)
        .component(DataComponents.WEAPON, new Weapon(2, ItemStack.EMPTY))
    )
);
```

### Why this is more powerful

Because components are independent, you can now combine them freely. A single item can simultaneously function as:
- A weapon (`WEAPON` + `ATTACK_DAMAGE`)
- A mining tool (`TOOL`)
- A piece of armour (`ARMOR`)

Previously, this would have required multiple inheritance or complex wrapper logic.

### `CustomModelData` upgrade

The `CustomModelData` component (used to drive custom item rendering) was upgraded from a simple integer to a rich object accepting:
- Lists of `float` values
- `boolean` flags
- `String` labels
- Hexadecimal color values

This allows item models to react to multiple independent state variables simultaneously.

---

## 5.3 Registering a Custom Data Component

When you need to store custom data on an item that doesn't fit any vanilla component, you register your own `DataComponentType`.

### Step 1 — Define your data as a Java record

Records are **strongly recommended** because they automatically guarantee immutability and provide the `hashCode` and `equals` implementations the engine requires for state comparison.

```java
public record ChargeData(int charges, boolean overcharged) {

    public static final Codec<ChargeData> CODEC = RecordCodecBuilder.create(instance ->
        instance.group(
            Codec.INT.fieldOf("charges").forGetter(ChargeData::charges),
            Codec.BOOL.fieldOf("overcharged").forGetter(ChargeData::overcharged)
        ).apply(instance, ChargeData::new)
    );

    public static final StreamCodec<ByteBuf, ChargeData> STREAM_CODEC =
        StreamCodec.composite(
            ByteBufCodecs.INT, ChargeData::charges,
            ByteBufCodecs.BOOL, ChargeData::overcharged,
            ChargeData::new
        );
}
```

| Codec | Purpose |
|---|---|
| `Codec` (MapCodec) | Serializes to/from JSON or NBT for **disk storage** |
| `StreamCodec` | Serializes to/from a byte buffer for **network transmission** |

Both are required for a fully persistent, network-synchronized component.

### Step 2 — Register the `DataComponentType` (NeoForge)

```java
public class MyComponents {

    public static final DeferredRegister<DataComponentType<?>> DATA_COMPONENTS =
        DeferredRegister.create(Registries.DATA_COMPONENT_TYPE, MOD_ID);

    public static final Supplier<DataComponentType<ChargeData>> CHARGE_DATA =
        DATA_COMPONENTS.register("charge_data", () ->
            DataComponentType.<ChargeData>builder()
                .persistent(ChargeData.CODEC)        // enables disk save/load
                .networkSynchronized(ChargeData.STREAM_CODEC) // enables sync to clients
                .build()
        );
}
```

### Step 3 — Register the `DataComponentType` (Fabric)

```java
public class MyComponents {

    public static final DataComponentType<ChargeData> CHARGE_DATA =
        Registry.register(
            Registries.DATA_COMPONENT_TYPE,
            ResourceLocation.fromNamespaceAndPath(MOD_ID, "charge_data"),
            DataComponentType.<ChargeData>builder()
                .persistent(ChargeData.CODEC)
                .networkSynchronized(ChargeData.STREAM_CODEC)
                .build()
        );
}
```

### Step 4 — Read and write on an `ItemStack`

```java
// Write a value
stack.set(MyComponents.CHARGE_DATA.get(), new ChargeData(3, false));

// Read a value (with fallback default)
ChargeData data = stack.getOrDefault(MyComponents.CHARGE_DATA.get(), new ChargeData(0, false));

// Read as Optional (when absence is meaningful)
Optional<ChargeData> data = stack.getOptional(MyComponents.CHARGE_DATA.get());

// Remove a component
stack.remove(MyComponents.CHARGE_DATA.get());
```

### Step 5 — Register in your mod initializer

Make sure `DATA_COMPONENTS.register(modEventBus)` is called in your mod constructor or init method:

```java
// NeoForge
MyComponents.DATA_COMPONENTS.register(modEventBus);
```

---

## Viewing Components in IntelliJ

Because components are registered Java objects, you can:
- **Set a breakpoint** inside `ChargeData` and inspect live values in the IntelliJ debugger while the game is running.
- Use **Find Usages** (`Alt+F7`) on `CHARGE_DATA` to track every place the component is read or written.
- Use the IntelliJ **Evaluate Expression** tool (`Alt+F8`) during a debug session to call `stack.get(MyComponents.CHARGE_DATA.get())` on a live stack.

---

> **Next:** [Writing Recipes](6.-Writing-Recipes)
