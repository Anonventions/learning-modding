# 1. Environment Setup

> **Wiki home:** [Home](Home) | **Next:** [Mod Loaders](2.-Mod-Loaders)

This page walks you through setting up a complete Minecraft 1.21+ modding workspace inside **IntelliJ IDEA**.

---

## 1.1 Installing Java 21 via IntelliJ IDEA

Minecraft 1.21 strictly requires **Java 21**. You do not need to install Java separately — IntelliJ can download and manage JDKs for you.

### Download a JDK from inside IntelliJ

1. Open IntelliJ IDEA.
2. Go to **File → Project Structure → SDKs**.
3. Click the **+** button → **Download JDK**.
4. Set the version to **21**.
5. Choose a vendor. Recommended options:
   - **Eclipse Temurin (Adoptium)** — community favourite, well tested
   - **Microsoft OpenJDK** — solid alternative
6. Click **Download** and wait for IntelliJ to install it automatically.
7. Once installed, set it as the **Project SDK** under **File → Project Structure → Project**.

> ⚠️ **You must use a 64-bit JDK.** IntelliJ will only show 64-bit options when using the built-in downloader, so you are covered as long as you use this method.

### Verify the JDK in the terminal

Open IntelliJ's built-in terminal (**View → Tool Windows → Terminal**) and run:

```bash
java -version
```

Expected output:

```
openjdk version "21.x.x" ...
64-Bit Server VM
```

If you see `32-Bit` anywhere, revisit **File → Project Structure** and confirm the correct SDK is selected.

---

## 1.2 Importing an MDK and Letting Gradle Run

### Getting an MDK

Download a **Mod Development Kit (MDK)** from your chosen mod loader:

- **NeoForge MDK:** [https://neoforged.net](https://neoforged.net)
- **Fabric:** Use the [Fabric Template Mod Generator](https://fabricmc.net/develop/template/)

Extract the MDK to a folder on your machine.

### Importing into IntelliJ

1. Open IntelliJ IDEA.
2. Click **Open** (or **File → Open**).
3. Navigate to the extracted MDK folder and select it.
4. IntelliJ will detect the `build.gradle` file and ask if you want to open it as a **Gradle project** — click **Open as Project**.
5. The Gradle tool window will appear in the right panel and begin syncing automatically.

> ⏳ **This first sync takes a long time.** Gradle is downloading Minecraft, decompiling the game, and pulling in all dependencies. Let it finish before touching anything.

### What Gradle does automatically

| Task | What happens |
|---|---|
| Downloads game binaries | Fetches the exact Minecraft version you targeted |
| Decompiles & remaps | Converts obfuscated bytecode into readable mapped source |
| Resolves dependencies | Downloads all libraries your mod and the loader need |
| Generates run configs | Creates IntelliJ run configurations for client, server, and data gen |

### Rules to follow

- ✅ Put your mod version, Minecraft version, and mapping versions in **`gradle.properties`**
- ❌ Do not edit `build.gradle` or `settings.gradle` unless you have a specific, well-understood reason
- If Gradle fails or gets into a bad state, run this in the terminal:

```bash
./gradlew --refresh-dependencies
```

Or on Windows:

```bash
gradlew.bat --refresh-dependencies
```

---

## 1.3 Mappings Explained

Minecraft's compiled code is heavily obfuscated. Class names like `a` and method names like `m_1234_` tell you nothing. **Mappings** are translation dictionaries that convert those names into readable, meaningful ones.

### The three mapping sets you will encounter

| Mapping Set | What it is | Status |
|---|---|---|
| **MojMap** | Mojang's officially published de-obfuscation maps | ✅ Current standard across all platforms |
| **Yarn** | Fabric's custom mapping project | ⚠️ Being phased out in favour of MojMap |
| **Parchment** | Community layer on top of MojMap — adds parameter names and Javadoc | ✅ Highly recommended |

### Why MojMap became the standard

Mojang began officially publishing their internal name mappings. Both the NeoForge and Fabric projects aligned to this standard. This means tutorials, source references, and community help now all use the same class and method names — a huge improvement over the old fragmented ecosystem.

### Adding Parchment in IntelliJ

MojMap alone gives you readable class and method names, but **no parameter names**. Parchment fixes this.

In your `build.gradle`, add the Parchment repository and specify both the Minecraft version and the Parchment release:

```groovy
// In your repositories block
repositories {
    maven { url = 'https://maven.parchmentmc.org' }
}
```

Then in `gradle.properties`, set your versions:

```properties
parchment_minecraft_version=1.21.1
parchment_mappings_version=2024.11.17
```

> ⚠️ You must match the Parchment version to the correct Minecraft version. Using a mismatched version will cause build failures. Check [https://parchmentmc.org](https://parchmentmc.org) for the latest releases.

### What Parchment looks like in IntelliJ

Without Parchment, a method signature in IntelliJ might look like:

```java
public void m_6843_(Level p_1, BlockPos p_2, BlockState p_3) { }
```

With Parchment applied:

```java
public void onPlace(Level level, BlockPos pos, BlockState state) { }
```

This makes navigating the vanilla codebase inside IntelliJ dramatically easier.

---

## Checklist Before Moving On

- [ ] Java 21 (64-bit) is installed and set as the Project SDK in IntelliJ
- [ ] MDK is imported and Gradle has fully synced without errors
- [ ] Run configurations (client/server) are visible in the IntelliJ toolbar
- [ ] Parchment is configured and parameter names are visible in decompiled sources

---

> **Next:** [Mod Loaders](2.-Mod-Loaders)
