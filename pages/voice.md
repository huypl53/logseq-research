- attributes
	- sample rates
	- codecs
	  collapsed:: true
		- |     file Format    |        Typical Codecs        |        Compression Type       |                                       Description                                       |                                  ASR Suitability                                  |
		  |:------------------:|:----------------------------:|:-----------------------------:|:---------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------:|
		  | WAV                | PCM, ADPCM                   | Lossless (PCM), Lossy (ADPCM) | Uncompressed or lightly compressed; stores raw audio. Large file sizes.                 | Ideal for ASR (e.g., VietASR, Wav2Vec2 require 16kHz WAV). No quality loss.       |
		  | MP3                | MP3 (MPEG-1 Audio Layer III) | Lossy                         | Widely used for music and speech. Compresses by removing inaudible frequencies.         | Common but lossy; may affect tone accuracy in Vietnamese. Convert to WAV for ASR. |
		  | M4A (MPEG-4 Audio) | AAC, ALAC                    | Lossy (AAC), Lossless (ALAC)  | Part of MPEG-4; used in iTunes, Apple devices. Better quality than MP3 at same bitrate. | Good quality (AAC); ALAC is lossless. Convert to WAV for ASR compatibility.       |
		  | FLAC               | FLAC                         | Lossless                      | Compresses audio without quality loss. Smaller than WAV.                                | Excellent for archival; convert to WAV for ASR due to model input requirements.   |