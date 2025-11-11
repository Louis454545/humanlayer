# Linux Build Workflow Usage

## Triggering the Workflow

### 1. Manual Trigger (Workflow Dispatch)
You can manually trigger the Linux build from GitHub Actions:

```bash
# Via GitHub CLI
gh workflow run "Build Linux Release Artifacts" --repo humanlayer/humanlayer
```

Or through the GitHub web interface:
1. Go to Actions tab
2. Select "Build Linux Release Artifacts"
3. Click "Run workflow"
4. Choose options:
   - **Release version**: Leave empty for auto-increment, or specify like `0.2.0`
   - **Build nightly**: Check for nightly build

### 2. Git Tag Push (Stable Release)
Push a semver tag to trigger a stable release:

```bash
git tag -a v0.2.0 -m "Release v0.2.0"
git push origin v0.2.0
```

## Build Outputs

### AppImage (Universal Linux)
- **File**: `CodeLayer-{version}-x86_64.AppImage`
- **Usage**:
  ```bash
  chmod +x CodeLayer-*.AppImage
  ./CodeLayer-*.AppImage
  ```

### Debian Package
- **File**: `codelayer_{version}_amd64.deb`
- **Installation**:
  ```bash
  sudo dpkg -i codelayer_*.deb
  sudo apt-get install -f  # Fix dependencies if needed
  ```

### Arch Linux
For Arch users, you can:
1. Use the AppImage directly
2. Convert the .deb using `debtap`
3. Extract and manually install files

## Installation Examples

### Ubuntu/Debian
```bash
# Download the .deb from releases
wget https://github.com/humanlayer/humanlayer/releases/download/v0.2.0/codelayer_0.2.0_amd64.deb
sudo dpkg -i codelayer_0.2.0_amd64.deb
sudo apt-get install -f

# Launch
codelayer
```

### Arch Linux (using debtap)
```bash
# Install debtap if needed
yay -S debtap

# Convert and install
debtap codelayer_0.2.0_amd64.deb
sudo pacman -U codelayer-0.2.0-1-x86_64.pkg.tar.zst
```

### Universal (AppImage)
```bash
# Download AppImage
wget https://github.com/humanlayer/humanlayer/releases/download/v0.2.0/CodeLayer-0.2.0-x86_64.AppImage
chmod +x CodeLayer-0.2.0-x86_64.AppImage

# Run
./CodeLayer-0.2.0-x86_64.AppImage
```

## Nightly vs Stable Builds

### Stable Builds
- Triggered by version tags (v0.2.0, v1.0.0, etc.)
- Use standard paths: `~/.humanlayer/daemon.db`, port 7777
- Binary names: `humanlayer`, `codelayer`, `hld`

### Nightly Builds
- Triggered by schedule or manual dispatch with nightly flag
- Use isolated paths: `~/.humanlayer/daemon-nightly.db`, port 7778
- Binary names: `humanlayer-nightly`, `codelayer-nightly`, `hld-nightly`
- Can be installed alongside stable builds

## Troubleshooting

### AppImage: "cannot execute: required file not found"
Install FUSE:
```bash
# Ubuntu/Debian
sudo apt-get install fuse libfuse2

# Arch Linux
sudo pacman -S fuse2
```

### Missing Dependencies
If the .deb installation fails:
```bash
sudo apt-get install -f
```

This will automatically install any missing dependencies.
