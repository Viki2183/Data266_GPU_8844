# Task 1 Results

## Configuration

- Dataset: roneneldan/TinyStories
- Seed: 8844
- Context length: 128
- Training sequences: 100000
- Validation sequences: 10000
- Epochs: 10
- Batch size: 64
- Embedding dimension: 256
- Transformer layers: 4
- Attention heads: 4
- Dropout: 0.1
- Learning rate: 0.0003
- Gradient clipping norm: 1.0
- Parameters: 3248640

## Dataset

The model uses a character-level TinyStories dataset.

The vocabulary was built from the training split only. Validation characters not found in the training vocabulary were mapped to the explicit `<UNK>` token.

The required 100K/10K interpretation is:

- 100,000 training sequences
- 10,000 validation sequences
- 128 input characters per sequence
- 129 characters total per example including the shifted target

## Training Results

| Metric | Value |
|---|---:|
| Training CE, dropout ON | 0.9062144078662284 |
| Training CE, dropout OFF | 0.8433332967150743 |
| Training perplexity, dropout OFF | 2.324100997948406 |
| Training BPC, dropout OFF | 1.2166727649873785 |
| Training top-1 accuracy, dropout OFF | 0.73213515625 |
| Validation CE | 0.8727689301891691 |
| Validation perplexity | 2.393529201688915 |
| Validation BPC | 1.2591394074258802 |
| Validation top-1 accuracy | 0.72531796875 |
| Generalization gap, dropout OFF | 0.029435633474094836 |
| Generalization gap, dropout ON | -0.0334454776770593 |

The dropout-off generalization gap is the preferred comparison because both training and validation metrics are evaluated with dropout disabled.

## Generation Results

Generation metrics were computed on the continuation only, excluding the prompt.

| Temperature | Distinct-1 Char | Distinct-2 Char | Distinct-3 Char | Repeated 4-gram Char |
|---:|---:|---:|---:|---:|
| 0.0 | 0.13024999999999998 | 0.38819095477386933 | 0.5222222222222223 | 0.4289340101522843 |
| 0.5 | 0.1426 | 0.5234673366834172 | 0.7501010101010104 | 0.15385786802030454 |
| 0.8 | 0.14745 | 0.5615075376884422 | 0.80020202020202 | 0.10903553299492386 |
| 1.0 | 0.15004999999999996 | 0.5751758793969849 | 0.8221212121212119 | 0.08908629441624365 |

## Resource Usage

- GPU: NVIDIA GeForce RTX 4090
- Device: cuda
- PyTorch: 2.14.0+cu130
- CUDA: 13.0
- Training time: 182.5694510936737 seconds
- Training tokens per second: 701103.0554850333
- Peak allocated memory: 1.4048843383789062 GB
- Peak reserved memory: 1.55078125 GB
- Generation speed: 274.0689248326798 characters per second

## Training Stability

- Mean gradient norm: 0.3684396242912351
- Maximum gradient norm: 1.7694355249404907
- Gradient norm standard deviation: 0.10082454792095322
- Loss spikes: 0.0
- Non-finite values: 0.0

## Checkpoint Mapping

- Reported checkpoint: `final_model.pt`
- Best checkpoint: `best_model.pt`
- Final checkpoint: `final_model.pt`
- Best-checkpoint selection rule: lowest validation loss

## Saved Artifacts

- `checkpoints/best_model.pt`
- `checkpoints/final_model.pt`
- `outputs/metrics_report.csv`
- `outputs/final_epoch_metrics.csv`
- `outputs/final_training_loss.png`
- `outputs/loss_curves.png`
- `outputs/gradient_norm_and_learning_rate.png`
- `outputs/final_generation_samples.json`
- `outputs/final_generation_metrics.json`
- `logs/train_raw.log`
