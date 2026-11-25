# 全体方針
以下に、研究で使うLLMの「デフォルト設定」を記す。
別途の指示がない限りは、これに従う。

ただし、この方針は2026-11-04に立てられ、2026-11-05に更新されたもので、時が経つとLLMが古くなる可能性がある。
「なんか設定が古臭いな」と思ったら、ユーザに更新を提言する。








# 推論フレームワーク
* LLMの推論(inference)をする場合、[llm-inference](https://github.com/HonoMi/llm-inference)を使う。ただし、それが不適当な場合は、vLLMを使う。
    - 当該ライブラリがローカル(`$PROJECTS/llm-inference`)にある場合は、それを使うこと。








# モデル
[Reasoning LLM](#reasoning-llm-の既定)を使う。
* 研究が1-pass回りきるまで: `ローカルモデル - 10B級`
* その後: `ローカルモデル - 30B級` や `APIモデル - 廉価版` を検討する。








# 生成の設定
* ローカルモデル
    - 下記の表に書かれている設定を使う。表では定まり切らない場合は、[補足](#補足)の「以下のような方針で決めた」を参考に定める。
    - また、高速化のために、[llm-inference - Tuning vLLM throughput - Recommended settings](https://github.com/HonoMi/llm-inference)に記載の、同程度のサイズのLLMの設定も、流用する。
* APIモデル
    - 可能なら、BatchやFlexといった機能で、安く済ませること。ただし、コードが煩雑になるなら、やらなくてよい。
* LLMの回答形式は、`\boxed{}`にする。








# Reasoning LLM の既定




## ローカルモデル


### 10B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B) | - | 16,384 | 1.0 | 1.0 | 1.5 | 0.95 | 20 | 0.0 |
| [Gemma 4 12B IT](https://huggingface.co/google/gemma-4-12B-it) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 64 | 0.0 |
| [OLMo 3 7B Think](https://huggingface.co/allenai/Olmo-3-7B-Think) | - | 16,384 | 0.6 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |
| [Nemotron Nano 9B v2](https://huggingface.co/nvidia/NVIDIA-Nemotron-Nano-9B-v2) | - | 16,384 | 0.6 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |


### 30B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-35B-A3B](https://huggingface.co/Qwen/Qwen3.5-35B-A3B) | - | 16,384 | 1.0 | 1.0 | 1.5 | 0.95 | 20 | 0.0 |
| [Gemma 4 26B-A4B IT](https://huggingface.co/google/gemma-4-26B-A4B-it) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 64 | 0.0 |
| [OLMo 3.1 32B Think](https://huggingface.co/allenai/Olmo-3.1-32B-Think) | - | 16,384 | 0.6 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |
| [Nemotron 3.5 Lightning 30B-A3B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |


### 100B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-122B-A10B](https://huggingface.co/Qwen/Qwen3.5-122B-A10B) | - | 16,384 | 1.0 | 1.0 | 1.5 | 0.95 | 20 | 0.0 |
| [GLM-4.5-Air(106B)](https://huggingface.co/zai-org/GLM-4.5-Air) | - | 16,384 | 0.6 | 1.0 | 0.0 | 1.00 | 0 | 0.0 |
| [Nemotron 3.5 Super VL 120B-A12B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16) | medium | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |




## APIモデル


### 廉価版

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---|---|---|---|---|---|
| GPT-6 Luna(`gpt-6-luna`) | medium | 16,384 | - | - | - | - | - | - |
| Claude 5.5 Sonnet(`claude-sonnet-5-5`) | medium | 16,384 | - | - | - | - | - | - |
| Gemini 3.8 Flash(`gemini-3.8-flash`) | medium | 16,384 | - | - | - | - | - | - |


### 最上位

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---|---|---|---|---|---|
| GPT-6.1 Sol(`gpt-6.1-sol`) | medium | 16,384 | - | - | - | - | - | - |
| Claude 5.5 Opus(`claude-opus-5-5`) | medium | 16,384 | - | - | - | - | - | - |
| Gemini 4 Argon(`gemini-4-argon`) | medium | 16,384 | - | - | - | - | - | - |








# Instruct LLM の既定




## ローカルモデル


### 10B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-9B](https://huggingface.co/Qwen/Qwen3.5-9B) | - | 16,384 | 0.7 | 1.0 | 1.5 | 0.80 | 20 | 0.0 |
| [Gemma 4 12B IT](https://huggingface.co/google/gemma-4-12B-it) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 64 | 0.0 |
| [OLMo 3 7B Instruct](https://huggingface.co/allenai/Olmo-3-7B-Instruct) | - | 16,384 | 0.6 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |
| [Nemotron Nano 9B v2](https://huggingface.co/nvidia/NVIDIA-Nemotron-Nano-9B-v2) | - | 16,384 | 0 | 1.0 | 0.0 | 1.00 | 0 | 0.0 |


### 30B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-35B-A3B](https://huggingface.co/Qwen/Qwen3.5-35B-A3B) | - | 16,384 | 0.7 | 1.0 | 1.5 | 0.80 | 20 | 0.0 |
| [Gemma 4 26B-A4B IT](https://huggingface.co/google/gemma-4-26B-A4B-it) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 64 | 0.0 |
| [OLMo 3.1 32B Instruct](https://huggingface.co/allenai/Olmo-3.1-32B-Instruct) | - | 16,384 | 0 | 1.0 | 0.0 | 1.00 | 0 | 0.0 |
| [Nemotron 3.5 Lightning 30B-A3B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-BF16) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |


### 100B級

| モデル | thinking effort | response_length | temperature | repetition_penalty | presence_penalty | top_p | top_k | min_p |
|---|---|---:|---:|---:|---:|---:|---:|---:|
| [Qwen3.5-122B-A10B](https://huggingface.co/Qwen/Qwen3.5-122B-A10B) | - | 16,384 | 0.7 | 1.0 | 1.5 | 0.80 | 20 | 0.0 |
| [GLM-4.5-Air(106B)](https://huggingface.co/zai-org/GLM-4.5-Air) | - | 16,384 | 0 | 1.0 | 0.0 | 1.00 | 0 | 0.0 |
| [Nemotron 3.5 Super VL 120B-A12B](https://huggingface.co/nvidia/NVIDIA-Nemotron-3.5-Super-VL-120B-A12B-BF16) | - | 16,384 | 1.0 | 1.0 | 0.0 | 0.95 | 0 | 0.0 |








# 補足
* 表の生成設定の値
    - 以下のような方針で決めた:
        * ローカルモデル
            1. 開発元の推奨値。モデルカードや公式ドキュメントに書かれた値を指す。
            2. 推奨値が無い場合は、[Artificial Analysis](https://artificialanalysis.ai/methodology/intelligence-benchmarking)の値。上の6項目のうち、Artificial Analysisが定めているのはtemperatureだけで、Thinking(推論モデル)は0.6、Instruct(非推論モデル)は0である。
            3. それでも定まらないなら、その機能を「使わない」という値にした。
                * top_p・top_k・min_pは、次に出力するトークンの候補を、確率の高いものだけに絞り込む設定である。絞り込みをしない値は `top_p=1.0`、`top_k=0`、`min_p=0.0` である。
                * ペナルティをかけない値は `presence_penalty=0.0`、`repetition_penalty=1.0` である。
        * APIモデル
            - 生成設定を送らず、提供元の既定値を使う。
    - より具体的な出所:
        * Qwen3.5: モデルカードの「general tasks」向け推奨値。Thinkingは `temperature=1.0`、`top_p=0.95`、Instructは `temperature=0.7`、`top_p=0.80` で、両方とも `top_k=20`、`presence_penalty=1.5` である。
        * Gemma 4: モデルカードが全用途に共通で推奨する `temperature=1.0`、`top_p=0.95`、`top_k=64`。
        * OLMo 3 7B Think/Instruct、OLMo 3.1 32B Think: モデルカードの推奨値 `temperature=0.6`、`top_p=0.95`。開発元の評価もこの設定で行われている。
        * OLMo 3.1 32B Instruct: モデルカードに推奨値が無いため、Artificial Analysisの値(`temperature=0`)にしている。同系列のOLMo 3 7B Instructの推奨は0.6である。
        * Nemotron Nano 9B v2: モデルカードの推奨値。推論ありは `temperature=0.6`、`top_p=0.95`、推論なしはgreedy生成(`temperature=0`)である。
        * Nemotron 3.5 Lightning、Nemotron 3.5 Super VL: モデルカードの推奨値 `temperature=1.0`、`top_p=0.95`。推論の有無で値は変わらない。
        * GLM-4.5-Air: モデルカードにも同梱の `generation_config.json` にも推奨値が無いため、temperatureはArtificial Analysisの値にしている。他の項目は、絞り込みもペナルティも使わない値である。
* Nemotronは、10B級に現行世代(Nemotron 3/3.5)のモデルが無いため、前世代のNemotron Nano 9B v2を使う。
* ローカルモデルの `repetition_penalty=1.0` と `presence_penalty=0.0` は、それぞれペナルティなしを表す。
* ローカルモデルの `top_k=0` と `min_p=0.0` は、それぞれ候補の絞り込みをしないことを表す。
* 表に記載していない `frequency_penalty` は、すべてのローカルモデルで `0.0` とする。
* response_lengthは生成トークン数の上限である。開発元の推奨値ではなく、共通の評価条件として16,384に統一している。QwenとOLMoの推奨(32,768)とは異なる。
* temperature=0での動作
    - vLLMでは、`temperature=0` のとき、内部的に `top_p=1.0`、`top_k=0`、`min_p=0.0` に設定してgreedy生成を行う。greedy生成とは、各ステップで最も確率の高いトークンを選ぶ方法である。したがって、temperature=0の行のtop_p/top_k/min_pは設定として保持する値であり、生成結果に影響しない。
