# Mac-Automation-Central

A completed learning project exploring Mac automation with Bash and Python, from extension-based file sorting and system setup to an experimental local AI organizer.

**Status: retired and no longer maintained.** I no longer use these scripts. The source remains here as a record of the project; there is no active feature roadmap. Read the [case study](CASE_STUDY.md) for the motivation, implementation, and what I would approach differently today.

## Known limitations — read before running anything

These are historical experiments, **not ready-to-use utilities for valuable files or your current Mac setup**. A later review identified the following unresolved issues:

- **Both organizers can silently overwrite existing destination files.** The Bash version uses plain `mv`; the Python version attempts `Path.rename` before its collision-handling menu, which does not protect ordinary file collisions on macOS.
- **The Python organizer can construct a destination outside the selected folder for extensionless files.** For example, a category can become `/Personal` instead of a path beneath the target directory.
- The Bash organizer also moves existing directories into `Others`, suppresses move errors, and can report success despite failures. Neither organizer provides a dry-run mode.
- **The Python dependency manifest needs repair before installation:** it lists `pytorch`, whose [PyPI page](https://pypi.org/project/pytorch/) directs users to `torch`. Dependencies are unpinned. Importing the helpers initializes the model, so even help or extraction tests can trigger model loading and downloads.
- The setup script records testing only on macOS Sonoma 14.6.1, assumes Homebrew at `/opt/homebrew/bin/brew`, and changes system preferences and shell configuration. It has not been validated for current macOS versions or safe repeated execution.

These findings are documented here rather than fixed in this closeout. Review and address them before adapting the code; a disposable directory alone does not contain the Python path issue. The [case study](CASE_STUDY.md#later-code-review) records the scope of the checks.

## What is in the repository

### Bash file organizer

[Scripts/organizer.sh](Scripts/organizer.sh) groups files by extension into `Organized/Images`, `Documents`, `Archives`, `Videos`, `Music`, `Programs`, and `Others`. It accepts a target directory and defaults to `~/Downloads` when none is supplied.

This was the straightforward automation approach: explicit extension rules and shell file operations. The safety limitations above apply even when using a custom target directory.

### Mac setup script

[Scripts/mac_setup.sh](Scripts/mac_setup.sh) changes Dock and Finder preferences, uses AppleScript for Finder view options, installs Homebrew, and installs Raycast, Rectangle, Alt-Tab, Hidden Bar, and Stats.

It is a snapshot of an earlier personal setup. My current configuration work lives in [dotfiles](https://github.com/lonlydwolf/dotfiles), which includes declarative macOS defaults and a Brewfile.

This script was inspired by [Coding Garden](https://github.com/CodingGarden) and [CodingGarden/mac-setup](https://github.com/CodingGarden/mac-setup).

### Experimental Smart Organizer

[Smart_Scripts/Smart Organizer](Smart_Scripts/Smart%20Organizer) is a Python CLI that separates configuration, document extraction/classification, and file movement:

- [main.py](Smart_Scripts/Smart%20Organizer/main.py) accepts and validates a target directory, defaulting to `~/Downloads`.
- [helpers/utils.py](Smart_Scripts/Smart%20Organizer/helpers/utils.py) extracts document text and uses the local `facebook/bart-large-mnli` transformers pipeline to classify document content or filenames. Model assets may need to be downloaded; the implementation does not use a hosted inference API.
- [system/os_handlers.py](Smart_Scripts/Smart%20Organizer/system/os_handlers.py) creates category directories and moves files.
- [tests/test_utils.py](Smart_Scripts/Smart%20Organizer/tests/test_utils.py) contains six extraction tests for PDF, DOCX, TXT, XLSX, PPTX, and Markdown. They do not test safe file movement or classification quality.

The experiment adds content-based categorization, but it does not establish that AI sorting is more useful or reliable than simple rules. There is no maintained installation or execution workflow.

## License

[MIT](LICENSE). Copyright © 2024 Ahmad Mollaei.
