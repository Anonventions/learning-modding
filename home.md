# 🎮 Comprehensive Minecraft Modding Wiki (1.21+)

Welcome to the **Comprehensive Minecraft Modding Wiki**. This wiki provides exhaustive documentation on starting, developing, porting, and distributing Minecraft modifications for version 1.21+ and beyond. Whether you're a complete beginner or an experienced developer transitioning to the modern ecosystem, this resource covers the full lifecycle of modding with emphasis on IntelliJ IDEA and the sophisticated toolchains that power contemporary modding.

---

## 📚 Table of Contents

### **Getting Started**
| Page | Description |
|---|---|
| [0. Introduction & Overview](0.-Introduction-Overview) | **START HERE** — The modding landscape, ecosystem evolution, and what makes 1.21 different |
| [1. Environment Setup](1.-Environment-Setup) | Java 21 JDK configuration, IntelliJ integration, Gradle fundamentals, and mappings |
| [2. Mod Loaders](2.-Mod-Loaders) | Forge vs Fabric vs NeoForge — architectural philosophies, performance, and use cases |

### **Core Architecture**
| Page | Description |
|---|---|
| [3. Cross-Platform Mods](3.-Cross-Platform-Mods) | Multi-loader development with Architectury and Multiloader-Template |
| [4. Porting to 1.21](4.-Porting-to-1.21) | Git versioning, asset standardization, ResourceLocation/Identifier migration |
| [5. Data Component System](5.-Data-Component-System) | NBT deprecation, removing hardcoded base classes, custom component registration |

### **Data-Driven Systems**
| Page | Description |
|---|---|
| [6. Writing Recipes](6.-Writing-Recipes) | Shaped/shapeless recipes, JSON syntax, Datagen approach, error handling |
| [7. Custom Totem of Undying](7.-Custom-Totem-of-Undying) | Complex item logic, vanilla internals, legacy approach, and modern death_protection component |

### **Advanced Topics**
| Page | Description |
|---|---|
| [8. Example Mod Setup](8.-Example-Mod-Setup) | Complete walkthrough of a NeoForge mod project structure |
| [9. Registry Systems](9.-Registry-Systems) | Creating blocks, items, entities, dimensions, and custom registries |
| [10. Events & Networking](10.-Events-Networking) | Event buses, network packets, and client-server synchronization |

---

## 🚀 Quick Start Paths

### **I'm completely new to modding**
1. Read [0. Introduction & Overview](0.-Introduction-Overview)
2. Follow [1. Environment Setup](1.-Environment-Setup)
3. Choose a loader in [2. Mod Loaders](2.-Mod-Loaders)
4. Set up your first project with [8. Example Mod Setup](8.-Example-Mod-Setup)

### **I'm porting a 1.20.x mod to 1.21+**
1. Review [4. Porting to 1.21](4.-Porting-to-1.21)
2. Study [5. Data Component System](5.-Data-Component-System)
3. Update recipes in [6. Writing Recipes](6.-Writing-Recipes)
4. Adapt complex logic with [7. Custom Totem of Undying](7.-Custom-Totem-of-Undying)

### **I want to support multiple loaders**
1. Understand the trade-offs in [2. Mod Loaders](2.-Mod-Loaders)
2. Choose your abstraction in [3. Cross-Platform Mods](3.-Cross-Platform-Mods)
3. Execute the strategy in your project

### **I need to implement advanced features**
- Custom registries: [9. Registry Systems](9.-Registry-Systems)
- Network synchronization: [10. Events & Networking](10.-Events-Networking)
- Complex items: [7. Custom Totem of Undying](7.-Custom-Totem-of-Undying)

---

## 📖 Documentation Philosophy

This wiki is organized with the following principles:

- **Depth-first coverage:** Each topic is explained thoroughly with rationale, architecture, and practical examples.
- **Evolution context:** We explain why systems changed, how they worked historically, and how modern approaches improve upon them.
- **Practical code examples:** Every concept includes runnable, copy-paste-ready examples.
- **IntelliJ-centric:** All tooling and workflows assume IntelliJ IDEA as the primary IDE.
- **1.21+ focus:** Legacy information is provided for context, but modern approaches take precedence.

---

## ⚙️ The Modern Modding Paradigm

### Key Shifts in 1.21

| Aspect | Legacy Approach | Modern Approach (1.21+) |
|---|---|---|
| **Item Data** | Unstructured NBT CompoundTag | Strongly-typed Data Components |
| **Item Classes** | Hardcoded base classes (SwordItem, etc.) | Generic Item + component composition |
| **De-obfuscation** | Fragmented mapping standards (Yarn, MCP) | Unified MojMap + Parchment |
| **Recipes** | Some Java-based, some JSON | Data-driven JSON exclusively |
| **Death Logic** | Hardcoded in LivingEntity | Abstracted death_protection component |
| **Workspace** | Manual decompilation | Gradle handles everything |

### Why These Changes Matter

These aren't cosmetic improvements—they represent a philosophical shift toward **data-driven, type-safe, modular architecture**:

- **Type Safety:** Compile-time errors replace runtime surprises
- **Performance:** Direct object manipulation replaces dynamic tag parsing
- **Modularity:** Components compose instead of requiring inheritance hierarchies
- **Maintainability:** Data-driven definitions are self-documenting and declarative

---

## 🔗 External Resources

- **NeoForge Documentation:** https://docs.neoforged.net/
- **Fabric Wiki:** https://fabricmc.net/wiki/
- **Minecraft Wiki (Technical):** https://minecraft.wiki/
- **Parchment Mappings:** https://parchmentmc.org/
- **IntelliJ IDEA:** https://www.jetbrains.com/idea/

---

## 💡 Tips for Success

1. **Bookmark this wiki** and return to it frequently—modding has many interconnected concepts
2. **Use IntelliJ's "Decompiled Source" feature** to read vanilla code directly
3. **Join modding communities** (Discord, Reddit, Minecraft Forums) for peer support
4. **Test incrementally** — compile and run your mod frequently to catch errors early
5. **Read error messages carefully** — Gradle and the game engine provide detailed diagnostics

---

> 🎯 **Ready to begin?** [Start with the Introduction & Overview →](0.-Introduction-Overview)
