# Ortho-LoRA Toolbox

The Ortho-LoRA Toolbox is a collection of utility functions designed to enhance the working efficiency of LoRA (Low-Rank Adaptation) models for Stable-Diffusion. LoRA is a technique that utilizes low-rank approximation to adapt pretrained models to new tasks effectively.

## Key Features

- LoRA Extraction: The toolbox provides methods to extract LoRA from a base model and a tuned model without instantiating the model. This allows for a fast extraction process, typically completing in less than 4 seconds.

- LoRA Merge: It also offers functionality to merge LoRA back into a model, enabling the integration of adapted knowledge without the need for extensive model instantiation.

- Orthogonalization: The Ortho-LoRA Toolbox introduces a novel approach called orthogonalization of LoRAs. It addresses the challenge of aligning multiple LoRAs in similar directions. By minimizing multipliers and reducing the mutual influence between aligned LoRAs, orthogonalization optimizes the weights to preserve principal directions in high-dimensional space.

## Orthogonalization Algorithm

The main algorithm for orthogonalization involves the following steps:

1. Determine the principal direction that minimizes the error between the concatenated LoRAs, denoted as $dW_{k,i}$, and their corresponding weights, represented by $v_i \lambda_k$. Here, $i$ iterates over the flattened weights of the LoRA for a specific layer, and $\lambda$ represents the scaling factor for a given LoRA.

2. Subtract the principal direction from the LoRA weights.

3. Identify a new direction (secondary) that is relatively orthogonal to the principal direction.

By performing orthogonalization using this algorithm, the Ortho-LoRA Toolbox ensures that the multipliers used in the LoRAs have less mutual influence, enhancing the effectiveness of the adaptation process. This process can be applied to 2 or more LoRAs.

## Advantages over Other Implementations

- No Model Instantiation: Unlike other implementations, such as [https://github.com/bmaltais/kohya_ss](https://github.com/bmaltais/kohya_ss), the Ortho-LoRA Toolbox avoids the need for model instantiation. It operates directly with the state-dict, resulting in faster extraction, saving valuable time and computational resources.

## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [Examples](#examples)
- [Contributing](#contributing)
- [License](#license)

## Installation

To use the LoRA Toolbox, you need to have Python 3.10 or higher installed. You can install the toolbox using pip:

TODO !

## Usage

The LoRA Toolbox provides several functions for extracting LoRA parameters, merging LoRA parameters into a base model, and performing orthogonalization of LoRA parameters. Here's an overview of the main functions:

- `extract_lora`: Extract LoRA parameters from a base model and a tuned model.
- `merge_lora`: Merge LoRA parameters into a base model.
- `ortho_lora`: Perform orthogonalization of multiple LoRAs.

## Examples

### Extract LoRA Parameters

```python
from ortho_lora import extract_lora

base_model_path = "base_model.safetensors"
tuned_model_path = "tuned_model.safetensors"

lora_params = extract_lora(
    base_model_path, tuned_model_path,
    dim=0.08,
    min_dim=8, max_dim=96,
    save_path='./LoRA')

```

This example demonstrates how to extract LoRA parameters from a base model and a tuned model. The `extract_lora` function takes the paths to the base model and tuned model files as input. Additional parameters such as `dim`, `min_dim`, and `max_dim` control the dimensionality of the extracted LoRA parameters. The extracted parameters are saved in the specified `save_path`.

### Merge LoRA Parameters

```python
from ortho_lora import merge_lora

base_model_path = "base_model.safetensors"
lora_params_path = "lora_params.safetensors"

merged_model = merge_lora(
    base_model_path, lora_params_path,
    multiplier=1, save_path='./models')

```

In this example, we demonstrate how to merge LoRA parameters into a base model. The `merge_lora` function takes the base model path and the path to the LoRA parameters file as input. The `multiplier` parameter controls the scaling of the LoRA parameters during the merge process. The merged model is saved in the specified `save_path`.

### Perform Orthogonalization of LoRA Parameters

```python
from ortho_lora import ortho_lora

loras_paths = ["lora1.safetensors", "lora2.safetensors", "lora3.safetensors"]

lora1, lora2 = ortho_lora(loras_paths, save_path="./LoRA")
```

This example demonstrates how to perform orthogonalization on multiple LoRA parameters. The `ortho_lora` function takes a list of paths to the LoRA parameter files as input. After orthogonalization, the resulting LoRA parameters are saved in the specified `save_path`. In this example, the resulting orthogonalized parameters are assigned to `lora1` and `lora2`.

Feel free to modify these examples based on your specific use cases, and refer to the documentation for more detailed information on the available parameters and their usage.

By improving the examples section, users will have a clearer understanding of how to use the Ortho-LoRA Toolbox in different scenarios.

## Contributing

Contributions to the ortho-LoRA are welcome! If you find a bug, have a feature request, or want to contribute code, please open an issue or submit a pull request.

## License

The ortho-LoRA is licensed under the [MIT License](LICENSE).
