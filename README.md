<div align="center">
  <img src="assets/anuttacon.svg" alt="Anuttacon" width="32" height="32" style="vertical-align: middle; margin-right: 10px;"/>
  <h1 style="display: inline-block; margin: 0;">Speech-DRAME: A Framework for Human-Aligned Benchmarks in Speech Role-Play</h1>
  
  <br><br>
  <img src="assets/speech-drame-logo.png" alt="Speech-DRAME Logo" width="300"/>
</div>

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![arXiv](https://img.shields.io/badge/arXiv-2511.01261-b31b1b.svg)](https://arxiv.org/abs/2511.01261)


**Speech-DRAME** introduces a unified framework for evaluating AI-generated speech in role-playing scenarios, addressing the critical gap in speech role-play evaluation through human-aligned benchmarks and specialized evaluation models.

## 📖 Abstract

Role-play has become a key testbed for generative models, expanding from text-only dialogue to multimodal interaction. Extending role-play to speech captures prosody, emotion, and delivery, but also poses new evaluation challenges. Current pipelines often use audio large language models (ALLMs) as zero-shot judges, which miss paralinguistic cues, collapse multiple aspects into coarse scores, and rely on synthetic speech references that fail to reflect real-world roles.

We present **Speech-DRAME**, a unified framework that contributes at three levels: (i) **Speech-DRAME-EvalBench**, an evaluation benchmark with bilingual human-annotated data and protocols for training and testing speech evaluation models (SEMs), (ii) **DRAME-Eval**, a fine-tuned evaluation model that substantially outperforms zero-shot and few-shot ALLMs, and (iii) **Speech-DRAME-RoleBench**, a speech role-play benchmark that leverages DRAME-Eval as an automatic judge to compare speech foundation models (SFMs).

## 🎯 Key Contributions

### 1. **Dual Evaluation Paradigm**
Speech-DRAME distinguishes between two complementary evaluation strategies:
- **🎪 Archetype Evaluation**: A top-down approach measuring adherence to broad role archetypes
- **🎭 Realism Evaluation**: A bottom-up approach grounded in real human speech that emphasizes nuanced role quality

### 2. **Human-Aligned Performance**
On the benchmark distribution, DRAME-Eval exceeds the strongest evaluated baseline under a consistent masked protocol:
- **Archetype Evaluation**: masked pooled Pearson correlation improves from 0.236 to 0.629
- **Realism Evaluation**: masked pooled Pearson correlation improves from 0.387 to 0.602

### 3. **Comprehensive Framework**
- **Multi-Modal Evaluation**: Combines audio and text analysis for comprehensive speech assessment
- **Bilingual Support**: English and Chinese evaluation capabilities
- **Multiple Approaches**: Supports zero-shot, few-shot, and fine-tuning evaluation methods
- **Reproducibility**: Rebuttal analysis code and explicit artifact requirements in [analysis/README.md](analysis/README.md)

## 🏗️ Framework Components

<div align="center">
  <img src="assets/Speech-DRAME.png" alt="Speech-DRAME Framework Overview" width="800" style="border: 1px solid #ddd; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1);"/>
  <p><em>Speech-DRAME Framework Architecture and Components</em></p>
</div>

Speech-DRAME consists of three main components that work together to provide comprehensive speech role-play evaluation:

### 1. **Speech-DRAME-EvalBench**
A comprehensive evaluation benchmark featuring:
- **Bilingual Human-Annotated Data**: High-quality English and Chinese datasets with detailed annotations
- **Training Protocols**: Standardized procedures for training speech evaluation models
- **Testing Protocols**: Rigorous evaluation procedures for model assessment
- **Quality Control**: Multiple annotator validation and inter-annotator agreement measures

### 2. **DRAME-Eval**
A specialized fine-tuned evaluation model that:
- **Outperforms Zero-shot ALLMs**: Achieves significantly better human alignment than general-purpose models
- **Handles Both Paradigms**: Supports both archetype and realism evaluation approaches
- **Scalable Inference**: Enables efficient evaluation of large-scale speech generation systems
- **Bilingual Capability**: Processes both English and Chinese speech role-play scenarios

### 3. **Speech-DRAME-RoleBench**
A systematic benchmark for comparing speech foundation models:
- **Automated Evaluation**: Uses DRAME-Eval as an automatic judge for consistent assessment
- **Model Comparison**: Enables fair comparison across different speech foundation models
- **Standardized Metrics**: Provides consistent evaluation criteria across different models
- **Reproducible Results**: Ensures reproducible and comparable evaluation outcomes

## 📁 Project Structure

```
speech_drame/
├── zeroshot/          # Zero-shot evaluation framework
│   ├── models/        # Audio LLM implementations
│   ├── prompts/       # Evaluation prompts and schemas
│   ├── pyscripts/     # Evaluation and scoring scripts
│   └── scripts/       # Shell scripts for batch evaluation
├── fewshot/           # Few-shot evaluation framework
│   ├── models/        # Audio LLM implementations
│   ├── prompts/       # Evaluation prompts and schemas
│   ├── pyscripts/     # Evaluation and scoring scripts
│   └── scripts/       # Shell scripts for batch evaluation
├── finetuning/        # Fine-tuning framework
│   ├── src/           # Training and testing source code
│   ├── conf/          # Configuration files
│   └── scripts/       # Training and evaluation scripts
└── data/              # Evaluation datasets
    ├── archetype_data/ # Character archetype evaluation data
    └── realism_data/   # Realism evaluation data
```

## 🚀 Quick Start

### Prerequisites

- Python 3.10+
- CUDA-compatible GPU (recommended for fine-tuning)
- Conda environment manager

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Anuttacon/speech_drame.git
   cd speech_drame
   ```

2. **Set up environment**
   ```bash
   conda create -n speech_drama python=3.10
   conda activate speech_drama
   pip install -r requirements.txt
   ```

3. **Configure API keys** (for cloud-based models)
   ```bash
   export OPENAI_API_KEY="your-openai-key"
   export GOOGLE_API_KEY="your-google-key"
   # Add other API keys as needed
   ```

### Basic Usage

#### Realism Evaluation (Zero-shot)
```bash
cd zeroshot
PYTHONPATH=. python pyscripts/eval.py \
    --model_name gemini25 \
    --model_tag gemini-2.5-pro \
    --prompt_version v1 \
    --data_path ../data/realism_data/test.jsonl \
    --output_path realism_results.jsonl \
    --prompt_type combined
```

#### Archetype Evaluation (Zero-shot)
```bash
cd zeroshot
PYTHONPATH=. python pyscripts/eval.py \
    --model_name gemini25 \
    --model_tag gemini-2.5-pro \
    --prompt_version v1 \
    --data_path ../data/archetype_data/test.jsonl \
    --output_path archetype_results.jsonl \
    --prompt_type archetype_combined
```

#### Few-shot Evaluation
```bash
cd fewshot
# For realism evaluation
PYTHONPATH=. python pyscripts/eval.py \
    --model_name gpt4o \
    --model_tag gpt-4o-audio-preview \
    --prompt_version v1 \
    --data_path ../data/realism_data/test.jsonl \
    --output_path fewshot_realism_results.jsonl \
    --prompt_type combined

# For archetype evaluation
PYTHONPATH=. python pyscripts/eval.py \
    --model_name gpt4o \
    --model_tag gpt-4o-audio-preview \
    --prompt_version v1 \
    --data_path ../data/archetype_data/test.jsonl \
    --output_path fewshot_archetype_results.jsonl \
    --prompt_type archetype_combined
```

#### Fine-tuning
```bash
cd finetuning
# Train for realism evaluation
bash run_realism_train.sh

# Train for archetype evaluation  
bash run_archetype_train.sh

# Or use Python directly
python src/train_sft.py \
    --model_name_or_path Qwen/Qwen2-Audio-7B-Instruct \
    --data_file /path/to/data/train_all_real_v2/data.jsonl \
    --out_dir exp/realism_sft_model \
    --yaml_config conf/sft_lora.yaml
```

## Release status and rebuttal updates

The public dataset destinations are
[Anuttacon/speech-drame-evalbench](https://huggingface.co/datasets/Anuttacon/speech-drame-evalbench)
and [ftshijt/speech-drame-evalbench](https://huggingface.co/datasets/ftshijt/speech-drame-evalbench).
At the October 16, 2026 preparation check, both contained introductory dataset
cards only; benchmark payloads and trained checkpoints were still pending.
The experimental corpus contains 8,272 archetype and 15,414 realism records;
these are experimental counts, not counts of publicly released data.

The planned public subset includes only records and components authorized for
release. The Anuttacon variant will contain metadata and mel features; ftshijt will add vocoder
reconstructions for the same authorized IDs and split memberships. The dataset
contributions are scoped to CC BY-NC 4.0; the code retains its Apache 2.0 license.
Source waveforms and additional development data are excluded from that plan.

[Rebuttal update review](docs/REBUTTAL_UPDATES.md) documents the corrections,
validation, and remaining reproduction limits. [Analysis guide](analysis/README.md)
explains the imported offline analyses, including reliability, uncertainty,
per-language breakdowns, shortcut controls, and training ablations. Full-corpus
results require the corresponding canonical metadata and saved predictions.

## 📊 Evaluation Methodology

Speech-DRAME introduces a novel dual evaluation paradigm that addresses the limitations of existing speech role-play evaluation approaches. The framework combines two complementary strategies that capture different aspects of speech role-play quality.

### 🎭 **Realism Evaluation (Bottom-up Approach)**
Grounded in real human speech patterns, this approach emphasizes nuanced role quality and narrative consistency. The evaluation focuses on how well generated speech captures the subtle characteristics of authentic human role-play.

**11 annotated dimensions; 10 model outputs:** Semantic Match remains an annotation and is excluded from the model-output average because it largely overlaps Local Scene Fit.

| Metric | Description | Research Rationale |
|--------|-------------|-------------------|
| **Pitch Dynamics** | Variety and appropriateness of pitch contours | Captures prosodic variation essential for emotional expression |
| **Rhythmic Naturalness** | Flow, timing, absence of awkward pauses | Measures temporal coherence in speech delivery |
| **Stress Emphasis** | Clarity and correctness of syllable/word emphasis | Evaluates linguistic stress patterns for meaning conveyance |
| **Emotion Accuracy** | Does expressed emotion match intended label? | Assesses emotional alignment with character context |
| **Emotion Intensity** | Strength of expressed emotion | Measures emotional expressiveness and authenticity |
| **Emotional Dynamic Range** | Variation of emotion over the utterance | Captures emotional complexity and development |
| **Voice Identity Matching** | Matches speaker's known vocal identity/timbre | Evaluates consistency with character voice characteristics |
| **Trait Embodiment** | Presence of profile traits (positive or opposite) | Measures character trait expression in speech |
| **Local Scene Fit** | Speech content & tone suit the immediate scene | Assesses contextual appropriateness |
| **Global Story Fit** | Consistent with overarching story/speaker profile | Evaluates narrative coherence and character consistency |
| **Semantic Matchness** | Spoken content perfectly advances the scene goal | Measures functional effectiveness of speech content |

### 🎪 **Archetype Evaluation (Top-down Approach)**
This approach measures adherence to broad role archetypes, focusing on role-playing accuracy and basic performance criteria. It provides a more structured evaluation of fundamental role-play capabilities.

**4-Dimensional Rubric:**
| Metric | Description | Research Rationale |
|--------|-------------|-------------------|
| **Content Pass** | Binary pass/fail for basic requirements (length, relevance, language) | Ensures minimum quality standards for evaluation validity |
| **Audio Quality** | Technical quality assessment (artifacts, glitches) | Separates technical quality from content evaluation |
| **Human Likeness** | Context-free naturalness of speech delivery | Measures fundamental speech naturalness independent of role |
| **Appropriateness** | Context-dependent fit to role and scene requirements | Evaluates role-specific appropriateness and adherence |

### 🔬 **Methodological Innovation**

**Dual Paradigm Benefits:**
- **Complementary Coverage**: Top-down and bottom-up approaches capture different evaluation dimensions
- **Human Alignment**: Both paradigms are validated against human judgments with improved correlation
- **Bilingual Support**: Framework supports both English and Chinese evaluation scenarios
- **Scalable Evaluation**: Enables efficient assessment of speech foundation models at scale

## 📈 Research Results

### **Human Alignment Performance**

The rebuttal uses human-label masking consistently for every model. The reported
Avg is the mean of dimension-specific Pearson correlations, rather than one
correlation over flattened dimensions.

| Evaluation setting | Strongest baseline | DRAME-Eval |
| --- | --- | --- |
| Archetype, pooled benchmark distribution | 0.236 (Gemini 2.5 Pro zero-shot) | **0.629** |
| Realism, benchmark distribution | 0.387 (Gemini 2.5 Pro few-shot) | **0.602** |
| Authentic speech, 1,084 clean held-out recordings | 0.227 (GPT-4o audio few-shot; saved five-trial result) | **−0.142** |

The revised manuscript lists the authentic baseline as 0.226; the saved five-trial
analysis computes 0.227289. This small discrepancy is recorded in the
[review notes](docs/REBUTTAL_UPDATES.md).

DRAME-Eval's within-speaker authentic-speech correlation is **+0.029**. The earlier
+0.247 authentic-speech result used a list containing training items and is
superseded by the clean evaluation. The full 2,164-record authentic pool contains
1,080 adaptation items and 1,084 held-out items; it must not be scored as one test set.
Archetype's pooled 0.629 becomes a within-language macro of 0.389, so pooled
alignment alone does not establish comparable discrimination within each language.

The result supports evaluation within the benchmark distribution and exposes a
synthetic-to-authentic transfer gap. It does not establish reliable judging of
acting differences among authentic human performances.

### **Benchmark Contributions**
- **Speech-DRAME-EvalBench**: First comprehensive benchmark for training speech evaluation models
- **Speech-DRAME-RoleBench**: Systematic comparison framework for speech foundation models
- **Human-Annotated Data**: High-quality bilingual datasets with detailed evaluation criteria

## 📁 Data Formats

### Realism Evaluation Data
**Input Format:**
```json
{
  "id": "role-eval_v1_0000",
  "local_scene": "Character description and scene context",
  "char_age": "Adult",
  "char_style": "Playful, Charismatic, Confident",
  "char_profile": "Detailed character background",
  "transcript": "Spoken text content",
  "wav_path": "path/to/audio/file.wav",
  "annotations": {
    "pitch_variation": [5, 3, 4, 5, 5, 5],
    "rhythmic_naturalness": [5, 4, 4, 5, 2, 5],
    "emotion_accuracy": [4, 4, 4, 5, 5, 4],
    "trait_embodiment": [5, 5, 5, 4, 5, 5]
  }
}
```

**Output Format:**
```json
{
  "id": "role-eval_v1_0000",
  "model_name": "gemini25",
  "response": "{\"pitch_dynamics\": 4.0, \"rhythmic_naturalness\": 3.0, \"emotion_accuracy\": 4.0, ...}",
  "audio_path": "path/to/audio/file.wav"
}
```

### Archetype Evaluation Data
**Input Format:**
```json
{
  "id": "250814_zh_SocialIdentity_0051",
  "question": "你是拖延症患者。朋友询问你之前答应帮忙做的一件事进展如何。你说：",
  "wav_path": "path/to/audio/file.wav",
  "annotations": {
    "content_pass": [true, true, true, true, true, true],
    "audio_quality": [4, 5, 4, 4, 4, 4],
    "human_likeness": [3, 4, 3, 4, 4, 4],
    "appropriateness": [4, 4, 4, 4, 5, 5]
  }
}
```

**Output Format:**
```json
{
  "id": "250814_zh_SocialIdentity_0051",
  "model_name": "gemini25",
  "response": "{\"content_pass\": true, \"audio_quality\": 4, \"human_likeness\": 3, \"appropriateness\": 4}",
  "audio_path": "path/to/audio/file.wav"
}
```

## 🤖 Supported Models

### Zero-shot & Few-shot
- **GPT-4o** - OpenAI's multimodal model
- **Gemini 2.5 Pro** - Google's advanced multimodal model
- **Qwen2.5 Omni** - Alibaba's multimodal model
- **Kimi Audio** - Moonshot's audio model
- **Qwen2 Audio** - Alibaba's audio-specialized model

### Fine-tuning
- **Qwen2-Audio-7B-Instruct** - Primary base model for fine-tuning
- Support for LoRA, QLoRA, and other PEFT methods

## 📈 Results Analysis

Use [the primary masked scorer](analysis/README.md) for revised-paper results:

```bash
pip install -r analysis/requirements.txt
python analysis/evaluate.py \
  --track realism \
  --metadata /path/to/canonical/metadata/test_base.jsonl \
  --predictions /path/to/predictions/role_eval_*_exact.json \
  --output analysis/out/realism.json
```

The scorer supports saved baseline trials and fine-tuned continuous probability
scores, coverage counts, per-language results, and shared-ID sample bootstrap CIs.
Authentic-speech evaluation additionally requires `--split authentic`,
`--train-ids`, and `--test-ids`; it rejects training/test overlap and filters the
metadata pool to explicit held-out IDs. See the analysis guide for full examples,
artifact layout, controls, and known differences from saved rebuttal intervals.

The older scorers in `zeroshot/pyscripts/`, `fewshot/pyscripts/`, and the plotting
parsers in `finetuning/scripts/` implement historical conventions and are retained
for historical comparisons. Their floor-imputed results are not the primary
masked numbers above.

## 🔧 Configuration

### Model Configuration
Models are configured through YAML files in the `conf/` directory:

```yaml
training:
  model_name_or_path: "Qwen/Qwen2-Audio-7B-Instruct"
  num_train_epochs: 2
  learning_rate: 0.00005
  per_device_train_batch_size: 4
```

### Evaluation Configuration
Evaluation parameters can be customized through command-line arguments or configuration files.

## 📚 Documentation

- [Zero-shot Evaluation Guide](zeroshot/README.md)
- [Few-shot Evaluation Guide](fewshot/README.md)
- [Fine-tuning Guide](finetuning/README.md)
- [API Reference](PROJECT_STRUCTURE.md)

## 📄 License

Copyright 2025 Anuttacon

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## 📚 Citation

If you use Speech-DRAME in your research, please cite our paper:

```bibtex
@inproceedings{shi2025speech,
  title={Speech-DRAME: A Framework for Human-Aligned Benchmarks in Speech Role-Play},
  author={Shi, Jiatong and Han, Jionghao and Lu, Yichen and Pascual, Santiago and Wu, Pengfei and Cui, Chenye and Watanabe, Shinji and Weng, Chao and Zhou, Cong},
  booktitle={ArXiv PrePrint},
  url={https://arxiv.org/abs/2511.01261}, 
  year={2026},
}
```

## 🙏 Acknowledgments

We thank the following organizations and communities for their contributions:
- **Qwen, KimiAudio, GLM** for providing the base audio models
- **OpenAI, Google** for API access to their multimodal models
- **Hugging Face** for hosting and distributing the evaluation datasets
- **The open-source community** for various tools and libraries that made this work possible
- **Human annotators** who contributed to the bilingual evaluation datasets

## 📞 Support

For questions and support:
- Create an issue in the repository
- Contact the development team (jiatong.shi@anuttacon.com or cong.zhou@anuttacon.com)
- Check the documentation in each module's README

---

**Note**: This project is actively maintained. Please check for updates regularly and report any issues you encounter.