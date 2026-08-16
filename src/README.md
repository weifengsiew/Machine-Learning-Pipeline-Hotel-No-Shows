# Source Code

This `src/` directory contains the Python source code that Kedro imports when running the project. `pyproject.toml` sets `source_dir = "src"` and `package_name = "hotel_no_show_prediction"`, so Kedro looks under `src/hotel_no_show_prediction/` for the project code.

## Directory Structure

The `src/` directory structure is as follows:

```text
src/
|-- README.md
`-- hotel_no_show_prediction/                 # main project package imported by Kedro
    |-- __init__.py                           # marks the package as importable
    |-- pipeline_registry.py                  # discovers stages and registers the default pipeline
    |-- pipelines/                            # modular Kedro stages; each stage has __init__.py, pipeline.py, and nodes.py
    |   |-- data_ingestion/
    |   |-- data_cleaning/
    |   |-- data_validation/
    |   |-- feature_engineering/
    |   |-- train_test_split/
    |   |-- ml_experiment/
    |   |-- pipeline_selection_and_calibration/
    |   |-- holdout_evaluation/
    |   |-- experiment_logging/
    |   `-- pipeline_persistence/
    `-- sklearn_pipeline_components/          # shared ML model, preprocessor, and config helpers
```

## Pipeline Registry

`pipeline_registry.py` tells Kedro which pipeline stages exist and how to combine them into the default runnable pipeline:

```python
discovered_pipelines = find_pipelines()
```

`find_pipelines()` searches `hotel_no_show_prediction/pipelines/`, imports stage packages such as `data_cleaning` and `ml_experiment`, and calls each stage's exposed `create_pipeline()` function.

Each discovered pipeline is then given the stage folder name (`module_name`), such as `data_cleaning` or `ml_experiment`, as its namespace:

```python
modular_pipeline(
    module_pipeline,
    namespace=module_name,
    prefix_datasets_with_namespace=False,
)
```

For example, the `drop_duplicate_rows` node belongs to the `data_cleaning` pipeline module, while the `parse_price_currency_and_amount` node belongs to the `feature_engineering` pipeline module:

```text
data_cleaning.drop_duplicate_rows
feature_engineering.parse_price_currency_and_amount
```

`prefix_datasets_with_namespace=False` keeps dataset names unprefixed, so an output such as `noshow_cleaned` from `data_cleaning` can be used as the input to `feature_engineering`.

If this were set to `True`, Kedro would prefix dataset names with each stage namespace too. For example, `noshow_cleaned` could become separate dataset names such as `data_cleaning.noshow_cleaned` and `feature_engineering.noshow_cleaned`, so the stages would no longer connect through the shared `noshow_cleaned` name unless the inputs and outputs were explicitly remapped.

Finally, the registry returns one default graph:

```python
return {"__default__": default_pipeline}
```

This default graph, containing all successfully discovered stages, is what `kedro run` executes. Individual stages can be run as follows:

```bash
kedro run --namespaces=ml_experiment
```

## Visualise the Pipeline

From the project root, start Kedro-Viz with:

```bash
kedro viz run
```

This opens the pipeline visualisation at `http://127.0.0.1:4141/`. Because this registry returns `__default__`, Kedro-Viz shows the same default graph that `kedro run` executes.

## Further Information

For more information, please refer to the Kedro documentation on [pipeline registries](https://docs.kedro.org/en/stable/build/pipeline_registry/), [namespaces](https://docs.kedro.org/en/stable/build/namespaces/), [running pipelines](https://docs.kedro.org/en/stable/build/run_a_pipeline/), and [Kedro-Viz](https://docs.kedro.org/projects/kedro-viz/en/stable/kedro-viz_visualisation/).
