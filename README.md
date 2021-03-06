Attention-based Neural Machine Translation
=====================================================================

### Installation
The following packages are needed:
* [Pytorch](https://github.com/pytorch/pytorch) >= 0.4.0
* NLTK

### Preparation
To obtain vocabulary for training, run:
```
python scripts/buildvocab.py --corpus /path/to/train.cn --output /path/to/cn.voc3.pkl \
--limit 30000 --groundhog
python scripts/buildvocab.py --corpus /path/to/train.en --output /path/to/en.voc3.pkl \
--limit 30000 --groundhog
```

### Training
Training the RNNSearch on Chinese-English translation datasets as follows:
```
python train.py \
--src_vocab /path/to/cn.voc3.pkl --trg_vocab /path/to/en.voc3.pkl \
--train_src corpus/train.cn-en.cn --train_trg corpus/train.cn-en.en \
--valid_src corpus/nist02/nist02.cn \
--valid_trg corpus/nist02/nist02.en0 corpus/nist02/nist02.en1 corpus/nist02/nist02.en2 corpus/nist02/nist02.en3 \
--eval_script scripts/validate.sh \
--model RNNSearch \
--optim RMSprop \
--batch_size 80 \
--half_epoch \
--cuda \
--info RMSprop-half_epoch 
```
### Evaluation
```
python translate.py \
--src_vocab /path/to/cn.voc3.pkl --trg_vocab /path/to/en.voc3.pkl \
--test_src corpus/nist03/nist03.cn \
--test_trg corpus/nist03/nist02.en0 corpus/nist03/nist03.en1 corpus/nist03/nist03.en2 corpus/nist03/nist03.en3 \
--eval_script scripts/validate.sh \
--model RNNSearch \
--name RNNSearch.best.pt \
--cuda 
```
The evaluation metric for Chinese-English we use is case-insensitive BLEU. We use the `muti-bleu.perl` script from [Moses](https://github.com/moses-smt/mosesdecoder) to compute the BLEU:
```
perl scripts/multi-bleu.perl -lc corpus/nist03/nist03.en < nist03.translated
```
### Acknowledgements
My implementation utilizes code from the following:
* [DeepLearnXMU's ABD-NMT repo](https://github.com/DeepLearnXMU/ABD-NMT)
