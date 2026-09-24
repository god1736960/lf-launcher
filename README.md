<div align="center">

<br/>

# LF Launcher

**A modern, high-performance Minecraft launcher built with Tauri, React & Rust**

[![Version](https://img.shields.io/badge/version-1.0.0--beta-0ea5e9?style=flat-square)](https://github.com/Danchoimod/lflauncher-pc/releases)
[![License](https://img.shields.io/badge/license-GPL--3.0-22c55e?style=flat-square)](LICENSE.txt)
[![Platform](https://img.shields.io/badge/platform-Windows-6366f1?style=flat-square)](#system-requirements)
[![Build](https://img.shields.io/badge/build-passing-22c55e?style=flat-square)](https://github.com/Danchoimod/lflauncher-pc/actions)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-f59e0b?style=flat-square)](CONTRIBUTING.md)

[Download](#installation) · [Documentation](#getting-started) · [Report Bug](https://github.com/Danchoimod/lflauncher-pc/issues) · [Request Feature](https://github.com/Danchoimod/lflauncher-pc/discussions)

<br/>

</div>

---

## Overview

LF Launcher is a desktop application for managing and launching Minecraft across multiple versions and mod loaders. Built on **Tauri 2** with a **React** frontend and **Rust** backend, it prioritizes memory efficiency, security, and a smooth user experience — all without the overhead of Electron-based alternatives.

> **Beta Notice:** Version 1.0.0 is in active development. Core features are stable; expect continued improvements.

---

## Features

### Game Management
- **Multi-loader support** — Vanilla, Forge, Fabric, Quilt, and Bedrock Edition
- **One-click installation** — automatic version download, extraction, and verification
- **Multi-instance launching** — run multiple game instances simultaneously
- **Custom launch parameters** — per-profile JVM arguments, resolution, and more

### Account & Authentication
- **Multiple account support** — manage several accounts in a single launcher
- **Ely.by & Offline** — flexible authentication providers
- **Secure token storage** — encrypted credential management with automatic refresh

### Java Management
- **Auto-detection** — scans your system for installed Java runtimes
- **Auto-install** — downloads the correct Java version for each Minecraft release
- **Multi-version support** — keeps separate runtimes for different game versions

### Performance & Monitoring
- **Low memory footprint** — GPU disabled by default, saving 60–100 MB RAM
- **Single process mode** — reduces background resource usage
- **Real-time resource monitor** — track memory usage while playing
- **Async runtime** — non-blocking operations via Tokio

### Developer & Power User
- **Log management** — view, filter, and export game logs
- **Debug mode** — detailed output for troubleshooting

---

## System Requirements

| | Minimum | Recommended |
|---|---|---|
| **OS** | Windows 10 (build 17763+) | Windows 11 |
| **RAM** | 2 GB | 4 GB+ |
| **Storage** | 2 GB free | 5 GB+ free |
| **Java** | 8+ (auto-installable) | 21 LTS |

### Build Requirements

| Tool | Version |
|---|---|
| Node.js | v16+ |
| Rust | 1.70+ |
| Visual Studio Build Tools | 2019+ |

---

## Installation

### Stable Release

Download the latest installer from [**Releases**](https://github.com/Danchoimod/lflauncher-pc/releases):

```
LFLauncher-v1.0.0-setup.exe
```

Run the installer and launch from your Start Menu or Desktop shortcut.

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/Danchoimod/lflauncher-pc.git
cd lflauncher-pc
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment (Optional)

```bash
cp .env.example .env
# Edit .env with your API keys, backend URLs, etc.
```

### 4. Start Development Server

```bash
npm run dev
```

This starts the Vite dev server at `http://localhost:5173` and opens the Tauri application window with hot reload enabled.

### Available Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start development server with hot reload |
| `npm run build` | Build for production |
| `npm run preview` | Preview the production build locally |
| `npm run tauri` | Run Tauri CLI commands |

---

## Architecture

### Project Structure

```
lflauncher-pc/
├── src/                        # Frontend (TypeScript + React)
│   ├── components/             # Reusable UI components
│   ├── pages/                  # Page layouts and routes
│   ├── hooks/                  # Custom React hooks
│   ├── styles/                 # Tailwind CSS configuration
│   ├── utils/                  # Shared utilities
│   └── App.tsx                 # Root component
│
└── src-tauri/                  # Backend (Rust + Tauri)
    ├── src/
    │   ├── main.rs             # Application entry point
    │   ├── lib.rs              # Command exports & Tauri setup
    │   ├── models.rs           # Data structures & serialization
    │   └── modules/
    │       ├── game/           # Launch, versions, installation
    │       ├── auth/           # Authentication & accounts
    │       ├── profiles/       # Profile management
    │       ├── settings/       # Launcher preferences
    │       ├── java/           # Java detection & installation
    │       └── shared_utils/   # Common utilities
    ├── Cargo.toml              # Rust dependencies
    └── build.rs                # Build script
```

### Technology Stack

| Layer | Technologies |
|---|---|
| **Frontend** | React 19, TypeScript 5.8, Vite 7, Tailwind CSS 4, Framer Motion, React Router, Skinview3D |
| **Backend** | Tauri 2, Rust 2021 Edition, Tokio, Reqwest, Serde, Windows API |
| **Security** | SHA-1/SHA-256 verification, encrypted credential storage, job object isolation |
| **Tooling** | Node.js, npm, Cargo, Vite |

---

## Building for Production

```bash
npm run build
```

Output locations:

| Artifact | Path |
|---|---|
| Frontend bundle | `dist/` |
| Compiled binary | `src-tauri/target/release/` |
| Installer packages | `src-tauri/target/release/bundle/` |

### Advanced Build Options

```bash
# Release build for the library crate only
cargo build --release -p lflauncher_lib

# Strip debug symbols for a smaller binary
cargo build --release -C strip=symbols
```

### Customizing `tauri.conf.json`

```json
{
  "package": {
    "productName": "LF Launcher",
    "version": "0.1.0"
  },
  "build": {
    "distDir": "../dist",
    "devUrl": "http://localhost:5173"
  }
}
```

---


> **Tip:** Setting `disable_gpu: true` is recommended for most users — it reduces baseline RAM usage significantly with no impact on gameplay.

---

## Troubleshooting

<details>
<summary><strong>High memory usage</strong></summary>

- Ensure `disable_gpu` is set to `true` in `config.json`
- Close other background applications
- Reduce the allocated RAM in your profile settings if over-provisioned

</details>

<details>
<summary><strong>Game won't launch</strong></summary>

1. Open the **Java Management** tab and verify a compatible runtime is installed
2. Check game logs via **Launcher → Logs** for error details
3. Review your profile settings (version, mod loader, JVM arguments)
4. Enable debug mode for verbose output

</details>

<details>
<summary><strong>Authentication issues</strong></summary>

- Go to **Settings → Accounts** and clear stored credentials
- Re-authenticate with your chosen backend
- Verify your network connection and that your authentication service is reachable

</details>

<details>
<summary><strong>Build failures</strong></summary>

```bash
# Clear cached build artifacts
cargo clean

# Verify your Rust toolchain is up to date
rustup update

# Confirm Node.js version (must be v16+)
node --version

# Rebuild
npm run build
```

</details>

---

## Contributing

Contributions are welcome. Please follow the standard GitHub workflow:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Commit your changes with a descriptive message
4. Push to your fork: `git push origin feature/your-feature-name`
5. Open a Pull Request against `main`

For significant changes, open a [Discussion](https://github.com/Danchoimod/lflauncher-pc/discussions) first to align on direction.

---

## Acknowledgments

LF Launcher draws inspiration from:

- [**MCMrARM/mc-w10-version-launcher**](https://github.com/MCMrARM/mc-w10-version-launcher) — pioneering multi-version Bedrock management on Windows
- [**PrismLauncher**](https://github.com/PrismLauncher/PrismLauncher) — the gold standard for modern, multi-platform Minecraft launchers

---

## License

This project is licensed under the **GNU General Public License v3.0**. See [LICENSE.txt](LICENSE.txt) for the full terms.

- **Source code:** GPL-3.0
- **Logo & assets:** Creative Commons BY-SA 4.0
- **Third-party libraries:** See [CREDITS.md](CREDITS.md) for full attribution (Tauri: Apache 2.0/MIT · React: MIT · Rust std: Apache 2.0/MIT)

---

<div align="center">

**[GitHub](https://github.com/Danchoimod/lflauncher-pc)** · **[Releases](https://github.com/Danchoimod/lflauncher-pc/releases)** · **[Issues](https://github.com/Danchoimod/lflauncher-pc/issues)**

<sub>Version 0.1.0 · Last updated May 2026 · Windows-first · Linux/macOS planned</sub>

</div>
