# Build Plan – Week by Week

## Week 1: Hardware Assembly and OS Setup

**Tasks:**
- Unbox and inventory all components
- Assemble Raspberry Pi with heatsink
- Flash Raspberry Pi OS (64-bit) to SD card
- Complete initial boot and system updates
- Install Python 3.10+ and essential packages (pip, git, build tools)
- Test USB microphone input (record test audio with `arecord`)
- Test speaker output (play test audio with `aplay`)

**Acceptance Criteria:**
- System boots reliably
- Microphone records clean 16 kHz audio
- Speaker plays audio without distortion
- SSH access configured for remote debugging

**Deliverables:**
- Journal entry documenting setup process
---

## Week 2: Audio Pipeline and Wake Word Detection

**Tasks:**
- Install PyAudio or sounddevice for real-time audio capture
- Set up continuous audio monitoring with ring buffer
- Install wake word detection library (Porcupine or alternative)
- Configure wake word model (choose wake phrase, e.g., "Hey Assistant")
- Test wake word detection with 20 test phrases
- Measure false positive rate and trigger latency

**Acceptance Criteria:**
- Wake word triggers reliably (<300ms latency)
- False positive rate <5% in quiet environment
- Audio buffer captures 3-5 seconds after wake word
- Detection works at 1-2 meters distance

**Deliverables:**
- Working wake word detection script
- Test results logged in JOURNAL.md
- Sample audio recordings in `media/`

---

## Week 3: Speech-to-Text (STT) Integration

**Tasks:**
- Install STT engine (Whisper-tiny or Vosk)
- Download and optimize model (quantization if needed)
- Integrate STT into wake word pipeline
- Test transcription accuracy on 20 common phrases
- Measure transcription latency and CPU usage
- Implement error handling for failed transcriptions

**Acceptance Criteria:**
- Transcription completes in <2 seconds for 5-second audio
- Word error rate (WER) <15% on clean speech
- System handles background noise gracefully
- Clear error messages logged for failures

**Deliverables:**
- STT module integrated into main pipeline
- Accuracy and latency benchmarks in JOURNAL.md
- Code committed to repo

---

## Week 4: Local Response Generation

**Tasks:**
- Install LLM runtime (llama.cpp or similar)
- Download small quantized model (e.g., TinyLlama, Phi-2)
- Create prompt template for assistant responses
- Test response generation with 10 sample queries
- Implement rule-based fallback for simple queries (time, date, weather)
- Optimize inference settings (context size, temperature)

**Acceptance Criteria:**
- Model generates coherent replies to simple questions
- Response generation completes in <3 seconds
- Fallback rules handle common queries instantly
- Memory usage stays under 4 GB

**Deliverables:**
- Working response generation module
- Sample query/response pairs in JOURNAL.md
- Performance metrics documented

---

## Week 5: Text-to-Speech (TTS) and Full Pipeline

**Tasks:**
- Install TTS engine (Coqui-TTS or eSpeak NG)
- Configure voice and speech parameters (speed, pitch)
- Integrate TTS into pipeline (text → audio → speaker)
- Test end-to-end pipeline: wake word → STT → LLM → TTS → playback
- Measure total response time for 10 test interactions
- Implement logging for each pipeline step

**Acceptance Criteria:**
- Full pipeline completes in <5 seconds for simple queries
- TTS voice is clear and understandable
- Audio playback works reliably
- All interactions logged with timestamps

**Deliverables:**
- Complete working prototype
- End-to-end demo video (1-2 minutes)
- Updated JOURNAL.md with full test results

---

## Week 6: Enclosure, Polish, and Documentation

**Tasks:**
- Design and build physical enclosure (3D print or laser cut)
- Mount all components securely inside case
- Add optional LED status indicator
- Test robustness in noisy environment (music, TV)
- Conduct 24-hour uptime test
- Finalize all documentation (README, DESIGN, JOURNAL)
- Create reproducible build instructions
- Record final demo and take photos

**Acceptance Criteria:**
- Device fits in enclosure with proper ventilation
- All cables managed and secured
- Device runs continuously for 24+ hours
- Documentation complete and clear
- Final demo video uploaded

**Deliverables:**
- Finished physical device
- Complete documentation set
- Final demo video and photos
- GitHub repo ready for public release

---

## Post-Week 6: Optional Stretch Goals

If time and budget allow, tackle stretch goals:
- Multi-microphone beamforming with array HAT
- Battery power for portable operation
- Touchscreen display for visual feedback
- Multi-language STT/TTS support
- Web interface for configuration
