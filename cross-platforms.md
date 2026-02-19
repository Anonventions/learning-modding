# 3. Cross-Platform Mods

> **Wiki home:** [Home](Home) | **Previous:** [Mod Loaders](2.-Mod-Loaders) | **Next:** [Porting to 1.21](4.-Porting-to-1.21)

Supporting both **Fabric** and **NeoForge** from a single codebase is tricky, but it avoids maintaining two separate projects. There are two established approaches.

---

## Why Bother?

Maintaining two separate codebases for the same mod means:
- Every bug fix must be applied twice
- Every new feature must be implemented twice
- Release cycles double in complexity
- The chance of the two versions drifting apart increases over time

A shared codebase solves all of this.

---

## Option 1: Architectury

[Architectury](https://github.com/architectury/architectury-api) is a framework and API that abstracts the differences between loaders so you can write most of your code once.

### Project structure

```
my-mod/
├── common/        ← shared, loader-agnostic code
├── fabric/        ← Fabric entry points and platform implementations
└── neoforge/      ← NeoForge entry points and platform implementations
```

### How it works

- The `common` module contains all your game logic and interacts only with vanilla Minecraft and the Architectury API.
- The `fabric` and `neoforge` modules handle loader-specific initialization and route platform events into the common module.
- The Architectury Gradle plugin manages the wiring between modules automatically.

### Setting up in IntelliJ

1. Clone or generate an Architectury project template.
2. Open the root folder in IntelliJ — it will be recognized as a multi-module Gradle project.
3. IntelliJ will show three separate modules in the **Project** panel: `common`, `fabric`, and `neoforge`.
4. Each module has its own source set but shares the root `gradle.properties`.

> 💡 Use the **Gradle tool window** to run tasks per-module. For example, run the `neoforge:runClient` task to launch the NeoForge client, or `fabric:runClient` for Fabric.

### `@ExpectPlatform`

When your shared `common` code needs to call platform-specific logic (e.g., a NeoForge-only registry API), use `@ExpectPlatform`:

```java
// In common/
@ExpectPlatform
public static boolean isModLoaded(String modId) {
    throw new AssertionError(); // replaced at compile time
}
```

```java
// In neoforge/ — must match the exact class path and method signature
public static boolean isModLoaded(String modId) {
    return ModList.get().isLoaded(modId);
}
```

```java
// In fabric/
public static boolean isModLoaded(String modId) {
    return FabricLoader.getInstance().isModLoaded(modId);
}
```

The Architectury Gradle plugin locates the platform implementations and injects the correct bytecode at build time.

### Downside

Users **must install the Architectury API mod** alongside your mod. This is a hard runtime dependency. If zero external dependencies for end users is a priority, use the option below.

---

## Option 2: Multiloader-Template

The [Multiloader-Template](https://github.com/jaredlll08/MultiLoader-Template) by jaredlll08 uses the same split structure as Architectury but requires **no external mods** for users.

### Project structure

```
my-mod/
├── common/        ← strictly vanilla-only code + Java interfaces
├── fabric/        ← implements common interfaces for Fabric
└── neoforge/      ← implements common interfaces for NeoForge
```

### How it works

1. The `common` module is compiled **only against vanilla Minecraft** — no loader APIs whatsoever.
2. For any platform-specific behaviour, you define a **Java interface** in `common`.
3. You write a concrete implementation of that interface in both `fabric` and `neoforge`.
4. The `common` code uses Java's built-in **`ServiceLoader`** to discover and instantiate the correct implementation at runtime.

### Example

**Interface in `common`:**

```java
public interface IPlatformHelper {
    boolean isModLoaded(String modId);
}
```

**NeoForge implementation:**

```java
public class NeoForgePlatformHelper implements IPlatformHelper {
    @Override
    public boolean isModLoaded(String modId) {
        return ModList.get().isLoaded(modId);
    }
}
```

**ServiceLoader registration** (`neoforge/src/main/resources/META-INF/services/`):

```
com.example.mymod.platform.IPlatformHelper
```

```
com.example.mymod.neoforge.NeoForgePlatformHelper
```

**Usage in `common`:**

```java
IPlatformHelper platform = ServiceLoader.load(IPlatformHelper.class)
    .findFirst()
    .orElseThrow();

platform.isModLoaded("jei");
```

### Setting up in IntelliJ

1. Clone the Multiloader-Template repository.
2. Open the root folder in IntelliJ as a Gradle project.
3. Three modules will appear: `common`, `fabric`, `neoforge`.
4. Each module has its own `runClient` / `runServer` Gradle task.

### Downside

There is significantly more manual work. You must write the bridging interface and ServiceLoader registration for every registry call, event hook, and rendering callback your mod uses. There is no automatic injection — every bridge is your responsibility.

---

## Comparison

| | Architectury | Multiloader-Template |
|---|---|---|
| User dependencies | Requires Architectury API mod | Zero external dependencies |
| Manual bridging required | Minimal — `@ExpectPlatform` handles it | Extensive — you write every bridge |
| IntelliJ multi-module support | ✅ Full | ✅ Full |
| Best for | Most cross-platform projects | Projects where zero dependencies is critical |

---

> **Next:** [Porting to 1.21](4.-Porting-to-1.21)
