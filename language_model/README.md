# Language Model

Tags: czech language, stt, speech recognition, voice transcription, asr, offline, free

Transcription dcker container used in this project is able to work with language model based on [KenLM](https://github.com/kpu/kenlm). Refer to compilation and installation of KenLM there.

# Processing with KenLM

## Build ARPA file
```
lmplz --text czech-speech-to-text/language_model/wikipedia/wikipedia.txt --arpa czech-speech-to-text/language_model/wikipedia/wikipedia.arpa --order 3
```

## Build LM Binary
```
sudo build_binary czech-speech-to-text/language_model/wikipedia/wikipedia.arpa czech-speech-to-text/language_model/wikipedia/lm.bin
```

```lm.bin``` is ready to move to directory for serving.


# Data Preparation

## Wikipedia
Download latest czech data from https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-2735. 

Untar the zip so that it fits into prepared directory structure ```czech-speech-to-text/language_model/wikipedia/cs.tgz```

Use wikipedia.ipynb to preprocess data.

