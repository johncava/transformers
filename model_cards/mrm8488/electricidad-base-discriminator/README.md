---
language: es
thumbnail: https://i.imgur.com/uxAvBfh.png


---

## ELECTRICIDAD: The Spanish Electra [Imgur](https://imgur.com/uxAvBfh)

**Electricidad-base-discriminator** (uncased) is a ```base``` Electra like model (discriminator in this case) trained on a + 20 GB of  the [OSCAR](https://oscar-corpus.com/) Spanish corpus.

As mentioned in the original [paper](https://openreview.net/pdf?id=r1xMH1BtvB):
**ELECTRA** is a new method for self-supervised language representation learning. It can be used to pre-train transformer networks using relatively little compute. ELECTRA models are trained to distinguish "real" input tokens vs "fake" input tokens generated by another neural network, similar to the discriminator of a [GAN](https://arxiv.org/pdf/1406.2661.pdf). At small scale, ELECTRA achieves strong results even when trained on a single GPU. At large scale, ELECTRA achieves state-of-the-art results on the [SQuAD 2.0](https://rajpurkar.github.io/SQuAD-explorer/) dataset.

For a detailed description and experimental results, please refer the paper [ELECTRA: Pre-training Text Encoders as Discriminators Rather Than Generators](https://openreview.net/pdf?id=r1xMH1BtvB).


## Model details ⚙

|Name| # Value|
|-----|--------|
|Layers|	12   |
|Hidden |768 	|
|Params| 110M|

## Evaluation metrics (for discriminator) 🧾

|Metric | # Score |
|-------|---------|
|Accuracy| 0.985|
|Precision| 0.726|
|AUC | 0.922|



## Fast example of usage 🚀

```python
from transformers import ElectraForPreTraining, ElectraTokenizerFast
import torch

discriminator = ElectraForPreTraining.from_pretrained("/content/electricidad-base-discriminator")
tokenizer = ElectraTokenizerFast.from_pretrained("/content/electricidad-base-discriminator")

sentence = "El rápido zorro marrón salta sobre el perro perezoso"
fake_sentence = "El rápido zorro marrón amar sobre el perro perezoso"

fake_tokens = tokenizer.tokenize(fake_sentence)
fake_inputs = tokenizer.encode(fake_sentence, return_tensors="pt")
discriminator_outputs = discriminator(fake_inputs)
predictions = torch.round((torch.sign(discriminator_outputs[0]) + 1) / 2)

[print("%7s" % token, end="") for token in fake_tokens]

[print("%7s" % prediction, end="") for prediction in predictions.tolist()]

# Output:
'''
el rapido  zorro  marro    ##n   amar  sobre     el  perro   pere ##zoso    0.0    0.0    0.0    0.0    0.0    0.0    1.0    1.0    0.0    0.0    0.0    0.0    0.0[None, None, None, None, None, None, None, None, None, None, None, None, None
'''
```

As you can see there are **1s** in the places where the model detected a fake token. So, it works! 🎉

## Acknowledgments

I thank [🤗/transformers team](https://github.com/huggingface/transformers) for allowing me to train the model (specially to [Julien Chaumond](https://twitter.com/julien_c)).



> Created by [Manuel Romero/@mrm8488](https://twitter.com/mrm8488)

> Made with <span style="color: #e25555;">&hearts;</span> in Spain