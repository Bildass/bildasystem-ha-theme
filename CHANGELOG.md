# Changelog

All notable changes to this project will be documented in this file.

## [1.2.0] - 2025-12-11

### Fixed
- **BREAKING**: Updated animation syntax for Home Assistant 2024.12+ compatibility
- Old `ha-state-icon` targeting no longer works in HA 2024.12+
- Icons now use nested `mwc-icon > ha-svg-icon > svg` structure

### Changed
- Animation examples now use CSS custom properties on `:host`
- All animation examples target multiple icon elements: `ha-state-icon, ha-icon, mwc-icon, ha-svg-icon, svg`
- Added `display: inline-block !important` and `transform-origin: center !important` to all animations
- Documentation completely rewritten with working HA 2024.12+ examples

### Added
- Complete grid example with multiple animated cards
- Variable speed fan animation based on percentage
- Troubleshooting section for common animation issues

## [1.1.0] - 2025-12-11

### Fixed
- Fixed dropdown/select menus showing white background
- Fixed HACS panel theming (white content area)
- Fixed sidebar background not applying
- Reordered variables so `primary-background-color` is defined first (required for `var()` references)

### Added
- MDC theme surface variables for better Material Design component support
- Input and select field styling
- Dialog and popup background colors
- RGB color versions for `rgba()` usage

### Changed
- Variables now use `var()` references where possible for better consistency
- Improved code organization with clear section headers

## [1.0.0] - 2025-12-11

### Added
- Initial release
- BildaSystem Dark theme with slate color palette
- BildaSystem Light theme variant
- Card-mod integration for animations and hover effects
- Animation examples for:
  - Spinning fans
  - Pulsing lights
  - Door open animations
  - Temperature-based colors
  - Battery level indicators
  - Vacuum states
  - Motion sensor flash
- HACS compatibility
- Comprehensive documentation
- Example dashboard configuration

### Features
- Modern dark design inspired by Tailwind CSS colors
- Blue accent color (#3b82f6) throughout
- Smooth card hover transitions
- State-based icon colors for all common domains
- Code editor syntax highlighting
- Energy dashboard colors
