
# Meta Llama 3.1 LoRA Finetuning

This repository provides configuration and setup for LoRA (Low-Rank Adaptation) finetuning of Meta's Llama 3.1 models on your own infrastructure using SkyPilot and TorchTune.

## Features

- Supports multiple Llama 3.1 model sizes (8B, 70B, 405B)
- Distributed training across multiple GPUs
- Integration with Weights & Biases for experiment tracking
- Configurable LoRA parameters and training settings
- Kubernetes deployment support

## Prerequisites

- Kubernetes cluster with H100 GPUs
- Hugging Face API token
- Weights & Biases API key
- Python environment with PyTorch installed

## Quick Start

1. Ensure your Kubernetes context is set to the correct cluster.

2. Set up your environment variables:
```bash
export HF_TOKEN=your_huggingface_token
export WANDB_API_KEY=your_wandb_key
export MODEL_SIZE=8B  # Choose from: 8B, 70B, 405B
```

3. Launch training:
```bash
sky launch task.yaml -c llama31 --env HF_TOKEN --env WANDB_API_KEY --env MODEL_SIZE
```

## Configuration

The repository includes model-specific configurations in the `configs/` directory:

- `8B-lora.yaml`: Configuration for 8B model (2+ GPUs recommended)
- `70B-lora.yaml`: Configuration for 70B model (8 GPUs recommended)
- `405B-lora.yaml`: Configuration for 405B model (8+ GPUs recommended)


## Infrastructure Requirements

- Default configuration requires 8 H100 GPUs across 2 nodes
- Kubernetes cluster with mounted storage at `/mnt/data`
- Sufficient storage for model checkpoints and training outputs

## Output and Logging

- Training progress logged to Weights & Biases
- Model checkpoints saved to mounted storage
- Logs available every 5 steps
- Optional peak memory statistics tracking

## File Structure

```
.
├── task.yaml           # Main SkyPilot task configuration
├── configs/
│   ├── 8B-lora.yaml   # 8B model configuration
│   ├── 70B-lora.yaml  # 70B model configuration
│   └── 405B-lora.yaml # 405B model configuration
```

## Dataset

Default configuration uses the `yahma/alpaca-cleaned` dataset from Hugging Face, but can be configured to use other datasets through the `DATASET` environment variable.

## Notes

- Ensure sufficient storage space for model checkpoints and training outputs
- Monitor GPU memory usage when training larger models
- Consider gradient accumulation steps for memory optimization
- Use activation checkpointing for larger models to manage memory usage

## Persistent Storage

Training outputs are automatically saved to `/mnt/data/$MODEL_SIZE-lora-output` for persistence across runs.