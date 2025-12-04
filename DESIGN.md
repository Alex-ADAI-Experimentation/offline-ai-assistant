# Design Plan for Offline AI Personal Assistant Device

This document explains how the device will be structured, how it will work, and how each subsystem connects.

---

## 1. System Architecture Overview

The device consists of four major subsystems:

### A. Audio Input Subsystem
- USB microphone or microphone array  
- Captures audio continuously  
- Filters background noise  
- Sends audio frames to wake-word model  

### B. Wake Word Detection Subsystem
- Lightweight neural network model  
- Listens for a specific wake word  
- Must run in real time with low latency  
- When triggered, activates full speech processing pipeline  

### C. Speech Processing & AI Subsystem
- Local speech-to-text engine  
- Local language model for generating responses  
- Simple intent interpretation  

### D. Audio Output Subsystem
- Speaker connected via USB or 3.5mm  
- Plays generated audio using text-to-speech  
- Volume control handled in software

---

## 2. Hardware Block Diagram (Text-Based)


       ┌─────────────────────────────────────┐
       │          User (Voice Input)         │
       └─────────────────────────────────────┘
                       │
                       ▼
    ┌──────────────────────────────┐
    │   Microphone / Mic Array     │
    └──────────────────────────────┘
                       │ Audio Stream
                       ▼
        ┌────────────────────────┐
        │ Wake Word Detection     │
        └────────────────────────┘
            │ No       │ Yes
            ▼          ▼
    (Ignore Audio)   ┌───────────────────────┐
                     │ Speech to Text Engine │
                     └───────────────────────┘
                               │
                               ▼
                  ┌────────────────────────────┐
                  │ Local AI Response Generator │
                  └────────────────────────────┘
                               │
                               ▼
                       ┌───────────────┐
                       │ Text to Speech │
                       └───────────────┘
                               │
                               ▼
                    ┌────────────────────────┐
                    │ Speaker / Audio Output │
                    └────────────────────────┘

---

## 3. Software Architecture

### A. Wake-Word Engine
- Lightweight model (e.g., Porcupine or custom)  
- Low CPU usage  
- Always running  

### B. Speech Recognition
- Offline STT model (e.g., Whisper tiny or custom small model)  
- Converts microphone audio into text in real time  

### C. Response Model
- Small, efficient language model (phi, GPT2-medium, or similar)  
- Runs fully offline  
- Takes the transcribed text and generates a response  

### D. TTS (Text-to-Speech)
- Small offline TTS engine  
- Converts generated response into audio  

---

## 4. Physical Design Concept

### Enclosure:
- Simple rectangular speaker-style shell  
- 3D printed or constructed with laser-cut acrylic  
- Holes for mic input and speaker output  
- Power input at the rear  

### Internal layout:
- SBC mounted on standoffs  
- Microphone placed at top/front  
- Speaker mounted facing forward  
- Cables routed under SBC for airflow  

---

## 5. Stretch Goals (Optional)
- Improved multi-microphone beamforming  
- Touchscreen display  
- Battery-powered portable mode  
- Custom wake-word model  
