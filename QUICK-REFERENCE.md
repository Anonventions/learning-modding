# 📋 Quick Reference Guide

This page provides a quick lookup for common tasks, syntax, and patterns in Minecraft modding.

---

## 🚀 Quick Start Commands

### Import MDK into IntelliJ
```bash
# 1. Extract MDK to a folder
# 2. Open IntelliJ → File → Open → Select MDK folder
# 3. Wait for Gradle sync to complete (10-30 minutes first time)
```

### Run Your Mod
```bash
# In IntelliJ Gradle tool window:
# Right-click: MyProject → Tasks → fg → runClient
# Or run via menu: Gradle → Tasks → fg → runClient
```

### Force Clean Gradle
```bash
./gradlew --refresh-dependencies
```

---

## 📦 Common Registrations

### Register Items
```java
public static final DeferredRegister<Item> ITEMS = 
    DeferredRegister.create(Registries.ITEM, MOD_ID);

public static final RegistryObject<Item> RUBY_INGOT = ITEMS.register("ruby_ingot", () ->
    new Item(new Item.Properties())
);
```

### Register Blocks
```java
public static final DeferredRegister<Block> BLOCKS = 
    DeferredRegister.create(Registries.BLOCK, MOD_ID);

public static final RegistryObject<Block> RUBY_ORE = BLOCKS.register("ruby_ore", () ->
    new Block(BlockBehaviour.Properties.of()
        .mapColor(MapColor.COLOR_RED)
        .strength(3.0f)
    )
);
```

### Register Block Items
```java
public static final RegistryObject<Item> RUBY_ORE = ITEMS.register("ruby_ore", () ->
    new BlockItem(ModBlocks.RUBY_ORE.get(), new Item.Properties())
);
```

### Register Entity
```java
public static final RegistryObject<EntityType<RubyGolem>> RUBY_GOLEM =
    ENTITY_TYPES.register("ruby_golem", () ->
        EntityType.Builder.of(RubyGolem::new, MobCategory.MONSTER)
            .sized(1.4f, 2.1f)
            .build("ruby_golem")
    );
```

---

## 📝 Common JSON Formats

### Shaped Recipe
```json
{
  "type": "minecraft:crafting_shaped",
  "category": "equipment",
  "pattern": [
    "XXX",
    " # ",
    " # "
  ],
  "key": {
    "X": "tutorial:ruby_ingot",
    "#": "minecraft:stick"
  },
  "result": {
    "id": "tutorial:ruby_sword",
    "count": 1
  }
}
```

### Shapeless Recipe
```json
{
  "type": "minecraft:crafting_shapeless",
  "category": "misc",
  "ingredients": [
    "tutorial:ruby_ingot",
    "tutorial:ruby_ingot",
    "minecraft:diamond"
  ],
  "result": {
    "id": "tutorial:custom_item",
    "count": 1
  }
}
```

### Smelting Recipe
```json
{
  "type": "minecraft:smelting",
  "category": "misc",
  "ingredient": {
    "item": "tutorial:raw_ruby"
  },
  "result": "tutorial:ruby_ingot",
  "experience": 0.7,
  "cookingtime": 200
}
```

### Item Model
```json
{
  "parent": "item/handheld",
  "textures": {
    "layer0": "tutorial:item/ruby_sword"
  }
}
```

### Block Model
```json
{
  "parent": "block/cube_all",
  "textures": {
    "all": "tutorial:block/ruby_ore"
  }
}
```

### Language File (en_us.json)
```json
{
  "item.tutorial.ruby_ingot": "Ruby Ingot",
  "block.tutorial.ruby_ore": "Ruby Ore"
}
```

---

## 🎣 Common Event Listeners

### Block Break Event
```java
@SubscribeEvent
public static void onBlockBreak(BlockEvent.BreakEvent event) {
    if (event.getState().getBlock() == ModBlocks.RUBY_ORE.get()) {
        event.getPlayer().displayClientMessage(
            Component.literal("Ruby found!"),
            false
        );
    }
}
```

### Player Attack Event
```java
@SubscribeEvent
public static void onLivingAttack(LivingAttackEvent event) {
    if (event.getSource().getEntity() instanceof Player player) {
        if (player.getMainHandItem().getItem() == ModItems.RUBY_SWORD.get()) {
            // Custom logic here
        }
    }
}
```

### Server Tick Event
```java
@SubscribeEvent
public static void onServerTick(TickEvent.ServerTickEvent event) {
    if (event.phase == TickEvent.Phase.END) {
        // Execute at end of tick
    }
}
```

### Player Join Event
```java
@SubscribeEvent
public static void onPlayerJoin(PlayerEvent.PlayerLoggedInEvent event) {
    Player player = event.getEntity();
    if (!player.level().isClientSide) {
        player.displayClientMessage(Component.literal("Welcome!"), false);
    }
}
```

---

## 🌐 Network Packets

### Define Packet
```java
public class MyPacket implements CustomPacketPayload {
    public static final ResourceLocation ID = new ResourceLocation("mymod", "my_packet");
    
    public static final StreamCodec<ByteBuf, MyPacket> CODEC =
        StreamCodec.unit(new MyPacket());

    @Override
    public ResourceLocation id() {
        return ID;
    }
}
```

### Handle Packet (Client Side)
```java
public static void handle(MyPacket packet, IPayloadContext context) {
    context.enqueueWork(() -> {
        // Client-side logic only
        Minecraft.getInstance().particleEngine.createTrackingEmitter(...);
    });
}
```

### Send to Client
```java
ModPacketHandler.sendToClient(new MyPacket(), player);
```

### Send to Server
```java
ModPacketHandler.sendToServer(new MyPacket());
```

---

## 🔧 Data Components

### Define Component Record
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

### Register Component Type
```java
public static final RegistryObject<DataComponentType<ChargeData>> CHARGE =
    DATA_COMPONENTS.register("charge", () ->
        DataComponentType.<ChargeData>builder()
            .persistent(ChargeData.CODEC)
            .networkSynchronized(ChargeData.STREAM_CODEC)
            .build()
    );
```

### Use Component on Item
```java
// Set data
itemStack.set(MyComponents.CHARGE, new ChargeData(50, false));

// Get data
ChargeData data = itemStack.get(MyComponents.CHARGE);

// Get with default
ChargeData data = itemStack.getOrDefault(MyComponents.CHARGE, 
    new ChargeData(0, false));
```

---

## 🎨 Common Block Properties

```java
new Block(BlockBehaviour.Properties.of()
    .mapColor(MapColor.COLOR_RED)          // Minimap color
    .strength(3.0f, 6.0f)                  // Hardness, blast resistance
    .sound(SoundType.STONE)                // Walking sound
    .noOcclusion()                         // Doesn't block light
    .requiresCorrectToolForDrops()         // Need right tool
    .replaceable()                         // Can replace with other blocks
)
```

---

## 🗂️ File Structure Checklist

```
src/main/java/com/example/mymod/
├── MyMod.java                          (main class)
├── block/ModBlocks.java
├── item/ModItems.java
├── entity/ModEntities.java
└── event/ModEvents.java

src/main/resources/
├── META-INF/neoforge.mods.toml         (metadata)
├── assets/mymod/
│   ├── lang/en_us.json
│   ├── models/item/                    (item JSON files)
│   ├── models/block/                   (block JSON files)
│   └── textures/item/                  (PNG files)
│   └── textures/block/
└── data/mymod/recipe/                  (recipe JSON files)
```

---

## ⚙️ gradle.properties Template

```properties
minecraft_version=1.21.5
neoforge_version=21.5.26
mapping_channel=official
mapping_version=1.21.5

mod_id=mymod
mod_name=My Mod
mod_version=1.0.0
mod_author=YourName
mod_description=My awesome mod
```

---

## 🏷️ neoforge.mods.toml Template

```toml
[[mods]]
modId="mymod"
version="1.0.0"
displayName="My Mod"
logoFile="icon.png"
logoBlur=true
authors="YourName"
description='''
My awesome mod description.
'''

[[dependencies.mymod]]
    modId="neoforge"
    mandatory=true
    versionRange="[21.5,)"
    ordering="NONE"
    side="BOTH"
```

---

## 🐛 Error Quick Fixes

| Error | Cause | Fix |
|---|---|---|
| "Cannot find symbol" | Class not imported | Add import statement |
| "No enum constant" | Registry not initialized | Call `.register(eventBus)` in main class |
| Crash on startup | Missing texture | Create PNG file and JSON model |
| Recipe doesn't appear | JSON syntax error | Validate with [JSONLint](https://jsonlint.com/) |
| NullPointerException | Accessing uninitialized object | Use `.get()` on RegistryObject |
| Texture is pink/magenta | Texture path mismatch | Check exact file path spelling |

---

## 🔍 IntelliJ Keyboard Shortcuts

| Action | Windows/Linux | Mac |
|---|---|---|
| Find class by name | Ctrl+N | Cmd+O |
| Find file by name | Ctrl+Shift+N | Cmd+Shift+O |
| Search everywhere | Shift+Shift | Shift+Shift |
| Navigate to definition | Ctrl+B | Cmd+B |
| Find usages | Alt+F7 | Option+F7 |
| Reformat code | Ctrl+Alt+L | Cmd+Option+L |
| Rebuild project | Ctrl+Shift+F9 | Cmd+Shift+F9 |
| Run current config | Shift+F10 | Ctrl+R |

---

## 📊 Common Damage Values

For creating weapons:

```java
// Attack Damage is in addition to tool damage
// Attack Speed: -4.0 (very slow) to 4.0 (very fast)
// Default is -2.4 for hand tools

new SwordItem(Tiers.IRON, 
    3,           // attack bonus damage (+3 to base)
    -2.4f,       // attack speed
    properties
)
```

---

## 🎯 Resource Naming Conventions

| Type | Namespace | Example |
|---|---|---|
| Item ID | `modid:item_name` | `tutorial:ruby_ingot` |
| Block ID | `modid:block_name` | `tutorial:ruby_ore` |
| Entity ID | `modid:entity_name` | `tutorial:ruby_golem` |
| Texture | `modid:textures/item/file` | `tutorial:textures/item/ruby_ingot` |
| Model | `modid:models/item/file` | `tutorial:models/item/ruby_ingot` |
| Recipe | `modid:recipe/recipe_name` | `tutorial:recipe/ruby_sword` |
| Sound | `modid:sound_name` | `tutorial:ruby_break` |

---

## 💾 Common Game Ticks

- **20 ticks** = 1 second (server ticks)
- **Tick event** happens 20 times per second
- **LevelTickEvent**: World-level updates
- **EntityTickEvent**: Entity-specific updates
- **PlayerTickEvent**: Player-specific updates

```java
// Check every second
if (event.getServer().getTickCount() % 20 == 0) {
    // Execute once per second
}
```

---

## 🔐 Important Security Rules

- ❌ **Never trust client input**
- ✅ Validate all packets on server
- ❌ **Never** store critical game logic on client
- ✅ Server is the source of truth
- ❌ Don't let client calculate damage/health changes
- ✅ Server calculates, sends result to client

---

## 📚 When to Use Each Registry

| Need | Use Registry |
|---|---|
| Add a breakable block | `BLOCK` |
| Add item to inventory | `ITEM` |
| Add living creature | `ENTITY_TYPE` |
| Add sound effect | `SOUND_EVENT` |
| Add visual effect | `PARTICLE_TYPE` |
| Add status effect (potion) | `MOB_EFFECT` |
| Add data persistently | Custom `DataComponentType` |
| Add visual variant | `PAINTING_VARIANT` |

---

## 🚦 Event Priority Levels

```java
@SubscribeEvent(priority = EventPriority.HIGHEST)    // Runs first
@SubscribeEvent(priority = EventPriority.HIGH)       
@SubscribeEvent(priority = EventPriority.NORMAL)     // Default
@SubscribeEvent(priority = EventPriority.LOW)        
@SubscribeEvent(priority = EventPriority.LOWEST)     // Runs last
```

Use `HIGHEST` to run before vanilla, `LOWEST` to run after all mods.

---

## 🎮 Testing Your Mod

```bash
# Run client (single-player with your mod)
./gradlew runClient

# Run server
./gradlew runServer

# Run data generation (for recipes, etc.)
./gradlew runData
```

---

## 📞 Quick Links to Full Pages

- **Getting Started:** [Introduction & Overview](0.-Introduction-Overview.md)
- **Setup:** [Environment Setup](Environment-Setup.md)
- **Registries:** [Registry Systems](9.-Registry-Systems.md)
- **Recipes:** [Writing Recipes](writing-recipes.md)
- **Events:** [Events & Networking](10.-Events-Networking.md)
- **Complex Items:** [Custom Totem of Undying](writing-recipes-totem-example.md)
- **Practical Example:** [Example Mod Setup](8.-Example-Mod-Setup.md)

---

**For detailed explanations of any topic, see the full wiki pages linked above.**

**Last updated:** February 2026 | **For:** Minecraft 1.21.5+


