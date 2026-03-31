# Awesome MLPatron Presets

Ready-to-run ML training presets for [MLPatron](https://mlpatron.com). Pick one below, or use them as a reference to build your own.

**Want to run your own code?** Your repo needs an `MLproject` file, a Docker image, and MLflow integration. The [mlpatron-demo README](https://github.com/mlpatron/mlpatron-demo/blob/main/README.md) walks through the full setup step by step — start there.

Each preset below links to a completed baseline run with full metrics and training logs. If you're building on top of one, use the existing baseline — no need to re-run it yourself.

---

## demo

[github.com/mlpatron/mlpatron-demo](https://github.com/mlpatron/mlpatron-demo)

The "hello world" of MLPatron — an MNIST digit classifier (PyTorch) that lets you try the full workflow end-to-end for a few cents.

| Entry Point | GPU | Time | Run |
|-------------|-----|------|-----|
| `main` | L4 | ~5 min | [baseline](https://mlpatron.com/jobs/3ec8bc66-666d-453b-85b2-6505a59fe07b) |

---

## nanochat

[github.com/mlpatron/mlpatron-nanochat](https://github.com/mlpatron/mlpatron-nanochat)

Fork of [karpathy/nanochat](https://github.com/karpathy/nanochat) — "the simplest experimental harness for training LLMs." Pretrains GPT-2 family models on FineWeb-Edu.

| Entry Point | Model | GPU | Steps | Time | Run |
|-------------|-------|-----|-------|------|-----|
| `a100_d10` | 196M params, 10 layers | A100 40GB | 1500 | ~62 min | [baseline](https://mlpatron.com/jobs/3a8ac67b-152a-4315-9be3-c50bf0752e64) |
| `h100_d12` | 135M params, 12 layers (reference) | H100 80GB | 2205 | ~47 min | [baseline](https://mlpatron.com/jobs/d4841e1c-a62e-4bfa-87c0-120a5686a5b6) |

Both presets train to convergence using [Chinchilla-optimal](https://arxiv.org/abs/2203.15556) compute budgets — the number of training tokens is matched to model size so that neither data nor parameters are wasted. All hyperparameters (learning rate, batch size, warmup schedule, etc.) are auto-computed from `depth`; you don't need to tune anything.

d12 is the nanochat community's standard baseline — results are directly comparable across setups.

### Post-training (d12)

| Entry Point | Stage | GPU | Time | Run |
|-------------|-------|-----|------|-----|
| `h100_d12_sft` | SFT (conversation, tool use, math) | H100 80GB | ~2 min | [baseline](https://mlpatron.com/jobs/d99d7573-317e-4ea6-bdc6-f7546f6f9425) |

SFT teaches the base model conversation format, tool use (calculator), multiple-choice, and math reasoning. It uses the upstream default data mixture (SmolTalk + MMLU×3 + GSM8K×4 + Identity + Spelling = ~1.07M rows, 1 epoch).

Each post-training preset downloads its prerequisite checkpoint from MLflow artifacts automatically — no manual setup needed. The `pretrain_mlflow_run_id` / `sft_mlflow_run_id` parameter defaults to the baseline checkpoint above.

---

## Contributing

Have a validated preset? Open an issue or PR. Requirements: public repo, at least one completed run on MLPatron, documented results.
