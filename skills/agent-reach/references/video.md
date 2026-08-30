# Video / Audio

YouTube subtitles and transcription.

## YouTube (yt-dlp)

### Get video metadata

```bash
yt-dlp --dump-json "URL"
```

### Download subtitles

```bash
# Download subtitles (no video download)
yt-dlp --write-sub --write-auto-sub --sub-lang "zh-Hans,zh,en" --skip-download -o "/tmp/%(id)s" "URL"

# Then read the .vtt files
cat /tmp/VIDEO_ID.*.vtt
```

### Get comments

```bash
# Extract comments (best-effort, not guaranteed complete)
yt-dlp --write-comments --skip-download --write-info-json \
  --extractor-args "youtube:max_comments=20" \
  -o "/tmp/%(id)s" "URL"
# Comments are in the .info.json `comments` field
```

### Search videos

```bash
yt-dlp --dump-json "ytsearch5:query"
```

> **Subtitles note**: manually uploaded subtitles extract reliably;
> auto-generated ones may contain duplicated lines between segments and need
> post-processing.
> **Comments note**: `--write-comments` is web-scraping based (not the YouTube
> Data API); some comments may be missing.

### No-subtitle fallback: Whisper audio transcription

```bash
# Fallback when a video has no subtitles: download audio and transcribe with
# Whisper (a free Groq key is enough)
agent-reach transcribe "https://www.youtube.com/watch?v=VIDEO_ID"
agent-reach transcribe ./local_audio.mp3 -o /tmp/transcript.txt
```

> `agent-reach transcribe` only accepts public http(s) URLs or local audio
> files. When searching with `ytsearch5:`, pick a concrete video URL from the
> yt-dlp results first, then transcribe.
> Requires a configured key: `agent-reach configure groq-key gsk_xxx` (free,
> console.groq.com) or `agent-reach configure openai-key sk-xxx`. Default auto
> mode: falls back to openai if groq fails.

## Selection guide

| Scenario | Recommended tool |
|-----|---------|
| YouTube subtitles | yt-dlp |
| No-subtitle audio/video | `agent-reach transcribe` |
