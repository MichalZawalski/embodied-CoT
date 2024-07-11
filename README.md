# Robotic Control via Embodied Chain-of-Thought Reasoning

[![arXiv](https://img.shields.io/badge/arXiv-2406.09246-df2a2a.svg?style=for-the-badge)](https://TODO)
[![HF Models](https://img.shields.io/badge/%F0%9F%A4%97-Models-yellow?style=for-the-badge)](https://huggingface.co/Embodied-CoT)
[![Python](https://img.shields.io/badge/python-3.10-blue?style=for-the-badge)](https://www.python.org)
[![License](https://img.shields.io/github/license/TRI-ML/prismatic-vlms?style=for-the-badge)](LICENSE)

[Michał Zawalski](https://michalzawalski.github.io/), [William Chen](https://verityw.github.io/), [Karl Pertsch](https://kpertsch.github.io/), [Oier Mees](https://www.oiermees.com/),  [Chelsea Finn](https://ai.stanford.edu/~cbfinn/), [Sergey Levine](https://people.eecs.berkeley.edu/~svlevine/)
<hr style="border: 2px solid gray;"></hr>

We present Embodied Chain-of-Thought Reasoning (ECoT): a novel approach for training robotic policies.
We train a vision-language-action model to generate reasoning steps in response to instructions and images before
choosing a robot action, enabling better performance, interpretability, and generalization.

Our codebase is built on top of [OpenVLA](https://github.com/openvla/openvla). We refer to it for the detailed
documentation of the code and dependencies.

![](media/ecot_teaser.jpg)

## Getting Started

To train the models, from scratch use the following command:

```bash
torchrun --standalone --nnodes 1 --nproc-per-node 8 vla-scripts/train.py  \
  --vla.type "prism-dinosiglip-224px+mx-bridge"  \
  --data_root_dir <path to training data root>  \
  --run_root_dir <path to checkpoint saving directory>  \
  --wandb_project <wandb project name>  \
  --wandb_entity <wandb user name>
```

To evaluate the model on the WidowX robot,

```bash
python3 experiments/bridge/eval_model_in_bridge_env.py
  --model.type prism-dinosiglip-224px+7b
  --pretrained_checkpoint <path to checkpoint>
  --host_ip <robot interface IP>
  --port <robot interface port>
```

## Pretrained models

We release two ECoT models trained as part of our work, and the dataset of reasonings, available [on our
HuggingFace page](https://huggingface.co/Embodied-CoT):
- [`ecot-openvla-7b-bridge`](https://huggingface.co/Embodied-CoT/ecot-openvla-7b-bridge): The main model that we used
for most of our experiments. It was trained on the Bridge dataset annotated with the reasoning for 80k steps.
- [`ecot-openvla-7b-oxe`](https://huggingface.co/Embodied-CoT/ecot-openvla-7b-oxe): A policy that was initially trained
on the Open-X-Embodiment dataset actions, fine-tuned on the mixture of OXE action-only data and our reasonings for
Bridge for another 20k steps.
- [`embodied_features_bridge`](https://huggingface.co/Embodied-CoT/embodied_features_bridge): A dataset of the embodied
features and reasonings collected for Bridge demonstrations.

**Explicit Notes on Model Licensing & Commercial Use**: While all code in this repository is released under an MIT
License, our pretrained models may inherit restrictions from the underlying base models we use. Specifically, both the
above models are derived from Llama-2, and as such are subject to the
[Llama Community License](https://ai.meta.com/llama/license/).

---

## Installation

See the original [OpenVLA](https://github.com/openvla/openvla) repository for detailed installation instructions.

## Repository Structure

High-level overview of repository/project file-tree:

+ `prismatic` - Package source; provides core utilities for model loading, training, data preprocessing, etc.
+ `experiments` - Code for evaluating the policies on a WidowX robot.
+ `vla-scripts/` - Core scripts for training, fine-tuning, and deploying VLAs.
+ `LICENSE` - All code is made available under the MIT License; happy hacking!
+ `Makefile` - Top-level Makefile (by default, supports linting - checking & auto-fix); extend as needed.
+ `pyproject.toml` - Full project configuration details (including dependencies), as well as tool configurations.
+ `README.md` - You are here!

---

#### Citation

If you find our code or models useful in your work, please cite [our paper](https://embodied-cot.github.io/static/paper.pdf):

```bibtex
@article{Zawalski24-ecot,
    title={Robotic Control via Embodied Chain-of-Thought Reasoning},
    author={Zawalski, Michał and Chen, William and Pertsch, Karl and Mees, Oier and Finn, Chelsea and Levine, Sergey},
    year={2024}
}
```
