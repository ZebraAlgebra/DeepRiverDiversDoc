---
weight: 3
title: "Configs"
---

# Configs

Config parameters configures training process and model architectures,
as well as routine i/o setups.

## Full Parameters

{{< tabs "git clone" >}}
{{% tab "Folded" %}}
```json
{
  "$schema": "../schemas/model_config_schema.json",
  "general": {...},
  "training_process": {...},
  "action_encoder": {...},
  "card_encoder": {...},
  "cross_attention": {...},
  "output_mlp": {...}
}
```
{{% /tab %}}
{{% tab "Full Defaults" %}}
```json
{
  "$schema": "../schemas/model_config_schema.json",
  "general": {
    "device": "cpu",
    "seed": 42069,
    "load_checkpoint_name": "default",
    "save_checkpoint_name": "default",
    "type_of_checkpoint": "final"
  },
  "training_process": {
    "batch_size": 32,
    "num_epochs": 300,
    "learning_rate": 0.0005,
    "weight_decay": 0.01,
    "optimizer": "AdamW",
    "momentum": 0,
    "dataset": "GTO",
    "warm_start": false,
    "p_train_test_split": 0.2,
    "min_epochs": 100,
    "patience": 50
  },
  "action_encoder": {
   "d_model": 32,
    "nhead": 4,
    "dim_feedforward": 128,
    "dropout": 0.1,
    "num_actions": 5,
    "num_positions": 6,
    "num_streets": 5,
    "action_embedding_dim": 4,
    "position_embedding_dim": 4,
    "street_embedding_dim": 4
  },
  "card_encoder": {
    "d_model": 32,
    "nhead": 4,
    "dim_feedforward": 128,
    "dropout": 0.1,
    "num_ranks": 13,
    "num_suits": 4,
    "num_streets": 4,
    "rank_embedding_dim": 8,
    "suit_embedding_dim": 4,
    "street_embedding_dim": 4
  },
  "cross_attention": {
    "num_heads": 4,
    "d_model": 32
  },
  "output_mlp": {
    "d_model": 32
  }
}
```
{{% /tab %}}
{{% tab "Schema" %}}
```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "ModelConfig",
  "type": "object",
  "properties": {
    "general": {
      "type": "object",
      "properties": {
        "device": { "type": "string", "enum": ["cpu", "cuda"] },
        "seed": { "type": "integer" },
        "load_checkpoint_name": { "type": "string" },
        "save_checkpoint_name": { "type": "string" },
        "type_of_checkpoint": { "type": "string", "enum": ["best", "final"] }
      }
    },
    "training_process": {
      "type": "object",
      "properties": {
        "batch_size": { "type": "integer" },
        "num_epochs": { "type": "integer" },
        "learning_rate": { "type": "number" },
        "weight_decay": { "type": "number" },
        "optimizer": { "type": "string", "enum": ["Adam", "SGD", "AdamW"] },
        "momentum": { "type": "number" },
        "dataset": { "type": "string", "enum": ["GTO", "Human"] },
        "warm_start": { "type": "boolean" },
        "p_train_test_split": { "type": "number" },
        "min_epochs": { "type": "integer" },
        "patience": { "type": "integer" }
      }
    },
    "action_encoder": {
      "type": "object",
      "properties": {
        "d_model": { "type": "integer" },
        "nhead": { "type": "integer" },
        "dim_feedforward": { "type": "integer" },
        "dropout": { "type": "number" },
        "num_actions": { "type": "integer" },
        "num_positions": { "type": "integer" },
        "num_streets": { "type": "integer" },
        "action_embedding_dim": { "type": "integer" },
        "position_embedding_dim": { "type": "integer" },
        "street_embedding_dim": { "type": "integer" }
      }
    },
    "card_encoder": {
      "type": "object",
      "properties": {
        "d_model": { "type": "integer" },
        "nhead": { "type": "integer" },
        "dim_feedforward": { "type": "integer" },
        "dropout": { "type": "number" },
        "num_ranks": { "type": "integer" },
        "num_suits": { "type": "integer" },
        "num_streets": { "type": "integer" },
        "rank_embedding_dim": { "type": "integer" },
        "suit_embedding_dim": { "type": "integer" },
        "street_embedding_dim": { "type": "integer" }
      }
    },
    "cross_attention": {
      "type": "object",
      "properties": {
        "num_heads": { "type": "integer" },
        "d_model": { "type": "integer" }
      }
    },
    "output_mlp": {
      "type": "object",
      "properties": {
        "d_model": { "type": "integer" }
      }
    }
  }
}
```
{{% /tab %}}
{{< /tabs >}}

## Detailed Descriptions

At the top level, there are seven accepted keys:

- `general`:
Configures general parameters for training process.
- `training_process`:
Configures dataloaders, optimizers in the training process.
- `action_encoder`:
Configures the action encoder layer of the model used in the training process.
- `card_encoder`:
Configures the card encoder layer of the model used in the training process.
- `cross_attention`:
Configures the cross attention layer of the model used in the training process.
- `output_mlp`:
Configures the output MLP layer of the model used in the training process.
- `$schema` (optional): 
For users to utilize the `schemas/model_config_schema.json` file.
This key will not affect the training process.

### `general`
- `device`: `"cpu"` (default) or `"cuda"`, specify whether to use GPU for training.
- `seed`: type `int` (default `42069`), for setting random seed for reproducible results.
- `load_checkpoint_name`, `save_checkpoint_name`: type `str` (default `"default"`), 
specifies which subdirectory to save the best and final models, training histories, configs from the training process;
e.g. if `save_checkpoint_name="my_checkpoint"`, it would save the models to `checkpoints/my_checkpoint`.
If `warm_start` is set to `true`, and `load_checkpoint_name="my_checkpoint"`,
it would attempt to load previous training loss histories and models from `checkpoints/my_checkpoint/final.pth`.
- `type_of_checkpoint`: `"final"` (default) or `"best"`,
for setting which model to load within the subdirectory specified by `load_checkpoint_name`.

### `training_process`
- `batch_size`: type `int` (default `32`),
size of batch for dataloaders.
- `num_epochs`: type `int` (default `50`),
number of epochs for training process.
- `learning_rate`: type `number` (default `0.001`),
learning rate for optimizer.
- `weight_decay`: type `number` (default `0.01`),
weight decay for optimizer.
- `optimizer`: `"AdamW"` (default) or `"Adam", "SGD"`,
type of optimizer to use.
- `momentum`: type `number` (default `0`),
momentum for optimizer;
ignored for certain types of optimizers, check torch docs.
- `dataset`: `"GTO"` (default) or `"Human"`,
training dataset - either on game-theoretic optimal, or on real human data.
- `warm_start`: type `bool` (default `false`), 
continue training model based on previous trainings
(which will load checkpoint from supplied `general.load_checkpoint_name` key)
or fresh new.
- `p_train_test_split`: type `number` (default `0.2`),
test set size ratio, e.g. with 0.2, 
testing set has size 20% of the whole training data.
- `min_epoch`: type `int` (default `100`); see `patience`.
- `patience`: type `int` (default `50`), 
when validation loss does not decrease for a period of time over this parameter,
and current epoch number is not less than `min_epoch`,
then early-stop the training process.

### `action_encoder`

The action encoder is a sequential combination of:
1. an initial encoder combining different encoding utilities
for action sequences (direct encoding, one-hot encoding, n-way embeddings),
2. a fully-connected layer,
3. a transformer encoder layer 
using `torch.nn.TransformerEncoderLayer` for self-attention.

- `d_model`: type `int` (default `32`),
intermediate dimension for the fully-connected layer.
- `nhead`: type `int` (default `4`),
number of heads for the transformer encoder layer.
- `dim_feedforward`: type `int` (default `128`),
the dimension of the feedforward layer in the transformer encoder layer.
- `dropout`: type `number` (default `0.1`),
dropout rate for the drpoout layer in the transformer encoder layer.
- `num_actions`: type `int` (default `5`),
the number of possible actions to encode in the initial encoding layer.
- `num_positions`: type `int` (default `6`),
the number of possible positions to encode in the initial encoding layer.
- `num_streets`: type `int` (default `5`),
the number of possible streets to encode in the initial encoding layer.
- `action_embedding_dim`: type `int` (default `4`),
the dimension for the action encoding in the initial encoding layer.
- `position_embedding_dim`: type `int` (default `4`),
the dimension for the position encoding in the initial encoding layer.
- `street_embedding_dim`: type `int` (default `4`)
the dimension for the street encoding in the initial encoding layer.

### `card_encoder`

The card sequence encoder is a sequential combination of:
1. an initial encoder combining different encoding utilities
for action sequences (direct encoding, one-hot encoding, n-way embeddings),
2. a fully-connected layer,
3. a transformer encoder layer 
using `torch.nn.TransformerEncoderLayer` for self-attention.

- `d_model`: type `int` (default `32`), 
intermediate dimension for the fully-connected layer.
- `nhead`: type `int` (default `4`),
number of heads for the transformer encoder layer.
- `dim_feedforward`: type `int` (default `128`),
the dimension of the feedforward layer in the transformer encoder layer.
- `dropout`: type `number` (default `0.1`),
dropout rate for the drpoout layer in the transformer encoder layer.
- `num_ranks`: type `int` (default `13`),
the number of possible ranks to encode in the initial encoding layer.
- `num_suits`: type `int` (default `4`),
the number of possible suits to encode in the initial encoding layer.
- `num_streets`: type `int` (default `4`),
the number of possible streets to encode in the initial encoding layer.
- `rank_embedding_dim`: type `int` (default `8`),
the dimension for the rank encoding in the initial encoding layer.
- `suit_embedding_dim`: type `int` (default `4`),
the dimension for the suit encoding in the initial encoding layer.
- `street_embedding_dim`: type `int` (default `4`) 
the dimension for the street encoding in the initial encoding layer.

### `cross_attention`

The cross attention layer 
uses `torch.nn.MultiheadAttention`,
with bidirectional attention mechanisms
(actions attending to cards,
as well as cards attending to actions).

- `num_heads`: type `int` (default `4`),
number of heads to use in multihead attention.
- `d_model`: type `int` (default `32)`
intermediate dimension for multihead attention.

### `output_mlp`

The final output MLP is a sequential combinations
of three layers of fully-connected (FC) layers,
with ReLU and dropout (in order) applied to the 2nd and 3rd FC layers.

- `d_model`: type `int` (default `3`),
designates hidden dimensions of these FC layers
(the middle one has `2 * d_model`, the final one has `d_model` dim).

### `$schema` (Optional)

Path to a schema json file for writing the schema.
