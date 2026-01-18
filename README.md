---
license: apache-2.0
language:
- kn
- en
base_model: Qwen/Qwen3-0.6B
library_name: peft
pipeline_tag: text-generation
tags:
- kannada
- qwen3
- lora
- peft
- instruction-tuned
- indian-languages
datasets:
- Cognitive-Lab/Kannada-Instruct-dataset
---

# KannadaGPT-0.6B

A Kannada language model fine-tuned on Qwen3-0.6B using LoRA (Low-Rank Adaptation).

[![HuggingFace](https://img.shields.io/badge/HuggingFace-Model-yellow)](https://huggingface.co/Mithun501/KannadaGPT-0.6B)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)

## Model Details

| Property | Value |
|----------|-------|
| **Base Model** | [Qwen/Qwen3-0.6B](https://huggingface.co/Qwen/Qwen3-0.6B) |
| **Language** | Kannada (ಕನ್ನಡ) |
| **Fine-tuning Method** | LoRA (Low-Rank Adaptation) |
| **Training Data** | [Cognitive-Lab/Kannada-Instruct-dataset](https://huggingface.co/datasets/Cognitive-Lab/Kannada-Instruct-dataset) |
| **Training Samples** | 389,608 |
| **Base Parameters** | 0.6B |
| **Trainable Parameters** | 2.29M (0.38%) |

## Training Configuration

| Parameter | Value |
|-----------|-------|
| LoRA Rank | 16 |
| LoRA Alpha | 32 |
| Dropout | 0.05 |
| Target Modules | q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj |
| Learning Rate | 2e-4 |
| Batch Size | 2 (with gradient accumulation 8) |
| Epochs | 2 |

## Installation

```bash
pip install transformers peft torch accelerate
```

## Usage

```python
from peft import PeftModel
from transformers import AutoModelForCausalLM, AutoTokenizer

# Load base model and tokenizer
base_model = AutoModelForCausalLM.from_pretrained(
    "Qwen/Qwen3-0.6B",
    torch_dtype="auto",
    device_map="auto"
)
tokenizer = AutoTokenizer.from_pretrained("Mithun501/KannadaGPT-0.6B")

# Load LoRA adapter
model = PeftModel.from_pretrained(base_model, "Mithun501/KannadaGPT-0.6B")

# Generate text
messages = [
    {"role": "user", "content": "ಭಾರತದ ರಾಜಧಾನಿ ಯಾವುದು?"}
]

text = tokenizer.apply_chat_template(
    messages,
    tokenize=False,
    add_generation_prompt=True,
    enable_thinking=False
)

inputs = tokenizer(text, return_tensors="pt").to(model.device)
outputs = model.generate(**inputs, max_new_tokens=256, temperature=0.7, top_p=0.8)
response = tokenizer.decode(outputs[0], skip_special_tokens=True)
print(response)
```

## Example Prompts

| Kannada Prompt | English Translation |
|----------------|---------------------|
| ಭಾರತದ ರಾಜಧಾನಿ ಯಾವುದು? | What is the capital of India? |
| ಆರೋಗ್ಯವಾಗಿರಲು ಮೂರು ಸಲಹೆಗಳನ್ನು ನೀಡಿ | Give three tips for staying healthy |
| ಕನ್ನಡದಲ್ಲಿ ಕವಿತೆ ಬರೆಯಿರಿ | Write a poem in Kannada |
| ಬೆಂಗಳೂರಿನ ಬಗ್ಗೆ ಹೇಳಿ | Tell me about Bangalore |
| ಮಳೆ ಏಕೆ ಬರುತ್ತದೆ? | Why does it rain? |

## Training Progress

The model was trained on Kaggle with P100 GPU. Training metrics from checkpoint-4500:

| Step | Loss | Learning Rate |
|------|------|---------------|
| 50 | 1.459 | 6.7e-06 |
| 500 | 0.675 | 6.8e-05 |
| 1000 | 0.613 | 1.4e-04 |
| 1500 | 0.572 | 2.0e-04 |
| 2000 | 0.534 | 2.0e-04 |
| 2500 | 0.518 | 2.0e-04 |
| 3000 | 0.502 | 1.9e-04 |
| 3500 | 0.492 | 1.9e-04 |
| 4000 | 0.488 | 1.9e-04 |
| 4500 | 0.470 | 1.9e-04 |

**Training Progress**: 4,500 / 48,702 steps (9.2% complete, epoch 0.185/2.0)
<<<<<<< HEAD
=======

## Project Structure

```
KannadaGPT-0.6B/
├── adapter_config.json      # LoRA configuration
├── adapter_model.safetensors # LoRA weights (38MB)
├── tokenizer.json           # Tokenizer
├── tokenizer_config.json    # Tokenizer config
├── vocab.json               # Vocabulary
├── merges.txt               # BPE merges
├── special_tokens_map.json  # Special tokens
├── added_tokens.json        # Added tokens
├── chat_template.jinja      # Chat template
└── README.md                # This file
```
>>>>>>> b9fa282 (Update to checkpoint-4500 with improved training metrics)

## Limitations

- This is a LoRA adapter and requires the base model (Qwen3-0.6B) to run
- Training is partial (checkpoint-4500 of ~48,700 total steps, ~9.2% complete)
- Best suited for Kannada instruction-following tasks
- May generate incorrect or nonsensical responses for complex queries

## Future Work

- [ ] Complete full 2-epoch training
- [ ] Merge LoRA weights into base model for easier loading
- [ ] Evaluate on Kannada benchmarks
- [ ] Fine-tune larger models (1.8B, 7B)

## License

Apache 2.0

## Citation

```bibtex
@misc{kannadagpt-0.6b,
  author = {Mithun501},
  title = {KannadaGPT-0.6B: A Kannada Language Model},
  year = {2025},
  publisher = {GitHub},
  url = {https://github.com/mithun50/KannadaGPT-0.6B}
}
```

## Acknowledgments

- [Qwen Team](https://huggingface.co/Qwen) for the base model
- [Cognitive-Lab](https://huggingface.co/Cognitive-Lab) for the Kannada instruction dataset
- [Hugging Face](https://huggingface.co) for the PEFT library

## Author

**Mithun501** - [GitHub](https://github.com/mithun50) | [HuggingFace](https://huggingface.co/Mithun501)
