---
name: audio-transcriber
description: "Transcribe audio messages (OGG, WAV, MP3, etc.) to text using speech recognition. Use when users send voice notes, audio files, or explicitly request transcription. Handles format conversion via ffmpeg and provides formatted transcription output."
deps: ["ffmpeg", "python3"]
---

# Audio Transcriber Skill

Transcribe audio files to text using Google Speech Recognition (free tier) or Whisper (offline).

## When to Use

- User sends an audio/voice message in Telegram
- User uploads an audio file and asks for transcription
- User mentions "transcribe", "what did I say", "convert audio to text"

## Prerequisites

Check and install dependencies:

```bash
# Check ffmpeg
which ffmpeg || sudo apt-get install -y ffmpeg

# Check speech_recognition
python3 -c "import speech_recognition" 2>&1 || pip3 install --break-system-packages speechrecognition
```

## Workflow

### Step 1: Locate Audio File

When user sends audio in Telegram, the file path will be in the message metadata. Typical location:
```
/home/sak/.microclaw/data/media/inbound/file_*.ogg
```

Or user may provide a direct path.

### Step 2: Convert to WAV

```bash
# Convert any audio format to WAV (required for speech_recognition)
ffmpeg -y -i /path/to/input.ogg -acodec pcm_s16le -ar 16000 -ac 1 /tmp/audio_transcribe.wav
```

Supported inputs: OGG, MP3, WAV, FLAC, M4A, AIFF

### Step 3: Transcribe

Create a simple Python script or run inline:

```python
import speech_recognition as sr

r = sr.Recognizer()
audio_path = "/tmp/audio_transcribe.wav"

try:
    with sr.AudioFile(audio_path) as source:
        audio = r.record(source)
    
    # Use Google's free speech recognition
    text = r.recognize_google(audio)
    print(text)
except sr.UnknownValueError:
    print("ERROR: Could not understand audio - poor quality or no speech detected")
except sr.RequestError as e:
    print(f"ERROR: API request failed - {e}")
except Exception as e:
    print(f"ERROR: {e}")
```

### Step 4: Format Output

Present transcription in a clean format:

```
🎤 **Audio Transcription**

> [Transcribed text here]

Duration: [X seconds]
```

If the audio contains a question or request, respond to it directly after the transcription.

## Alternative: Whisper (Offline)

For better accuracy or privacy:

```bash
# Install Whisper
pip3 install --break-system-packages openai-whisper

# Transcribe
python3 -c "import whisper; model = whisper.load_model('base'); result = model.transcribe('/tmp/audio.wav'); print(result['text'])"
```

## Error Handling

**"Audio file could not be read"**
- Ensure WAV conversion completed successfully
- Check file exists: `ls -lh /tmp/audio_transcribe.wav`

**"Could not understand audio"**
- Audio quality too poor
- Background noise too loud
- Non-speech audio (music, silence)
- Ask user to resend or provide text

**"API rate limit exceeded"**
- Google Speech API temporary limit
- Wait 60 seconds and retry
- Or switch to Whisper

**"No module named 'speech_recognition'"**
- Install: `pip3 install --break-system-packages speechrecognition`

## Performance Notes

- **Processing time:** ~2-5 seconds per minute of audio (Google API)
- **Accuracy:** ~90-95% for clear English speech
- **File size:** WAV conversion creates ~1MB per minute
- **Cleanup:** Remove temp files after transcription: `rm /tmp/audio_transcribe.wav`

## Privacy Considerations

- Google Speech API sends audio to Google servers
- For sensitive content, use Whisper (fully offline)
- Temp files in `/tmp/` are auto-cleaned on reboot

## Example Usage

```bash
# Full pipeline
INPUT="/path/to/voice_note.ogg"
ffmpeg -y -i "$INPUT" -acodec pcm_s16le -ar 16000 -ac 1 /tmp/audio.wav
python3 -c "
import speech_recognition as sr
r = sr.Recognizer()
with sr.AudioFile('/tmp/audio.wav') as source:
    audio = r.record(source)
print(r.recognize_google(audio))
"
rm /tmp/audio.wav
```

## Tips

- For long audio (>5 min), consider splitting into chunks
- Test with short clips first to verify setup
- Keep temp files until transcription succeeds
- Provide context if transcription seems wrong (accents, technical terms)
