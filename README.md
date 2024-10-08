sglm
==============================

A GLM Pipeline for Neuroscience Analyses

Built using sklearn.ElasticNet,Ridge, ElasticNetCV, RidgeCV, and GridSearchCV.

All necessary packages listed in requirements.txt and are pip installable!

## Jupyter Notebooks
There are three notebooks within this repository: gridSearch_CPU, gridSearch_GPU, and fitGLM.
* The gridSearch notebooks are used to find the best parameters and will help you select the best regression model for your data. They have been optimized for use with sklearn (gridSearch_CPU) and pytorch (gridSearch_GPU). 
* The fitGLM notebook is used to fit the model for known parameters and/or searching through a small list of different parameters. There is also a validation step included in this notebook. 

Both notebooks are similar and have many of the same elements. They will output a project directory with the necessary files to continue your analysis
and plot some figures for visualization.

You can also run the pipeline in Google Colab. Please visit our [Google Colab Notebook](https://githubtocolab.com/jbwallace123/sabatini-glm-workflow/blob/main/notebooks/colab_grid_search_tutorial.ipynb) to get started.


## Command Line Interface
You may also run the pipeline using the command line. You must first create a project directory, edit your config.yaml, move your data to the data folder, and then run the following command:

```bash
cd path/to/repo
python ./main/run_pipeline.py path/to/config.yaml
```

Please note that this *does not* include the gridSearch functionality or the additional validation steps that are included in the notebooks.

## Project Organization

The notebooks will output a project directory with the following structure:

```bash
Project directory will be created to include: 
|
| Project_Name
| ├── data
|   ├── 00001.csv
|   ├── 00002.csv
|   └── combined_output.csv
| ├── models
|   └── project_name.pkl
| ├── results
|   ├── model_fit.png
|   ├── predicted_vs_actual.png
|   └── residuals.png
| config.yaml
```

data folder: will include all of your data in .csv format. Please refer to the notebook for formatting.

models folder: will include outputs saved from the model_dict.

results folder: includes some figures for quick visualization. 

config.yaml: your config file to set your parameters for the model. 

## Recommended Steps for Installation:

It is recommended that you create an enviorment to run this pipeline. 

```bash
conda create -n sglm python=3.9
conda activate sglm
pip install -r requirements.txt
```

## Troubleshooting:

* This is a work in progress and will be updated as needed. Please feel free to reach out with any questions or concerns.

* Help! My kernel keeps dying! 
    * This is likely due to the size of your data. You can sparsify your data to help with this issue and also set
    `n_jobs` to -2.

* I have large datasets, is there GPU support?
    * Yes! Thanks to [torch_linear_regression](https://github.com/RichieHakim/torch_linear_regression/tree/master), we now have pytorch supported Ridge and Linear Regression models.
    You can set `pytorch` to `True`. This method is faster with both the CPU and GPU versions of pytorch. So, if you have larger datasets, this will be a great option for you.
    
    ```bash
    model, y_pred, score, beta, intercept = glm_fit.fit_glm(config, X_train, X_test, y_train, y_test, cross_validation=False, pytorch=True)
    ```

* I need to fit the same X-data to many different y-data. How can I do this efficiently? 
    * If using the pytorch supported models, you can use the `prefit` argument in the model iniitalization so you can avoid recomputing the same thing over and over again.
    This can be done with OLS and Ridge models. 
    ```bash
    model = tlr.OLS(prefit_X=X)
    model.fit(X=X, y=Y) # Pass X again even though it is already fit
    y_pred = model.predict(X)
    ```
