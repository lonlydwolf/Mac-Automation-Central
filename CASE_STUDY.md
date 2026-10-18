# Mac-Automation-Central — learning through personal automation

## Motivation and ownership

I started this project to learn automation and build useful automations for my Mac. I worked on the whole project, combining a Bash file organizer, a personal setup script, and a later Python experiment in AI-assisted file organization. The setup script was inspired by [CodingGarden/mac-setup](https://github.com/CodingGarden/mac-setup).

I no longer use these scripts. I am keeping the repository as a completed learning project, with its limitations visible, rather than committing to maintenance without a current need.

## What I built

The [Bash organizer](Scripts/organizer.sh) maps file extensions to folders. The [setup script](Scripts/mac_setup.sh) combines macOS preferences, AppleScript, Homebrew, and application installation into a personal setup recipe.

The [Smart Organizer](Smart_Scripts/Smart%20Organizer) explores a different approach: extract document text or inspect filenames, classify them with a local zero-shot model, then move each file into a category directory. It separates the CLI, configuration, helper functions, and filesystem operations, and includes six document-extraction tests.

Together, these show the project's progression from small shell automations to a Python application integrating document libraries and a machine-learning model. They are implementation examples, not evidence of production reliability or measured time savings.

## Later code review

A review on October 18, 2026 examined the implementation at commit [`0a1e99d`](https://github.com/lonlydwolf/Mac-Automation-Central/tree/0a1e99db133ec91aa90e8ec2eaa62c376a93f024). These are findings from that later review, not a reconstruction of what I noticed or found difficult while building the project:

- **Preserving files needs to come before categorizing them.** Temporary-directory checks reproduced silent replacement of existing files in both organizers. The Python mover's collision menu runs only after a rename error, while ordinary file replacement on macOS does not raise that error.
- **Destination paths need explicit boundaries.** For an extensionless filename, the Python categorizer produced an absolute category path outside the selected directory. The review checked the computed path without attempting a write there.
- **Dependency and startup behavior are part of usability.** The requirements list uses the `pytorch` placeholder instead of `torch`, and model initialization occurs during helper imports, before CLI argument parsing.
- **Tests should cover the consequence of a failure.** The existing tests exercise extraction, but not collisions, destination containment, partial moves, or classification quality. Six extraction tests passed in a scoped review environment using real extraction libraries, `fpdf2` in place of the manifest's `fpdf`, and a stubbed model initializer. That does not establish that the full dependency manifest or AI workflow works.

The review did not run the Mac setup script, test full model inference, or validate current macOS compatibility. These documentation changes leave the implementation unchanged. See the [README limitations](README.md#known-limitations--read-before-running-anything) before considering reuse.

## What I would do differently now

I would start with a recurring task that saves real day-to-day time, then choose the simplest approach that meets that need. I would use AI where it adds useful behavior, rather than adding complexity everywhere. For this organizer, that would mean comparing classification with extension rules on representative files before treating the model as necessary.

For any tool that moves personal files, I would first define safe behavior: no silent replacement, destinations confined to the chosen folder, a preview before changes, visible failures, and tests around those guarantees. I would also separate extraction from model initialization so that each part could be used and tested independently.

That is the direction I want to take as a product-minded software engineer building stronger backend and systems foundations, and eventually applying AI where it earns its cost. The useful next step is to carry those priorities into current projects. This repository remains a record of an earlier experiment; current Mac configuration work belongs in [dotfiles](https://github.com/lonlydwolf/dotfiles).
