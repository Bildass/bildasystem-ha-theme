# BildaSystem Theme - Animation Guide

This guide shows how to add animations to your Home Assistant dashboard using card-mod.

## Requirements

- [card-mod](https://github.com/thomasloven/lovelace-card-mod) (Install via HACS)
- [Mushroom Cards](https://github.com/piitaya/lovelace-mushroom) (Recommended)

## Built-in Animations

The BildaSystem theme includes these CSS animations:

| Animation | CSS Class | Use Case |
|-----------|-----------|----------|
| `spin` | Built-in | Fans, loading |
| `pulse` | Built-in | Lights, alerts |

## Animation Examples

### Spinning Fan

When the fan is on, the icon spins. Speed varies based on fan percentage.

```yaml
type: custom:mushroom-fan-card
entity: fan.bedroom
card_mod:
  style: |
    ha-state-icon {
      {% if is_state(config.entity, 'on') %}
        {% set speed = state_attr(config.entity, 'percentage') | int %}
        {% if speed > 66 %}
          animation: spin 0.5s linear infinite;
        {% elif speed > 33 %}
          animation: spin 1s linear infinite;
        {% else %}
          animation: spin 2s linear infinite;
        {% endif %}
      {% endif %}
    }
```

### Pulsing Light

Light icon gently pulses when on.

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

### Door Open Animation

Icon tilts when door/window is open.

```yaml
type: custom:mushroom-entity-card
entity: binary_sensor.front_door
icon: mdi:door
card_mod:
  style: |
    ha-state-icon {
      transition: transform 0.3s ease;
      {% if is_state(config.entity, 'on') %}
        transform: rotateY(-20deg);
      {% endif %}
    }
```

### Temperature-Based Colors

Icon color changes based on temperature value.

```yaml
type: custom:mushroom-entity-card
entity: sensor.living_room_temperature
card_mod:
  style: |
    ha-state-icon {
      {% set temp = states(config.entity) | float(0) %}
      {% if temp < 18 %}
        color: #3b82f6 !important;  /* Cold - Blue */
      {% elif temp < 20 %}
        color: #22d3ee !important;  /* Cool - Cyan */
      {% elif temp < 24 %}
        color: #22c55e !important;  /* Normal - Green */
      {% elif temp < 26 %}
        color: #f59e0b !important;  /* Warm - Orange */
      {% else %}
        color: #ef4444 !important;  /* Hot - Red */
      {% endif %}
    }
```

### Climate Card with Heat/Cool Animation

```yaml
type: custom:mushroom-climate-card
entity: climate.living_room
card_mod:
  style: |
    ha-state-icon {
      {% if is_state_attr(config.entity, 'hvac_action', 'heating') %}
        color: #ef4444 !important;
        animation: pulse 1s ease-in-out infinite;
      {% elif is_state_attr(config.entity, 'hvac_action', 'cooling') %}
        color: #3b82f6 !important;
        animation: pulse 1.5s ease-in-out infinite;
      {% endif %}
    }
```

### Battery Level Colors

```yaml
type: custom:mushroom-entity-card
entity: sensor.phone_battery
card_mod:
  style: |
    ha-state-icon {
      {% set level = states(config.entity) | int(0) %}
      {% if level < 20 %}
        color: #ef4444 !important;
        animation: pulse 0.5s ease-in-out infinite;
      {% elif level < 50 %}
        color: #f59e0b !important;
      {% else %}
        color: #22c55e !important;
      {% endif %}
    }
```

### Vacuum Cleaning Animation

```yaml
type: custom:mushroom-entity-card
entity: vacuum.roborock
card_mod:
  style: |
    ha-state-icon {
      {% if is_state(config.entity, 'cleaning') %}
        animation: spin 2s linear infinite;
        color: #3b82f6 !important;
      {% elif is_state(config.entity, 'returning') %}
        animation: pulse 1s ease-in-out infinite;
        color: #22c55e !important;
      {% endif %}
    }
```

### Motion Sensor Flash

```yaml
type: custom:mushroom-entity-card
entity: binary_sensor.motion_hallway
icon: mdi:motion-sensor
card_mod:
  style: |
    @keyframes flash {
      0%, 100% { opacity: 1; }
      50% { opacity: 0.3; }
    }
    ha-state-icon {
      {% if is_state(config.entity, 'on') %}
        color: #f59e0b !important;
        animation: flash 0.5s ease-in-out 3;
      {% endif %}
    }
```

## Card Hover Effects

BildaSystem theme includes hover effects by default. You can customize them:

```yaml
type: custom:mushroom-entity-card
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
type: custom:mushroom-light-card
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

## Tips

1. **Performance**: Avoid too many animations on one page
2. **Accessibility**: Some users prefer reduced motion
3. **Testing**: Refresh page after changing animations
4. **Debugging**: Use browser DevTools to inspect applied styles

## Custom @keyframes

You can define custom animations:

```yaml
card_mod:
  style: |
    @keyframes wobble {
      0%, 100% { transform: rotate(0deg); }
      25% { transform: rotate(-5deg); }
      75% { transform: rotate(5deg); }
    }
    ha-state-icon {
      animation: wobble 0.5s ease-in-out;
    }
```

## Troubleshooting

### Animation not working?

1. Make sure card-mod is installed and configured
2. Check if entity state matches the condition
3. Try adding `!important` to CSS properties
4. Clear browser cache and refresh

### Animation stuttering?

1. Reduce number of animated cards
2. Use simpler animations (spin, pulse)
3. Avoid multiple animations on same element
