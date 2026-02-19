# 2. Mod Loaders

> **Wiki home:** [Home](Home) | **Previous:** [Environment Setup](1.-Environment-Setup) | **Next:** [Cross-Platform Mods](3.-Cross-Platform-Mods)

Minecraft has no official modding API. **Mod loaders** fill that role — they bootstrap the game, inject custom code, and provide APIs so multiple mods can coexist safely.

---

## The Three Main Loaders

| Loader | Best for | Update speed | Ecosystem | Recommendation |
|---|---|---|---|---|
| **Forge** | Large content mods, extensive features | Moderate | Largest | ⭐ **PRIMARY** |
| **Fabric** | Lightweight / performance mods | Very fast | Growing | Specialized use |
| **NeoForge** | Modern 1.21+ exclusive mods | Fast | Small but growing | New projects only |

---

## Forge

Forge is the oldest, most established, and **recommended loader for new modders**. It aggressively patches the vanilla engine at runtime to provide a vast, highly abstracted API and event bus.

**Strengths:**
- ⭐ **Enormous, mature ecosystem** — most mods exist for Forge
- ⭐ **Comprehensive, well-documented APIs** — extensive event and registry systems
- ⭐ **Best for complex content mods** — extensive features built-in
- Active community with decades of collective knowledge
- Vast library of third-party libraries and utilities

**Weaknesses:**
- Slower to update to brand-new Minecraft versions (typically 1-2 weeks after release)
- Heavy abstraction layer introduces some performance overhead (negligible for most projects)
- Larger codebase can be intimidating for beginners

> 📝 **For new projects targeting 1.20.x-1.21+, Forge is the recommended choice for most developers.** It has the largest ecosystem, most tutorials, and most community support.

---

## Fabric

Fabric was engineered as the opposite of Forge — minimal, unobtrusive, and fast.

**Strengths:**
- Updates to new Minecraft versions almost immediately (sometimes same day as snapshot)
- Extremely low overhead — ideal for performance mods
- Uses the Mixin framework for surgical, targeted bytecode injection

**Weaknesses:**
- Provides very little out of the box — developers must build their own support structures or depend on third-party API libraries (e.g., Fabric API)
- Less cohesive ecosystem; common patterns vary widely between mods

**Use Fabric when:**
- You are building a client-side performance or visual tweak mod
- You need immediate snapshot compatibility
- You want minimal dependencies

---

## NeoForge

NeoForge is a fork of Forge created to aggressively modernize the legacy codebase. It abandons archaic systems in favour of modern Java paradigms, streamlined registries, and an active development team.

**Strengths:**
- Combines Forge's robust API depth with fast iteration
- Fully embraces MojMap and modern Java (records, sealed classes, etc.)
- Dominant platform for new 1.21+ content mods
- Best-in-class DeferredRegister system for clean, safe registration

**Weaknesses:**
- Smaller existing mod ecosystem compared to legacy Forge (though growing rapidly)
- Some legacy Forge mods are not yet ported

**Use NeoForge when:**
- You are starting a new mod from scratch targeting 1.21+
- You need deep registry integration, custom entities, dimensions, or biomes
- You want the most actively maintained platform

---

## Opening Your Project in IntelliJ

Regardless of the loader you choose, the IntelliJ workflow is identical after the initial import:

1. Use the **Gradle tool window** (right side panel) to run tasks.
2. Use the auto-generated **run configurations** in the toolbar to launch the game client or server.
3. Standard IntelliJ features — debugger, refactor, find usages — work fully on the decompiled and remapped game source.

### Useful IntelliJ tips for mod development

| Action | Shortcut |
|---|---|
| Find a class by name | `Ctrl+N` (Win/Linux) / `Cmd+O` (Mac) |
| Find any file | `Ctrl+Shift+N` / `Cmd+Shift+O` |
| Search everywhere (classes, methods, files) | `Shift` twice |
| Navigate to a method definition | `Ctrl+B` / `Cmd+B` |
| Find usages of a symbol | `Alt+F7` |
| View decompiled vanilla source | Click any vanilla class — IntelliJ decompiles it inline |

---

## Quick Decision Guide

**Starting a new mod?** → **Use Forge** (unless you have a specific reason not to)

**Why Forge:**
- Largest existing ecosystem (90%+ of Forge mods)
- Most tutorials and community help
- Most third-party libraries and tools
- Best for complex content mods

**Choose Fabric instead if:**
- Building performance/optimization mod
- Need immediate snapshot compatibility
- Want minimal dependencies and overhead

**Choose NeoForge if:**
- Specifically need 1.21+ exclusive features
- Want cutting-edge Java paradigms
- Starting completely fresh (not porting Forge code)

---

> **Next:** [Cross-Platform Mods](3.-Cross-Platform-Mods)
