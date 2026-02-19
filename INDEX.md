# 📑 Complete Wiki Index

This document provides a complete file listing and navigation guide for the Minecraft Modding Wiki.

---

## 📚 Complete File Structure

### **Main Entry Points**

| File | Purpose |
|---|---|
| **[README.md](README.md)** | Overview of the entire wiki, quick reference, and learning paths |
| **[home.md](home.md)** | Main wiki homepage with navigation and quick start paths |
| **[QUICK-REFERENCE.md](QUICK-REFERENCE.md)** | Lookup guide for common syntax, registrations, and JSON formats |

---

### **Learning Path: Foundations (1-2)**

| # | File | Topics | Duration |
|---|---|---|---|
| **0** | [**Introduction & Overview**](0.-Introduction-Overview.md) | <ul><li>Modding ecosystem architecture</li><li>Historical evolution (eras 1-4)</li><li>1.21 paradigm shifts: MojMap, Data Components, De-hardcoding</li><li>Mod loader comparison</li><li>Toolchain overview</li><li>Learning paths and prerequisites</li></ul> | 30-45 min |
| **1** | [**Environment Setup**](Environment-Setup.md) | <ul><li>Installing Java 21 JDK via IntelliJ</li><li>Importing MDK and Gradle sync</li><li>Understanding Gradle tasks</li><li>Mappings: MojMap, Yarn, Parchment</li><li>Adding Parchment for parameter names</li><li>Verification checklist</li></ul> | 1 hour |

---

### **Learning Path: Platform Decision (2-3)**

| # | File | Topics | Duration |
|---|---|---|---|
| **2** | [**Mod Loaders**](mod-loaders.md) | <ul><li>Forge: oldest, largest ecosystem, recommended</li><li>Fabric: lightweight, fast, minimal</li><li>NeoForge: modern, balanced, 1.21+ exclusive</li><li>Trade-offs and decision matrix</li><li>IntelliJ workflow for each loader</li><li>Useful IDE shortcuts</li></ul> | 30-45 min |
| **3** | [**Cross-Platform Mods**](cross-platforms.md) | <ul><li>Multi-loader development rationale</li><li>Architectury framework (with API dependency)</li><li>Multiloader-Template (zero dependencies)</li><li>@ExpectPlatform annotation</li><li>ServiceLoader pattern</li><li>Project structure for both approaches</li></ul> | 1 hour |

---

### **Learning Path: Modern Architecture (4-7)**

| # | File | Topics | Duration |
|---|---|---|---|
| **4** | [**Porting to 1.21**](porting-to-1.21.md) | <ul><li>Git versioning strategy</li><li>Asset directory standardization (singular vs plural)</li><li>ResourceLocation → Identifier migration</li><li>mods.toml changes</li><li>Manual refactoring steps</li><li>Gradle dependency updates</li></ul> | 1-1.5 hours |
| **5** | [**Data Component System**](data-component-system.md) | <ul><li>Why NBT was removed (type safety, performance)</li><li>Removal of hardcoded base classes</li><li>Component registration with Codecs</li><li>MapCodec vs StreamCodec</li><li>Network synchronization</li><li>Creating custom components</li></ul> | 1.5-2 hours |
| **6** | [**Writing Recipes**](writing-recipes.md) | <ul><li>Recipe JSON structure</li><li>Shaped crafting matrices</li><li>Shapeless recipes</li><li>Smelting, smoking, blasting</li><li>Stonecutting and custom recipe types</li><li>Recipe conditions</li><li>Datagen for programmatic generation</li><li>Error handling and debugging</li></ul> | 1 hour |
| **7** | [**Custom Totem of Undying**](writing-recipes-totem-example.md) | <ul><li>Vanilla resurrection logic (hardcoded in LivingEntity)</li><li>Legacy approach: Event interception + Mixins</li><li>Legacy approach: Custom network packets</li><li>Modern approach: death_protection component</li><li>Advantages of data-driven design</li><li>End-to-end example implementation</li></ul> | 1.5-2 hours |

---

### **Learning Path: Practical Implementation (8-10)**

| # | File | Topics | Duration |
|---|---|---|---|
| **8** | [**Example Mod Setup**](8.-Example-Mod-Setup.md) | <ul><li>Step-by-step MDK import and setup</li><li>gradle.properties configuration</li><li>neoforge.mods.toml metadata</li><li>Creating registration classes (ModBlocks, ModItems)</li><li>DeferredRegister pattern</li><li>Creating texture assets</li><li>Item and block model JSONs</li><li>Recipe JSON files</li><li>Language files (en_us.json)</li><li>Running in-game test</li><li>Complete file structure reference</li></ul> | 2-3 hours |
| **9** | [**Registry Systems**](9.-Registry-Systems.md) | <ul><li>What is a registry and why it matters</li><li>DeferredRegister pattern and safety</li><li>Registering blocks, items, entities</li><li>Entity rendering setup</li><li>Sound registration</li><li>Dimension registration</li><li>Organization best practices</li><li>Common mistakes and solutions</li></ul> | 1-1.5 hours |
| **10** | [**Events & Networking**](10.-Events-Networking.md) | <ul><li>Event Bus architecture</li><li>Creating event listeners (@SubscribeEvent)</li><li>Common event types (entity, player, block, world)</li><li>Event prioritization and phases</li><li>Custom network packets</li><li>Client-server synchronization</li><li>Critical networking rules</li><li>Debugging network issues</li></ul> | 2 hours |

---

## 🎯 Quick Navigation

### **By Topic**

#### Setup & Configuration
- [Environment Setup](Environment-Setup.md) — Java, IDE, Gradle
- [Example Mod Setup](8.-Example-Mod-Setup.md) — Complete walkthrough

#### Architecture & Design
- [Introduction & Overview](0.-Introduction-Overview.md) — Ecosystem and paradigm
- [Mod Loaders](mod-loaders.md) — Platform selection
- [Cross-Platform Mods](cross-platforms.md) — Multi-loader support

#### Core Systems
- [Registry Systems](9.-Registry-Systems.md) — Game object registration
- [Data Component System](data-component-system.md) — Item data storage
- [Writing Recipes](writing-recipes.md) — Crafting and smelting

#### Advanced Features
- [Events & Networking](10.-Events-Networking.md) — Game hooks and packets
- [Custom Totem of Undying](writing-recipes-totem-example.md) — Complex items
- [Porting to 1.21](porting-to-1.21.md) — Upgrading legacy mods

#### Reference
- [QUICK-REFERENCE.md](QUICK-REFERENCE.md) — Code snippets and templates

---

### **By Experience Level**

#### Absolute Beginner
1. [Introduction & Overview](0.-Introduction-Overview.md)
2. [Environment Setup](Environment-Setup.md)
3. [Mod Loaders](mod-loaders.md)
4. [Example Mod Setup](8.-Example-Mod-Setup.md)
5. [Registry Systems](9.-Registry-Systems.md)
6. [Events & Networking](10.-Events-Networking.md)

#### Experienced Java Developer
1. [Introduction & Overview](0.-Introduction-Overview.md) (skim)
2. [Mod Loaders](mod-loaders.md) (decision)
3. [Data Component System](data-component-system.md) (deep dive)
4. [Example Mod Setup](8.-Example-Mod-Setup.md) (practical)
5. [Registry Systems](9.-Registry-Systems.md)
6. [Events & Networking](10.-Events-Networking.md)

#### Porting Existing Mod
1. [Porting to 1.21](porting-to-1.21.md) (structural changes)
2. [Data Component System](data-component-system.md) (major rewrites)
3. [Writing Recipes](writing-recipes.md) (syntax updates)
4. [Events & Networking](10.-Events-Networking.md) (modernization)

#### Multi-Loader Development
1. [Mod Loaders](mod-loaders.md) (trade-offs)
2. [Cross-Platform Mods](cross-platforms.md) (architecture)
3. [Registry Systems](9.-Registry-Systems.md)
4. [Events & Networking](10.-Events-Networking.md)

---

### **By Time Available**

#### 1-2 Hours
- [Introduction & Overview](0.-Introduction-Overview.md)
- [Mod Loaders](mod-loaders.md)

#### 2-4 Hours
- [Environment Setup](Environment-Setup.md)
- [Data Component System](data-component-system.md)
- [QUICK-REFERENCE.md](QUICK-REFERENCE.md)

#### 4-8 Hours (Complete Beginner)
- Complete learning path 1-2 above (Foundations, 2 hours)
- [Example Mod Setup](8.-Example-Mod-Setup.md) (3 hours)
- [Registry Systems](9.-Registry-Systems.md) (1-2 hours)
- [Events & Networking](10.-Events-Networking.md) (2 hours)

#### 8+ Hours (Comprehensive)
- Read all 10 learning path documents
- Study [QUICK-REFERENCE.md](QUICK-REFERENCE.md)
- Complete [Example Mod Setup](8.-Example-Mod-Setup.md) practical exercise

---

## 📊 Wiki Statistics

| Metric | Value |
|---|---|
| **Total Pages** | 14 documents |
| **Core Learning Pages** | 10 pages |
| **Reference/Index Pages** | 4 pages |
| **Total Code Examples** | 50+ |
| **Total Words** | ~35,000+ |
| **Estimated Full Read Time** | 8-12 hours |
| **Target Minecraft Version** | 1.21.5+ |
| **Target Loader** | NeoForge (Fabric notes included) |
| **Target IDE** | IntelliJ IDEA |
| **Target Language** | Java 21 |

---

## 🔗 File Relationships

```
README.md ──────── Overview & learning paths
    ↓
home.md ────────── Navigation hub
    ├─→ 0. Introduction & Overview ──→ Foundations
    │   ├─→ 1. Environment Setup ───────┐
    │   │   ├─→ 2. Mod Loaders ────────┐
    │   │   │   ├─→ 3. Cross-Platform  │
    │   │   │   └─→ 4. Porting ────────┤
    │   │   └─→ 5. Data Components ────┤─→ 8. Example Mod
    │   ├─→ 6. Writing Recipes ────────┤   (Practical)
    │   └─→ 7. Custom Totem ──────────┴─→ 9. Registry
    │                                   └─→ 10. Events
    │
    └─→ QUICK-REFERENCE.md (Lookup for any page)
```

---

## 🎯 Key Concepts by Page

### Introduction & Overview
- **Core concept:** Understanding the evolution and paradigm shifts
- **Key takeaway:** 1.21 represents a shift from hardcoded to data-driven

### Environment Setup
- **Core concept:** Setting up the development toolchain
- **Key takeaway:** Gradle handles compilation, decompilation, and remapping

### Mod Loaders
- **Core concept:** Different platforms have different philosophies
- **Key takeaway:** NeoForge is best for new 1.21+ content mods

### Cross-Platform Mods
- **Core concept:** Supporting multiple loaders from one codebase
- **Key takeaway:** Trade-off between dependency count and maintenance effort

### Porting to 1.21
- **Core concept:** Systematic approach to upgrading legacy code
- **Key takeaway:** Asset renames and APIidentifier migration are major pain points

### Data Component System
- **Core concept:** Type-safe, immutable data storage on items
- **Key takeaway:** Replaces unstructured NBT with strongly-typed components

### Writing Recipes
- **Core concept:** Data-driven crafting and smelting
- **Key takeaway:** JSON is self-documenting and doesn't require recompilation

### Custom Totem of Undying
- **Core concept:** Complex item behavior from hardcoded to data-driven
- **Key takeaway:** Modern approach eliminates Mixins and network packets

### Example Mod Setup
- **Core concept:** Hands-on implementation of all prior concepts
- **Key takeaway:** "Learn by doing" with a complete, working example

### Registry Systems
- **Core concept:** Safe, deferred initialization of game objects
- **Key takeaway:** DeferredRegister prevents initialization order bugs

### Events & Networking
- **Core concept:** Hooking into game lifecycle and client-server communication
- **Key takeaway:** Server is source of truth; client-side code is for rendering

---

## 🚀 Getting Started Paths

### Path A: Quick Start (Complete Beginner)
```
HOME → Overview (30 min) → Setup (1 hr) → 
Example Mod (3 hrs) → Running mod in-game ✓
```
**Total: ~4.5 hours**

### Path B: Comprehensive (Complete Beginner)
```
HOME → All 10 pages in order → 
Example Mod (3 hrs) → Registry (1.5 hrs) → 
Events (2 hrs) → Advanced project ✓
```
**Total: ~12 hours**

### Path C: Modernizing Legacy (Experienced Dev)
```
HOME → Overview (skim 15 min) → Data Components (2 hrs) →
Porting (1.5 hrs) → Registry (1 hr) → Events (1.5 hrs) ✓
```
**Total: ~6.5 hours**

### Path D: Multi-Loader Mastery (Advanced)
```
HOME → Overview → Mod Loaders → Cross-Platform →
Registry → Events → Example Mod → Multi-loader project ✓
```
**Total: ~8 hours**

---

## ✅ Completion Checklist

After reading through the entire wiki, you should be able to:

- [ ] Understand the history and evolution of Minecraft modding
- [ ] Set up a complete development environment with Java 21 and IntelliJ
- [ ] Choose the appropriate mod loader for your project
- [ ] Create and register custom items, blocks, and entities
- [ ] Write JSON recipes for crafting and smelting
- [ ] Use the Data Component system for custom item data
- [ ] Implement complex game logic using events and networking
- [ ] Debug compilation and runtime errors
- [ ] Deploy mods across different loader platforms
- [ ] Port legacy mods to 1.21+

---

## 📞 Support & External Resources

### When Stuck
- **Check:** [QUICK-REFERENCE.md](QUICK-REFERENCE.md) for syntax
- **Search:** Use your browser's find (Ctrl+F) within relevant page
- **Read:** Related pages (linked at top/bottom of each page)
- **Join:** NeoForge Discord, Fabric Discord, r/MinecraftModding

### Official Docs
- [NeoForge Documentation](https://docs.neoforged.net/)
- [Fabric Wiki](https://fabricmc.net/wiki/)
- [Minecraft Wiki (Technical)](https://minecraft.wiki/)

### Community
- NeoForge Discord: [https://discord.neoforged.net/](https://discord.neoforged.net/)
- Fabric Discord: [https://discord.gg/v6v4pMv](https://discord.gg/v6v4pMv)
- r/MinecraftModding: [https://reddit.com/r/MinecraftModding](https://reddit.com/r/MinecraftModding)

---

## 📝 Document History

| Version | Date | Changes |
|---|---|---|
| **1.0** | Feb 2026 | Initial comprehensive wiki release |
| | | 10 learning path documents |
| | | 4 reference documents |
| | | 50+ code examples |
| | | Complete Example Mod walkthrough |

---

## 🎓 Learning Outcomes

By the end of this wiki, learners will:

1. **Understand** the modding ecosystem and its evolution
2. **Master** the development environment and toolchain
3. **Implement** proper registration patterns
4. **Build** data-driven systems
5. **Create** complex game mechanics
6. **Deploy** across multiple platforms
7. **Debug** systematically
8. **Maintain** and port legacy code

---

## 💡 Pro Tips

- 📌 **Bookmark this index** — Reference it often
- 🔍 **Use Ctrl+F** — Search within pages for keywords
- 💾 **Keep QUICK-REFERENCE handy** — Lookup syntax without searching
- 📚 **Read in order first time** — Later you can jump between pages
- 🤝 **Share with others** — Help the community grow

---

## 🎯 Next Steps

1. **Choose your starting point** above based on experience level
2. **Open the first recommended page**
3. **Follow along with the material**
4. **Complete the Example Mod Setup**
5. **Build your own mod!**

---

**Ready to start modding?** Begin with [Introduction & Overview](0.-Introduction-Overview.md) →

**Need a quick code snippet?** Check [QUICK-REFERENCE.md](QUICK-REFERENCE.md) →

**Lost in navigation?** You're in the right place! Check the sections above.

---

**Last updated:** February 2026 | **For:** Minecraft 1.21.5+ | **Using:** NeoForge, IntelliJ IDEA, Java 21


