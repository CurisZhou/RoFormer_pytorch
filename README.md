# PyTorch RoFormer
原版Tensorflow权重(https://github.com/ZhuiyiTechnology/roformer)
- [chinese_roformer_L-12_H-768_A-12.zip](https://pan.baidu.com/s/1fiss862YsGCwf2HvU_Jm-g) (提取码：xy9x)

已经转化为PyTorch权重
- [chinese_roformer_base.zip](https://pan.baidu.com/s/1P9Dcgq13Fs7O7yKyMLsXWw) (提取码：a79k)

## 使用
```python
https://huggingface.co/junnyu/roformer_chinese_base
import torch
from model import RoFormerModel, RoFormerTokenizer
tokenizer = RoFormerTokenizer.from_pretrained("junnyu/roformer_chinese_base")
model = RoFormerModel.from_pretrained("junnyu/roformer_chinese_base")
inputs = tokenizer(text, return_tensors="pt")
with torch.no_grad():
    outputs = model(**inputs).last_hidden_state
print(outputs.shape)
```
 
## 手动权重转换
```bash
python convert_roformer_original_tf_checkpoint_to_pytorch.py \
    --tf_checkpoint_path=xxxxxx/chinese_roformer_L-12_H-768_A-12/bert_model.ckpt \
    --roformer_config_file=pretrained_models/chinese_roformer_base/config.json \
    --pytorch_dump_path=pretrained_models/chinese_roformer_base/pytorch_model.bin
```
## tf与pytorch精度对齐
```python
python compare_model.py
mean difference : tensor(4.3925e-07)
max  difference : tensor(7.6294e-06)
```
## bert4keras的WoBertTokenizer与huggingface版本的WoTokenizer比较
```python
python compare_tokenizer.py
True
```

## 中文情感分类(chnsenti)
```bash
bash run_chnsenti.sh
```
<p align="center">
    <img src="figure/loss.png" width="100%" />
</p>

### 结果

| model | chnsenti  |
| --------------- | --------- |
| tensorflow-NEZHA(base-wwm)      | 94.75     |
| pytorch-NEZHA(base-wwm)         | 94.92     |
| pytorch-RoFormer(base)          | **95.08** |

## 参考
https://github.com/pengming617/bert_classification

https://github.com/bojone/bert4keras

https://github.com/ZhuiyiTechnology/roformer 

https://github.com/lonePatient/NeZha_Chinese_PyTorch 

https://github.com/lonePatient/TorchBlocks
