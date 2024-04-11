
**[中文](README.md)|English**

## Resume dataset of listed company personnel

This dataset utilizes publicly available resumes of listed company personnel as the original data, aiming to identify the graduation institutions, graduation dates, and majors studied in the resumes. The dataset contains a total of 14,487 entries, all in Chinese. The length of the dataset samples is relatively long, with an average length of 216.74 characters. The longest sample is 1,285 characters, while the shortest is 10 characters. The original data can be found in [data.txt](./data/data.txt).

## Original Data Annotation

This dataset has undergone manual annotation for all 14,487 original data entries. The annotated data can be found in [data_label.json](./data/data_label.json). It is allowed in this dataset for a single entity to have multiple category labels. For instance, a certain school can be labeled as both the undergraduate institution and the graduate institution, and in such cases, the entity will have multiple category attributes in the labels. The format of the annotated data is as follows:

     [
       {
        "data":Data Text 1,
        "label":
                [
                 {"start":Start Index of Entity 1,"end":End Index of Entity 1,"text":Entity 1,"labels":[label 1, label 2]},
                 {"start":Start Index of Entity 2,"end":End Index of Entity 2,"text":Entity 2,"labels":[label 3, ...]}
                ]
       },
       {
        "data":Data Text 2,
        "label":[
                 {"start":Start Index of Entity 3,"end":End Index of Entity 3,"text":Entity 3,"labels":[label 1, ...]},
                 {"start":Start Index of Entity 4,"end":End Index of Entity 4,"text":Entity 4,"labels":[label 2]}
                ]
       },
       ...
     ]

## Entity Category Classification

In this dataset, entity categories are divided into three major categories with 38 subcategories. The three major categories include "毕业院校"，"毕业时间" and "专业". The subcategories further refine these categories based on educational qualifications and other information, such as "学校-本科", "学校-硕士", "毕业时间-本科", "毕业时间-硕士", and so on. The specific category classifications are listed in the following table.

Major Category of Label|Subcategory of Label
---|---
毕业院校|学校-EMBA，学校-MBA，学校-博士后，学校-博士，学校-硕士，学校-本科，学校-大专，学校-高中，学校-初中，学校-小学，学校-函授，学校-中专，学校-高职，学校-总裁班
毕业时间|毕业时间-EMBA，毕业时间-MBA，毕业时间-博士后，毕业时间-博士，毕业时间-硕士，毕业时间-本科，毕业时间-大专，毕业时间-高中，毕业时间-初中，毕业时间-小学，毕业时间-函授，毕业时间-中专，毕业时间-高职，毕业时间-总裁班
专业|专业-EMBA，专业-MBA，专业-博士后，专业-博士，专业-硕士，专业-本科，专业-大专，专业-函授，专业-中专，专业-高职

## BERT Data Annotation

In addition to the original data annotation, this dataset has undergone further Bert-related data annotation and processing. In the original data annotation, the annotation was conducted at the character level. However, when using Bert to process the data, there may be deviations in the annotated data due to Bert's tokenization. Therefore, based on the character-level annotation, this paper first performs Bert tokenization on the text. Then, through annotation mapping, a set of data annotations suitable for Bert usage is obtained, which can be found in [data_label_bert.json](./data/data_label_bert.json). The format of the annotated data is as follows:

     [
       {
          "data":"罗 英 武 先 生 : 1969 年 出 生, 中 国 国 籍, 无 境 外 永 久 居 留 权, 浙 江 大 学 化 学 工 程 与 技 术 博 士 。 2013 年 至 今 担 任 浙 江 大 学 化 学 工 程 与 生 物 工 程 学 院 教 授 、 博 士 生 导 师",
          "label":"[1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 21, 20, 20, 20, 25, 24, 24, 24, 24, 24, 24, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]"
       },
       ...
     ]

The annotation method adopted for Bert data is the BIO tagging method. First, the text is tokenized using Bert, and then the tokenized data is compared with the original data. If a tokenized token corresponds to only one character in the original data, the tag category of the original character is used. If a tokenized token corresponds to multiple categories of original characters, the category with high representativeness is used. For example, if the token is BIII, it is annotated as B, and if it is IIIO, it is annotated as I. Based on the entity category classification, the Bert dataset has a total of 78 label categories. Among them, 38 entity categories correspond to 38*2=76 label categories, respectively, while the remaining two label categories are used for non-entity labels O and padding labels 0. The label categories can be found in label_schema.json. In the file, only the numerical value of the B tag corresponding to the entity is marked. The numerical value of the I tag for the entity category is B-1. For example, "学校-EMBA":3 represents that the B category label for the entity category "学校-EMBA" is 3, i.e., "学校-EMBA-B":3, and its corresponding I category label is 2, i.e., "学校-EMBA-I":2.

