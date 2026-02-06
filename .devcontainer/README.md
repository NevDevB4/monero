# Monero Development Container

This directory contains the configuration for developing Monero in a containerized environment using Visual Studio Code Insiders (or VS Code).

## What's Included

This development container includes:

- **Base OS**: Ubuntu 22.04 LTS
- **Build Tools**: GCC, G++, CMake, Make, pkg-config
- **Monero Dependencies**: All required libraries including Boost, ZMQ, libunbound, libsodium, etc.
- **Development Tools**: 
  - GDB debugger
  - ccache for faster compilation
  - Valgrind for memory analysis
  - Doxygen for documentation generation
  - Git and GitHub CLI
- **VS Code Extensions**:
  - C/C++ IntelliSense
  - CMake Tools
  - GitHub Copilot (if available)
  - GitLens

## Prerequisites

1. Install [Docker Desktop](https://www.docker.com/products/docker-desktop)
2. Install [Visual Studio Code Insiders](https://code.visualstudio.com/insiders/) or [Visual Studio Code](https://code.visualstudio.com/)
3. Install the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

## Getting Started

1. Open VS Code (or VS Code Insiders)
2. Open this repository folder
3. When prompted, click "Reopen in Container" (or use Command Palette: `Dev Containers: Reopen in Container`)
4. Wait for the container to build (first time will take several minutes)
5. Once ready, you'll have a fully configured Monero development environment

## Building Monero

Once inside the container, you can build Monero:

```bash
# Initialize and update submodules (done automatically on container creation)
git submodule update --init --recursive

# Build using make
make -j$(nproc)

# Or build using CMake directly
mkdir -p build && cd build
cmake ..
make -j$(nproc)
```

## Port Forwarding

The container automatically forwards these ports:
- **18080**: Monero P2P port
- **18081**: Monero RPC port

These will be accessible on your host machine when running the Monero daemon.

## Running Monero

After building, you can run the Monero daemon:

```bash
./build/release/bin/monerod
```

Or the wallet CLI:

```bash
./build/release/bin/monero-wallet-cli
```

## Customization

You can customize the development environment by editing:
- `.devcontainer/devcontainer.json` - VS Code settings and extensions
- `.devcontainer/Dockerfile` - Container image and installed packages

## Debugging

The container is configured with debugging support:
- GDB is pre-installed
- SYS_PTRACE capability is enabled
- Security options are configured for debugging

To debug, use VS Code's built-in debugger with the C++ extension.

## Notes

- The container runs as a non-root user `vscode` for security
- ccache is configured to speed up recompilation
- All git operations work normally inside the container
- Your git credentials are automatically forwarded from the host
