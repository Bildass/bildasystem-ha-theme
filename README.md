# BildaSystem Theme for Home Assistant

A modern, sleek theme for Home Assistant with animated icons and a beautiful dark design.

![BildaSystem Theme Preview](docs/preview.png)

## Features

- **Modern Dark Design** - Slate color palette with blue accents
- **Animated Icons** - Spinning fans, pulsing lights, temperature colors
- **Card-mod Integration** - Hover effects, transitions, custom styling
- **Light & Dark Modes** - Both variants included
- **HACS Compatible** - Easy installation and updates

## Installation

### HACS (Recommended)

1. Open HACS in Home Assistant
2. Go to **Frontend** section
3. Click the 3 dots menu → **Custom repositories**
4. Add `https://github.com/Bildass/bildasystem-ha-theme` as **Theme**
5. Click **Install**
6. Restart Home Assistant

### Manual Installation

1. Download `themes/bildasystem.yaml`
2. Copy to your `config/themes/` directory
3. Add to `configuration.yaml`:
   ```yaml
   frontend:
     themes: !include_dir_merge_named themes/
   ```
4. Restart Home Assistant

## Activation

1. Go to your **Profile** (bottom left sidebar)
2. Select **BildaSystem Dark** from the Theme dropdown
3. Enjoy!

## Requirements

| Component | Required | Description |
|-----------|----------|-------------|
| [card-mod](https://github.com/thomasloven/lovelace-card-mod) | Yes | For animations and custom styling |
| [Mushroom Cards](https://github.com/piitaya/lovelace-mushroom) | Recommended | Modern card designs |

Install both via HACS for best experience.

## Animations

The theme includes beautiful state-based animations. See [Animation Guide](docs/animations.md) for full documentation.

### Quick Examples

**Spinning Fan:**
```yaml
type: custom:mushroom-fan-card
entity: fan.bedroom
card_mod:
  style: |
    ha-state-icon {
      {% if is_state(config.entity, 'on') %}
        animation: spin 1s linear infinite;
      {% endif %}
    }
```

**Pulsing Light:**
```yaml
type: custom:mushroom-light-card
entity: light.living_room
card_mod:
  style: |
    ha-state-icon {
      {% if is_state(config.entity, 'on') %}
        animation: pulse 2s ease-in-out infinite;
      {% endif %}
    }
```

**Temperature Colors:**
```yaml
type: custom:mushroom-entity-card
entity: sensor.temperature
card_mod:
  style: |
    ha-state-icon {
      {% set temp = states(config.entity) | float %}
      {% if temp < 18 %}
        color: #3b82f6 !important;
      {% elif temp < 24 %}
        color: #22c55e !important;
      {% else %}
        color: #ef4444 !important;
      {% endif %}
    }
```

## Color Palette

### Dark Mode (Default)

| Color | Hex | Usage |
|-------|-----|-------|
| Primary Blue | `#3b82f6` | Accents, active states |
| Accent Blue | `#60a5fa` | Highlights |
| Background | `#020617` | Main background (slate-950) |
| Card BG | `#1e293b` | Card backgrounds (slate-800) |
| Text | `#f1f5f9` | Primary text (slate-100) |
| Muted | `#94a3b8` | Secondary text (slate-400) |

### State Colors

| State | Color | Hex |
|-------|-------|-----|
| Light On | Yellow | `#fbbf24` |
| Fan On | Cyan | `#22d3ee` |
| Heat | Red | `#ef4444` |
| Cool | Blue | `#3b82f6` |
| Success | Green | `#22c55e` |
| Warning | Orange | `#f59e0b` |
| Error | Red | `#ef4444` |

## Screenshots

### Dashboard
![Dashboard](docs/dashboard.png)

### Sidebar
![Sidebar](docs/sidebar.png)

### Cards with Animations
![Animations](docs/animations.gif)

## Customization

### Modify Colors

Copy the theme file and edit the color values:

```yaml
# Your custom variant
BildaSystem Custom:
  primary-color: "#YOUR_COLOR"
  accent-color: "#YOUR_ACCENT"
```

### Disable Hover Effects

Remove the `card-mod-card` section from the theme file.

## Changelog

See [CHANGELOG.md](CHANGELOG.md) for version history.

## Contributing

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

## Credits

- Created by [BildaSystem.cz](https://bildassystem.cz)
- Inspired by Tailwind CSS color palette
- Built for the Home Assistant community

## License

MIT License - see [LICENSE](LICENSE)

---

**Made with love by BildaSystem.cz**
