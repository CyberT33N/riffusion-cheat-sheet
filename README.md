# riffusion-cheat-sheet

## Install
- https://github.com/riffusion/riffusion-app-hobby
- https://github.com/riffusion/riffusion-app-hobby

Clone `riffusion-hobby` & `riffusion-app-hobby`
```shell
cd ~/Projects/ai/audio

git clone https://github.com/riffusion/riffusion-hobby.git
git clone https://github.com/riffusion/riffusion-app-hobby.git
```

<br>

Because the head commit is deprecated of riffusion-hobby and the latest torchaudio of requirements.txt is not supported you must modify self.inverse_mel_scaler:
- ~/Projects/ai/audio/riffusion-hobby/riffusion/spectrogram_converter.py
```python
# https://pytorch.org/audio/stable/generated/torchaudio.transforms.InverseMelScale.html
self.inverse_mel_scaler = torchaudio.transforms.InverseMelScale(
    n_stft=params.n_fft // 2 + 1,
    n_mels=params.num_frequencies,
    sample_rate=params.sample_rate,
    f_min=params.min_frequency,
    f_max=params.max_frequency,
    norm=params.mel_scale_norm,
    mel_scale=params.mel_scale_type,
).to(self.device)
```

Install and start riffusion-hobby:
```shell
cd ~/Projects/ai/audio/riffusion-hobby

# pyenv install 3.9
pyenv local 3.9

python3 -m venv venv
source venv/bin/activate

sudo apt install sox

# Those dependencies are missing in the head commit of requirements.txt
pip install numpy typing_extensions

python -m pip install -r requirements.txt

# cache_download not longer supported in latest version so we downgrade
pip install huggingface_hub==0.24.7
```

<br>

Install and start riffusion-app-hobby. Create `.env.local` and insert `RIFFUSION_FLASK_URL=http://127.0.0.1:3013/run_inference/`. Then run:
```shell
cd ~/Projects/ai/audio/riffusion-app-hobby

npm i
npm run dev
```
