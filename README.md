# 🎬 YouTube Downloader: Save Videos as MP4 or MP3 in One Click

> A simple Streamlit web app built on yt-dlp and FFmpeg. Paste a YouTube link, see the video's title and length, choose MP4 (best video and audio, merged) or MP3 (192 kbps audio), and save the file straight from your browser.

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-Web%20App-FF4B4B?logo=streamlit&logoColor=white)
![yt-dlp](https://img.shields.io/badge/yt--dlp-downloader-red)
![FFmpeg](https://img.shields.io/badge/FFmpeg-audio%2Fvideo-007808?logo=ffmpeg&logoColor=white)
![Codespaces](https://img.shields.io/badge/Dev%20Container-ready-2088FF?logo=github&logoColor=white)

---

## Table of Contents

- [Features](#features)
- [How It Works](#how-it-works)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Deploying](#deploying)
- [Project Structure](#project-structure)
- [Troubleshooting](#troubleshooting)
- [Roadmap](#roadmap)
- [Responsible Use](#responsible-use)

---

## Features

- **🎥 MP4 video.** Downloads the best available video and audio streams and merges them into one MP4.
- **🎵 MP3 audio.** Extracts the audio track and converts it to a 192 kbps MP3.
- **🔍 Preview first.** Shows the video's title and duration before downloading anything.
- **💾 Save from the browser.** The finished file comes back through a normal download button, so it works the same locally or on a hosted server.
- **🧹 No leftover files.** Each download runs in a temporary folder that's deleted automatically.
- **🖥️ Works on any OS.** Finds FFmpeg on your `PATH` or in common install locations on macOS, Linux and Windows.
- **☁️ Codespaces-ready.** The dev container installs FFmpeg and the Python packages, then starts the app.

---

## How It Works

```
 YouTube URL + format (MP4 / MP3)
              │
              ▼
   yt-dlp: extract_info(download=False)  ──► show title and duration
              │
         [Download]
              │
              ▼
   yt-dlp download into a temporary folder
     ├─ MP4: bestvideo[ext=mp4] + bestaudio[ext=m4a] ─► merged by FFmpeg
     └─ MP3: bestaudio ─► FFmpegExtractAudio (mp3, 192 kbps)
              │
              ▼
   File read into memory ─► st.download_button ─► saved by the browser
              │
              ▼
   Temporary folder deleted
```

---

## Tech Stack

| Area | Tools |
|---|---|
| UI | Streamlit |
| Downloading | yt-dlp |
| Audio and video processing | FFmpeg, through yt-dlp's post-processors |
| Developer setup | Dev Container / GitHub Codespaces |

---

## Getting Started

### Prerequisites

- **Python 3.11 or later**
- **FFmpeg**, needed to merge MP4 streams and convert to MP3:

  | OS | Install command |
  |---|---|
  | macOS | `brew install ffmpeg` |
  | Ubuntu/Debian | `sudo apt install ffmpeg` |
  | Windows | `winget install ffmpeg`, or extract a build to `C:\ffmpeg\` |

### Install and run

```bash
git clone <your-repo-url> youtube-downloader
cd youtube-downloader

python3 -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt

streamlit run app.py
```

Then open **http://localhost:8501**.

> ☁️ **Prefer zero setup?** Open the repo in **GitHub Codespaces**. The dev container installs FFmpeg (from `packages.txt`) and the Python packages, then starts the app on port 8501.

---

## Usage

1. Paste a YouTube link. Both `youtube.com/watch?v=…` and `youtu.be/…` links work.
2. Choose **MP4** or **MP3**.
3. Check the title and duration, then click **Download**.
4. When it finishes, click **Click to save file**.

### Command-line version

`yt.py` is a minimal script version that downloads in the best single-file format. Change the `url` at the bottom of the file, then run:

```bash
python yt.py
```

---

## Deploying

The repo is set up for **[Streamlit Community Cloud](https://streamlit.io/cloud)**:

- `requirements.txt` lists the Python packages
- `packages.txt` lists the system packages to install with apt (`ffmpeg`)

Connect the repo, set the main file to `app.py`, and deploy.

> ⚠️ YouTube often blocks requests from cloud and data-center IP addresses, so hosted copies may fail on some videos even when a local copy works.

---

## Project Structure

```
youtube-downloader/
├── app.py                    # Streamlit app: URL input, format choice, download
├── yt.py                     # Minimal command-line download script
├── requirements.txt          # Python packages
├── packages.txt              # System packages (ffmpeg) for Codespaces and Streamlit Cloud
└── .devcontainer/
    └── devcontainer.json     # Codespaces setup that starts the app automatically
```

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `ffmpeg not found`, or the MP3 or merged MP4 is missing | Install FFmpeg and make sure `ffmpeg -version` works in your terminal |
| `HTTP Error 403` or "Sign in to confirm you're not a bot" | Update yt-dlp (`pip install -U yt-dlp`), since YouTube changes often, and try again from a home network |
| Long videos are slow or use too much memory | The whole file is loaded into memory before saving, so very long videos are better downloaded with `yt.py` |

---

## Roadmap

- [ ] Live download progress bar
- [ ] Quality picker (1080p, 720p, audio bitrate)
- [ ] Playlist and batch downloads
- [ ] Thumbnails and metadata (title, artist) embedded in MP3s
- [ ] Command-line arguments for `yt.py` (URL, format, output folder)

---

## Responsible Use

This project is for **educational and personal use only**. Only download content you own, content that's in the public domain or under a permissive license, or content you have permission to download. Downloading videos may break [YouTube's Terms of Service](https://www.youtube.com/t/terms) and copyright law in your country, and you're responsible for how you use this tool.