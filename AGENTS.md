# AGENTS.md

> Agentic coding guidelines for Ableton User Remote Scripts

## Project Overview

This repository contains Ableton Live User Remote Scripts — INI-style configuration files that map MIDI controller CC numbers to Ableton's transport, mixer, and device controls. No Python code or build system is required.

## Repository Structure

```
Ableton_UserRemote_Scripts/
├── ableton-remote-script/          # Skill for generating scripts
│   ├── SKILL.md                    # Skill documentation
│   └── TEMPLATE.md                 # Skill template
├── Ableton_Template_and_instructions/
│   ├── UserConfiguration.txt       # Full template with all options
│   └── InstantMappings-HowTo.txt   # Official Ableton documentation
├── My_Custom_UserRemote_Scripts/   # User-created configurations
│   ├── nanoKONTROL2_NF/
│   ├── microKONTROL_NF/
│   ├── nanoKEY Studio_NF/
│   └── Akai MPD218_NF/
├── Korg_nanoKONTROL2_CCMappings/   # Reference documentation
├── Korg_nanoKEY_Studio_CCMappings/ # Reference documentation
└── index.html                      # Web app for visualizing controller mappings
```

## Validation & Testing

### Manual Validation

1. **Syntax Check**: Ensure all section headers `[SectionName]` are valid
2. **CC Range**: All CC numbers must be 0-127
3. **Channel Range**: All MIDI channels must be 0-15 (or -1 for disabled)
4. **Consistency**: Verify CC numbers don't conflict across controls

### Installing to Ableton

1. Copy the controller folder to Ableton's User Remote Scripts directory:
   - **macOS**: `~/Library/Application Support/Ableton/Live X Suite/User Remote Scripts/`
   - **Windows**: `C:\Users\[username]\AppData\Roaming\Ableton\Live X Suite\User Remote Scripts\`

2. In Ableton Live: Preferences → MIDI/Sync → Select controller from Control Surface list

3. Map mode validation: Test encoders with different `EncoderMapMode` values:
   - `Absolute` — standard faders/knobs
   - `LinearSignedBit` / `LinearSignedBit2` — endless encoders
   - `AccelSignedBit` / `AccelSignedBit2` — accelerometric encoders

## Code Style Guidelines

### File Naming

- **Configuration file**: MUST be exactly `UserConfiguration.txt`
- **Folder name**: MUST start with a letter (not number or underscore)
- **Case sensitivity**: Use consistent casing; prefer TitleCase for readability

### Section Structure

Follow the standard Ableton InstantMappings format with these sections:

```
[Globals]              # Global settings (channel, port names)
[DeviceControls]       # Device/plugin parameter controls
[MixerControls]        # Volume, mute, solo, arm, sends
[TransportControls]    # Play, stop, record, loop, etc.
```

### Value Conventions

| Setting | Valid Values | Convention |
|---------|--------------|------------|
| CC Numbers | 0-127, or -1 (disabled) | Use sequential ranges for related controls |
| MIDI Channels | 0-15, or -1 (use GlobalChannel) | Most controllers use 0 |
| Map Modes | Absolute, LinearSignedBit, etc. | Use `Absolute` for standard knobs/faders |
| Boolean | True/False | Capitalize first letter |

### Disabling Controls

Use `-1` to disable any control. This is the Ableton standard for "not mapped".

### Comments

Use `#` for comments at the top of files. Inline comments should be avoided for clarity.

```ini
# Controller-specific configuration
[Globals]
GlobalChannel: 0
InputName: Controller (Port1)
```

### Spacing and Formatting

- Use consistent spacing: `Key: Value` (colon + space)
- Group related settings together
- Use blank lines to separate logical sections within a section
- No trailing whitespace

### Naming Conventions

| Element | Convention | Example |
|---------|------------|---------|
| Folder name | PascalCase or snake_case | `nanoKONTROL2_NF`, `MPD218_NF` |
| Port names | Match Ableton exactly | `nanoKONTROL2 (SLIDER/KNOB)` |
| CC variables | CamelCase | `VolumeSlider1`, `TrackMuteButton1` |
| Map modes | PascalCase | `Absolute`, `LinearSignedBit` |

## Common Patterns

### Transport Controls

```ini
[TransportControls]
PlayButton: 41
StopButton: 42
RwdButton: 43
FfwdButton: 44
RecButton: 45
LoopButton: 46
```

### Mixer Controls (8-channel strip)

```ini
[MixerControls]
VolumeSlider1: 0
VolumeSlider2: 1
...
VolumeSlider8: 7
TrackMuteButton1: 48
...
TrackSoloButton1: 32
...
TrackArmButton1: 64
...
```

### Device Encoders

```ini
[DeviceControls]
Encoder1: 16
Encoder2: 17
...
Encoder8: 23
EncoderMapMode: Absolute
```

## Important Notes

- CC numbers are **0-indexed** (not 1-indexed)
- MIDI channels are **0-15** (not 1-16)
- Button feedback can be inverted with `InvertMuteButtonFeedback: True`
- Use `MixerButtonsToggle: False` for momentary buttons, `True` for toggles
- The `Send1Knob*` and `Send2Knob*` controls map to track sends A and B

## Reference Templates

- **Full template**: `Ableton_Template_and_instructions/UserConfiguration.txt`
- **Real-world example**: `My_Custom_UserRemote_Scripts/nanoKONTROL2_NF/UserConfiguration.txt`
- **CC mappings**: `Korg_nanoKontrol2_CCMappings/nanoKontrol2mappings.md`

## Web Application

The repository includes a reference web app (`index.html`) that visualizes MIDI controller mappings for all supported controllers. Hosted on GitHub Pages.

### Development Workflow

1. **Work locally on `dev` branch:**
   ```bash
   git checkout dev
   # Make changes to index.html
   git add .
   git commit -m "description"
   git push origin dev
   ```

2. **Create PR on GitHub:**
   - Go to: `https://github.com/cracklynode/Ableton_UserRemote_Scripts/pull/new/dev`
   - Set base: `main` ← compare: `dev`
   - Create and merge the pull request

3. **GitHub Pages auto-updates** after PR merge (~1-2 minutes)

4. **Update local branches** after merge:
   ```bash
   git checkout main
   git pull origin main
   git checkout dev
   git merge main
   ```
