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
sudo docker run -ti --gpus all -v `pwd`/czech-speech-to-text/accoustic_model/data:/workspace/data -p 8888:8888 --net=host --ipc=host --entrypoint=/bin/bash slesinger/deepspeech:latest
```

You can start Jupyter from within the container
```
jupyter-notebook --ip="*" --no-browser --allow-root
```

## Training
From within the container run following PyTorch command. Please make sure to update paths to respective data folders.
```
cd /workspace/deepspeech.pytorch

python train.py data.train_manifest='/workspace/data/manifests/train.csv' data.val_manifest='/workspace/data/manifests/dev.csv' checkpointing.save_folder='/workspace/data/models' training.epochs=25 data.labels_path='/workspace/data/labels.json'  optim=adam
```

If you continue from previously interrupted training, append this to command above:
```
 checkpointing.continue_from=/workspace/data/models/deepspeech_checkpoint_epoch_XX.pth
```

### Multip GPU
```
sudo apt-get install etcd
python -m torchelastic.distributed.launch --standalone --nnodes=1 --nproc_per_node=4 train.py data.train_manifest='/works
pace/data/manifests/train.csv' data.val_manifest='/workspace/data/manifests/dev.csv' checkpointing.save_folder='/workspace/data/models' training.epochs=25 data.
labels_path='/workspace/data/labels.json' apex.opt_level=O1 data.num_workers=8 data.batch_size=8 checkpointing.checkpoint=true checkpointing.save_n_recent_model
s=8 optim=adam
```

## Last
```
python -m torchelastic.distributed.launch --standalone --nnodes=1 --nproc_per_node=4 train.py data.train_manifest='/workspace/data/manifests/train.csv' data.val_manifest='/workspace/data/manifests/dev.csv' checkpointing.save_folder='/workspace/data/models' data.labels_path='/workspace/data/labels.json' apex.opt_level=O1 data.num_workers=4 data.batch_size=4 checkpointing.checkpoint=true checkpointing.save_n_recent_models=8 optim.learning_anneal=1.01 visualization.visdom=true apex.loss_scale=1 optim=adam
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

More than 678 hours of audio data

Merge multiple file to one
```
paste -d '\n' a/train.txt b/train.txt > train.txt
```



## Mozilla common voice ~45h
Download latest czech data from https://commonvoice.mozilla.org/cs/datasets. 

Updated datasets are published in June and December.

Untar the zip so that it fits into prepared directory structure ```czech-speech-to-text/accoustic_model/commonvoice/cs```

Use commonvoice.ipynb to preprocess data.

## Czech Parliament meetings (CPM) ~88h
Based on thesis [Developing an automatic speech recognition system based on Czech spoken language](https://dspace.cvut.cz/bitstream/handle/10467/88012/F8-DP-2020-Werner-Richard-thesis.pdf) from Richard Werner.

Main Github repo referenced: https://github.com/opendatalabcz/speech2text

## VYSTADIAL ~43h
https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-1740

## Makon
https://lindat.mff.cuni.cz/repository/xmlui/handle/11372/LRT-1455
(nothing used as transcript is not so precise)

## Companion ~5h
https://lindat.mff.cuni.cz/repository/xmlui/handle/11858/00-097C-0000-0023-1D76-9

## Parczech PS7
https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-3436
(not used)

## Moravec ~35h
https://lindat.mff.cuni.cz/repository/xmlui/handle/11858/00-097C-0000-000D-EC98-3

## Snemovna ~462h
https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-3126

No wav conversion needed.

## Podcasts
https://cs.wikibooks.org/wiki/Jak_stahovat_podcasty
