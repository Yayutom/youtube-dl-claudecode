# youtube-dl-claudecode

> 🇯🇵 日本語 / 🇬🇧 English — bilingual README

A tiny [Claude Code](https://claude.com/claude-code) slash command that downloads a YouTube video as **MP4** by just passing its URL.

YouTube の URL を渡すだけで動画を **MP4** で保存する、Claude Code の slash command です。

```
/yt https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

The video is saved into an `MP4/` folder created in your current directory.
動画は、実行時のカレントディレクトリに作られる `MP4/` フォルダへ保存されます。

---

## Requirements / 前提条件

- [`yt-dlp`](https://github.com/yt-dlp/yt-dlp)
- [`ffmpeg`](https://ffmpeg.org/) — **required** to merge the separate video + audio streams into one MP4 / 映像と音声の別ストリームを1つの MP4 に結合するために**必須**

```bash
# macOS (Homebrew)
brew install yt-dlp ffmpeg

# Linux (Debian/Ubuntu)
sudo apt install -y ffmpeg && pip install -U yt-dlp

# pip (yt-dlp only — install ffmpeg separately)
pip install -U yt-dlp
```

---

## Install / インストール

Copy the command file into your Claude Code commands directory.
コマンドファイルを Claude Code のコマンドディレクトリへコピーするだけです。

```bash
git clone https://github.com/Yayutom/youtube-dl-claudecode.git
cp youtube-dl-claudecode/commands/yt.md ~/.claude/commands/yt.md
```

Restart Claude Code (or start a new session) and `/yt` becomes available.
Claude Code を再起動（または新しいセッションを開始）すると `/yt` が使えます。

---

## Usage / 使い方

```
/yt <YouTube URL>
```

Example / 例:

```
/yt https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

---

## How it works / 仕組み

Under the hood it runs:
内部では以下を実行します:

```bash
mkdir -p "$(pwd)/MP4" && \
yt-dlp --no-playlist -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]" \
  -o "$(pwd)/MP4/%(title)s-%(id)s.%(ext)s" \
  "<URL>"
```

- `--no-playlist` — download only the single video, even for a playlist URL / 再生リスト URL でも単一動画のみ取得
- `bv*[ext=mp4]+ba[ext=m4a]` — best MP4 video + best M4A audio, merged by ffmpeg / 最良の MP4 映像 + M4A 音声を ffmpeg で結合
- `/b[ext=mp4]` — fallback to a single pre-muxed MP4 if separate streams are unavailable / 別ストリームが無い場合は結合済み MP4 にフォールバック
- Output filename: `<title>-<id>.mp4`

---

## Notes / 注意

- Only download videos you own or are licensed to download. / 自分が権利を持つ、または許諾された動画のみダウンロードしてください。
- This is a thin wrapper around `yt-dlp`; all the heavy lifting is done by yt-dlp and ffmpeg. / 本体は `yt-dlp` の薄いラッパーです。実処理は yt-dlp と ffmpeg が担います。

---

## License / ライセンス

[MIT](./LICENSE)
