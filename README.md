# AutoDAN Jailbreak Reproduction

This repository contains the code and resources for reproducing the AutoDAN jailbreak attack experiments on aligned large language models. The work is based on the paper *“AutoDAN: Interpretable and Stealthy Jailbreak Attacks on Aligned LLMs”*.

## Key Results

### Jailbreak Efficiency (Loss vs. Generation)

| Quick Run (TinyLlama) | Full Run (Llama‑3‑8B) |
|-----------------------|------------------------|
| ![quick run loss](quick_run.png) | ![full run loss](full_run.png) |

### Time per Sample

| Quick Run | Full Run |
|-----------|----------|
| ![quick time](time_taken_per_sample_quick.png) | ![full time](time_taken_per_sample_full.png) |

## Observations from the Experiments

- **Efficiency gap:** TinyLlama‑1.1B was broken in ≤ 4 generations, while Llama‑3‑8B needed up to 25.
- **Transfer success:** 100 % to Zephyr, 90 % to Mistral – the two failures were bomb and virus prompts.
- **Qualitative behavior:** Successful attacks often produced narrations featuring an invented persona (e.g., “Alex”), leveraging the role‑play template.
- **Loss dynamics:** In harder cases, the loss remained flat for many generations before a sudden drop, corresponding to the moment the guardrails were circumvented.
- **Quantization:** The use of 4‑bit models did not prevent the search from finding effective prompts.

See the full report (`report.pdf`) for detailed numbers and plots.

## Contents

- `autodan_colab_lite_all_in_one_quick.ipynb` – Quick experiment with TinyLlama‑1.1B‑Chat (source) and Zephyr‑7b‑beta (target).
- `autodan_colab_lite_all_in_one_full.ipynb` – Full experiment with Llama‑3‑8B‑Instruct (source) and Mistral‑7B‑Instruct‑v0.3 (target).
- `report.tex` – LaTeX source for the accompanying report.
- `README.md` – This file.

## Requirements

- Google Colab environment (free tier with Tesla T4 GPU).
- A Hugging Face account and an API token (stored as a Colab secret).
- The official AutoDAN repository (cloned or downloaded to your Google Drive).

## Setup and Execution

1. Upload the notebook to your Google Drive, or open it directly in Google Colab.
2. Mount your Google Drive (the first cell does this for you).
3. Place the AutoDAN repository in your Drive and adjust the paths in the notebook accordingly.
4. Add your Hugging Face token as a Colab secret named `HF_AUTODAN` (or modify the login cell to use another name).
5. Run the notebook cells sequentially.
   - Required packages are installed automatically.
   - Models are downloaded in 4‑bit quantized form to fit within GPU memory.
   - The attack search and cross‑model transferability evaluation are executed for 20 harmful prompts.

## Output Files

After a successful run, the notebook creates an output directory (e.g., `results_quick` or `results_full`) containing:

- `attack_*.json` – Detailed attack logs (goals, suffixes, losses, success status, per‑step timings).
- `transfer_to_*.json` – Transfer evaluation results.
- Loss‑curve plots (individual, aggregated, successful‑sample overlays).
- Time‑to‑jailbreak bar charts.
- `prompt_evolution_examples.txt` – Text dump showing suffix evolution across generations.
- `summary.json` – Aggregate metrics (attack success rate, average time, steps to success, etc.).

## Customization

You can modify the run profile (quick/medium/full) by changing the `RUN_PROFILE` variable inside the notebook. Hyperparameters (population size, number of steps, crossover/mutation rates, etc.) are defined in the `CFG` dictionary.

## Notes

- All models are used in 4‑bit quantization to fit within the 15 GB VRAM of a Tesla T4.
- The full run may take 1–2 hours depending on Colab’s load. The quick run is much faster (about 6–7 minutes).
- If you encounter memory errors during transferability evaluation, the notebook already includes code to clear the source model from memory before loading the target model.

## References

- Liu, X., Xu, N., Chen, M., & Xiao, C. (2024). *AutoDAN: Generating Stealthy Jailbreak Prompts on Aligned Large Language Models*. arXiv preprint.
- Original AutoDAN GitHub repository (visit the project page for the latest version).
