# Design Documentation

## System Architecture Overview

This device operates as a standalone offline voice assistant with four main subsystems:

### 1. Audio Input
- USB microphone or microphone array HAT captures ambient audio
- Audio stream processed in real-time at 16 kHz sample rate
- Ring buffer maintains recent audio for wake word detection

### 2. Wake Word Detection
- Lightweight model (e.g., Porcupine) runs continuously
- Low CPU usage (~5–10% on Raspberry Pi 5)
- Triggers capture pipeline when wake phrase detected
- Target latency: <300ms from utterance to trigger

### 3. Speech Processing & AI
- **Speech-to-Text (STT)**: Whisper-tiny (quantized) or Vosk model transcribes captured audio
- **Response Generation**: Small quantized language model (e.g., GGML format) or rule-based system generates reply text
- **Processing Pipeline**: Orchestrated by Python daemon with error handling and logging

### 4. Audio Output
- **Text-to-Speech (TTS)**: Coqui-TTS or eSpeak NG converts reply to audio
- Playback through USB speaker or 3.5mm output
- Volume control and audio quality settings configurable

## Hardware Block Diagram
```
┌─────────────────────────────────────────────────────────┐
│                    Power Supply (USB-C)                  │
└────────────────────────┬────────────────────────────────┘
                         │
         ┌───────────────┴───────────────┐
         │   Raspberry Pi 5 (8GB RAM)     │
         │   - Quad-core ARM Cortex-A76   │
         │   - 32-64 GB SD card           │
         │   - Heatsink / passive cooling │
         └───┬─────────────────┬──────────┘
             │                 │
    ┌────────┴────────┐   ┌────┴─────────┐
    │  USB Microphone │   │ USB/3.5mm    │
    │  or Mic Array   │   │ Speaker      │
    │  HAT            │   │              │
    └─────────────────┘   └──────────────┘
             │                 │
         [Audio In]        [Audio Out]
             │                 │
    ┌────────┴─────────────────┴─────────┐
    │   Optional: LED status indicator    │
    │   Optional: Physical button         │
    └─────────────────────────────────────┘
```

## Software Architecture

### Stack

- **Operating System**: Raspberry Pi OS (64-bit, Debian-based)
- **Language**: Python 3.10+
- **Wake Word**: Porcupine (Picovoice) or custom trained model
- **STT Engine**: Whisper-tiny with INT8 quantization, or Vosk lightweight model
- **LLM Runtime**: llama.cpp with GGUF models, or rule-based fallback
- **TTS Engine**: Coqui-TTS (VITS model) or eSpeak NG
- **Audio Interface**: PyAudio or sounddevice for capture/playback

### Orchestration Flow
```python
# Pseudo-code pipeline
while True:
    audio_frame = microphone.read()
    
    if wake_word_detector.detect(audio_frame):
        # Capture 3-5 seconds of audio after wake word
        full_audio = microphone.record(duration=5)
        
        # Transcribe
        text = stt_engine.transcribe(full_audio)
        
        # Generate response
        reply_text = llm.generate(text)
        
        # Convert to speech
        audio_reply = tts_engine.synthesize(reply_text)
        
        # Play back
        speaker.play(audio_reply)
        
        # Log interaction
        journal.log(text, reply_text)
```

### Data Flow

1. Continuous audio monitoring with ring buffer (2-second window)
2. Wake word triggers full audio capture (3-5 seconds)
3. Audio → STT → Text input
4. Text → LLM inference → Text reply
5. Text reply → TTS → Audio waveform
6. Audio waveform → Speaker playback
7. All steps logged to JOURNAL.md with timestamps

## Physical Design Concept

### Enclosure
- 3D-printed case or laser-cut acrylic box (dimensions ~120×80×40mm)
- Top surface: microphone opening with acoustic mesh
- Front panel: speaker grille, optional LED indicator
- Side panel: USB-C power port, 3.5mm audio jack (optional)
- Bottom: rubber feet for stability and vibration damping

### Internal Layout
- Raspberry Pi mounted on standoffs
- Speaker positioned behind front grille
- Cable management for USB mic and power
- Airflow channels for passive cooling (heatsink on CPU)

## Stretch Goals

1. **Multi-microphone beamforming**: Use 4-mic array HAT for improved noise rejection
2. **Battery operation**: Add 5V battery pack (10,000 mAh) for portable mode
3. **Visual feedback**: Small OLED or LED matrix for status/waveform display
4. **Touchscreen option**: 3.5" display for settings and visual responses
5. **Multi-language support**: Add STT/TTS models for additional languages

## Tradeoffs

### Model Size vs. Latency vs. Accuracy
- **Tiny models** (50–200 MB): Fast (<1s inference), lower accuracy
- **Small models** (500 MB–1 GB): Balanced performance (~2-3s inference)
- **Medium models** (2–4 GB): High accuracy but slower (5–10s inference)

**Decision**: Use small quantized models for MVP, with option to swap larger models for specific use cases.

### Power and Heat Management
- Raspberry Pi 5 can draw 3–5W under load
- Heatsink required for sustained AI workloads
- Small fan optional for enclosed designs

### Cost Constraints
- Target: £200–£350 total
- Core components: ~£150–£200
- Enclosure and extras: £50–£100
- Budget buffer for shipping: £50

## Testing Plan

### Unit Tests
- Wake word detection: 100 test phrases, <2% false positive rate
- STT accuracy: WER (word error rate) <10% on clean audio
- TTS quality: MOS (mean opinion score) >3.5
- Response latency: <5 seconds end-to-end

### Integration Tests
- Full pipeline test with 20 common queries
- Multi-turn conversation simulation
- Error recovery (mic disconnect, model failure)

### Robustness Tests
- Background noise (TV, music, conversations)
- Different accents and speech patterns
- Various distances from microphone (1m, 2m, 3m)
- Continuous operation test (24-hour uptime)

### Safety Tests
- Volume limiting (max 85 dB at 1m)
- Thermal monitoring under sustained load
- Graceful shutdown on power loss
