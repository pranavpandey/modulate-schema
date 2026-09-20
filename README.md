<img src="./graphics/icon.png" height="160">

# Modulate Schema

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)
[![Latest Release](https://img.shields.io/github/v/release/pranavpandey/modulate-schema?color=e75889&label=schema)](https://github.com/pranavpandey/modulate-schema/releases)
[![Download](https://img.shields.io/badge/modulate-v1.0.0-e75889?style=flat&logo=gumroad&logoColor=82e1ca)](https://modulate.pranavpandey.com)

The **Modulate Schema** is an open, declarative JSON specification for defining real-time DSP audio algorithms, dynamic control parameters, dynamic bindings, and adaptive UI themes. It powers the Modulate Audio Engine, allowing creators and developers to build, distribute, and monetize customizable preset files (`.json`) or bundled preset packs (`.json` / `.modpack` / `.zip`) without writing C++ or compiling native binaries.

---

## Subtle Tremolo

Here is a complete, production-ready preset JSON demonstrating smooth sine-wave amplitude modulation with custom light theme styling.

```json
{
  "version": "1.0.0",
  "metadata": {
    "name": "Subtle Tremolo",
    "author": "Pranav Pandey",
    "category": "Modulation",
    "pack_id": "factory",
    "description": "Smooth sine-wave amplitude modulation with dynamic depth scaling and wave shape warping."
  },
  "dsp": {
    "block_formula": "lfoFreq := 2.0 * PI * (0.1 + K2 * 9.9); depth := K1 * 0.5 * (1.0 + envelopeValue * 0.5);",
    "sample_formula": "x * (1.0 - (depth * (1.0 + sin(lfoFreq * t + (K3 * sin(lfoFreq * t))))))",
    "buffer_size": 512,
    "oversampling": 1
  },
  "controls": [
    {
      "id": "K1",
      "label": "Depth",
      "default": 0.5,
      "min": 0.0,
      "max": 1.0,
      "step": 0.01,
      "is_bipolar": false
    },
    {
      "id": "K2",
      "label": "Rate (Hz)",
      "default": 0.3,
      "min": 0.0,
      "max": 1.0,
      "step": 0.01,
      "is_bipolar": false
    },
    {
      "id": "K3",
      "label": "Wave Shape Warp",
      "default": 0.0,
      "min": 0.0,
      "max": 1.0,
      "step": 0.01,
      "is_bipolar": false
    }
  ],
  "bindings": [
    {
      "symbol": "envelopeValue",
      "target_param_id": "K1",
      "min": 0.0,
      "max": 1.0
    }
  ],
  "ui": {
    "theme_mode": "light",
    "background_color": "#E0F2FE",
    "surface_color": "#F0F9FF",
    "accent_color": "#0284C7",
    "text_primary_color": "#0C4A6E",
    "text_secondary_color": "#0369A1",
    "error_color": "#EF4444"
  }
}
```

---

<img src="./graphics/thumbnail.png" height="160">

## Preset Schema

The generic blueprint below outlines the data types, allowed enumerations, and execution scopes required for building compliant Modulate presets.

```json
{
  "version": "string (e.g., 1.0.0)",
  "metadata": {
    "name": "string",
    "author": "string",
    "category": "string",
    "pack_id": "string",
    "description": "string"
  },
  "dsp": {
    "block_formula": "string (runs once per block; e.g., 'gain := K1 * 2.0;')",
    "sample_formula": "string (runs per sample; returns float; e.g., 'x * gain')",
    "buffer_size": "integer (16|32|64|128|256|512|1024|2048|4096|8192)",
    "oversampling": "integer (1|2|4|8)"
  },
  "controls": [
    {
      "id": "string (K1|K2|K3|K4)",
      "label": "string",
      "default": "float",
      "min": "float",
      "max": "float",
      "step": "float",
      "is_bipolar": "boolean"
    }
  ],
  "bindings": [
    {
      "symbol": "string (envelopeValue|modWheelValue|pitchBendValue|lfoSine|lfoTri|lfoSaw|lfoSqr)",
      "target_param_id": "string (K1|K2|K3|K4)",
      "min": "float",
      "max": "float"
    }
  ],
  "ui": {
    "theme_mode": "auto|dark|light",
    "background_color": "optional|color",
    "surface_color": "optional|color",
    "accent_color": "optional|color",
    "text_primary_color": "optional|color",
    "text_secondary_color": "optional|color",
    "error_color": "optional|color"
  }
}
```

---

## Schema Properties Breakdown

### 1. Metadata Block (`metadata`)

Defines public catalog information for preset selection and pack management.

| Property | Type | Required | Description |
| :------- | :--: | :------: | :---------- |
| `name` | String | Yes | Display name of the preset shown in the UI. |
| `author` | String | Yes | Creator or vendor name. |
| `category` | String | Yes | Sub-category grouping (e.g., Modulation, Transformation, Distortion). |
| `pack_id` | String | Yes | Target pack identifier (`factory`, `essentials`, or custom pack ID). |
| `description` | String | No | Short explanation of the DSP algorithm and acoustic output. |

---

### 2. DSP Block (`dsp`)

Configures real-time mathematical processing evaluated by the embedded **ExprTk** expression engine.

| Property | Type | Default | Description |
| :------- | :--: | :-----: | :---------- |
| `block_formula` | String | `""` | Optional block-level pre-computation script executed once per buffer block. |
| `sample_formula` | String | `"x"` | Per-sample audio processing expression. Evaluates and returns the processed output sample. |
| `buffer_size` | Integer | `512` | Native processing block size in frames ($16 \le \text{size} \le 8192$). |
| `oversampling` | Integer | `1` | Internal oversampling factor ($1\times, 2\times, 4\times, 8\times$). |

#### Standard Built-in DSP Environment Variables

The runtime engine automatically injects the following variables into the processing environment:

- **Audio Input & Output:** `x` (Current input sample value), `out` (Evaluated output sample value).
- **Time & Delta:** `t` (Running elapsed time in seconds).
- **Control Modulators:** `K1`, `K2`, `K3`, `K4` (Dynamic parameter values bound to UI controls).
- **Multi-Channel & Spatialization:** `ch` (Current channel index), `numCh` (Total channel count), `azimuth`, `elevation`, `divergence`.
- **Filter Feedback Buffers:** `x1`, `x2` (Previous input samples), `y1`, `y2` (Previous output samples).
- **Dynamic Modulators:** `env` / `envelopeValue`, `mod_wheel` / `modWheelValue`, `pitch` / `pitchBendValue`, `lfo_sine` / `lfoSine`, `lfo_tri` / `lfoTri`, `lfo_saw` / `lfoSaw`, `lfo_sqr` / `lfoSqr`.
- **Constants:** `PI`, `TAU`, `E`, `CH_LEFT`, `CH_RIGHT`, `CH_CENTER`, `CH_LFE`, `CH_LS`, `CH_RS`, `CH_BL`, `CH_BR`.

---

### 3. Controls Block (`controls`)

Defines interactive UI rotary knobs or sliders that map directly to DSP parameters (`K1`–`K4`).

| Property | Type | Default | Description |
| :------- | :--: | :-----: | :---------- |
| `id` | String | `"K1"` | Parameter key referenced inside DSP formulas (`K1` to `K4`). |
| `label` | String | `"Param"` | Human-readable UI label displayed beneath the knob/slider. |
| `default` | Float | `0.0` | Initial default position of the control. |
| `min` | Float | `0.0` | Minimum control value boundary. |
| `max` | Float | `1.0` | Maximum control value boundary. |
| `step` | Float | `0.01` | Incremental stepping resolution. |
| `is_bipolar` | Boolean | `false` | When `true`, control renders from the center (-1.0 to +1.0); when `false`, renders unipolar (0.0 to 1.0). |

---

### 4. Dynamic Bindings Block (`bindings`)

Optional array defining explicit links between host modulation sources and user controls.

| Property | Type | Description |
| :------- | :--: | :---------- |
| `symbol` | String | Source modulator identifier (e.g., `envelopeValue`, `modWheelValue`, `pitchBendValue`, `lfoSine`). |
| `target_param_id` | String | Target control variable bound to the modulator (`K1` to `K4`). |
| `min` | Float | Minimum modulation scaling multiplier. |
| `max` | Float | Maximum modulation scaling multiplier. |

---

### 5. UI Block (`ui`)

Defines the visual theme mode and custom color palette for the preset interface. All color tokens are optional standard 6-digit hex strings (`#RRGGBB`). 

> Omitted tokens fall back directly to host application system defaults.

| Property | Type | Format / Allowed Values | Description |
| :------- | :--: | :---------------------: | :---------- |
| `theme_mode` | String | `"auto"` / `"dark"` / `"light"` | Operating system or user theme synchronization mode. |
| `background_color` | String | `"optional"` / `#RRGGBB` | Outer window background color. |
| `surface_color` | String | `"optional"` / `#RRGGBB` | Editor canvas and container surface color. |
| `accent_color` | String | `"optional"` / `#RRGGBB` | Active control fills, focus indicators, and highlights. |
| `text_primary_color` | String | `"optional"` / `#RRGGBB` | Standard text label color. |
| `text_secondary_color` | String | `"optional"` / `#RRGGBB` | Secondary text and disabled state color. |
| `error_color` | String | `"optional"` / `#RRGGBB` | Compiler syntax error highlight color. |

---

## Creating and Packaging

Modulate supports two primary methods for distributing and importing preset packs: direct multi-preset `JSON` files (arrays) and compressed file archives (`.modpack` or `.zip`).

### Direct Pack JSON Array

Multiple presets sharing a common `pack_id` can be written into a single, top-level JSON array inside a standalone file (e.g., `Professional.json`). The audio engine directly parses array-based JSON files and registers all contained presets into the host browser catalog:

```json
[
  {
    "version": "1.0.0",
    "metadata": {
      "name": "Lo-Fi Tape Flutter & Wow",
      "author": "Pranav Pandey",
      "category": "Modulation",
      "pack_id": "professional",
      "description": "Simulates organic tape deck speed instability using dual-rate engine LFOs."
    },
    "dsp": {
      "block_formula": "targetGain := 1.0 + (K3 * 2.0) + (env * 0.5); modDepth := (lfoSine * K1 * 0.08) + (lfoTri * K2 * 0.15);",
      "sample_formula": "tanh(x * targetGain) * (1.0 + modDepth)",
      "buffer_size": 256,
      "oversampling": 2
    },
    "controls": [
      { "id": "K1", "label": "Flutter Depth", "default": 0.35, "min": 0.0, "max": 1.0, "step": 0.01 },
      { "id": "K2", "label": "Wow Depth", "default": 0.50, "min": 0.0, "max": 1.0, "step": 0.01 },
      { "id": "K3", "label": "Tape Saturation", "default": 0.40, "min": 0.0, "max": 1.0, "step": 0.01 }
    ],
    "ui": {
      "background_color": "#1C130E",
      "surface_color": "#2C1E16",
      "accent_color": "#FF8A65",
      "text_primary_color": "#FFF3E0",
      "text_secondary_color": "#FFCCBC"
    }
  },
  {
    "version": "1.0.0",
    "metadata": {
      "name": "Quadrature Ring Modulator",
      "author": "Pranav Pandey",
      "category": "Modulation",
      "pack_id": "professional",
      "description": "Frequency shifting effect combining quadrature engine LFOs with pitch tracking."
    },
    "dsp": {
      "block_formula": "carrier := 100.0 + (K1 * 2000.0) + (pitch * 300.0); mixWet := K3; phaseOff := K2 * 3.14159265;",
      "sample_formula": "(x * (1.0 - mixWet)) + ((x * sin(6.2831853 * carrier * t + phaseOff) + x * lfoTri * (1.0 + modWheelValue)) * 0.5 * mixWet)",
      "buffer_size": 256,
      "oversampling": 2
    },
    "controls": [
      { "id": "K1", "label": "Carrier Freq", "default": 0.30, "min": 0.0, "max": 1.0, "step": 0.01 },
      { "id": "K2", "label": "Phase Offset", "default": 0.20, "min": 0.0, "max": 1.0, "step": 0.01 },
      { "id": "K3", "label": "Wet Mix", "default": 0.70, "min": 0.0, "max": 1.0, "step": 0.01 }
    ],
    "ui": {
      "background_color": "#120A21",
      "surface_color": "#21123B",
      "accent_color": "#C084FC",
      "text_primary_color": "#FAF5FF",
      "text_secondary_color": "#E9D5FF"
    }
  }
]
```

### Pack File Archives (`.modpack` / `.zip`)

For larger collections, commercial packs, or assets that include artwork images, bundle files into a folder structure.

#### Directory Structure

Organize your project directory with a `pack.json` manifest, individual preset JSON files, and cover artwork:

```text
MyCustomPack/
├── pack.json               # Pack manifest file
├── cover.png               # Cover image artwork (PNG/JPG)
├── 01_RobotVoice.json      # Preset 1
├── 02_AnalogSaturator.json # Preset 2
└── 03_BitCrusher.json      # Preset 3
```

#### Pack Manifest (`pack.json`)

```json
{
  "pack_id": "my_custom_pack",
  "title": "My Custom Expansion Pack",
  "vendor": "Sound Designer Studio",
  "version": "1.0.0",
  "description": "Collection of futuristic vocal transformers and saturation models.",
  "is_premium": true
}
```
---

## Exporting and Installing

Modulate presets and packs can be deployed as standalone single-preset files, consolidated multi-preset JSON arrays, or compressed archives.

### Single Preset Files (`.json`)

Save an individual preset schema object directly into a `.json` file.

### Direct Pack JSON Arrays (`.json`)

Save a JSON array containing multiple preset schema objects (sharing a common `pack_id`) directly into a single `.json` file.

### Archive Packs (`.modpack` / `.zip`)

1. Select all files inside your pack folder (including `pack.json` and artwork).
2. Compress the items directly into a standard `.zip` file.
3. _(Optional) Change the file extension from `.zip` to `.modpack` for brand consistency._

> For archive packs, ensure `pack.json` resides at the top level of the archive rather than inside an extra nested subfolder.

### Installation Directories

#### Standalone Presets & Direct Pack JSON Files (`.json`)

- **Windows:** `%APPDATA%\Modulate\UserPresets\`
- **macOS:** `~/Library/Application Support/Modulate/UserPresets/`
- **Linux:** `~/.local/share/Modulate/UserPresets/` or `$XDG_DATA_HOME/Modulate/UserPresets/`

#### Imported Archive Packs (`.modpack` / `.zip` / unpacked folders)

- **Windows:** `%APPDATA%\Modulate\ImportedPacks\`
- **macOS:** `~/Library/Application Support/Modulate/ImportedPacks/`
- **Linux:** `~/.local/share/Modulate/ImportedPacks/` or `$XDG_DATA_HOME/Modulate/ImportedPacks/`

---

## Author

Pranav Pandey

[![GitHub](https://img.shields.io/github/followers/pranavpandey?label=GitHub&style=social)](https://github.com/pranavpandey)
[![Follow on Twitter](https://img.shields.io/twitter/follow/pranavpandeydev?label=Follow&style=social)](https://twitter.com/intent/follow?screen_name=pranavpandeydev)
[![Donate via PayPal](https://img.shields.io/static/v1?label=Donate&message=PayPal&color=blue)](https://paypal.me/pranavpandeydev)

---

## License

    Copyright 2026 Pranav Pandey

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
