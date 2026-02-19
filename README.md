# 🎮 Comprehensive Minecraft Modding Wiki - README

This is a **comprehensive, production-ready wiki** for learning to develop Minecraft mods from scratch through advanced implementation, with emphasis on 1.21+ and modern development paradigms.

---

## 📚 Wiki Contents

### **Core Learning Path**

1. **[0. Introduction & Overview](0.-Introduction-Overview.md)**
   - The modding ecosystem and why 1.21 is revolutionary
   - Timeline of modding evolution
   - Three major breakthroughs: MojMap, Data Components, De-hardcoding
   - Critical mindset shifts (Imperative→Declarative, Magic Strings→Type Safety, Inheritance→Composition)
   - **Read this first** to understand the landscape

2. **[1. Environment Setup](Environment-Setup.md)**
   - Installing Java 21 JDK via IntelliJ
   - Importing a Mod Development Kit (MDK)
   - Understanding Gradle and what it does automatically
   - Mappings explained: MojMap, Yarn, and Parchment
   - **Prerequisites:** Have IntelliJ IDEA installed

3. **[2. Mod Loaders](mod-loaders.md)**
   - Comparison: Forge vs Fabric vs NeoForge
   - Architectural philosophies and trade-offs
   - Performance implications and update speed
   - Decision guide for choosing your platform
   - **Decision point:** Which loader to target?

4. **[3. Cross-Platform Mods](cross-platforms.md)**
   - Supporting both Fabric and NeoForge
   - Architectury framework (with external API dependency)
   - Multiloader-Template (zero external dependencies)
   - ServiceLoader pattern for platform abstraction
   - **Advanced topic:** Not required for single-loader projects

5. **[4. Porting to 1.21](porting-to-1.21.md)**
   - Git versioning and workspace management
   - Asset directory standardization (singular vs plural)
   - ResourceLocation → Identifier API migration
   - Updating metadata files (neoforge.mods.toml)
   - **For:** Developers migrating legacy mods

6. **[5. Data Component System](data-component-system.md)**
   - Why NBT was removed from ItemStack
   - Removal of hardcoded base classes (SwordItem, DiggerItem, etc.)
   - Creating custom components with Codecs
   - Registration and network synchronization
   - **Most important:** This is the biggest 1.21 change

7. **[6. Writing Recipes](writing-recipes.md)**
   - JSON recipe structure and syntax
   - Shaped vs shapeless crafting
   - Smelting and other furnace-type recipes
   - Datagen for programmatic recipe generation
   - Error handling and debugging

8. **[7. Custom Totem of Undying](writing-recipes-totem-example.md)**
   - Complex item logic deep dive
   - Vanilla internals and hardcoded resurrection logic
   - Legacy approach: Event interception and Bytecode injection
   - Modern approach: death_protection component
   - **Example:** Shows the paradigm shift in action

### **Practical Implementation**

9. **[8. Example Mod Setup](8.-Example-Mod-Setup.md)**
   - Complete step-by-step walkthrough
   - Building a "Tutorial Mod" with Ruby items/blocks
   - Project structure and file organization
   - Running your mod in-game
   - **Hands-on:** Follow this to create your first working mod

10. **[9. Registry Systems](9.-Registry-Systems.md)**
    - Understanding registries and DeferredRegister
    - Safe registration patterns
    - Custom blocks, items, entities
    - Sound registration
    - Dimension registration
    - **Key concept:** How to properly register game objects

11. **[10. Events & Networking](10.-Events-Networking.md)**
    - NeoForge Event Bus and lifecycle hooks
    - Common events (block breaks, entity spawns, player joins)
    - Custom network packets
    - Client-server synchronization
    - Critical networking rules
    - **Advanced:** Hooks into game lifecycle

---

## 🚀 How to Use This Wiki

### **Complete Beginner?**
Follow this path in order:
1. Introduction & Overview (30 min) — Understand the landscape
2. Environment Setup (1 hour) — Set up your workspace
3. Mod Loaders (30 min) — Choose NeoForge for new projects
4. Example Mod Setup (2-3 hours) — Create your first working mod
5. Registry Systems (1 hour) — Learn proper registration
6. Events & Networking (1.5 hours) — Add interactivity

**Total time: ~6-8 hours** to have a functional, extendable mod project

### **Experienced Java Developer?**
Jump to topics as needed:
- Overview of paradigm shifts: [Data Component System](data-component-system.md)
- Practical example: [Example Mod Setup](8.-Example-Mod-Setup.md)
- Advanced patterns: [Events & Networking](10.-Events-Networking.md)

### **Porting a 1.20.x Mod?**
1. [Porting to 1.21](porting-to-1.21.md) — Structural changes
2. [Data Component System](data-component-system.md) — Major rewrites needed
3. [Writing Recipes](writing-recipes.md) — Update recipe syntax
4. [Events & Networking](10.-Events-Networking.md) — Modernize event handling

### **Building Multi-Loader Support?**
1. Understand trade-offs: [Mod Loaders](mod-loaders.md)
2. Choose architecture: [Cross-Platform Mods](cross-platforms.md)
3. Follow your chosen template's structure

---

## 📊 Wiki Statistics

| Metric | Value |
|---|---|
| **Total Pages** | 10 comprehensive documents |
| **Total Code Examples** | 50+ complete, copy-paste-ready samples |
| **Estimated Read Time** | 6-8 hours for complete beginner |
| **Primary Target Version** | Minecraft 1.21.5+ |
| **Primary Loader** | NeoForge (with Fabric notes) |
| **IDE Assumption** | IntelliJ IDEA |
| **Programming Language** | Java 21 |

---

## 🎯 Key Concepts Covered

### Architecture & Theory
- ✅ Evolution of the modding ecosystem
- ✅ Mod loader philosophies and trade-offs
- ✅ Data-driven vs imperative architecture
- ✅ Type safety and immutability
- ✅ Client-server architecture

### Practical Skills
- ✅ Setting up development environment
- ✅ Creating and registering items/blocks
- ✅ Writing JSON recipes
- ✅ Custom entity implementation
- ✅ Event-driven programming
- ✅ Network synchronization
- ✅ Complex item behavior

### Advanced Topics
- ✅ Cross-platform development
- ✅ Version migration and porting
- ✅ Custom components and codecs
- ✅ Data generation (Datagen)
- ✅ Bytecode-level modifications (legacy)

---

## 🔗 External Resources Referenced

| Resource | Purpose |
|---|---|
| [NeoForge Documentation](https://docs.neoforged.net/) | Official NeoForge API docs |
| [Fabric Wiki](https://fabricmc.net/wiki/) | Fabric framework documentation |
| [Minecraft Wiki (Technical)](https://minecraft.wiki/) | Vanilla game mechanics reference |
| [Parchment Mappings](https://parchmentmc.org/) | Parameter names and documentation |
| [IntelliJ IDEA](https://www.jetbrains.com/idea/) | Primary IDE |

---

## 💡 Philosophy Behind This Wiki

### Depth Over Breadth
Each topic is explored thoroughly with rationale, history, architecture, and practical examples. We explain **why** systems work the way they do, not just **how** to use them.

### Evolution Context
We document historical approaches alongside modern ones, explaining what changed and why. This helps readers understand not just code, but the reasoning behind it.

### Copy-Paste Ready
Every code example is tested and ready to use. Readers can copy entire sections directly into their projects.

### IntelliJ-Centric
All workflows and UI references assume IntelliJ IDEA. This is the de facto standard in the modding community.

### 1.21+ Forward
While we explain legacy approaches for context, modern methods take absolute priority. This wiki is **not** for Minecraft 1.20.x or earlier.

---

## 🛠️ The Example Mod: Tutorial Mod

Throughout this wiki, we reference a practical example: the **Tutorial Mod**, a simple NeoForge mod that adds:

- Ruby Ore (custom block)
- Raw Ruby (raw material)
- Ruby Ingot (smelted from raw ruby)
- Ruby Sword (crafted from ingots + stick)
- Ruby Block (crafted from 9 ingots)

This mod demonstrates:
- ✅ Item and block registration
- ✅ Custom textures and models
- ✅ Shaped and shapeless recipes
- ✅ Smelting recipes
- ✅ Language files (item/block names)

Complete implementation is in [Example Mod Setup](8.-Example-Mod-Setup.md).

---

## ✅ Verification Checklist

After working through this wiki, you should be able to:

- [ ] Install Java 21 and configure it in IntelliJ
- [ ] Download and import a mod development kit
- [ ] Explain the architectural differences between Forge, Fabric, and NeoForge
- [ ] Register custom blocks, items, and entities using DeferredRegister
- [ ] Write shaped and shapeless recipe JSON files
- [ ] Understand and use the Data Component system
- [ ] Hook into game events using the Event Bus
- [ ] Create and send custom network packets
- [ ] Debug compilation and runtime errors
- [ ] Port a legacy mod to 1.21+

---

## 🐛 Troubleshooting Common Issues

| Problem | Solution | Reference |
|---|---|---|
| Java version mismatch | Install Java 21 via IntelliJ | [Environment Setup](Environment-Setup.md) |
| Gradle sync hangs | Run `./gradlew --refresh-dependencies` | [Environment Setup](Environment-Setup.md) |
| Items don't appear in inventory | Check DeferredRegister is registered in main class | [Registry Systems](9.-Registry-Systems.md) |
| Missing textures (pink/magenta) | Verify texture file paths match exactly | [Example Mod Setup](8.-Example-Mod-Setup.md) |
| Recipes don't work | Validate JSON syntax with [JSONLint](https://jsonlint.com/) | [Writing Recipes](writing-recipes.md) |
| Network desync between client/server | Use `context.enqueueWork()` in packet handlers | [Events & Networking](10.-Events-Networking.md) |

---

## 📖 Reading Order Recommendations

### Path 1: From Zero to First Mod (Beginner)
```
Overview → Setup → Mod Loaders → Example Mod → Registry → Events
↓
You have a working mod with custom blocks, items, and recipes
```

### Path 2: Modernizing Legacy Knowledge (Intermediate)
```
Overview → Data Components → Porting → Registry → Events & Networking
↓
You understand 1.21 paradigm shifts and can port existing mods
```

### Path 3: Multi-Loader Excellence (Advanced)
```
Overview → Mod Loaders → Cross-Platform → Registry → Events
↓
You maintain a single codebase for multiple loaders
```

### Path 4: Complex Features (Advanced)
```
Overview → Data Components → Events & Networking → Registry Systems
↓
You can implement sophisticated, networked game mechanics
```

---

## 📝 Contributing to This Wiki

This wiki is designed to be comprehensive but maintainable. If you notice:

- **Outdated information** (game version changed, API modified)
- **Unclear explanations** (something doesn't make sense)
- **Missing examples** (a concept needs demonstration)
- **Broken links** (references don't work)

Please note these for improvement!

---

## 🎓 Learning Outcomes

By completing this wiki, learners will:

1. **Understand** the modding ecosystem, its history, and modern paradigm
2. **Master** the development environment and toolchain (Gradle, IntelliJ, mappings)
3. **Apply** proper registration patterns for game objects
4. **Implement** data-driven systems (recipes, components, configurations)
5. **Build** complex game logic using events and networking
6. **Deploy** mods across multiple loader platforms
7. **Port** legacy code to modern standards
8. **Debug** compilation and runtime errors systematically

---

## 🌟 Special Features

### Comprehensive Code Examples
Every section includes real, working code that can be copied directly into a project. Examples progress from simple to complex.

### Architectural Explanations
We don't just show "how" — we explain "why" each system is designed the way it is.

### Visual Diagrams
Complex concepts are supplemented with ASCII diagrams for clarity.

### Cross-References
Topics link to related material, allowing readers to explore connections between concepts.

### Historical Context
Legacy approaches are documented for understanding, with clear marking of modern replacements.

---

## 📞 Support Resources

While this wiki is comprehensive, you may also want to:

- **Join the modding community:** NeoForge Discord, Fabric Discord, r/Minecraft Modding
- **Read official docs:** [NeoForge Docs](https://docs.neoforged.net/), [Fabric Wiki](https://fabricmc.net/wiki/)
- **Search existing mods:** Study open-source mods on GitHub
- **Ask questions:** Forums, Discord, Reddit modding communities

---

## 📜 License & Attribution

This wiki synthesizes knowledge from:
- Official Minecraft and mod loader documentation
- Community best practices
- Years of modding ecosystem evolution

Created as a comprehensive educational resource for the Minecraft modding community.

---

## 🎯 Quick Links

- **[Home Page](home.md)** — Main navigation hub
- **[Start Learning](0.-Introduction-Overview.md)** — Begin with the overview
- **[Get Hands-On](8.-Example-Mod-Setup.md)** — Jump to practical example
- **[Quick Reference](#wiki-contents)** — All topics listed above

---

**Last updated:** February 2026 | **For:** Minecraft 1.21.5+ | **Using:** NeoForge, IntelliJ IDEA, Java 21

Happy modding! 🎮


