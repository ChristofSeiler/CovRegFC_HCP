# Multivariate Heteroscedasticity Models for FC

Low-dimensional and full covariance regression model applied to data from the Human Connectome Project (HCP) to compare functional brain connectivity between short and conventional sleepers. Before running the ``Rmd`` script, you need to install ``R`` package ``CovRegFC`` and download the data from HCP server.

To install ``CovRegFC``:

```
R -e "devtools::install_github('ChristofSeiler/CovRegFC')
```

The HCP data is available in this repository in multipart zip file. To unzip:

```
zip -s 0 HCP_PTN820.zip --out unsplit-HCP_PTN820.zip
unzip unsplit-HCP_PTN820.zip
```

You should see the following files and folders structure:

```
HCP_PTN820/sample_info.csv
HCP_PTN820/node_timeseries/3T_HCP820_MSMAll_d15_ts2/*.txt
HCP_PTN820/groupICA/groupICA_3T_HCP820_MSMAll_d15.ica/melodic_IC_sum.sum/*.png
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
