---
title: Rasa NLU框架学习
date: 2018-12-20 17:19:18
tags: rasa
---
* Rasa Core: 0.12.3
* Rasa NLU: 0.13.8

#### 模块解释
Rasa Core：对话控制的模块，通过对stories训练来获取对话流程模型
Rasa NLU：intent识别的模块，看名称就知道是NLU，将用户输入的内容识别为intent
NLU Pipeline：字面意思为管道，是nlu训练使用的算法和基础模型。最常见的pipeline有两个，spacy_sklearn和tensorflow_embedding，前者带有已有的训练结果，适合数据量较小的模型训练(1000)，后者不包含已有的训练模型，适合数据流较大的模型训练。

#### 语言支持
| backend  |  supported languages |

| --- | --- |

| spacy-sklearn  |  english (en), german (de), spanish (es), portuguese (pt), italian (it), dutch (nl), french  (fr)MITIE    english (en) |
| Jieba-MITIE  |  chinese (zh) * |

Jieba-MITIE需要预先训练好的模型文件`data/total_word_feature_extractor_zh.dat`，训练工具使用(MITIE Wordrep Tool)[https://github.com/mit-nlp/MITIE/tree/master/tools/wordrep ]

#### 训练数据的格式
有markdown和json两种格式。
json格式的每条数据包含三个要点：text，intent，entities：
* The text is the search query; An example of what would be submitted for parsing. [required]
* The intent is the intent that should be associated with the text. [optional]
* The entities are specific parts of the text which need to be identified. [optional]

#### 环境配置
安装rasa_core，这个会包含rasa_nlu的模块