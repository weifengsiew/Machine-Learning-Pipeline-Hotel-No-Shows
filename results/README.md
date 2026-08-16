# Results

This `results/` directory stores generated outputs from the Kedro workflow.

## Directory Structure

The `results/` directory structure is as follows:

```text
results/
|-- README.md
|-- ml_experiments/
|   |-- final_no_show_model/
|   |   `-- best_model.joblib        # persisted best model
|   |-- persisted_pipeline_summary.json  # saved model summary, e.g. best params, model path, and metrics
|   `-- test_set_metrics.json
|-- mlruns/                          # MLflow tracking records
`-- validation/                      # data validation reports
    |-- noshow_validation_results.json
    |-- noshow_validation_failures.csv
    |-- noshow_cleaned_validation_results.json
    `-- noshow_cleaned_validation_failures.csv
```

These files are produced by later stages in the pipeline:

- `data_validation` writes data validation reports.
- `holdout_evaluation` writes test-set metrics.
- `experiment_logging` writes MLflow tracking records.
- `pipeline_persistence` saves the best model and summary.

## Further Information

To inspect runs stored in `results/mlruns/`, start the MLflow UI from the project root:

```bash
mlflow server --backend-store-uri results/mlruns
```

Then open `http://localhost:5000`.

For more information, please refer to the [MLflow Tracking documentation](https://mlflow.org/docs/latest/ml/tracking/).
