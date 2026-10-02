# Autonomous LLM Research Agent

**Student Name:** Rajkumar  
**Roll Number:** 2613037  
**Project:** autoresearch

## 1. Agent Built

I built an autonomous research agent for experimenting with language-model pretraining on a single NVIDIA GPU. The agent operates within a compact GPT-style training project and is guided by a set of research instructions in `program.md`.

The agent is designed to behave like an automated machine-learning researcher: it proposes changes to the training program, runs controlled experiments, measures the results, and keeps only changes that improve the validation metric.

## 2. Need for the Agent

Improving a language model normally requires repeated manual work from a machine-learning engineer. Each experiment may involve changing the model architecture, optimizer, learning rate, batch size, or training loop; running training; examining metrics; and deciding whether the change should be retained.

This process is time-consuming and difficult to perform continuously, especially when experiments must be repeated many times on limited computing resources. The agent addresses this need by automating the experiment cycle and allowing research to continue with minimal human supervision.

The project is particularly useful for single-GPU experimentation because every trial uses a fixed training-time budget of approximately five minutes. This makes experiments comparable and supports rapid iteration.

## 3. What the Agent Does

The agent follows this workflow:

1. Reads the experiment instructions and understands the current repository state.
2. Establishes a baseline by running the unmodified training program.
3. Selects an experimental idea, such as changing a model hyperparameter, optimizer setting, architecture component, batch size, or training-loop behavior.
4. Modifies `train.py`, which contains the model, optimizer, and training loop.
5. Commits the experiment so that the change can be compared or reverted.
6. Runs the training job for the fixed time budget.
7. Extracts the validation bits-per-byte metric (`val_bpb`) and resource measurements such as peak GPU memory.
8. Records the experiment result in a tab-separated results file.
9. Keeps the change when the validation metric improves; otherwise, reverts the change and tries another idea.
10. Diagnoses simple crashes, such as coding errors or missing imports, and retries when appropriate.

## 4. Inputs and Outputs

### Inputs

- The existing model and training code in `train.py`
- Fixed data preparation and evaluation utilities in `prepare.py`
- Experiment instructions in `program.md`
- Prepared training data and tokenizer files
- Available single-GPU compute resources

### Outputs

- Improved versions of the training program
- Experiment commits and reproducible code changes
- Validation `val_bpb` measurements
- Peak GPU-memory and training-performance measurements
- A results table containing the status and description of each experiment

## 5. Evaluation and Benefits

The main objective is to minimize validation bits-per-byte (`val_bpb`) within the fixed training-time budget. Lower values indicate better validation performance. The agent also tracks memory usage, number of training steps, token throughput, and model size.

The main benefits are:

- Reduces repetitive manual experimentation.
- Enables many experiments to run automatically.
- Makes trials reproducible through Git commits and structured result logging.
- Uses a consistent time budget for fair comparison.
- Allows researchers to explore model and optimization ideas on a single GPU.
- Automatically rejects changes that do not improve the measured result.

## 6. Limitations

The agent depends on access to a compatible NVIDIA GPU and prepared training data. Its experiments are limited by the fixed time budget and by the quality of the ideas it generates. Automated changes also require monitoring to avoid inefficient, unstable, or impractical configurations. The evaluation metric is focused on validation performance and does not by itself guarantee broader model quality.

## Conclusion

The project demonstrates how an AI agent can automate a practical machine-learning research loop. By combining code modification, short training experiments, metric extraction, result logging, and automatic keep-or-revert decisions, the agent turns language-model optimization into a repeatable autonomous process suitable for single-GPU research.
