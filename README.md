# Offline TTS E-book Reader

This application provides Text-to-Speech (TTS) functionality for PDF e-books, optimized for offline use with local language models.

## Features

- PDF text extraction and processing
- Text-to-Speech conversion using local models
- Audio caching for faster playback
- Chapter export as MP3
- RESTful API for controlling playback
- Fully offline operation

## Setup

### Prerequisites

- Miniconda or Anaconda
- FFmpeg (included in conda environment)
- 2GB+ free disk space for models and dependencies

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/offline-tts-reader.git
   cd offline-tts-reader
   ```

2. Run the setup script:
   ```bash
   chmod +x setup.sh
   ./setup.sh
   ```

   For CUDA support (if you have a compatible GPU):
   ```bash
   ./setup.sh --cuda
   ```

This script will:
- Create a conda environment called "ebook"
- Install all required dependencies
- Download and save the TTS model locally
- Run a test to verify everything works

## Usage

### Starting the Application

1. Activate the conda environment:
   ```bash
   conda activate ebook
   ```

2. Start the application:
   ```bash
   python app.py --model models/tacotron2-DDC
   ```

   With CUDA support:
   ```bash
   python app.py --model models/tacotron2-DDC --cuda
   ```

### API Endpoints

The application exposes the following REST API endpoints:

- `POST /load`: Load a PDF file
  ```json
  { "pdf_path": "/path/to/your/ebook.pdf" }
  ```

- `GET /speak`: Get the current text chunk as audio

- `POST /control`: Control playback
  ```json
  { "action": "next" }  # Options: next, prev, reset, goto
  ```
  
  For goto:
  ```json
  { "action": "goto", "index": 5 }
  ```

- `GET /download_chapter`: Download the full chapter as MP3

- `GET /status`: Get current application status

## Troubleshooting

### Model Issues

If you encounter issues with the local model:

1. Try removing the `models` directory and running the setup script again:
   ```bash
   rm -rf models
   ./setup.sh
   ```

2. Verify the model path is correct when starting the application:
   ```bash
   python app.py --model models/tacotron2-DDC
   ```

### Audio Issues

If no audio is generated:

1. Check that FFmpeg is installed correctly:
   ```bash
   conda activate ebook
   ffmpeg -version
   ```

2. Ensure there is sufficient disk space for audio caching

## Offline Mode Considerations

- The application caches TTS output to improve performance
- All models are stored locally in the `models` directory
- No internet connection is required after initial setup

## Advanced Configuration

You can customize the application behavior by editing the `app.py` file:

- Adjust chunk size for text processing
- Change audio formats or quality
- Modify caching behavior
