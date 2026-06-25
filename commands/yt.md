---
description: YouTube の URL を MP4 でダウンロード / Download a YouTube video as MP4
argument-hint: <YouTube URL>
allowed-tools: Bash(mkdir:*), Bash(yt-dlp:*)
---

# /yt — YouTube → MP4 downloader

**用途 / Purpose**: YouTube の URL を渡すだけで MP4 形式で動画をダウンロードする Claude Code slash command. / A Claude Code slash command that downloads a YouTube video as MP4 by just passing its URL.

## 形式 / Usage

```
/yt <URL>
```

引数 / Argument: $ARGUMENTS

## 入力例 / Example

```
/yt https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

## 動作 / Behavior

1. カレントディレクトリ直下に `MP4/` フォルダを作成（なければ） / Create an `MP4/` folder in the current directory (if missing)
2. yt-dlp で指定 URL の動画を MP4 形式でダウンロード / Download the video as MP4 with yt-dlp
3. 保存先 / Output: `<current directory>/MP4/`
4. ファイル名 / Filename: `%(title)s-%(id)s.%(ext)s`
5. 完了後、保存パスを表示 / Print the saved path on completion

## 実行手順 / Implementation

以下の Bash コマンドをそのまま実行する。 / Run the following Bash command as-is.

```bash
mkdir -p "$(pwd)/MP4" && \
yt-dlp --no-playlist -f "bv*[ext=mp4]+ba[ext=m4a]/b[ext=mp4]" \
  -o "$(pwd)/MP4/%(title)s-%(id)s.%(ext)s" \
  "$ARGUMENTS"
```

## 注意 / Notes

- URL は `https://www.youtube.com/watch?v=...` の形式で指定 / Pass a standard watch URL
- `--no-playlist` 指定により、再生リスト URL でも単一動画のみ取得 / `--no-playlist` keeps it to a single video even for a playlist URL
- `MP4/` フォルダは実行時のカレントディレクトリに作成される / The `MP4/` folder is created in the current working directory
- 別ストリームの映像+音声を結合するため `ffmpeg` が必要 / `ffmpeg` is required to merge the separate video + audio streams
- ダウンロードは完了まで実行が続く（回線速度による） / The download runs until complete (depends on your connection)
- 自分が権利を持つ、または許諾された動画のみダウンロードすること / Only download videos you own or are licensed to download
