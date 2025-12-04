# Offline AI Personal Assistant Device

A privacy-focused home assistant that runs entirely offline using a single-board computer, microphone, and speaker. This device detects wake words, transcribes speech locally, generates responses using a lightweight language model, and plays replies through text-to-speech—all without cloud connectivity.

## Project Goals

- **Privacy-first**: All speech processing and AI inference happen on-device
- **Educational**: Hands-on learning with embedded hardware, edge AI, and real-time audio processing
- **Reproducible**: Documented build process within a modest budget (£200–£350)
- **Practical**: Demonstrates how edge AI can work in resource-constrained environments

## Main Features

- Wake word detection with low false-positive rate
- Offline speech-to-text (STT) transcription
- Local language model for response generation
- Text-to-speech (TTS) for audio replies
- Simple voice commands (time, timers, basic Q&A)

## Hardware Overview

Built around a Raspberry Pi 5 (or similar SBC) with USB microphone, powered speaker, and optional 3D-printed enclosure. See [PARTS.md](PARTS.md) for the complete parts list and budget breakdown.

## Timeline

This is a 6-week build project with weekly milestones:
- Weeks 1–2: Hardware assembly and OS setup
- Weeks 3–4: Wake word and STT integration
- Weeks 5–6: Response generation, TTS, and final testing

See [BUILD_PLAN.md](BUILD_PLAN.md) for detailed weekly tasks.

## Why This Project Matters

Privacy concerns around always-listening cloud assistants are growing. This project demonstrates that useful voice interaction can happen entirely on affordable, locally-controlled hardware—empowering users to keep their data private while learning practical embedded AI skills.

## Documentation

- **[DESIGN.md](DESIGN.md)** – System architecture, software stack, and design decisions
- **[PARTS.md](PARTS.md)** – Complete parts list with costs and sourcing info
- **[BUILD_PLAN.md](BUILD_PLAN.md)** – Week-by-week build schedule with acceptance criteria
- **[JOURNAL.md](JOURNAL.md)** – Build log for tracking progress, problems, and solutions

## How to Contribute

Contributions are welcome! To get involved:

1. **Report issues**: Open an issue to report bugs or suggest features
2. **Submit PRs**: Fork the repo, create a feature branch, and submit a pull request
3. **Follow the journal**: Check JOURNAL.md for current progress and next steps

Please be respectful and constructive in all interactions.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please read our [Code of Conduct](CODE_OF_CONDUCT.md) before contributing.
