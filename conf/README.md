# Configuration

This `conf/` directory contains the Kedro configuration used when running the project. The source code in `src/` defines what the pipeline does; the files in `conf/` define which datasets, file paths, and parameters the pipeline uses.

## Directory Structure

The `conf/` directory structure is as follows:

```text
conf/
|-- README.md
|-- base/                                      # default project configuration
|   |-- catalog/                              # maps dataset names to storage types and file paths
|   |   |-- experiment_artifacts.yml          # where ML experiment artifacts are saved, e.g. metrics
|   |   |-- great_expectations.yml            # temporary objects used for data validation
|   |   |-- persisted_datasets.yml            # where cleaned and feature-engineered datasets are saved
|   |   `-- validation_artifacts.yml          # where data validation reports are saved
|   `-- parameters/                           # values passed to nodes as params:<name>
|       |-- data_ingestion.yml                # path and table name for raw data
|       |-- data_validation.yml               # table name for data validation
|       |-- ml_experiment.yml                 # settings for the ML experiment
|       `-- model_hyperparams/                # model-specific hyperparam search grids
|           |-- lightgbm.yml
|           |-- random_forest.yml
|           `-- xgboost.yml
`-- local/                                    # machine-specific overrides for matching config in base/
```

## Further Information

For more information, please refer to the Kedro documentation on [configuration](https://docs.kedro.org/en/stable/configure/configuration_basics/), [the Data Catalog](https://docs.kedro.org/en/stable/catalog-data/data_catalog/), and [Data Catalog YAML examples](https://docs.kedro.org/en/stable/catalog-data/data_catalog_yaml_examples/).
