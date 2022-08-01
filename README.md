# "中国法律智能技术评测"司法人工智能挑战赛数据说明

English version can be found [here](https://github.com/thunlp/CAIL/blob/master/README_en.md).

## 目录

* [简介](#一简介)
* [任务说明](#二任务说明)
* [FAQ](#faq)

## 一、简介

法律智能旨在赋予机器阅读理解法律文本与定量分析案例的能力，完成罪名预测、法律条款推荐、刑期预测等具有实际应用需求的任务，有望辅助法官、律师等人士更加高效地进行法律判决。近年来，以深度学习和自然语言处理为代表的人工智能技术取得巨大突破，也开始在法律智能领域崭露头角，受到学术界和产业界的广泛关注。

挑战赛将提供海量的刑事法律文书数据作为数据集，旨在为研究者提供学术交流平台，推动语言理解和人工智能领域技术在法律领域的应用，促进法律人工智能事业的发展。每年比赛结束后将举办技术交流和颁奖活动。诚邀学术界和工业界的研究者和开发者积极参与该挑战赛！


## 二、任务说明

### 2.1 介绍

* 任务一（罪名预测）：根据刑事法律文书中的案情描述和事实部分，预测被告人被判的罪名；
* 任务二（法条推荐）：根据刑事法律文书中的案情描述和事实部分，预测本案涉及的相关法条；
* 任务三（刑期预测）：根据刑事法律文书中的案情描述和事实部分，预测被告人的刑期长短。

参赛者可选择一个或者多个任务参与挑战赛。同时，为了鼓励参赛者参与到更多的任务中，组委会将单独奖励参与更多任务的参赛者。

### 2.2 数据介绍

本次挑战赛所使用的数据集是来自“[中国裁判文书网](http://wenshu.court.gov.cn/)”公开的刑事法律文书，其中每份数据由法律文书中的案情描述和事实部分组成，同时也包括每个案件所涉及的法条、被告人被判的罪名和刑期长短等要素。

数据集共包括**268万刑法法律文书**，共涉及[202条罪名](meta/accu.txt)，[183条法条](meta/law.txt)，刑期长短包括**0-25年、无期、死刑**。

我们将先后发布CAIL2018-Small和CAIL2018-Large两组数据集。CAIL2018-Small包括19.6万份文书样例，直接在该网站发布，包括15万训练集，1.6万验证集和3万测试集。CAIL2018-Large数据集，包括150万文书样例。剩余90万份文书将作为第一阶段的测试数据CAIL2018-Large-test。

#### 2.2.1 字段及意义
数据利用json格式储存，每一行为一条数据，每条数据均为一个字典。

* **fact**: 事实描述  
* **meta**: 标注信息，标注信息中包括:   
    * **criminals**: 被告(数据中均只含一个被告)  
    * **punish\_of\_money**: 罚款(单位：元)
    * **accusation**: 罪名  
    * **relevant\_articles**: 相关法条  
    * **term\_of\_imprisonment**: 刑期  
        刑期格式(单位：月)
        * **death\_penalty**: 是否死刑  
        * **life\_imprisonment**: 是否无期
        * **imprisonment**: 有期徒刑刑期

```
这里是简单的一条数据展示:  
{   
	"fact": "2015年11月5日上午，被告人胡某在平湖市乍浦镇的嘉兴市多凌金牛制衣有限公司车间内，与被害人孙某因工作琐事发生口角，后被告人胡某用木制坐垫打伤被害人孙某左腹部。经平湖公安司法鉴定中心鉴定：孙某的左腹部损伤已达重伤二级。",   
	"meta": 
	{  
		"relevant_articles": [234],  
		"accusation": ["故意伤害"], 
		"criminals": ["胡某"],  
		"term_of_imprisonment": 
		{  
			"death_penalty": false,  
			"imprisonment": 12,  
			"life_imprisonment": false
		}
	}
}
```

### 2.3 数据集下载

**CAIL2018数据集**已经正式发布下载，下载地址为 [CAIL2018数据集](https://cail.oss-cn-qingdao.aliyuncs.com/CAIL2018_ALL_DATA.zip)。

##### 引用说明

如果您引用CAIL2018数据集发表论文或取得科研成果，请您在发表论文和申报成果时声明“使用了CAIL2018数据集”，并按如下格式引用：

* Chaojun Xiao, Haoxi Zhong, Zhipeng Guo, Cunchao Tu, Zhiyuan Liu, Maosong Sun, Yansong Feng, Xianpei Han, Zhen Hu, Heng Wang, Jianfeng Xu. CAIL2018: A Large-Scale Legal Dataset for Judgment Prediction. arXiv preprint arXiv:1807.02478, 2018. [link](https://arxiv.org/abs/1807.02478)

### 2.4 评价方法

本次挑战赛使用的数据集均为来自中国裁判文书网上的刑事法律文书，标准答案是案件的判决结果。我们提供了评测时使用的评分程序共选手使用，评测方法、环境和模型提交说明请看[链接](https://github.com/thunlp/CAIL2018)。

每项任务满分100分，下面将对三项任务的评价方法分别进行说明：

#### 2.4.1 任务一、任务二

任务一（罪名预测）、任务二（法条推荐）两项任务将采用分类任务中的微平均F1值（Micro-F1-measure）和宏平均F1值（Macro-F1-measure）作为评价指标，其计算方式为：

![f1](pic/f1.png)

则任务的最终分数为：

![score1](pic/score_1.png)

#### 2.4.2 任务三

任务三（刑期预测）将采用下列公式，根据预测出的刑期与案件标准刑期之间的差值距离作为评价指标。设预测出的刑期为`lp`，标准答案为`la`，则

![v](pic/v.png)

```
若v≤0.2，则score=1；
若0.2<v≤0.4，则score=0.8
……
以此类推。
```
**特殊的情况**

若案件刑期的标准答案为**死刑**则`lp=-2`， **无期**则`lp=-1`，才计分。具体请见[评测部分源码](https://github.com/thunlp/CAIL2018/blob/a258c1dae88e8fc576529e6dcb012a430da00b95/judger/judger.py#L90)。

最后，将任务三所有测试点的分数相加并除以测试点总数乘以100作为任务三的评价得分：

![score3](pic/score_3.png)

#### 2.4.3 三项任务总分的计算方式

每个任务的满分均为100，则总分为：

![score_all](pic/score_all.png)


### 2.5 基线系统

竞赛组织方提供了一个开源的针对不同任务的基线系统（[LibSVM](https://github.com/thunlp/CAIL2018/tree/master/baseline)）。

## FAQ

### 0. 有没有选手交流的平台？

选手交流QQ群：237633234。

### 1. 从哪里可以下载到数据集？

[这里](https://cail.oss-cn-qingdao.aliyuncs.com/CAIL2018_ALL_DATA.zip)。
