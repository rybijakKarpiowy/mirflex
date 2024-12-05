# Environment Setup


The repository has been tested with the following environment set up. To recreate, please follow the below installations in order. The steps in the README expect the conda environment name to be mirflex.


### Base

* Ubuntu 22.04
* Python 3.8

```
conda create -n mirflex python=3.8.18
```

### Additional installations (in order of installation)

```
pip install essentia
pip install -f https://essentia.upf.edu/python-wheels/ essentia-tensorflow
pip install librosa
pip install madmom
sudo apt-get install portaudio19-dev
pip install pyaudio
pip install git+https://github.com/mjhydri/BeatNet
pip install openai

pip install torch
pip install pandas
pip install pyrubberband
pip install pyyaml
pip install mir_eval
pip install pretty_midi
pip uninstall pysoundfile -y
pip uninstall soundfile -y
pip install soundfile

pip install tensorflow==2.7.0

conda install -c conda-forge ffmpeg libsndfile
pip install spleeter
pip install httpx==0.26.0

pip install pydub
pip install protobuf==3.19.0

pip install jams
pip uninstall librosa -y
pip install librosa==0.6.2
pip uninstall resampy numba -y
pip install numba==0.48 resampy

conda install -c conda-forge cudatoolkit=11.2.2
conda install -c conda-forge cudnn=8.2.1
pip install numpy==1.23
pip install pygame
```

### ENVIRONMENT VARIABLES

```
export TF_FORCE_GPU_ALLOW_GROWTH=true
```

# Add libcudart to path

```
sudo find /home -name 'libcudart.so.11.0'
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:[path to envs/mirflex/lib from above command]
export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH
```