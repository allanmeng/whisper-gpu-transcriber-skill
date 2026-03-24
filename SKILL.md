---
name: whisper-gpu-transcribe
description: Convert audio to SRT subtitles using OpenAI Whisper with automatic GPU acceleration for Intel XPU / NVIDIA CUDA / AMD ROCm / Apple Metal. Ideal for content creators as a free alternative to paid subtitle generation.
version: 1.0.0
metadata:
  openclaw:
    emoji: "🎙️"
    homepage: https://github.com/allanmeng/whisper-gpu-transcriber-skill
    requires:
      bins:
        - python
    install:
      - kind: pip
        package: openai-whisper
---

# 🎙️ Whisper GPU Audio Transcriber / 音频转字幕

Convert audio files to SRT subtitles using local Whisper models — **completely free**, offline, and GPU accelerated.

使用本地 Whisper 模型将音频文件转录为 SRT 字幕，**完全免费**，无需联网，支持 GPU 加速。

---

## Use Cases / 适用场景

- Content creation, free alternative to paid subtitle features (e.g., 剪映) — 自媒体视频制作，替代剪映付费字幕功能
- Meeting recording to text — 会议录音转文字
- Podcast/course subtitles — 播客/课程内容转字幕

---

## Supported GPU Acceleration / 支持的 GPU 加速

| Device | Acceleration | FP16 |
|--------|-------------|------|
| Intel Arc Series | XPU | ❌ Auto disabled |
| NVIDIA GPUs | CUDA | ✅ Auto enabled |
| AMD GPUs | ROCm | ✅ Auto enabled |
| Apple M Series | Metal | ✅ Auto enabled |
| No GPU | CPU | ❌ Auto disabled |

---

## Usage / 使用方法

### Basic Usage / 基础用法

Place the audio file in your current working directory and tell the AI:

将音频文件放入当前工作目录，然后告诉 AI：

```
Convert xxx.mp3 to SRT subtitles
把 xxx.mp3 转成 SRT 字幕文件
```

Or specify the full path directly:

或者直接指定路径：

```
Convert /path/to/audio.mp3 to SRT subtitles
把 /path/to/audio.mp3 转成 SRT 字幕
```

### Advanced Usage / 高级用法

```
Convert xxx.mp3 to English subtitles using large-v3-turbo model
把 xxx.mp3 用 large-v3-turbo 模型转成英文字幕

Convert xxx.mp3 to subtitles, language is Japanese
把 xxx.mp3 转成字幕，语言是日语
```

---

## Execution / 执行方式

AI will execute the `scripts/transcribe.py` script in your project directory, which will:

AI 会调用项目目录中的 `scripts/transcribe.py` 脚本执行转录，脚本会：

1. Automatically detect available GPU and select optimal acceleration — 自动检测可用 GPU 设备并选择最优加速方式
2. Load Whisper model (default: `turbo`) — 加载 Whisper 模型（默认 `turbo`）
3. Transcribe audio to SRT format — 将音频转录为 SRT 格式字幕
4. Save output in the same directory as the audio — 输出文件保存在与音频同目录

---

## Requirements / 环境要求

- Python 3.8+
- PyTorch (version matching your hardware)
  - Intel GPU: `pip install torch==2.10.0+xpu`
  - NVIDIA GPU: `pip install torch --index-url https://download.pytorch.org/whl/cu121`
  - CPU: `pip install torch`
- openai-whisper: Automatically installed by ClawHub via `pip install openai-whisper`

---

## Notes / 注意事项

- First run will auto-download the model file (turbo ~1.5GB) — 首次运行会自动下载模型文件（turbo 约 1.5GB）
- Models cache in `~/.cache/whisper` by default, use symlink/Junction to redirect — 模型默认缓存在 `~/.cache/whisper`，可用软链接/Junction 指向其他磁盘
- Intel XPU requires Intel Arc GPU + matching PyTorch version — Intel XPU 需要 Intel Arc 独显 + 对应版本 PyTorch

> **Tip for China users / 国内用户提示**: First run auto-downloads models. If download fails, manually download from mirror sites and place in `~/.cache/whisper/` — 首次运行会自动下载模型，如下载失败可手动从镜像站下载后放入 `~/.cache/whisper/`

---

## Supported Models / 支持的模型

| Model | Size | Speed | Accuracy |
|-------|------|-------|----------|
| `tiny` | 39M | Fastest | Low |
| `base` | 74M | Fast | Medium |
| `small` | 244M | Medium | Medium |
| `medium` | 769M | Slow | High |
| `turbo` | 809M | Medium | High ✅ Recommended / 推荐 |
| `large-v3` | 1550M | Slowest | Highest |
| `large-v3-turbo` | 1550M | Slow | Highest |
