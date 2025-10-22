---
language: 
- en
license: mit
---

# HARDMath2 Evaluator

This repository contains a comprehensive query and evaluation framework to automatically query state-of-the-art Large Language Models (LLMs) against the HARDMath2 benchmark and parse and evaluate their mathematical responses using a custom LaTeX to SymPy parser.

## Overview

The HARDMath2 Evaluator is designed to systematically test LLMs on challenging mathematical problems, providing automated evaluation of their mathematical reasoning capabilities. The framework includes:

- **Automated LLM Querying**: Support for multiple LLM providers (OpenAI, Anthropic, Google, DeepSeek)
- **Custom LaTeX Parser**: Robust parser that converts LaTeX mathematical expressions to SymPy for evaluation
- **Comprehensive Evaluation**: Automated comparison of model responses against reference solutions
- **Statistical Analysis**: Detailed reporting and visualization of evaluation results

## Project Structure

```
hardmath2_evaluation/
├── src/hm2eval/                    # Core evaluation package
│   ├── __init__.py                 # Package exports
│   ├── evaluator.py                # Main evaluation logic
│   ├── parser.py                   # LaTeX to SymPy conversion
│   ├── parser_rules.py             # Parser transformation rules
│   ├── query.py                    # LLM querying functionality
│   └── config/
│       └── model_config.json       # Supported model configurations
├── scripts/                        # Execution scripts
│   ├── evaluate.py                 # Run evaluations on query results
│   ├── query_models.py             # Query LLMs with benchmark problems
│   ├── summarize_eval.py           # Generate evaluation summaries and plots
│   └── config/                     # Configuration files
│       ├── eval_config.yaml        # Evaluation settings
│       ├── query_config.yaml       # Query settings
│       └── summary_config.yaml     # Summary generation settings
├── pyproject.toml                  # Project dependencies and metadata
└── README.md                       # This file
```

## Installation

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd hardmath2_evaluation
   ```

2. **Install dependencies**:
   ```bash
   pip install -e .
   ```

3. **Set up API keys** (create a `.env` file):
   ```bash
   OPENAI_API_KEY=your_openai_key
   ANTHROPIC_API_KEY=your_anthropic_key
   GOOGLE_API_KEY=your_google_key
   DEEPSEEK_API_KEY=your_deepseek_key
   ```

## Usage

### 1. Query LLMs

Configure which models to test in `scripts/config/query_config.yaml`:

```yaml
models:
  GPT-4o: True
  Claude 3.5 Sonnet: True
  Gemini 2.0 Flash: True
  # Set to False to skip a model
```

Run queries:
```bash
cd scripts
python query_models.py
```

Optional parameters:
- `--model_idx N`: Test only the Nth model from config
- `--prompt_idx N`: Test only the Nth prompt from the dataset

### 2. Evaluate Responses

Configure evaluation settings in `scripts/config/eval_config.yaml`:

```yaml
models:
  GPT-4o: latest  # Use latest query results
  # or specify exact timestamp: "1234567890"
query_results_dir: "query_results"
eval_results_dir: "eval_results"
```

Run evaluation:
```bash
cd scripts
python evaluate.py
```

### 3. Generate Summaries and Visualizations

Configure summary settings in `scripts/config/summary_config.yaml`:

```yaml
eval_results_dir: "eval_results"
eval_directory: latest  # Use latest evaluation results
```

Generate summaries:
```bash
cd scripts
python summarize_eval.py
```

This creates:
- Statistical summaries (`eval_stats_summary.yaml`, `parser_stats_summary.yaml`)
- Visualization plots (success rates, problem type distributions, etc.)

## Core Components

### LaTeX Parser

The custom parser (`src/hm2eval/parser.py`) handles:

- **LaTeX Functions**: `\frac`, `\sqrt`, `\sin`, `\cos`, etc.
- **Mathematical Notation**: Superscripts, subscripts, integrals, sums
- **Special Functions**: Gamma, error functions, Airy functions
- **Unicode Conversion**: Handles various Unicode mathematical symbols
- **Error Handling**: Robust parsing with detailed error reporting

### Supported Models

The framework supports models from multiple providers:

- **OpenAI**
- **Anthropic**
- **Google**
- **DeepSeek**

### Evaluation Metrics

The evaluation provides:

- **Parser Success Rate**: Percentage of responses successfully parsed
- **Pass@1 Rate**: Percentage of responses equivalent to reference solutions
- **Per-Problem-Type Analysis**: Success rates broken down by mathematical problem types
- **Detailed Error Reporting**: Comprehensive error analysis for failed evaluations

## Configuration

### Model Configuration

Edit `src/hm2eval/config/model_config.json` to:
- Add new model providers
- Configure model-specific parameters
- Set system instructions for consistent response formatting

### Query Configuration

The `query_config.yaml` controls:
- Which models to test
- Dataset source (HuggingFace dataset name)
- Output directories
- Query parameters

### Evaluation Configuration

The `eval_config.yaml` controls:
- Which query results to evaluate
- Evaluation parameters
- Output formatting

## Dataset

This code is designed to be used with the HARDMath2 dataset available on HuggingFace (`JVRoggeveen/HARDMath2`), which contains 211 problems in graduate applied math, sorted into the following categories:
- Boundary Layers
- Asymptotic Series
- Integrals
- Nonlinear PDEs
- WKB
The dataset contains the correct solution and parameter strings compatible with this parser.

For more details about the dataset and benchmark, see the paper:
**HARDMath2: A Benchmark for Applied Mathematics Built by Students as Part of a Graduate Class**  
[James V. Roggeveen, Erik Y. Wang, et al. (2025)](https://arxiv.org/abs/2505.11774)

## Output Files

### Query Results
- `query_results/{model_name}_{timestamp}.json`: Raw LLM responses

### Evaluation Results
- `eval_results/{timestamp}/success_results.json`: Successfully evaluated responses
- `eval_results/{timestamp}/failed_results.json`: Failed evaluations with error details

### Summary Reports
- `eval_stats_summary.yaml`: Overall success rates and statistics
- `parser_stats_summary.yaml`: Parser performance metrics
- Various `.png` files: Visualization plots

## Citation

If you use this evaluation framework or the HARDMath2 dataset in your research, please cite the original paper:

```bibtex
@inproceedings{roggeveen2025hardmath2,
  title={HARDMath2: A Benchmark for Applied Mathematics Built by Students as Part of a Graduate Class},
  author={Roggeveen, James V. and Wang, Erik Y. and Flintoft, Will and Donets, Peter and Nathwani, Lucy S. and Gutierrez, Nickholas and Ettel, David and Graf, Anton Marius and Dandavate, Siddharth and Nageswaran, Arjun and Ward, Raglan and Williamson, Ava and Mykland, Anne and Migacz, Kacper K. and Wang, Yijun and Bostan, Egemen and Nguyen, Duy Thuc and He, Zhe and Descoteaux, Marc L. and Yeung, Felix and Liu, Shida and Garc{\'\i}a Ponce, Jorge and Zhu, Luke and Chen, Yuyang and Ivshina, Ekaterina S. and Fernandez, Miguel and Kim, Minjae and Gumbs, Kennan and Tan, Matthew Scott and Yang, Russell and Hoang, Mai and Brown, David and Silveira, Isabella A. and Sykes, Lavon and Roman, Ahmed and Fredenberg, William and Chen, Yiming and Martin, Lucas and Tang, Yixing and Smith, Kelly Werker and Liao, Hongyu and Wilson, Logan G. and Cai, Alexander Dazhen and Biju, Andrea Elizabeth and Brenner, Michael P.},
  booktitle={Advances in Neural Information Processing Systems (NeurIPS)},
  year={2025}
}
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.
