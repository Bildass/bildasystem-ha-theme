# Changelog

All notable changes to this project will be documented in this file.

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
