# Redot Engine Installer (Flatpak & Snap)

This repository provides multiple installation options for Redot Engine, currently supporting both
**Flatpak** and **Snap** mechanisms.

## Installation Options

### Flatpak Installation

This version of Redot via Flatpak is not available on Flathub yet, but you can install it manually
by downloading the necessary files.

#### Steps to Install Flatpak Version:

**1. Download the Flatpak artifact:**

Choose one of the following download methods:

- **Manual Download**:

  Visit
  [this release page](https://github.com/Redot-Engine/org.redotengine.Redot/releases/tag/v4.3-beta.2)
  and download the file named `Redot-x86_64.zip`.

- **Download via `curl`**:

  ```bash
  curl https://github.com/Redot-Engine/org.redotengine.Redot/releases/download/v4.3-beta.2/Redot-x86_64.zip > Redot-x86_64.zip
  ```

**2. Install the Flatpak:**

- Unzip and install the downloaded `.flatpak` file:

   ```bash
   unzip Redot-x86_64.zip
   flatpak install --user Redot-x86_64.flatpak
   ```

#### Updating the Flatpak Version

For now, Flatpak updates aren't automatic. You'll need to manually download and install the latest
file each time a new version is released, following the above procedure.

#### Additional Steps for Using External Tools in Flatpak:

- **Using Blender**: Redot can integrate with Blender installed on your system. Follow the
  [Blender Integration for Flatpak Users](#using-blender-with-flatpak).
- **Using External Script Editor**: Learn how to configure external editors like Visual Studio Code
  to work within the Flatpak version in the
  [External Editor Configuration for Flatpak](#using-an-external-script-editor-in-flatpak).

### Snap Installation

This version of Redot via Snap is not available on the Snap Store yet, but you can install it
manually by downloading the necessary files.

#### Steps to Install Snap Version:

**1. Download the Snap Artifact:**

- **Manual Download**:

  Head over to
  [this release page](https://github.com/Redot-Engine/org.redotengine.Redot/releases/tag/v4.3-beta.2)
  to download the `Redot.snap` file.

- **Download using `curl`**:

  ```bash
  curl https://github.com/Redot-Engine/org.redotengine.Redot/releases/download/v4.3-beta.2/Redot.snap > Redot.snap
  ```

**2. Install the Snap:**

- Install the Snap file by running:

```bash
sudo snap install --dangerous Redot.snap
```

The `--dangerous` flag is required because the file is manually downloaded and not from the Snap
Store.

#### Updating the Snap Version

For now, snap updates aren't automatic. You'll need to manually download and install the latest
`.snap` file each time a new version is released, following the above procedure.

### Choosing Between Flatpak and Snap

Both formats offer similar capabilities in terms of functionality. Flatpak provides more flexibility
with external editor and tool integration, while Snap users may experience quicker installs. Choose
the one based on your system preference or distribution support.

---

## Integration with External Tools

### Using Blender with Flatpak

Redot can be integrated with Blender for project asset creation. Users of the Flatpak version need
to configure Blender using the following setup.

If Blender is installed from the system (e.g., using distribution packages):

```bash
#!/bin/bash
flatpak-spawn --host blender "$@"
```

If Blender is installed from Flathub:

```bash
#!/bin/bash
flatpak-spawn --host flatpak run org.blender.Blender "$@"
```

After saving this script, ensure it's executable using `chmod +x blender`. In Redot, navigate to
**Editor Settings > Filesystem > Import > Blender > Blender 3 Path** and set the directory to the
location of the `blender` script.

### Using an External Script Editor in Flatpak

Redot can spawn external editors from inside the Flatpak sandbox. You must prefix the command with
`flatpak-spawn --host`.

For instance, if you're using Visual Studio Code, configure it as follows:

- Normally:

  ```text
  Exec Path:  code
  Exec Flags: --reuse-window {project} --goto {file}:{line}:{col}
  ```

- With Flatpak:

  ```text
  Exec Path:  flatpak-spawn
  Exec Flags: --host code --reuse-window {project} --goto {file}:{line}:{col}
  ```

This ensures that the editor is launched outside the sandbox.

## Limitations (For Flatpak & Snap)

- C#/Mono support via org.redotengine.RedotSharp is **not yet supported** at this time.

## Building from the Source (Flatpak)

```bash
git clone --recursive https://github.com/Redot-Engine/org.redotengine.Redot.git
cd org.redotengine.Redot/
git submodule init
git submodule update
flatpak install --user flathub org.freedesktop.Sdk//24.08 -y
flatpak-builder --force-clean --install --user -y builddir org.redotengine.Redot.yaml
```

1. Ensure you have Git installed. Follow the
   [Flatpak builder setup guide](https://docs.flatpak.org/en/latest/first-build.html).

2. Clone and prepare the repository:

   ```bash
   git clone --recursive https://github.com/Redot-Engine/org.redotengine.Redot.git
   cd org.redotengine.Redot/
   git submodule init
   git submodule update
   flatpak install --user flathub org.freedesktop.Sdk//24.08 -y
   ```

3. Build and install the Flatpak (using ccache for faster builds):

   ```bash
   flatpak-builder --force-clean --install --user --ccache -y builddir org.redotengine.Redot.yaml
   ```
