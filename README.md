# Multivariate Heteroscedasticity Models

Low-dimensional and full covariance regression model applied to data from the Human Connectome Project (HCP) to compare functional brain connectivity between short and conventional sleepers. Before running the ``Rmd`` script, you need to install ``R`` package ``CovRegFC`` and download the data from HCP server.

To install ``CovRegFC``:

```
R -e "devtools::install_github('ChristofSeiler/CovRegFC')
```

Download HCP data and unzip in the same folder where you clone this repository.

```
HCP_PTN820/node_timeseries/3T_HCP820_MSMAll_d15_ts2
HCP_PTN820/groupICA/groupICA_3T_HCP820_MSMAll_d15.ica/melodic_IC_sum.sum
```

To run low-dimensional model:

```
R -e "rmarkdown::render('Low_Dimensional.Rmd', \
params = list(num_regions = '15',num_subjects = 'Inf',tp_per_subject = 'Auto',num_samples = '1'))"
```

To run full model:

```
R -e "rmarkdown::render('Full.Rmd',params = list(num_regions = '15'))"
```
