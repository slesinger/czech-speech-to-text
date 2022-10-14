# Speech to Text for Czech language

Tags: czech language, stt, speech recognition, voice transcription, asr, offline, free

## Alternative project

Check [another model that is trained on similar dataset](https://coqui.ai/czech/comodoro/v0.3.0). It is part of Conqui.ai

## Background

Czech language lacks a modern speech recognition engine that can be used offline, e.g. home automation, robotics.

Mozilla Common Voice is an initiative that provides some hope but there are very little czech speaking supporters giving their voice. Given current speed of gathering data, it will still take significant time to capture enough recordings to train a official model.

Please contribute your voice: :point_right: https://commonvoice.mozilla.org/cs/speak :point_left:

Thank you :thumbsup:

Goal of this project is to train a model now, from existing data, though not perfect, and make it available to everyone.

This project is based on [Baidu Deep Speeech 2 paper](http://proceedings.mlr.press/v48/amodei16.pdf) and https://github.com/SeanNaren/deepspeech.pytorch. 

[Mozilla Deep Speech](https://github.com/mozilla/DeepSpeech) implementation in Tesorflow was failing training on Segmentation Fault after several epochs which I was not able to resolve.

## Features
- Czech language - no konečně aspoň něco.
- Runs locally in docker, no dependencies.
- REST API for whole audio file transcription. Streaming is not supported yet.

## Next Steps
 - Streaming transcription
 - Dedicated Dockerfile.serving that will be minimized version with just needed for serving API
 - Integrate additional datasets

# Models
Transcription happens in two phases. First an accoustic model estimates best representation of audio by characters in czech alphabet. This is sometimes incorrect as there can be multiple options for similarly sounding characters. Hence a language model kicks in as a second phase to cross-check against czech words and its relevant context. 

## Accoustic Model
See specific [README.md](accoustic_model/README.md)

Based on:
 - 45 hours of Mozilla Common Voice

## Language Model
See specific [README.md](language_model/README.md)

Based on:
 - 700MB of czech wikipedia
 
# Installation (for using the model)

## Requirements:
- Docker

## Procedure:

1. Download latest models from releases https://deepspeech.slesinger.info

There are two files:
- Accoustic model: ```deepspeech_final.pth```
- Language model: ```lm.bin```

Place the files to any directory, for example ```/home/hass/docker-volumes/deepspeech```

2. Download and start a container image that will be ready for serving transcription API with this command:
```
sudo docker run -v /home/hass/docker-volumes/deepspeech:/workspace/data --tmpfs /tmp -p 8888:8888 -p 10456:10456 --net=host --ipc=host --name deepspeech -e MODEL=/workspace/data/deepspeech_final.pth -e LM=/workspace/data/lm.bin slesinger/deepspeech:latest
```

Inspiration for [Dockerfile](https://github.com/arpabet/deepspeech-docker/blob/master/Dockerfile).

Make sure environment variable point to correct model files.

## Using the model

Once container is started you can make HTTP POST queries supplying a WAV file (PCM, 16bit, mono, 16kHz)

Example: curl command:
```
curl -X POST http://localhost:10456/transcribe -H "Content-type: multipart/form-data" -F "file=@/tmp/wav/sample_voice.wav"
```
