<img src="./graphics/icon.png" height="160">

# Modulate Schema

[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg)](https://www.apache.org/licenses/LICENSE-2.0.html)
[![Latest Release](https://img.shields.io/github/v/release/pranavpandey/modulate-schema?color=e75889&label=schema)](https://github.com/pranavpandey/modulate-schema/releases)
[![Download](https://img.shields.io/badge/modulate-v1.1.0-e75889?style=flat&logo=gumroad&logoColor=82e1ca)](https://modulate.pranavpandey.com)

The **Modulate Schema** is an open, declarative JSON specification for defining real-time DSP audio algorithms, dynamic control parameters, dynamic bindings, multi-track groovebox sequence patterns, and adaptive UI themes. It powers the Modulate Audio Engine, allowing creators and developers to build, distribute, and monetize customizable preset files (`.json`) or bundled preset packs (`.json` / `.modpack` / `.zip`) without writing C++ or compiling native binaries.

---

## Preset Schema

The blueprint below outlines all supported keys, data types, allowed enumerations, and execution scopes required for building fully compliant Modulate presets.

```json
{
  "version": "string (e.g., 1.1.0)",
  "metadata": {
    "name": "string",
    "author": "string",
    "category": "string",
    "pack_id": "string",
    "description": "string"
  },
  "groovebox": {
    "enabled": "boolean",
    "bpm": "float (40.0 to 240.0)",
    "swing": "float (0.0 to 0.75)",
    "playback_mode": "string (pattern|chain|loop)",
    "current_pattern": "string",
    "song_chain": ["array of pattern_keys"],
    "arp": {
      "enabled": "boolean",
      "mode": "string (up|down|upDown|random)",
      "rate": "string (1/4|1/4t|1/8|1/8t|1/16|1/16t|1/32)",
      "octaves": "integer (1 to 4)",
      "gate": "float (0.01 to 1.0)",
      "latch": "boolean"
    },
    "scale": {
      "enabled": "boolean",
      "root_note": "string (C|C#|D|D#|E|F|F#|G|G#|A|A#|B)",
      "scale_type": "string (chromatic|major|minor|dorian|phrygian|lydian|mixolydian|locrian|pentatonic)"
    },
    "synths": {
      "voice_key": {
        "name": "string",
        "sample_formula": "string",
        "block_formula": "string",
        "polyphony": "integer (1 to 16)",
        "attack": "float (seconds)",
        "decay": "float (seconds)",
        "sustain": "float (0.0 to 1.0)",
        "release": "float (seconds)",
        "env_curve": "string (linear|exponential|logarithmic)",
        "unison_voices": "integer (1 to 8)",
        "unison_detune": "float (0.0 to 100.0 cents)",
        "arp": {
          "enabled": "boolean",
          "mode": "string",
          "rate": "string",
          "octaves": "integer",
          "gate": "float",
          "latch": "boolean"
        },
        "sample_slot": {
          "sample_path": "string",
          "start_point": "float (0.0 to 1.0)",
          "end_point": "float (0.0 to 1.0)",
          "loop_enabled": "boolean",
          "root_note": "float (0.0 to 127.0)",
          "play_mode": "string (sample|slice|granular)",
          "slice_count": "integer (1 to 64)",
          "grain_size_ms": "float (1.0 to 500.0)",
          "grain_density": "float (0.1 to 4.0)",
          "grain_spray": "float (0.0 to 1.0)"
        }
      }
    },
    "patterns": {
      "pattern_key": {
        "name": "string",
        "length_steps": "integer (1 to 128)",
        "time_sig_numerator": "integer (1 to 16)",
        "time_sig_denominator": "integer (1 to 16)",
        "is_fill_pattern": "boolean",
        "alt_pattern_key": "string",
        "tracks": {
          "track_key": {
            "name": "string",
            "mute": "boolean",
            "solo": "boolean",
            "volume": "float (0.0 to 2.0)",
            "pan": "float (-1.0 to 1.0)",
            "fx_send": "float (0.0 to 1.0)",
            "delay_send": "float (0.0 to 1.0)",
            "reverb_send": "float (0.0 to 1.0)",
            "filter_cutoff": "float (20.0 to 20000.0 Hz)",
            "track_length_steps": "integer (1 to 128)",
            "clock_divider": "float (0.125 to 4.0)",
            "swing_offset": "float (-0.25 to 0.25)",
            "playback_direction": "string (forward|reverse|ping_pong|random)",
            "sidechain_source_track": "string",
            "arp": {
              "enabled": "boolean",
              "mode": "string",
              "rate": "string",
              "octaves": "integer",
              "gate": "float",
              "latch": "boolean"
            },
            "euclidean": {
              "enabled": "boolean",
              "pulses": "integer (0 to 128)",
              "steps": "integer (1 to 128)",
              "rotation": "integer (-64 to 64)"
            },
            "scale": {
              "enabled": "boolean",
              "root_note": "string",
              "scale_type": "string"
            },
            "steps": [
              {
                "step": "integer",
                "active": "boolean",
                "note": "string (e.g., C3)",
                "notes": ["array of polyphonic note strings"],
                "gate": "float (0.01 to 1.0)",
                "velocity": "float (0.0 to 1.0)",
                "pitch_bend": "float (-1.0 to 1.0)",
                "ratchet": "integer (1 to 8)",
                "probability": "float (0.0 to 1.0)",
                "micro_timing": "float (-0.5 to 0.5)",
                "trig_condition": "string (always|1:2|2:2|1:4|fill|not_fill)",
                "lfo_phase_lock": "float (-1.0 to 1.0)",
                "lfo_speed_lock": "float (-1.0 to 1.0)",
                "slice_index": "integer (-1 to 63)",
                "locks": { "param_id": "float" }
              }
            ]
          }
        }
      }
    }
  },
  "master_fx": {
    "master_volume": "float (0.0 to 2.0)",
    "master_filter_cutoff": "float (20.0 to 20000.0 Hz)",
    "delay_send_level": "float (0.0 to 1.0)",
    "reverb_send_level": "float (0.0 to 1.0)"
  },
  "dsp": {
    "block_formula": "string (pre-computation script executed once per buffer block)",
    "sample_formula": "string (evaluated per sample; returns float signal)",
    "buffer_size": "integer (16 to 4096)",
    "oversampling": "integer (1 to 16)"
  },
  "controls": [
    {
      "id": "string (K1 to K8)",
      "label": "string",
      "default": "float (-100.0 to 100.0)",
      "min": "float (-100.0 to 100.0)",
      "max": "float (-100.0 to 100.0)",
      "step": "float (0.001 to 10.0)",
      "is_bipolar": "boolean"
    }
  ],
  "macros": [
    {
      "id": "string (M1 to M8)",
      "label": "string",
      "value": "float (0.0 to 1.0)",
      "targets": [
        {
          "target_param_id": "string (e.g., K1)",
          "min_val": "float",
          "max_val": "float"
        }
      ]
    }
  ],
  "bindings": [
    {
      "symbol": "string (mod_wheel|env|pitch|lfo_sine|lfo_tri|lfo_saw|lfo_sqr)",
      "target_param_id": "string (K1 to K8)",
      "min": "float",
      "max": "float"
    }
  ],
  "ui": {
    "theme_mode": "auto|dark|light",
    "background_color": "#RRGGBB",
    "surface_color": "#RRGGBB",
    "accent_color": "#RRGGBB",
    "text_primary_color": "#RRGGBB",
    "text_secondary_color": "#RRGGBB",
    "error_color": "#RRGGBB"
  }
}

```

---

## Schema Properties Breakdown

### 1. Metadata Block (`metadata`)

Defines public catalog information for preset selection and pack management.

| Property | Type | Required | Description |
| :------- | :--: | :-------:| :---------- |
| `name` | String | Yes | Display name of the preset shown in the UI. |
| `author` | String | Yes | Creator or vendor name. |
| `category` | String | Yes | Sub-category grouping (e.g., Modulation, Transformation, Distortion, Groovebox / Sequence). |
| `pack_id` | String | Yes | Target pack identifier (`factory`, `essentials`, or custom pack ID). |
| `description` | String | No | Short explanation of the DSP algorithm and acoustic output. |

---

### 2. Groovebox Block (`groovebox`)

Configures multi-track synth engines, sequencing patterns, polyrhythms, and song arrangements.

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `enabled` | Boolean | `false` | - | Enables the internal sequence engine. |
| `bpm` | Float | `120.0` | `40.0` .. `240.0` | Sequencer tempo in BPM. |
| `swing` | Float | `0.0` | `0.0` .. `0.75` | Global shuffle amount. |
| `playback_mode` | String | `"chain"` | `pattern` / `chain` / `loop` | Arrangement playback mode. |
| `current_pattern` | String | `"pat_verse"` | - | Default active pattern key. |
| `song_chain` | Array | `[]` | Max 64 | Pattern key arrangement sequence. |
| `arp` | Object | `{}` | - | Global Arpeggiator configuration. |
| `scale` | Object | `{}` | - | Global scale quantization. |
| `synths` | Map | `{}` | Max 8 | Synthesis voice configurations. |
| `patterns` | Map | `{}` | Max 32 | Multi-track sequence patterns. |

#### Synth Voice Configuration (`synths.<voice_key>`)

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `name` | String | Voice Key | - | Display label for voice. |
| `sample_formula` | String | `"0.0"` | - | Expression executed per audio frame. |
| `block_formula` | String | `""` | - | Expression executed per buffer block. |
| `polyphony` | Integer | `1` | `1` .. `16` | Voice polyphony count. |
| `attack` | Float | `0.005` | $\ge 0.0001$s | Envelope attack time. |
| `decay` | Float | `0.2` | $\ge 0.0001$s | Envelope decay time. |
| `sustain` | Float | `0.7` | `0.0` .. `1.0` | Envelope sustain level. |
| `release` | Float | `0.3` | $\ge 0.0001$s | Envelope release time. |
| `env_curve` | String | `"exponential"` | `linear` / `exponential` / `logarithmic` | Envelope response curve. |
| `unison_voices` | Integer | `1` | `1` .. `8` | Stacked unison voice count. |
| `unison_detune` | Float | `0.0` | `0.0` .. `100.0` | Unison pitch detuning in cents. |
| `arp` | Object | `{}` | - | Voice-level Arpeggiator settings. |
| `sample_slot` | Object | `{}` | - | Granular/PCM sample slot settings. |

#### Sample Slot & Granular Settings (`synths.<voice_key>.sample_slot`)

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `sample_path` | String | `""` | - | Relative path to WAV/PCM asset. |
| `start_point` | Float | `0.0` | `0.0` .. `1.0` | Sample start region boundary. |
| `end_point` | Float | `1.0` | `0.0` .. `1.0` | Sample end region boundary. |
| `loop_enabled` | Boolean | `false` | - | Enables sample looping. |
| `root_note` | Float | `60.0` | `0.0` .. `127.0` | Base MIDI pitch mapping. |
| `play_mode` | String | `"sample"` | `sample` / `slice` / `granular` | Playback mode. |
| `slice_count` | Integer | `1` | `1` .. `64` | Transients auto-slice count. |
| `grain_size_ms` | Float | `50.0` | `1.0` .. `500.0` | Granular size in milliseconds. |
| `grain_density` | Float | `1.0` | `0.1` .. `4.0` | Granular density multiplier. |
| `grain_spray` | Float | `0.0` | `0.0` .. `1.0` | Granular position randomization. |

> Not yet supported by the Modulate Audio Engine.

#### Track Settings (`patterns.<pattern_key>.tracks.<track_key>`)

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `mute` | Boolean | `false` | - | Track mute state. |
| `solo` | Boolean | `false` | - | Track solo state. |
| `volume` | Float | `1.0` | `0.0` .. `2.0` | Track gain. |
| `pan` | Float | `0.0` | `-1.0` .. `1.0` | Stereo panning. |
| `fx_send` | Float | `1.0` | `0.0` .. `1.0` | Master FX bus send level. |
| `delay_send` | Float | `0.0` | `0.0` .. `1.0` | Delay effect send level. |
| `reverb_send` | Float | `0.0` | `0.0` .. `1.0` | Reverb effect send level. |
| `filter_cutoff` | Float | `1000.0` | `20.0` .. `20000.0` | Lowpass filter cutoff frequency in Hz. |
| `track_length_steps` | Integer | `16` | `1` .. `128` | Individual track step length (polyrhythm). |
| `clock_divider` | Float | `1.0` | `0.125` .. `4.0` | Speed rate multiplier. |
| `swing_offset` | Float | `0.0` | `-0.25` .. `0.25` | Per-track swing offset. |
| `playback_direction` | String | `"forward"` | `forward` / `reverse` / `ping_pong` / `random` | Sequence playback movement direction. |
| `sidechain_source_track` | String | `""` | - | Source track for dynamic sidechain compression. |
| `euclidean` | Object | `{}` | - | Algorithmic Euclidean pattern generator. |

#### Sequence Step Parameters (`patterns.<pattern_key>.tracks.<track_key>.steps[i]`)

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `step` | Integer | `0` | `0` .. `127` | Step index on grid. |
| `active` | Boolean | `false` | - | Trigger state. |
| `note` | String | `"C3"` | - | Primary note label. |
| `notes` | Array | `["C3"]` | - | Array of note labels for polyphonic chord steps. |
| `gate` | Float | `0.5` | `0.01` .. `1.0` | Gate length fraction. |
| `velocity` | Float | `1.0` | `0.0` .. `1.0` | Step velocity. |
| `pitch_bend` | Float | `0.0` | `-1.0` .. `1.0` | Pitch bend offset in semitones. |
| `ratchet` | Integer | `1` | `1` .. `8` | Sub-step re-triggers per step interval. |
| `probability` | Float | `1.0` | `0.0` .. `1.0` | Trigger probability. |
| `micro_timing` | Float | `0.0` | `-0.5` .. `0.5` | Micro-timing offset. |
| `trig_condition` | String | `"always"` | `always` / `1:2` / `2:2` / `1:4` / `fill` / `not_fill` | Conditional trigger logic. |
| `lfo_phase_lock` | Float | `-1.0` | `-1.0` .. `1.0` | Per-step LFO phase reset/lock. |
| `lfo_speed_lock` | Float | `-1.0` | `-1.0` .. `1.0` | Per-step LFO rate lock. |
| `slice_index` | Integer | `-1` | `-1` .. `63` | Sliced sample marker index. |
| `locks` | Map | `{}` | - | Parameter automation locks (e.g. `{"K1": 0.8}`). |

---

### 3. DSP Block (`dsp`)

Configures real-time mathematical processing evaluated by the embedded **ExprTk** expression engine, as well as the Master FX bus.

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `sample_formula` | String | `"x"` | - | Per-sample processing signal formula. |
| `block_formula` | String | `""` | - | Pre-computation script run per buffer block. |
| `buffer_size` | Integer | `512` | `16` .. `4096` | Processing block frame count. |
| `oversampling` | Integer | `1` | `1` .. `16` | Oversampling factor. |
| `master_volume` | Float | `1.0` | `0.0` .. `2.0` | Master output volume. |
| `master_filter_cutoff` | Float | `20000.0` | `20.0` .. `20000.0` | Master lowpass filter cutoff frequency in Hz. |
| `delay_send_level` | Float | `0.0` | `0.0` .. `1.0` | Master delay send routing level. |
| `reverb_send_level` | Float | `0.0` | `0.0` .. `1.0` | Master reverb send routing level. |

#### Standard Built-in DSP Environment Variables

The runtime engine automatically injects the following variables into the processing environment:

| Variable | Type | Limits / Range | Description |
| :------- | :--: | :------------: | :---------- |
| `x` | Audio | $[-1.0, 1.0]$ | Current input sample value. |
| `out` | Audio | $[-1.0, 1.0]$ | Evaluated output sample value. |
| `t` | Time | $\ge 0.0$ | Running elapsed time in seconds. |
| `t_step` | Time | $\ge 0.0$ | Step-relative running time for synth voices. |
| `K1` .. `K8` | Control | $[-100.0, 100.0]$ | Dynamic parameter values bound to UI controls. |
| `note_freq` / `freq` | Pitch | $0.0 .. 20000.0$ | Pitch frequency in Hz. |
| `seq_note` / `note` | Pitch | $0.0 .. 127.0$ | MIDI note number. |
| `bpm` | Tempo | $40.0 .. 240.0$ | Sequencer tempo in BPM. |
| `swing` | Groove | $0.0 .. 0.75$ | Global shuffle swing amount. |
| `seq_step` / `step` | Sequence | $0 .. 127$ | Current sequence step index. |
| `seq_gate` / `gate` | Sequence | $0.01 .. 1.0$ | Active step gate length fraction. |
| `seq_vel` / `vel` / `velocity` | Sequence | $0.0 .. 1.0$ | Active step velocity. |
| `attack` | Envelope | $\ge 0.0001$s | Voice envelope attack time. |
| `decay` | Envelope | $\ge 0.0001$s | Voice envelope decay time. |
| `sustain` | Envelope | $0.0 .. 1.0$ | Voice envelope sustain level. |
| `release` | Envelope | $\ge 0.0001$s | Voice envelope release time. |
| `ch` | Channel | $0 .. (\text{numCh} - 1)$ | Current channel index. |
| `numCh` | Channel | $1 .. 8$ | Total active audio channel count. |
| `azimuth` | Spatial | $-180.0^\circ .. 180.0^\circ$ | Spatial panning azimuth angle. |
| `elevation` | Spatial | $-90.0^\circ .. 90.0^\circ$ | Spatial elevation angle. |
| `divergence` | Spatial | $0.0 .. 1.0$ | Spatial sound field divergence. |
| `x1`, `x2` | Feedback | Unbounded | Previous input samples ($n-1, n-2$). |
| `y1`, `y2` | Feedback | Unbounded | Previous output samples ($n-1, n-2$). |
| `env` / `envelopeValue` | Modulator | $0.0 .. 1.0$ | Voice envelope generator state. |
| `mod_wheel` / `modWheelValue` | Modulator | $0.0 .. 1.0$ | Host MIDI modulation wheel value. |
| `pitch` / `pitchBendValue` | Modulator | $-1.0 .. 1.0$ | Host MIDI pitch bend value. |
| `lfo_sine` / `lfoSine` | Modulator | $[-1.0, 1.0]$ | Engine LFO sine wave output. |
| `lfo_tri` / `lfoTri` | Modulator | $[-1.0, 1.0]$ | Engine LFO triangle wave output. |
| `lfo_saw` / `lfoSaw` | Modulator | $[-1.0, 1.0]$ | Engine LFO sawtooth wave output. |
| `lfo_sqr` / `lfoSqr` | Modulator | $\{-1.0, 1.0\}$ | Engine LFO square wave output. |
| `PI` / `pi` | Constant | $3.14159265...$ | Archimedes' constant $\pi$. |
| `TAU` / `tau` | Constant | $6.28318530...$ | Circle constant $2\pi$. |
| `E` / `e` | Constant | $2.71828182...$ | Euler's number $e$. |
| `CH_LEFT` | Constant | $0.0$ | Left audio channel index token. |
| `CH_RIGHT` | Constant | $1.0$ | Right audio channel index token. |
| `CH_CENTER` | Constant | $2.0$ | Center channel index token. |
| `CH_LFE` | Constant | $3.0$ | Low-Frequency Effects channel token. |
| `CH_LS` | Constant | $4.0$ | Left Surround channel index token. |
| `CH_RS` | Constant | $5.0$ | Right Surround channel index token. |
| `CH_BL` | Constant | $6.0$ | Back Left channel index token. |
| `CH_BR` | Constant | $7.0$ | Back Right channel index token. |

#### Built-in Waveform & DSP Functions

In addition to standard ExprTk mathematical functions (`sin`, `cos`, `tanh`, `exp`, `abs`, etc.), the compiler registers custom audio DSP functions:

| Function | Arguments | Input Limits / Parameter Clamps | Output Range | Description |
| :------- | :-------: | :-----------------------------: | :----------: | :---------- |
| `saw(phase)` | `1` | Phase in radians | $[-1.0, 1.0]$ | Sawtooth wave generator ($0 \le \text{phase} < 2\pi$). |
| `tri(phase)` | `1` | Phase in radians | $[-1.0, 1.0]$ | Triangle wave generator ($0 \le \text{phase} < 2\pi$). |
| `square(phase)` / `sqr_wave(phase)` | `1` | Phase in radians | $\{-1.0, 1.0\}$ | Square wave generator ($0 \le \text{phase} < 2\pi$). |
| `pulse(phase, duty)` | `2` | Phase in rads; $0.01 \le \text{duty} \le 0.99$ | $\{-1.0, 1.0\}$ | Pulse wave generator with variable duty cycle. |
| `noise()` | `0` | None | $[-1.0, 1.0]$ | Uniform white noise generator. |
| `wrap(val, min, max)` | `3` | Any `val`; $\text{min} < \text{max}$ | $[\text{min}, \text{max}]$ | Wraps value within specified `min` and `max` limits. |
| `lerp(a, b, t)` | `3` | Clamped $0.0 \le t \le 1.0$ | $[a, b]$ | Linear interpolation between `a` and `b` by fraction `t`. |
| `db2gain(db)` | `1` | Clamped $-144.0 \le \text{dB} \le +36.0$ | $[0.0, 63.09]$ | Converts decibel value to linear amplitude gain. |
| `gain2db(gain)` | `1` | Floor at $1.0 \times 10^{-5}$ | $\ge -100.0 \text{ dB}$ | Converts linear amplitude gain to decibels. |
| `drive(x, k)` | `2` | Clamped $0.0 \le k \le 100.0$ | $(-1.0, 1.0)$ | Hyperbolic saturation drive $\tanh(x \cdot (1 + k))$. |
| `foldback(x, thresh)` | `2` | Clamped $0.001 \le \text{thresh} \le 10.0$ | $[-\text{thresh}, \text{thresh}]$ | Wave-folding distortion above threshold limit. |

---

### 4. Controls & Macro Morphing Blocks (`controls` / `macros`)

#### Dynamic Knobs / Sliders (`controls`)

Defines interface parameters bound to `K1..K8` inside DSP expressions.

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `id` | String | `"K1"` | `K1` .. `K8` | Parameter key referenced in formulas. |
| `label` | String | `"Param"` | - | Human-readable UI label. |
| `default` | Float | `0.0` | `-100.0` .. `100.0` | Default parameter value. |
| `min` | Float | `-100.0` | `-100.0` .. `100.0` | Minimum value bound. |
| `max` | Float | `100.0` | `min` .. `100.0` | Maximum value bound. |
| `step` | Float | `0.01` | `0.001` .. `10.0` | Stepping resolution. |
| `is_bipolar` | Boolean | `false` | - | Enables bipolar UI rendering (-100 to +100). |

#### Macro Morphing Scenes (`macros`)

Defines performance Macro knobs (`M1..M8`) scaling multiple `K1..K8` parameters simultaneously.

| Property | Type | Default | Limits | Description |
| :------- | :--: | :-----: | :----: | :---------- |
| `id` | String | `"M1"` | `M1` .. `M8` | Macro identifier. |
| `label` | String | `"Morph"` | - | Interface display label. |
| `value` | Float | `0.0` | `0.0` .. `1.0` | Position of Macro knob. |
| `targets` | Array | `[]` | Max 8 | Mappings defining target parameter scaling. |

---

### 5. Dynamic Bindings Block (`bindings`)

Defines link associations between host modulators and parameter controls.

| Property | Type | Allowed Symbols | Description |
| :------- | :--: | :-------------: | :---------- |
| `symbol` | String | `mod_wheel`, `env`, `pitch`, `lfo_sine`, `lfo_tri`, `lfo_saw`, `lfo_sqr` | Source modulator identifier. |
| `target_param_id` | String | `K1` .. `K8` | Target parameter key. |
| `min` | Float | - | Minimum scaling multiplier. |
| `max` | Float | - | Maximum scaling multiplier. |

---

### 6. UI Block (`ui`)

Defines the visual theme mode and custom color palette for the preset interface. All color tokens are optional standard 6-digit hex strings (`#RRGGBB`).

> Omitted tokens fall back directly to host application system defaults.

| Property | Type | Allowed Values | Description |
| :------- | :--: | :------------: | :---------- |
| `theme_mode` | String | `"auto"` / `"dark"` / `"light"` | Operating system or user theme synchronization mode. |
| `background_color` | String | `#RRGGBB` | Outer window background color. |
| `surface_color` | String | `#RRGGBB` | Editor canvas and container surface color. |
| `accent_color` | String | `#RRGGBB` | Active control fills, focus indicators, and highlights. |
| `text_primary_color` | String | `#RRGGBB` | Standard text label color. |
| `text_secondary_color` | String | `#RRGGBB` | Secondary text and disabled state color. |
| `error_color` | String | `#RRGGBB` | Compiler syntax error highlight color. |

---

## Creating and Packaging

Modulate supports two primary methods for distributing and importing preset packs: direct multi-preset `JSON` files (arrays) and compressed file archives (`.modpack` or `.zip`).

### Direct Pack JSON Array

Multiple presets sharing a common `pack_id` can be written into a single, top-level JSON array inside a standalone file (e.g., `Professional.json`). The audio engine directly parses array-based JSON files and registers all contained presets into the host browser catalog:

```json
[
  {
    "version": "1.1.0",
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
    "version": "1.1.0",
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
3. *(Optional) Change the file extension from `.zip` to `.modpack` for brand consistency.*

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

```
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

```
