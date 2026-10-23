A **changelog** is a file that contains a curated, chronologically ordered list of notable changes made to a project (typically software) during each version or release.
It helps users, contributors, and developers see exactly what has changed between different versions of the software without having to read through commit logs.
### Key Components of a Changelog
A well-maintained changelog usually groups modifications under specific headings:
- **Added**: For new features.
- **Changed**: For changes in existing functionality.
- **Deprecated**: For soon-to-be-removed features.
- **Removed**: For features now removed.
- **Fixed**: For any bug fixes.
- **Security**: In case of vulnerabilities addressed.
### Standard Format Example
Changelogs are commonly written in Markdown and saved as `CHANGELOG.md` in the root directory of a repository.
```markdown
# Changelog
## [1.1.0] - 2026-06-15
### Added
* User authentication via OAuth2.
* Dark mode toggle in user settings.
### Fixed
* Memory leak issue during large file uploads.
## [1.0.0] - 2026-05-01
### Added
* Initial release of the production API.
```
## Who created Changelog?
A changelog is a general software development practice and not a single tool created by one individual. However, the modern, widely accepted standard for formatting them—known as **Keep a Changelog**—was created by **Olivier Lacan**.
If you are referring to the template structure and convention used by most developers globally today, it was established to make changelogs human-readable and standardized across open-source projects.
# See also
- [keepachangelog.com](http://keepachangelog.com/)