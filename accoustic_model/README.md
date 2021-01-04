# Accoustic Model

Tags: czech language, stt, speech recognition, voice transcription, asr, offline, free


# Installation (for model training)

## Build Docker Image
```
git clone https://github.com/slesinger/deepspeech.pytorch.git
cd deepspeech.pytorch
docker build -t deepspeech:latest .
```

## Start container for training
Please note that [training data has to be prepared in advance](#data-preparation).

Change directory to folder that contains folder ```data```
```
sudo docker run -ti --gpus all -v `pwd`/czech-speech-to-text/accoustic_model/data:/workspace/data -p 8888:8888 --net=host --ipc=host --entrypoint=/bin/bash deepspeech:latest
```

You can start Jupyter from within the container
```
jupyter-notebook --ip="*" --no-browser --allow-root
```

## Training
From within the container run following PyTorch command. Please make sure to update paths to respective data folders.
```
cd /workspace/deepspeech.pytorch

python train.py data.train_manifest='/workspace/data/manifests/validated.csv' data.val_manifest='/workspace/data/manifests/dev.csv' checkpointing.save_folder='/workspace/data/models' training.epochs=25 data.labels_path='/workspace/data/labels.json'
```

If you continue from previously interrupted training, append this to command above:
```
 checkpointing.continue_from=/workspace/data/models/deepspeech_checkpoint_epoch_XX.pth
```

## Infer
You can test current model while training from within the container.

- Using just accusting model (how the model hears it):
```
python transcribe.py model.model_path='/workspace/data/models/deepspeech_final.pth' audio_path='/workspace/data/wav/common_voice_cs_21227902.wav' model.cuda=false model.use_half=false
```

- Using accusting and language model (how the model hears and understands):
```
python transcribe.py model.model_path='/workspace/data/models/deepspeech_final.pth' audio_path='/workspace/data/wav/common_voice_cs_21227902.wav' model.cuda=false model.use_half=false lm.decoder_type=beam lm.lm_path=/workspace/data/lm.bin offsets=true
```

# Data Preparation

## Mozilla common voice
Download latest czech data from https://commonvoice.mozilla.org/cs/datasets. 

Updated datasets are published in June and December.

Untar the zip so that it fits into prepared directory structure ```czech-speech-to-text/accoustic_model/commonvoice/cs```

Use commonvoice.ipynb to preprocess data.

## Czech Parliament meetings (CPM)

Based on thesis [Developing an automatic speech recognition system based on Czech spoken language](https://dspace.cvut.cz/bitstream/handle/10467/88012/F8-DP-2020-Werner-Richard-thesis.pdf) from Richard Werner.

Main Github repo referenced: https://github.com/opendatalabcz/speech2text
