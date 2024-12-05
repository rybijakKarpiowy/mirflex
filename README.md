# MIRFLEX

[![Status](https://img.shields.io/badge/status-stable-green.svg)](https://github.com/AMAAI-Lab) [![Version](https://img.shields.io/badge/version-v1.0.0-blue.svg)](https://github.com/AMAAI-Lab)

## Description

MIRFLEX is a tool for analyzing various musical features from audio clips. It extracts details like key, chord progression, tempo, and genre, and can tell the difference between vocal and instrumental sections. The outputs can be saved as latents or labels in a JSON file. It also creates simple captions describing the music using the OpenAI API.

The system is modular, so you can easily turn different features on or off and customize the features to be extracted by even integrating your own feature extractor. Whether you're using it for research, creative projects, or just to better understand music, MIRFLEX offers a practical way to analyze and describe audio.

## Contribute to MIRFLEX! 🚀 

We welcome contributions from the community to help MIRFLEX evolve into a comprehensive, powerful tool for Music Information Retrieval (MIR). Whether you’re interested in adding new feature extractors, improving existing ones, or optimizing performance, your input is invaluable in making MIRFLEX even more versatile and useful.

### Ways to Contribute
- **Add Feature Extractors**: Enhance MIRFLEX with new models for tasks such as genre detection, mood analysis, or instrument recognition.
- **Optimize Existing Modules**: Suggestions to optimise architecture, and solve the ever growing issue of reconciling requirement conflicts when integrating many feature extractors.
- **Documentation & Examples**: Help others learn and use MIRFLEX effectively by contributing to the documentation or creating example notebooks.

### Get Started
1. Fork the repository and start exploring the codebase.
2. Check out our Contributing Guidelines (Will be up soon! Until then feel free to experiment and use the existing feature extractors as an example)
3. Submit a pull request with your proposed changes.

Join us in making MIRFLEX a valuable resource for the MIR community

## Features and Models

MIRFLEX uses advanced models for different music analysis tasks. Here's a quick rundown of the main features:

1. **Key Detection**: 
   - **Model**: CNNs with Directional Filters ([Schreiber and Müller, 2019](https://github.com/hendriks73/key-cnn))
   - **Description**: Identifies the key of the music using a CNN-based model.
   
2. **Chord Detection**:
   - **Model**: Bidirectional Transformer ([Park et al., 2019](https://github.com/jayg996/BTC-ISMIR19))
   - **Description**: Extracts chord sequences from the audio using deep auditory models and Conditional Random Fields.

3. **Tempo Estimation & Downbeat Transcription**:
   - **Model**: BeatNet: CRNN and Particle Filtering ([Heydari, Cwitkowitz, and Duan, 2021](https://github.com/mjhydri/BeatNet))
   - **Description**: Estimates the tempo and identifies downbeats in the music.

4. **Vocals / Instrumental Detection**:
   - **Model**: EfficientNet trained on Discogs ([Essentia Library](https://essentia.upf.edu))
   - **Description**: Classifies each clip as a track with vocals or an instrumental track

5. **Instrument, Mood, and Genre Detection**:
   - **Model**: Essentia's Jamendo Baseline Models ([Essentia Library](https://essentia.upf.edu))
   - **Description**: Classifies instruments, mood/themes, and genres using CNN-based models.

---

## Request to Cite 🙏

If MIRFLEX has supported your work or contributed to your research, we kindly ask you to cite our paper to help others discover this resource.

```bibtex
@inproceedings{chopra2024mirflex,
  title={MIRFLEX: Music Information Retrieval Feature Library for Extraction},
  author={Chopra, Anuradha and Roy, Abhinaba and Herremans, Dorien},
  booktitle={Proceedings of the Late-Breaking Demo Session of the 25th International Society for Music Information Retrieval Conference (ISMIR)},
  year={2024},
  address={San Francisco, United States}
}
```

## Usage

To use the music captioning system, follow these steps:

### 1. Clone repository

Clone repository
```
git clone https://github.com/AMAAI-Lab.git
```

Change directory into repository

```
cd mirflex
```

Download zip file with pre-trained weights required for source separation during pre-processing
### NOTE: Running this command downloads html file instead of .zip, open this link in a browser and download the file manually
```
wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1yFs9ncHiV5-bI3_xrsgk7eL2QN9Hlldd' -O spleeter_pretrained_weights.zip
```

Unzip pre-trained weights 
```
unzip spleeter_pretrained_weights.zip
```

### 2. Set up environment

Follow the steps mentioned in [SETUP.md](SETUP.md) to prepare a conda environment to be used for running the MIRFLEX tool

### 3. Configuration

Create / modify the configuration file (config/feature_extractor_config.yaml) with the necessary parameters. Example configuration file and details on configuration options are provided in the configs section of this README.

### 4. Run the Feature Extraction System

#### Environment

Activate conda environment in a new terminal
```
conda activate mirflex
```

#### Preprocess

Generate raw input json file for all mp3 files in given directory. Set directories with mp3 files in utils/create_input_json.py at line 23 to 27.

```
if __name__ == "__main__":
    # Example usage:
    input_directories = ["./samples/fma_top_downloads/", "./samples/generic_pop_audio/", "./samples/mtg_jamendo/"]
    output_json_file = "./files/audio_files.json"

```

Run create json with command

```
python utils/create_input_json.py
```

Run pre-processing to split audio into 30 second segments and split sources into 4 stems

```
python preprocess.py config/feature_extractor_config.yaml
```


#### Extract features

Run the main script (main.py) with the path to your configuration file as a command-line argument:

```
python -W ignore main.py config/feature_extractor_config.yaml
```

### 5. Extracted Features

The generated captions will be saved in the specified output file as JSON format.

## Configuration (config/feature_extractor_config.yaml)

The configuration file includes settings for input/output paths, feature extractors, and the OpenAI GPT-3.5 Turbo model. Here is a breakdown of the configuration parameters:

    files:
	    input:  "./path/to/audio_files.json"
	    output:  "./path/to/features.json"
	caption_generator:
		api_key:  "YOUR_OPENAI_API_KEY"
		model_id:  "gpt-3.5-turbo"
	extractors:
		mood_extractor:
			active:  True
			model:  "./path/to/mood_model.pb"
			model_metadata:  "./path/to/mood_model_metadata.json"
			embedding_model:  "./path/to/embedding_model.pb"
	# Add configurations for other extractors (genre, instrument, voice, gender, auto)...


### 6. Visualiser

The generated captions can be easily visualised using the following command

```
python display_tags.py files/features.json ./
```

the requirements to run this are the following packages
- argparse
- tkinter
- pygame
- json

This will open a simple gui that can display the extracted feature values as stored in the features.json, while playing the music.

There are buttons available to flipping to next or previous audio. Additionally there is a button to restart the audio if required.