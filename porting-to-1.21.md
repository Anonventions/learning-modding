# 4. Porting to 1.21

> **Wiki home:** [Home](Home) | **Previous:** [Cross-Platform Mods](3.-Cross-Platform-Mods) | **Next:** [Data Component System](5.-Data-Component-System)

The 1.21 update introduced some of the most aggressive breaking changes in recent Minecraft history. This page walks through every step of the porting process using **IntelliJ IDEA**.

---

## 4.1 Branch Your Code First

Never port directly on your main branch. Create a dedicated port branch using IntelliJ's built-in Git integration or the terminal.

### Using IntelliJ Git UI

1. Go to **Git → Branches** (bottom-right of the window, or top menu **Git → Branches**).
2. Click **New Branch**.
3. Name it something like `port/1.21.1`.
4. Make sure **Checkout branch** is ticked, then click **Create**.

### Using the terminal

```bash
git checkout -b port/1.21.1
```

This preserves your working 1.20.x code so you can still apply hotfixes to the old version independently.

---

## 4.2 Update `gradle.properties`

Open `gradle.properties` in IntelliJ and update the following values:

```properties
# Minecraft version
minecraft_version=1.21.1

# NeoForge version (check https://neoforged.net for the latest)
neoforge_version=21.1.x

# Parchment (update to matching version)
parchment_minecraft_version=1.21.1
parchment_mappings_version=2024.11.17
```

After saving, force Gradle to fully re-resolve all dependencies. In the IntelliJ terminal:

```bash
./gradlew --refresh-dependencies
```

Or trigger it from the **Gradle tool window** → click the **Reload All Gradle Projects** button (the circular arrow icon).

> ⚠️ Gradle's caching is aggressive. If you see stale errors after updating versions, always use `--refresh-dependencies` rather than just re-syncing.

Your project will now show a large number of **red compilation errors** in IntelliJ. This is expected — work through them systematically using the sections below.

---

## 4.3 Rename Asset and Data Directories

Mojang changed all data/resource folder names from plural to singular in 1.21. These must be renamed manually.

### Required folder renames

| Old path | New path |
|---|---|
| `data/<modid>/tags/blocks/` | `data/<modid>/tags/block/` |
| `data/<modid>/tags/items/` | `data/<modid>/tags/item/` |
| `data/<modid>/advancements/` | `data/<modid>/advancement/` |
| `data/<modid>/loot_tables/` | `data/<modid>/loot_table/` |
| `data/<modid>/recipes/` | `data/<modid>/recipe/` |

### Renaming in IntelliJ

1. In the **Project** panel, right-click the folder.
2. Select **Refactor → Rename**.
3. Enter the new name and confirm.

> ⚠️ If you miss any of these renames, the game will **silently skip** loading the affected files. There will be no crash — just missing textures, broken recipes, or empty loot tables. Always double-check every folder.

### NeoForge-specific file rename

Rename the mod manifest file:

```
META-INF/mods.toml  →  META-INF/neoforge.mods.toml
```

This distinguishes NeoForge mods from legacy Forge mods. If this file is not renamed, the mod loader will silently ignore your compiled `.jar` on launch.

Update your `build.gradle` to reference the new filename if it explicitly copies or processes `mods.toml`.

### Tag namespace change

Replace all uses of the `forge:` tag namespace with `c:` (the unified common namespace shared by Fabric and NeoForge):

| Old | New |
|---|---|
| `forge:ingots/iron` | `c:ingots/iron` |
| `forge:ores/diamond` | `c:ores/diamond` |
| `forge:storage_blocks/iron` | `c:storage_blocks/iron` |

Use IntelliJ's **Find and Replace in Files** (`Ctrl+Shift+R` / `Cmd+Shift+R`) to catch all occurrences across your resource files at once.

---

## 4.4 Update `ResourceLocation` Calls

The `ResourceLocation` class had its public constructors **privatized** in 1.21. Any call using `new ResourceLocation(...)` will now fail to compile.

### What to change

```java
// ❌ Broken in 1.21
new ResourceLocation("modid", "item_name")
new ResourceLocation("modid:item_name")
```

```java
// ✅ Correct replacements
ResourceLocation.fromNamespaceAndPath("modid", "item_name")
ResourceLocation.parse("modid:item_name")

// Shorthand when your namespace is a constant
ResourceLocation.withDefaultNamespace("item_name")
```

Also remove any calls to `isValidResourceLocation()` — this method was **deleted entirely**.

### Finding all occurrences in IntelliJ

Use **Find in Files** (`Ctrl+Shift+F` / `Cmd+Shift+F`) and search for:

```
new ResourceLocation(
```

Then work through each result and replace it with the appropriate static factory method.

---

## 4.5 The `ResourceLocation` → `Identifier` Rename (Modern Mappings)

On modern NeoForge (version 21.11+) and modern Fabric, the `ResourceLocation` class has been **globally renamed to `Identifier`** to align with official Mojang naming conventions.

If you are targeting these modern environments, you will need a workspace-wide rename:

1. In IntelliJ, open any file that uses `ResourceLocation`.
2. Click on the class name and press `Shift+F6` (Rename refactor).
3. This will update all usages across the entire project automatically.

Alternatively, use **Find and Replace in Files** for a bulk text replacement if the Rename refactor does not catch resource files or string literals.

> 📝 This rename only applies to environments using updated MojMap-aligned naming. Check your specific loader version's changelog to confirm whether this applies to your target.

---

## Porting Checklist

- [ ] New Git branch created (`port/1.21.1`)
- [ ] `gradle.properties` updated with new Minecraft, loader, and Parchment versions
- [ ] `--refresh-dependencies` run successfully
- [ ] All asset/data folders renamed (plural → singular)
- [ ] `META-INF/mods.toml` renamed to `META-INF/neoforge.mods.toml`
- [ ] All `forge:` tag namespaces replaced with `c:`
- [ ] All `new ResourceLocation(...)` calls replaced with static factory methods
- [ ] `isValidResourceLocation()` calls removed
- [ ] `ResourceLocation` → `Identifier` rename applied if targeting NeoForge 21.11+
- [ ] Project compiles with zero errors

---

> **Next:** [Data Component System](5.-Data-Component-System)
