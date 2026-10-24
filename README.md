# watazawwadu-models

Speech models that the وتزوّدوا (Watazawwadu) app downloads on request. They let
it recognize Quran recitation on the device, without internet, for حفظني
(reciting from memory) and voice tasbeeh. Audio never leaves the phone.

The files are published as [releases](https://github.com/mano270699/watazawwadu-models/releases).
The app pins each file by its SHA-256.

## ggml-quran-tiny-q8_0.bin (current)

Release `quran-whisper-tiny-v1`. The app uses this model: about 2.5 times
faster than base on phones, so recognition keeps up with the reciter.

| | |
|---|---|
| Model | [tarteel-ai/whisper-tiny-ar-quran](https://huggingface.co/tarteel-ai/whisper-tiny-ar-quran), revision `c3d7e624af5c81bef25a10a2af3a5a84cb4dd0f0` |
| Authors | Tarteel AI. OpenAI Whisper tiny, fine-tuned on Quran recitation |
| License | Apache-2.0 (see [LICENSE](LICENSE)) |
| Format | GGML for [whisper.cpp](https://github.com/ggml-org/whisper.cpp), quantized to q8_0 |
| Size | 43,537,433 bytes |
| SHA-256 | `ef01ab441b004f9e6f1ea98d397b43452b3a45efab7c3928ab37af655dde8b52` |

### Changes from the original

The file was converted to GGML and quantized. The weights were not otherwise
changed.

1. Converted with whisper.cpp's `models/convert-h5-to-ggml.py`.
2. Quantized with `whisper-quantize ggml-model.bin ggml-quran-tiny-q8_0.bin q8_0`.
   q8_0 rather than q5_1: on phones without the dot-product instructions
   (whisper.cpp's default Android build), it is the faster of the two.

These steps reproduce the file byte for byte.

## ggml-quran-base-q5_1.bin (earlier)

Release `quran-whisper-base-v1`.

| | |
|---|---|
| Model | [tarteel-ai/whisper-base-ar-quran](https://huggingface.co/tarteel-ai/whisper-base-ar-quran), revision `5c3c53fdf9272c4f6ee0bee09a1e5a4a615ee25c` |
| Authors | Tarteel AI. OpenAI Whisper base, fine-tuned on Quran recitation |
| License | Apache-2.0 (see [LICENSE](LICENSE)) |
| Format | GGML for [whisper.cpp](https://github.com/ggml-org/whisper.cpp), quantized to q5_1 |
| Size | 59,707,625 bytes |
| SHA-256 | `acc1f03c8b1468b674b4fab50e0b2312cff982ba0001cd16fe36ca23862b6536` |

### Changes from the original

The file was converted to GGML and quantized. The weights were not otherwise
changed.

1. In `config.json`, `max_length` was set to `max_target_positions` (448)
   before converting. The fine-tune's `max_length` (1024) is a generation
   setting, but whisper.cpp's converter reads it as the decoder's context, and
   the model would not load.
2. Converted with whisper.cpp's `models/convert-h5-to-ggml.py`.
3. Quantized with `whisper-quantize ggml-model.bin ggml-quran-base-q5_1.bin q5_1`.

These steps reproduce the file byte for byte.

### Check

```sh
whisper-cli -m ggml-quran-base-q5_1.bin -l ar -f al-ikhlas.wav
# قُلْ هُوَ اللَّهُ أَحَدٌ اللَّهُ الصَّمَدُ لَمْ يَلِدْ وَلَمْ يُولَدْ وَلَمْ يَكُنْ لَهُ كُفُوًا أَحَدٌ
```
