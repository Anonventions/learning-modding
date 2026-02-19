# 2. Mod Loaders

> **Wiki home:** [Home](Home) | **Previous:** [Environment Setup](1.-Environment-Setup) | **Next:** [Cross-Platform Mods](3.-Cross-Platform-Mods)

Minecraft has no official modding API. **Mod loaders** fill that role — they bootstrap the game, inject custom code, and provide APIs so multiple mods can coexist safely.

---

## The Three Main Loaders

| Loader | Best for | Update speed | Overhead |
|---|---|---|---|
| **Forge** | Large, established content mods | Slow | High |
| **Fabric** | Lightweight / performance mods | Very fast | Minimal |
| **NeoForge** | Modern content mods (1.21+) | Fast | Moderate |

---

## Forge

Forge is the oldest and most established loader. It aggressively patches the vanilla engine at runtime to provide a vast, highly abstracted API and event bus.

**Strengths:**
- Enormous existing mod ecosystem
- Comprehensive, mature event and registry APIs
- Well-documented for legacy versions

**Weaknesses:**
- Slowest to update to new Minecraft versions
- Heavy abstraction layer introduces noticeable performance overhead
- Legacy codebase accumulated significant technical debt, leading to the NeoForge fork

> 📝 For new projects targeting 1.21+, Forge is generally **not recommended**. NeoForge is its modern successor.

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

```
Are you building a performance or client-side tweak?
  └─ Yes → Fabric

Are you starting a new content mod for 1.21+?
  └─ Yes → NeoForge

Do you need to support an existing Forge mod ecosystem?
  └─ Yes → Forge (but consider porting to NeoForge long-term)

Do you need both Fabric and NeoForge support?
  └─ Yes → See Cross-Platform Mods
```

---

> **Next:** [Cross-Platform Mods](3.-Cross-Platform-Mods)
