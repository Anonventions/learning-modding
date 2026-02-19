# 6. Writing Recipes

> **Wiki home:** [Home](Home) | **Previous:** [Data Component System](5.-Data-Component-System) | **Next:** [Custom Totem of Undying](7.-Custom-Totem-of-Undying)

Recipes define how players transform raw materials into items at crafting tables, furnaces, and other workstations. All recipes are defined as **JSON files** — no Java required for standard recipes.

---

## 6.1 How the Recipe System Works

### File location

All recipe files go in:

```
src/main/resources/data/<modid>/recipe/
```

> ⚠️ Note the singular `recipe/` — plural `recipes/` was removed in 1.21. See [Porting to 1.21](4.-Porting-to-1.21) for the full folder rename list.

### How the engine loads recipes

At world initialization, the `RecipeManager` (a persistent server-side singleton) scans all `data/<namespace>/recipe/` directories. For each JSON file it finds:

1. It reads the `"type"` field to identify which `RecipeSerializer` to use.
2. The serializer uses a `MapCodec` to decode the JSON into a Java `Recipe` object.
3. That recipe is embedded into game memory for the duration of the session.

Changes to recipe files take effect after a world restart or the `/reload` command.

### Standard recipe types

| Type | Workstation |
|---|---|
| `minecraft:crafting_shaped` | Crafting table (specific pattern) |
| `minecraft:crafting_shapeless` | Crafting table (any arrangement) |
| `minecraft:smelting` | Furnace |
| `minecraft:blasting` | Blast furnace |
| `minecraft:smoking` | Smoker |
| `minecraft:stonecutting` | Stonecutter |
| `minecraft:smithing_transform` | Smithing table |

Modded workstations define their own types (e.g., `create:mixing`, `thermal:smelter`).

---

## 6.2 Shaped Recipes

Shaped recipes require items to be placed in a **specific geometric pattern** in the crafting grid.

### Full example (1.21.4+ syntax)

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
    "id": "tutorial:ruby_pickaxe",
    "count": 1
  }
}
```

### Schema breakdown

| Field | Description |
|---|---|
| `pattern` | Array of up to 3 strings, each representing a row in the 3×3 grid. Use `" "` for empty slots. All strings must be the same length. |
| `key` | Maps each character in the pattern to an item or tag. |
| `result` | The output item. `id` is required; `count` defaults to 1. |
| `category` | The Recipe Book tab: `"building"`, `"redstone"`, `"equipment"`, or `"misc"`. |

### Key syntax (1.21.4+)

In 1.21.4, key declarations were simplified significantly:

```json
// ✅ Single item — flat string (new in 1.21.4)
"X": "tutorial:ruby_ingot"

// ✅ Tag — prefix with #
"W": "#minecraft:logs"

// ✅ Multiple valid items — array (still supported)
"P": ["minecraft:oak_planks", "minecraft:birch_planks"]
```

> ⚠️ The old verbose format (`"X": { "item": "tutorial:ruby_ingot" }`) still works but is no longer necessary.

### Result with components

In 1.21+, the `result` block can include a `components` object to yield items pre-loaded with data components:

```json
"result": {
  "id": "tutorial:ruby_pickaxe",
  "count": 1,
  "components": {
    "minecraft:custom_name": "\"Ruby Pickaxe\""
  }
}
```

---

## 6.3 Shapeless Recipes

Shapeless recipes only check that the correct items are **present anywhere** in the grid — arrangement is irrelevant.

### Full example (1.21.4+ syntax)

```json
{
  "type": "minecraft:crafting_shapeless",
  "category": "misc",
  "ingredients": [
    "tutorial:pink_garnet_block",
    "minecraft:diamond",
    "minecraft:diamond"
  ],
  "result": {
    "id": "tutorial:infused_garnet",
    "count": 1,
    "components": {
      "minecraft:item_name": "{\"color\":\"dark_red\",\"text\":\"Infused Garnet\"}"
    }
  }
}
```

### Schema breakdown

| Field | Description |
|---|---|
| `ingredients` | A flat JSON array of item ID strings (or `#tag` strings). Up to 9 entries for a 3×3 grid. |
| `result` | Same as shaped recipes. |

> 📝 To require multiple copies of the same item, include the string **multiple times** in the array — once per required instance.

---

## 6.4 Error Handling and Debugging

The `RecipeManager` is extremely strict. A single syntax error will cause that recipe to silently disappear from the game with no crash.

### Common failure causes

| Mistake | Consequence |
|---|---|
| Trailing comma in JSON | `JsonParseException` — recipe not loaded |
| Unclosed bracket | `MalformedJsonException` — recipe not loaded |
| Uppercase letter in filename | File not found — recipe silently skipped |
| Wrong folder name (`recipes/` instead of `recipe/`) | Entire folder silently skipped |
| Using `recipes/` namespace in JSON `type` | Type not found — recipe silently skipped |

### Debugging in IntelliJ

1. Launch the game via the IntelliJ run configuration.
2. Open the IntelliJ **Run** panel and search for `Failed to load recipe` or `Skipping` in the log output.
3. The log will print the exact file path and reason for any recipe that failed to parse.

### Preventing errors with Datagen

For large projects, use **Data Generation (Datagen)** to write Java classes that programmatically output recipe JSON files during compilation. This eliminates hand-written JSON entirely.

```java
// NeoForge Datagen example
public class ModRecipeProvider extends RecipeProvider {
    @Override
    protected void buildRecipes(RecipeOutput output) {
        ShapedRecipeBuilder.shaped(RecipeCategory.EQUIPMENT, ModItems.RUBY_PICKAXE.get())
            .pattern("XXX")
            .pattern(" # ")
            .pattern(" # ")
            .define('X', ModItems.RUBY_INGOT.get())
            .define('#', Items.STICK)
            .unlockedBy("has_ruby", has(ModItems.RUBY_INGOT.get()))
            .save(output);
    }
}
```

Run the `runData` Gradle task in IntelliJ to generate the JSON files into your resources directory.

---

> **Next:** [Custom Totem of Undying](7.-Custom-Totem-of-Undying)
