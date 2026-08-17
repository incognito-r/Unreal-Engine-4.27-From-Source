# Unreal Engine 4.27.2 — Source Build on Windows

Simple installation and build guide for Unreal Engine 4.27.2 from source.

## 1. Prerequisites

Install:

- Windows 10 or Windows 11
- Git
- Visual Studio 2019
- Visual Studio **Desktop development with C++** workload
- A compatible Windows 10 SDK

Recommended for UE 4.27.2:

- MSVC / Visual Studio 2019
- Windows 10 SDK 10.0.19041.0

## 2. Clone UE 4.27.2

Open Command Prompt:

```bat
cd /d E:\
git clone --branch 4.27.2-release --depth 1 https://github.com/EpicGames/UnrealEngine.git UnrealEngine
cd /d E:\UnrealEngine
```

Verify the checkout:

```bat
git log -1 --oneline
git describe --tags
git rev-parse HEAD
```

The tag should be:

```text
4.27.2-release
```

## 3. Download Dependencies

Open Command Prompt in the Unreal Engine directory:

```bat
cd /d E:\UnrealEngine
```

Run:

```bat
Setup.bat
```

Wait for Setup.bat to finish downloading the required dependencies.

### If Setup.bat gives a CDN 403 error

If Setup.bat reports:

```text
cdn.unrealengine.com ... 403 Forbidden
```

replace:

```text
Engine\Build\Commit.gitdeps.xml
```

with the correct updated Epic dependency manifest for UE 4.27.2.

Then run:

```bat
Setup.bat
```

again.

## 4. Generate Project Files

After Setup.bat finishes, run:

```bat
GenerateProjectFiles.bat
```

This generates the UE4 solution and project files.

## 5. Build UE4

Open the generated:

```text
UE4.sln
```

in a supported IDE such as Visual Studio.

Select:

```text
Development Editor
Win64
```

Build the **UE4** / **UE4Editor** target.

The first source build can take a long time.

## 6. UE 4.27.2 Build Issue: UnrealFileServer / HeadlessChaos

UE 4.27.2 can encounter a known build failure involving:

```text
UnrealFileServer
HeadlessChaos
```

with cascading errors such as:

```text
GetWorld override did not override
FChaosEngineInterface undefined
ShaderCompiler.h cannot be opened
FAsyncPreRegisterDDCRequest undeclared
```

If this occurs during a normal Development Editor build, exclude these two projects/targets from the **Development Editor / Win64** solution configuration:

```text
HeadlessChaos
UnrealFileServer
```

Keep:

```text
UE4 / UE4Editor
```

enabled.

Do **not** delete their source directories.

They are auxiliary targets and are not required for the normal UE4 Editor build.

## 7. After a Successful Build

The source-built editor will be located at:

```text
Engine\Binaries\Win64\UE4Editor.exe
```

For example:

```text
E:\UnrealEngine\Engine\Binaries\Win64\UE4Editor.exe
```

This is the editor built from your source tree.

## 8. Recommended Setup

Keep the source-built engine separate from any Epic Games Launcher installation:

```text
Epic Games Launcher UE 4.27
    → Normal/reference UE4 installation

E:\UnrealEngine
    → Source-built UE 4.27.2

UE5
    → Separate UE5 installation
```

This allows the source-built engine to be modified without affecting the normal Launcher installation.

## Notes

- A shallow Git clone (`--depth 1`) is sufficient for building the tagged UE 4.27.2 source.
- If a build is interrupted, you normally do not need to delete the entire `Intermediate` or `Binaries` directory. UnrealBuildTool can reuse valid build outputs when the build is started again.
- Keep the original UE4 source intact until the initial build succeeds.
