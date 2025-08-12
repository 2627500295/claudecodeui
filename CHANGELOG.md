# Changelog

All notable changes to Claude Code WebUI will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

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