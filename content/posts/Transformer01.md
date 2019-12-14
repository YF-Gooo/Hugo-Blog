---
title: "Transformer01"
date: 2019-12-14T15:26:44+08:00
linktitle: "Transformer01"
description: "Transformer结构分析01"
categories: [ "DeepLearning", "pytorch","NLP"]
tags: ["DeepLearning", "pytorch","NLP"]
weight: 10
---

白瞟bert也有一年多了，都没有好好研究过transformer的具体结构，是时候仔细研究下了.  
代码保存在 : https://github.com/YF-Gooo/DeepLearning-NLP/blob/master/transformer/transformer.py
#### 结构:
![transformer](/img/transformer/transformer.png)

## 流程:
### 数据:
    ＃输入  
    sentences = ['ich mochte ein bier P', 
                 'S i want a beer', 
                 'i want a beer E']
    ＃输出
    enc_inputs＝tensor([[1, 2, 3, 4, 0]]) 
    dec_inputs＝tensor([[5, 1, 2, 3, 4]]) 
    target_batch＝tensor([[1, 2, 3, 4, 6]])

### 模型:
##### Transformer:

    class Transformer(nn.Module):
        def __init__(self):
            super(Transformer, self).__init__()
            self.encoder = Encoder()
            self.decoder = Decoder()
            self.projection = nn.Linear(d_model, tgt_vocab_size, bias=False)
        def forward(self, enc_inputs, dec_inputs):
            enc_outputs, enc_self_attns = self.encoder(enc_inputs)
            dec_outputs, dec_self_attns, dec_enc_attns = self.decoder(dec_inputs, enc_inputs, enc_outputs)
            dec_logits = self.projection(dec_outputs) # dec_logits : [batch_size x src_vocab_size x tgt_vocab_size]
            return dec_logits.view(-1, dec_logits.size(-1)), enc_self_attns, dec_self_attns, dec_enc_attns

##### Encoder:
没啥说的就是用了个位置向量和一个MultiHeadAttention
(基于self-attention)

##### Decoder:
Decoder层的attention就比较有意思了,
不容易理解的主要是这两句

        dec_outputs, dec_self_attn = self.dec_self_attn(dec_inputs, dec_inputs, dec_inputs, dec_self_attn_mask)
        dec_outputs, dec_enc_attn = self.dec_enc_attn(dec_outputs, enc_outputs, enc_outputs, dec_enc_attn_mask)
##### 第一句
简单解释下，乍一看是在做dec_inputs的self attention,但因为引入了dec_self_attn_mask这个mask.
其实是在合并单词含义，为一会的并行计算做准备．  

    dec_outputs的第一个元素包含dec_inputs第一个元素的信息  
    dec_outputs的第二个元素包含dec_inputs前两个个元素的信息  

这是符合逻辑的，因为我们一会是要用dec_outputs的第一个元素去预测我们第一个单词  .
我们预测第一个单词的时候，是不知道后面的单词信息的，能用的只有我们输入的Ｓ.  
再次强调，现在的dec_outputs的每个元素，是多个dec_inputs信息的结合体.

##### 第二句
这一句就不是做self attention，做的是融合的attention,融合啥？  
dec_outputs已经包含dec_inputs的信息了，但这不够，我们需要更多信息也就是来自enc_output的信息.  
当把encode的信息融合进入dec_outputs以后我们就可以放心的去预测了.


### 总结
其实总体看下来transformer绝对不能说是什么特大创新．明显有coattention等更早的论文的影子．