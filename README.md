<div align="center">
<p align="center">
<img src="https://github.com/01-ai/Yi/raw/main/assets/img/Yi.svg?sanitize=true" width="200px">
</p>
<a href="https://github.com/01-ai/Yi/actions/workflows/ci.yml">
  <img src="https://github.com/01-ai/Yi/actions/workflows/ci.yml/badge.svg">
</a>
<a href="https://huggingface.co/01-ai">
  <img src="https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-01--ai-blue">
</a>
<a href="https://www.modelscope.cn/organization/01ai/">
  <img src="https://img.shields.io/badge/ModelScope-01--ai-blue">
</a>
<a href="https://github.com/01-ai/Yi/blob/main/LICENSE">
  <img src="https://img.shields.io/badge/Code_License-Apache_2.0-lightblue">
</a>
<a href="https://github.com/01-ai/Yi/blob/main/MODEL_LICENSE_AGREEMENT.txt">
  <img src="https://img.shields.io/badge/Model_License-Model_Agreement-lightblue">
</a>
<a href="mailto:oss@01.ai">
  <img src="https://img.shields.io/badge/✉️-yi@01.ai-FFE01B">
</a>
</div>

## Introduction

The **Yi** series models are large language models trained from scratch by
developers at [01.AI](https://01.ai/). The first public release contains two
bilingual (English/Chinese) base models with the parameter sizes of 6B and 34B.
Both of them are trained with 4K sequence length and can be extended to 32K
during inference time.

## News

- 🎯 **2023/11/05**: The base model of `Yi-6B-200K` and `Yi-34B-200K` with 200K context length.
- 🎯 **2023/11/02**: The base model of `Yi-6B` and `Yi-34B`.

## Model Performance


| Model         |   MMLU   |  CMMLU   |  C-Eval  |  GAOKAO  |   BBH    | Common-sense Reasoning | Reading Comprehension | Math & Code |
| :------------ | :------: | :------: | :------: | :------: | :------: | :--------------------: | :-------------------: | :---------: |
|               |  5-shot  |  5-shot  |  5-shot  |  0-shot  | 3-shot@1 |           -            |           -           |      -      |
| LLaMA2-34B    |   62.6   |    -     |    -     |    -     |   44.1   |          69.9          |         68.0          |    26.0     |
| LLaMA2-70B    |   68.9   |   53.3   |    -     |   49.8   |   51.2   |          71.9          |         69.4          |    36.8     |
| Baichuan2-13B |   59.2   |   62.0   |   58.1   |   54.3   |   48.8   |          64.3          |         62.4          |    23.0     |
| Qwen-14B      |   66.3   |   71.0   |   72.1   |   62.5   |   53.4   |          73.3          |         72.5          |  **39.8**   |
| Skywork-13B   |   62.1   |   61.8   |   60.6   |   68.1   |   41.7   |          72.4          |         61.4          |    24.9     |
| InternLM-20B  |   62.1   |   59.0   |   58.8   |   45.5   |   52.5   |          78.3          |           -           |    30.4     |
| Aquila-34B    |   67.8   |   71.4   |   63.1   |    -     |    -     |           -            |           -           |      -      |
| Falcon-180B   |   70.4   |   58.0   |   57.8   |   59.0   |   54.0   |          77.3          |         68.8          |    34.0     |
| Yi-6B         |   63.2   |   75.5   |   72.0   |   72.2   |   42.8   |          72.3          |         68.7          |    19.8     |
| Yi-6B-200K    |   64.0   |   75.3   |   73.5   |   73.9   |   42.0   |          72.0          |         69.1          |    19.0     |
| **Yi-34B**    | **76.3** | **83.7** |   81.4   |   82.8   | **54.3** |        **80.1**        |         76.4          |    37.1     |
| Yi-34B-200K   |   76.1   |   83.6   | **81.9** | **83.4** |   52.7   |          79.7          |       **76.6**        |    36.3     |

While benchmarking open-source models, we have observed a disparity between the
results generated by our pipeline and those reported in public sources (e.g.
OpenCompass). Upon conducting a more in-depth investigation of this difference,
we have discovered that various models may employ different prompts,
post-processing strategies, and sampling techniques, potentially resulting in
significant variations in the outcomes. Our prompt and post-processing strategy
remains consistent with the original benchmark, and greedy decoding is employed
during evaluation without any post-processing for the generated content. For
scores that were not reported by the original authors (including scores reported
with different settings), we try to get results with our pipeline.

To evaluate the model's capability extensively, we adopted the methodology
outlined in Llama2. Specifically, we included PIQA, SIQA, HellaSwag, WinoGrande,
ARC, OBQA, and CSQA to assess common sense reasoning. SquAD, QuAC, and BoolQ
were incorporated to evaluate reading comprehension. CSQA was exclusively tested
using a 7-shot setup, while all other tests were conducted with a 0-shot
configuration. Additionally, we introduced GSM8K (8-shot@1), MATH (4-shot@1),
HumanEval (0-shot@1), and MBPP (3-shot@1) under the category "Math & Code". Due
to technical constraints, we did not test Falcon-180 on QuAC and OBQA; the score
is derived by averaging the scores on the remaining tasks. Since the scores for
these two tasks are generally lower than the average, we believe that
Falcon-180B's performance was not underestimated.

## Usage

Feel free to [create an issue](https://github.com/01-ai/Yi/issues/new) if you
encounter any problem when using the **Yi** series models.

### 1. Prepare development environment

The best approach to try the **Yi** series models is through Docker with GPUs. We
provide the following docker images to help you get started.

<!-- - `ghcr.io/01-ai/yi:latest` -->
<!-- - `registry.lingyiwanwu.com/ci/01-ai/yi:latest` -->

Note that the `latest` tag always points to the latest code in the `main`
branch. To test a stable version, please replace it with a specific
[tag](https://github.com/01-ai/Yi/tags).

If you prefer trying out with your local development environment. First, create
a virtual environment and clone this repo. Then install the dependencies with
`pip install -r requirements.txt`. For the best performance, we recommend you
also install the latest version (`>=2.3.3`) of
[flash-attention](https://github.com/Dao-AILab/flash-attention#installation-and-features).

### 2. Download the model (optional)

By default the model weights and tokenizer will be downloaded from
[HuggingFace](https://huggingface.co/01-ai) automatically in the next step. You
can also download them manually from the following places:

- [ModelScope](https://www.modelscope.cn/organization/01ai/)
- [WiseModel](https://wisemodel.cn/models) (Search for `Yi`)
- Mirror site (remember to extract the content with `tar`)
  - [Yi-6B.tar](https://storage.lingyiwanwu.com/yi/models/Yi-6B.tar)
  - [Yi-6B-200K.tar](https://storage.lingyiwanwu.com/yi/models/Yi-6B-200K.tar)
  - [Yi-34B.tar](https://storage.lingyiwanwu.com/yi/models/Yi-34B.tar)
  - [Yi-34B-200K.tar](https://storage.lingyiwanwu.com/yi/models/Yi-34B-200K.tar)

### 3. Examples

#### 3.1 Use the base model

```bash
python demo/text_generation.py
```

To reuse the downloaded models in the previous step, you can provide the extra
`--model` argument:

```bash
python demo/text_generation.py  --model /path/to/model
```

Or if you'd like to get your hands dirty:

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

model = AutoModelForCausalLM.from_pretrained("01-ai/Yi-34B", device_map="auto", torch_dtype="auto", trust_remote_code=True)
tokenizer = AutoTokenizer.from_pretrained("01-ai/Yi-34B", trust_remote_code=True)
inputs = tokenizer("There's a place where time stands still. A place of breath taking wonder, but also", return_tensors="pt")
max_length = 256

outputs = model.generate(
    inputs.input_ids.cuda(),
    max_length=max_length,
    eos_token_id=tokenizer.eos_token_id,
    do_sample=True,
    repetition_penalty=1.3,
    no_repeat_ngram_size=5,
    temperature=0.7,
    top_k=40,
    top_p=0.8,
)
print(tokenizer.decode(outputs[0], skip_special_tokens=True))
```

<details>

<summary>Output</summary>

**Prompt**: There's a place where time stands still. A place of breath taking wonder, but also

**Generation**: There's a place where time stands still. A place of breath taking wonder, but also of great danger. A place where the very air you breathe could kill you. A place where the only way to survive is to be prepared.
The place is called the Arctic.
The Arctic is a vast, frozen wilderness. It is a place of extremes. The temperatures can drop to -40 degrees Celsius. The winds can reach speeds of 100 kilometers per hour. The sun can shine for 24 hours a day, or not at all for weeks on end.
The Arctic is also a place of great beauty. The ice and snow are a pristine white. The sky is a deep blue. The sunsets are spectacular.
But the Arctic is also a place of great danger. The ice can be treacherous. The winds can be deadly. The sun can be blinding.
The Arctic is a place where the only way to survive is to be prepared.
The Arctic is a place of extremes. The temperatures can drop to -40 degrees Celsius. The winds can reach speeds of 100 kilometers per hour. The sun can shine for 24 hours a day, or not at all for weeks on end.
The Arctic is a place of great beauty. The ice and snow are a

</details>

For more advanced usage, please refer the
[doc](https://github.com/01-ai/Yi/tree/main/demo).

#### 3.2 Finetuning from the base model:

```bash
bash finetune/scripts/run_sft_Yi_6b.sh
```

Once finished, you can compare the finetuned model and the base model with the
following command:

```bash
bash finetune/scripts/run_eval.sh
```

For more advanced usage like fine-tuning based on your custom data, please refer
the [doc](https://github.com/01-ai/Yi/tree/main/finetune).

<!-- #### 3.3 Quantization

```bash
python quantization/gptq/quant_autogptq.py \
  --model /base_model                      \
  --output_dir /quantized_model            \
  --trust_remote_code
```

Once finished, you can then evaluate the resulted model as follows:

```bash
python quantization/gptq/eval_quantized_model.py \
  --model /quantized_model                       \
  --trust_remote_code
```

For more detailed explanation, please read the [doc](https://github.com/01-ai/Yi/tree/main/quantization/gptq) -->

## Disclaimer

We use data compliance checking algorithms during the training process, to
ensure the compliance of the trained model to the best of our ability. Due to
complex data and the diversity of language model usage scenarios, we cannot
guarantee that the model will generate correct, and reasonable output in all
scenarios. Please be aware that there is still a risk of the model producing
problematic outputs. We will not be responsible for any risks and issues
resulting from misuse, misguidance, illegal usage, and related misinformation,
as well as any associated data security concerns.

## License

The source code in this repo is licensed under the [Apache 2.0
license](https://github.com/01-ai/Yi/blob/main/LICENSE). The Yi series models
are fully open for academic research and free commercial usage with permission
via applications. All usage must adhere to the [Model License
Agreement 2.0](https://github.com/01-ai/Yi/blob/main/MODEL_LICENSE_AGREEMENT.txt).
To apply for the official commercial license, please contact us
([yi@01.ai](mailto:yi@01.ai)).
