# BildaSystem Theme - Animation Guide

This guide shows how to add animations to your Home Assistant dashboard using card-mod.

## Requirements

- [card-mod](https://github.com/thomasloven/lovelace-card-mod) (Install via HACS)
- [Mushroom Cards](https://github.com/piitaya/lovelace-mushroom) (Recommended)
- Home Assistant 2024.12 or newer

## IMPORTANT: HA 2024.12+ Changes

Home Assistant 2024.12+ changed how icons are rendered internally. The icon now uses a nested structure:
`mwc-icon > ha-svg-icon > svg`

**Old syntax (no longer works):**
```yaml
ha-state-icon {
  animation: spin 1s linear infinite;
}
```

**New syntax (HA 2024.12+):**
```yaml
card_mod:
  style: |
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    :host { --icon-animation: none; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: spin 1s linear infinite; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

## Animation Examples

### Spinning Fan (Cyan)

When the fan is on, the icon spins with cyan color.

```yaml
type: custom:button-card
entity: fan.bedroom
icon: mdi:fan
card_mod:
  style: |
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: spin 1s linear infinite; --icon-color: #22d3ee !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

For Mushroom cards:
```yaml
type: custom:mushroom-fan-card
entity: fan.bedroom
card_mod:
  style: |
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: spin 1s linear infinite; --icon-color: #22d3ee !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Variable Speed Fan

Speed varies based on fan percentage:

```yaml
type: custom:button-card
entity: fan.bedroom
icon: mdi:fan
card_mod:
  style: |
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
      {% set speed = state_attr(config.entity, 'percentage') | int %}
      {% if speed > 66 %}
    :host { --icon-animation: spin 0.5s linear infinite; --icon-color: #22d3ee !important; }
      {% elif speed > 33 %}
    :host { --icon-animation: spin 1s linear infinite; --icon-color: #22d3ee !important; }
      {% else %}
    :host { --icon-animation: spin 2s linear infinite; --icon-color: #22d3ee !important; }
      {% endif %}
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Pulsing Light (Yellow)

Light icon gently pulses when on.

```yaml
type: custom:button-card
entity: light.living_room
icon: mdi:lightbulb
card_mod:
  style: |
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.15); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: pulse 2s ease-in-out infinite; --icon-color: #fbbf24 !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

For Mushroom cards:
```yaml
type: custom:mushroom-light-card
entity: light.living_room
card_mod:
  style: |
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.15); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: pulse 2s ease-in-out infinite; --icon-color: #fbbf24 !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Door Open Animation

Icon tilts when door/window is open.

```yaml
type: custom:button-card
entity: binary_sensor.front_door
icon: mdi:door
card_mod:
  style: |
    @keyframes door-open {
      from { transform: perspective(100px) rotateY(0deg); }
      to { transform: perspective(100px) rotateY(-20deg); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: door-open 0.5s ease-out forwards; --icon-color: #f59e0b !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Temperature-Based Colors

Icon color changes based on temperature value.

```yaml
type: custom:button-card
entity: sensor.living_room_temperature
icon: mdi:thermometer
card_mod:
  style: |
    {% set temp = states(config.entity) | float(0) %}
    :host {
      {% if temp < 18 %}
      --icon-color: #3b82f6 !important;  /* Cold - Blue */
      {% elif temp < 20 %}
      --icon-color: #22d3ee !important;  /* Cool - Cyan */
      {% elif temp < 24 %}
      --icon-color: #22c55e !important;  /* Normal - Green */
      {% elif temp < 26 %}
      --icon-color: #f59e0b !important;  /* Warm - Orange */
      {% else %}
      --icon-color: #ef4444 !important;  /* Hot - Red */
      {% endif %}
    }
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      color: var(--icon-color) !important;
    }
```

### Climate Card with Heat/Cool Animation

```yaml
type: custom:button-card
entity: climate.living_room
icon: mdi:thermostat
card_mod:
  style: |
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.15); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state_attr(config.entity, 'hvac_action', 'heating') %}
    :host { --icon-animation: pulse 1s ease-in-out infinite; --icon-color: #ef4444 !important; }
    {% elif is_state_attr(config.entity, 'hvac_action', 'cooling') %}
    :host { --icon-animation: pulse 1.5s ease-in-out infinite; --icon-color: #3b82f6 !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Battery Level Colors with Low Battery Pulse

```yaml
type: custom:button-card
entity: sensor.phone_battery
icon: mdi:battery
card_mod:
  style: |
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.15); }
    }
    {% set level = states(config.entity) | int(0) %}
    :host {
      {% if level < 20 %}
      --icon-animation: pulse 0.5s ease-in-out infinite;
      --icon-color: #ef4444 !important;
      {% elif level < 50 %}
      --icon-animation: none;
      --icon-color: #f59e0b !important;
      {% else %}
      --icon-animation: none;
      --icon-color: #22c55e !important;
      {% endif %}
    }
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Vacuum Cleaning Animation

```yaml
type: custom:button-card
entity: vacuum.roborock
icon: mdi:robot-vacuum
card_mod:
  style: |
    @keyframes spin {
      from { transform: rotate(0deg); }
      to { transform: rotate(360deg); }
    }
    @keyframes pulse {
      0%, 100% { opacity: 1; transform: scale(1); }
      50% { opacity: 0.6; transform: scale(1.15); }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'cleaning') %}
    :host { --icon-animation: spin 2s linear infinite; --icon-color: #3b82f6 !important; }
    {% elif is_state(config.entity, 'returning') %}
    :host { --icon-animation: pulse 1s ease-in-out infinite; --icon-color: #22c55e !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

### Motion Sensor Flash

```yaml
type: custom:button-card
entity: binary_sensor.motion_hallway
icon: mdi:motion-sensor
card_mod:
  style: |
    @keyframes flash {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }
    :host { --icon-animation: none; --icon-color: inherit; }
    {% if is_state(config.entity, 'on') %}
    :host { --icon-animation: flash 0.5s ease-in-out 3; --icon-color: #f59e0b !important; }
    {% endif %}
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      color: var(--icon-color) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

## Card Hover Effects

BildaSystem theme includes hover effects by default. You can customize them:

```yaml
type: custom:button-card
entity: light.bedroom
card_mod:
  style: |
    ha-card {
      transition: all 0.3s ease !important;
    }
    ha-card:hover {
      transform: scale(1.02) !important;
      border-color: rgba(59, 130, 246, 0.5) !important;
    }
```

## Glow Effect for Active Entities

```yaml
type: custom:button-card
entity: light.kitchen
card_mod:
  style: |
    ha-card {
      {% if is_state(config.entity, 'on') %}
        box-shadow: 0 0 20px rgba(251, 191, 36, 0.3) !important;
        border-color: rgba(251, 191, 36, 0.3) !important;
      {% endif %}
    }
```

## Complete Example Grid

Here's a complete grid with multiple animated cards:

```yaml
type: grid
columns: 4
square: false
cards:
  # Fan with spin animation
  - type: custom:button-card
    entity: fan.bedroom
    icon: mdi:fan
    name: Ventilátor
    card_mod:
      style: |
        @keyframes spin {
          from { transform: rotate(0deg); }
          to { transform: rotate(360deg); }
        }
        :host { --icon-animation: none; --icon-color: inherit; }
        {% if is_state(config.entity, 'on') %}
        :host { --icon-animation: spin 1s linear infinite; --icon-color: #22d3ee !important; }
        {% endif %}
        ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
          animation: var(--icon-animation) !important;
          color: var(--icon-color) !important;
          transform-origin: center !important;
          display: inline-block !important;
        }

  # Light with pulse animation
  - type: custom:button-card
    entity: light.living_room
    icon: mdi:lightbulb
    name: Světlo
    card_mod:
      style: |
        @keyframes pulse {
          0%, 100% { opacity: 1; transform: scale(1); }
          50% { opacity: 0.6; transform: scale(1.15); }
        }
        :host { --icon-animation: none; --icon-color: inherit; }
        {% if is_state(config.entity, 'on') %}
        :host { --icon-animation: pulse 2s ease-in-out infinite; --icon-color: #fbbf24 !important; }
        {% endif %}
        ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
          animation: var(--icon-animation) !important;
          color: var(--icon-color) !important;
          transform-origin: center !important;
          display: inline-block !important;
        }
```

## Tips

1. **Performance**: Avoid too many animations on one page (max 10-15)
2. **Accessibility**: Some users prefer reduced motion
3. **Testing**: Refresh page after changing animations (Ctrl+Shift+R)
4. **Debugging**: Use browser DevTools (F12) to inspect applied styles
5. **Cache**: Clear browser cache if animations don't update

## Custom @keyframes

You can define custom animations. Always define them inside the `style: |` block:

```yaml
card_mod:
  style: |
    @keyframes wobble {
      0%, 100% { transform: rotate(0deg); }
      25% { transform: rotate(-5deg); }
      75% { transform: rotate(5deg); }
    }
    :host { --icon-animation: wobble 0.5s ease-in-out; }
    ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg {
      animation: var(--icon-animation) !important;
      transform-origin: center !important;
      display: inline-block !important;
    }
```

## Troubleshooting

### Animation not working?

1. **Check HA version** - Must be 2024.12 or newer for this syntax
2. **Verify card-mod is installed** - Check in HACS > Frontend
3. **Use correct syntax** - Must include `:host` with CSS variables
4. **Target all icon elements** - Use `ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg`
5. **Include display: inline-block** - Required for transform animations
6. **Clear browser cache** - Ctrl+Shift+Del or hard refresh
7. **Restart HA** - Sometimes needed after card-mod updates

### Animation stuttering?

1. Reduce number of animated cards on page
2. Use simpler animations (spin, pulse)
3. Avoid multiple animations on same element
4. Check browser performance in DevTools

### Colors not changing?

1. Add `!important` to color declarations
2. Use CSS variables pattern shown in examples
3. Check entity state matches condition
