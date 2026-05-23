# submerge

Subtitle tools for OCR, ASR, and intelligent transcript merging.

`submerge` is a local-first subtitle toolkit for:
- extracting hardcoded subtitles with OCR
- transcribing speech with Whisper ASR
- intelligently merging OCR + ASR into cleaner subtitles

Built for:
- YouTube videos
- local media files
- multilingual subtitles
- noisy dialogue
- Traditional Chinese content
- subtitle restoration workflows

---

# Features

## OCR Transcription
Extract hardcoded subtitles directly from video frames using EasyOCR.

Supports:
- Traditional Chinese
- English
- outlined subtitles
- low-resolution YouTube video
- embedded subtitles

Outputs:
- `.txt`
- `.json`
- `.srt`

---

## ASR Transcription
Generate subtitles directly from speech audio using MLX Whisper.

Supports:
- YouTube URLs
- local media files
- Apple Silicon acceleration
- word timestamps
- confidence metrics
- hallucination mitigation

Outputs:
- `.txt`
- `.json`
- `.srt`
- `.vtt`

---

## Intelligent OCR + ASR Fusion
Merge OCR subtitles with ASR transcripts using:
- timestamps
- confidence scoring
- text similarity
- arbitration logic

Goal:
- cleaner subtitles
- fewer hallucinations
- better proper nouns
- improved subtitle accuracy

Outputs:
- `.json`
- `.txt`
- `.srt`
- `.vtt`

---

# Included Scripts

| Script | Purpose |
|---|---|
| `OCR_transcriber.py` | OCR hardcoded subtitles from video |
| `ASR_transcriber.py` | Speech transcription using MLX Whisper |
| `ASR+OCR_merger.py` | Merge OCR + ASR intelligently |

---

# Installation

## System Dependencies (macOS)

Install required tools:

```bash
brew install ffmpeg yt-dlp
```

---

## Create Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## OCR Dependencies

```bash
pip install easyocr rapidfuzz opencv-python
```

Optional but recommended:

```bash
pip install torch torchvision
```

---

## ASR Dependencies

```bash
pip install mlx-whisper
```

---

# Usage

---

# OCR Transcriber

Extract hardcoded subtitles from video.

## YouTube Example

```bash
python3 OCR_transcriber.py \
"https://www.youtube.com/watch?v=VIDEO_ID"
```

## Local File Example

```bash
python3 OCR_transcriber.py video.mp4
```

## Recommended Settings

```bash
python3 OCR_transcriber.py \
video.mp4 \
--fps 1 \
--crop-y 0.75 \
--crop-height 0.25 \
--scale-factor 4 \
--min-confidence 0.35 \
--min-persistence 1
```

## OCR Output Example

```text
[539.00] [conf=0.98] 今天的影片就在這邊結束啦
[540.00] [conf=0.67] 如果你對國旅的話題很感興趣的話
[541.00] [conf=0.99] 現在不限學生
```

---

# ASR Transcriber

Generate subtitles directly from speech.

## YouTube Example

```bash
python3 ASR_transcriber.py \
"https://www.youtube.com/watch?v=VIDEO_ID"
```

## Local File Example

```bash
python3 ASR_transcriber.py video.mp4
```

## Recommended Settings

```bash
python3 ASR_transcriber.py \
video.mp4 \
--model mlx-community/whisper-large-v3-mlx \
--temperature 0 \
--best-of 5 \
--condition-on-previous-text False \
--no-speech-threshold 0.6 \
--word-timestamps True \
--hallucination-silence-threshold 2
```

## ASR Output Example

```text
[00:08:42.400 --> 00:08:43.600] [conf=0.91]
他們今年要去國外畢業旅行
```

---

# OCR + ASR Merger

Fuse OCR subtitles and ASR transcripts together.

## Merge Example

```bash
python3 ASR+OCR_merger.py \
OCR/OCR.json \
ASR/ASR.json
```

## Strict OCR Preference

```bash
python3 ASR+OCR_merger.py \
OCR/OCR.json \
ASR/ASR.json \
--min-ocr-confidence 0.80
```

## Merge Output Example

```text
[00:06:26.220 --> 00:06:29.580]
[conf=0.90]
你如果說你出去逛街什麼的
```

---

# Example Workflow

## 1. OCR hardcoded subtitles

```bash
python3 OCR_transcriber.py video.mp4
```

## 2. Generate ASR transcript

```bash
python3 ASR_transcriber.py video.mp4
```

## 3. Merge both transcripts

```bash
python3 ASR+OCR_merger.py \
OCR.json \
ASR.json
```

---

# Why OCR + ASR?

ASR and OCR each have different strengths.

| System | Good At | Weak At |
|---|---|---|
| OCR | visible subtitles, names, signs | stylized fonts, motion |
| ASR | speech flow, grammar | noisy audio, proper nouns |

Combining both systems often produces better subtitles than either alone.

---

# Use Cases

## Restore Hardcoded Subtitles
Recover subtitles from videos with burned-in captions.

---

## Improve ASR Accuracy
Use OCR to correct:
- names
- places
- brands
- signs
- subtitles

---

## YouTube Archival
Generate subtitles for:
- interviews
- podcasts
- travel videos
- documentaries
- livestreams

---

## Traditional Chinese Subtitle Workflows
Useful for:
- Taiwanese YouTube
- Mandarin subtitle extraction
- mixed Chinese/English subtitles

---

## Local-First Subtitle Generation
No cloud APIs required.

Runs locally using:
- EasyOCR
- MLX Whisper
- FFmpeg

---

# Notes

- First EasyOCR run downloads OCR models
- First MLX Whisper run downloads Whisper models
- Apple Silicon strongly recommended for ASR speed
- OCR quality depends heavily on subtitle clarity
- Low-resolution video may still produce OCR artifacts

---

# Future Ideas

Potential future additions:
- subtitle cleanup
- translation
- speaker detection
- chapter extraction
- subtitle alignment
- diarization
- subtitle search indexing

---

# License

MIT License
