---
weight: 2
title: "Commands"
---

# Commands

{{% hint info %}}
For what follows, 
it is assumed that the commands are ran from the root of the project.
{{% /hint %}}

## `trainer.py`

This script is used to run trainer. The command:

```bash
python3 trainer.py
```

runs the trainer with default configs.
The default configs are given in `configs/default.json`;
currently, it should be the consistent with `schemas/model_config.json`.

`trainer.py` can take in an additional parameter for running specified config file:

```bash
python3 trainer.py --config my_config.json
```

which would read in `configs/my_config.json`,
and update the default parameters
(that is: the unspecified parameters will use the default ones).

For users using code editors with json language servers,
(e.g. [`vscode-json-languageservice`](https://github.com/microsoft/vscode-json-languageservice))
a schema json file is given,
located at `configs/model_config_schema.json`,
for autocompletions and basic lintings.

## `pytest`

Some basic unit tests are written,
and are given in the `tests` folder.

The unit tests are implemented in `pytest`,
and can be called via:

```bash
pytest
```

Other useful options: 
passing `-s` for printing stdouts,
passing specified tests to run parts of tests.
