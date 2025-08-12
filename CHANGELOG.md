# Changelog

All notable changes to Claude Code WebUI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).
All notable changes to this project will be documented in this file.

## [Unreleased]

### Added
- Enhanced project deletion functionality - projects can now be deleted regardless of session count
- Additional CodeMirror dependencies for improved code editor support
  - @codemirror/language for enhanced language support
  - @codemirror/state for better state management
  - @codemirror/view for improved editor views

### Changed
- Removed restriction preventing deletion of projects with existing sessions
- Updated project deletion UI to show delete button for all projects
- Modified confirmation dialog text to reflect new deletion capabilities
- Streamlined project management workflow for better user experience

### Technical Details
- Modified `deleteProject()` function in server/projects.js to remove empty project check
- Updated Sidebar.jsx component to remove conditional delete button rendering
- Enhanced user interface consistency across mobile and desktop views

---

## Previous Releases

*This changelog was created with this release. Previous releases were not documented in this format.*
- **Model Selection Control**: Interactive model selector supporting Sonnet (default), Opus, and Haiku models
  - Color-coded visual indicators for each model type
  - Click-to-cycle interface with smooth transitions
  - Purple indicator for Sonnet, Amber for Opus, Emerald for Haiku
  - Positioned to the left of Permission Mode selector

- **Thinking Mode Control**: Five-level thinking intensity selector
  - Auto, Think, Think Hard, Think Harder, UltraThink modes
  - Progressive bar chart visualization showing intensity levels
  - Color-coded themes from gray (Auto) to purple (UltraThink)
  - Graduated visual indicators with 1-5 bar heights

- **Backend Integration**: Enhanced message protocol
  - Model and thinking mode parameters passed to Claude CLI
  - WebSocket message format updated to include model/thinkingMode fields
  - Backward-compatible parameter structure

- **Documentation**: Comprehensive feature analysis and requirements
  - Multi-expert perspective documentation (Requirements Analysis, Business Decomposition, Architecture Design, Project Management, Technical Solutions)
  - User personas and journey mapping
  - System architecture diagrams and technical specifications
  - Implementation guidelines and best practices

### Changed
- **Chat Interface Layout**: Enhanced control panel design
  - Reorganized selector positioning for better visual hierarchy
  - Improved responsive design for mobile and desktop
  - Enhanced hover states and interaction feedback

### Technical Details
- React state management for model and thinking mode selection
- Tailwind CSS responsive design with dark/light theme support
- Cyclic state transitions with keyboard and click interactions
- Type-safe thinking mode definitions: `"auto" | "think" | "think_hard" | "think_harder" | "ultrathink"`

### Files Modified
- `src/components/ChatInterface.jsx`: Added model selection and thinking mode controls
- `docs/thinking-mode-feature.md`: Feature-specific documentation
- `docs/comprehensive-feature-analysis.md`: Multi-expert analysis document

---

🤖 Generated with [Claude Code](https://claude.ai/code)
