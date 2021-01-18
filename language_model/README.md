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

## Databazeknih
eBook conversion tool ```sudo aptitude instal calibre```
```
ebook-convert input.epub output.txt
```

or massive convert
```
for file  in ./*.epub; do   ebook-convert "$file" "$file.txt"; done
```

https://www.databazeknih.cz/eknihy-zdarma-ke-stazeni

https://lindat.cz/repository/xmlui/handle/11234/1-1847

Cesky Ekumenicky Preklad

http://www.ujc.cas.cz/phword/

## CWC2011
https://lindat.mff.cuni.cz/repository/xmlui/handle/11858/00-097C-0000-0006-B847-6

## czes
https://lindat.mff.cuni.cz/repository/xmlui/handle/11858/00-097C-0000-0001-CCCF-C

## Ortofon
https://lindat.mff.cuni.cz/repository/xmlui/handle/11234/1-2580

