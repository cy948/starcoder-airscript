# AirScript Coder

## Introduction

### Background

Today, more and more non-profession user using generic code LLM as copilot to accelerate their work by programing. Such as writing report, doing data analysis or doing workflow automation. However, though these generic code LLM like CodeX, code Llama have great standing at code comprehension, but they may not align to some organization's principals or internal conversions which is a bearer to doing above tasks. 

[AirScript](https://airsheet.wps.cn/docs/guide/summary.html) is a script to allow user manipulate WPS office files with programing interfaces. Our survey shows a lot of non-professional user are needed to programing on this script. After talking to some user, we found that, the non-professional user are leak of coding experience and they don't know how to guide a generic code LLM to write these codes.

To bridge the gap, we fine-tune a code model named `starcoder-airscript` to improve its zero-shot ability on AirScript, to help those non-profession user to write AirSciprt better.


### Tech Roadmap

Our pipeline is:
```
Collect data ===> Annotate data ===> Build dataset ==> Fine-tune model
```

### Project files

- `collectdata.ipynb`: Collect the raw data and upload to the annotation platform `doccano`
- `dataset.ipynb`: Build the dataset 
- `train.ipynb`: Load the dataset, setup training environment and train the model. After finished, upload to huggingface.

## Dataset

### Datset building

- Data collection: We collect the metadata with web spider and the whole porcess is in `collectdata.ipynb`, refer to it.

- Data annotation: We upload the ouput from the above step and upload to `doccano`. Then some experts would annotate it.

- Transform the annotation to huggingface dataset: See `dataset.ipynb`

### Human Annotation Guidelines

We invite some domain experts who has code experience on AirScript to add annotations for the code example form document site in lines. For example:

```diff
/*本示例判断如果活动工作表上区域 B1:B10 中第二个（AboveAverage）条件格式的类型为xlAboveAverageCondition，则删除该条件格式。*/
function test() {
+// 从工作表上区域 B1:B10 中选择第二个条件格式
    let aboveAverage = ActiveSheet.Range("B1:B10").FormatConditions.Item(2)
+// 若条件格式的类型为 `xlAboveAverageCondition`
    if (aboveAverage.Type == xlAboveAverageCondition) {
+// 删除该条件
        aboveAverage.Delete()
    }
}
```

## Training setup

We train our model in these setup

- Install conda 

```sh
# Install conda env
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py312_24.4.0-0-Linux-x86_64.sh
chmod +x Miniconda3-py312_24.4.0-0-Linux-x86_64.sh
```

- Install torch on https://pytorch.org/get-started/locally/