## Complete setup guide RTX20 (SM75) and RTX30 (SM86)


### 1. Install NeuralScreen

Download and extract [NeuralScreen v2.6.4](https://github.com/perseval-BLR/NeuralScreen/releases/tag/v2.6.4).

### 2. Install the faster Neural Rendering runtime

Download the **F5 RTX 20/30** variant:

[nvngx_dlssnr_310.9.16-F5-RTX20-30.zip](https://github.com/thx114/HoYoShadeHub-DLSSNR-Runtimes/releases/download/nr-310.9.16/nvngx_dlssnr_310.9.16-F5-RTX20-30.zip)

Extract `nvngx_dlssnr.dll` and place it here:

```text
NeuralScreen\native\libraries\nvngx_dlssnr.dll
```

Back up the original runtime before replacing it.

In NeuralScreen, open **Fine tuning → NR preset → B**.

Preset B is the newer, smaller and faster model introduced with runtime 310.9.16. F5 is the RTX 20/30-specific runtime variant; B is the model selection.

### 3. Install the DLSS-G SM86 unlock

Download the complete x64 package:

[dlssg_for_sm86 v0.4.1](https://github.com/sdli1995/dlssg_for_sm86/releases/tag/0.4.1)

From the package root, copy these three items:

- `version.dll`
- `dlssg_sm86.ini`
- `dlssg_sm86\` (entire folder)

Place them inside:

```text
NeuralScreen\native\
```

This is important: put the proxy next to NeuralScreen's actual worker executable, `native\nvngx.dll`, not in the main application folder or the game's directory.

Do not replace or rename NeuralScreen's original `nvngx_dlssg.dll`. The SM86 proxy redirects its loading to the matching runtime bundled in `dlssg_sm86\runtime`.

### 4. Diagnostic configuration

For the initial test, use this `dlssg_sm86.ini`:

```ini
[General]
Enabled=1

[FrameGeneration]
Engine=DLSS
MaxGeneratedFrames=1

[DLSSG]
Optimized=0
Preset=A

[Logging]
Level=2
Directory=dlssg_sm86\logs

[Runtime]
Mode=Bundled

[SuperResolution]
Mode=Off
```

This uses 2X Frame Generation, stock kernel behavior and the bundled 310.9.16 runtime.

Note: `[DLSSG] Preset=A` is the Frame Generation preset. It is unrelated to NeuralScreen's Neural Rendering preset B.

### 5. Verify the installation

Launch NeuralScreen and inspect:

```text
NeuralScreen\native\dlssg_sm86\logs\dlssg_sm86.log
```

Look for:

```text
backend_loaded: loaded=true
kernel_pack: kernels_matched=71
install: actual_sm=86
backend_install: status=0
runtime_redirect: selected_source=bundled
```

These entries confirm that the SM86 backend, matching runtime and Ampere kernel pack have been loaded inside NeuralScreen's worker.

**Important test status:** The current diagnostic log still reports `0xBAD00012` at the NGX feature-requirements check, before `CreateFeature`. That is the remaining integration point to investigate.

### Hardware requirements

- NVIDIA RTX 20 / 30 / 40 / 50.
- Windows 10/11 x64.
- NVIDIA driver R580 or newer.
- Hardware-accelerated GPU scheduling enabled.

**References:**
- [NeuralScreen](https://github.com/perseval-BLR/NeuralScreen)
- [DLSSG SM86](https://github.com/sdli1995/dlssg_for_sm86)
- [NR runtime variants](https://github.com/thx114/HoYoShadeHub-DLSSNR-Runtimes)
- [SM86 technical background](https://dnim.dev/blog/dlssg-open-ampere-proton)
