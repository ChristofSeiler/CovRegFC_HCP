# Multivariate Heteroscedasticity Models for FC

Low-dimensional and full covariance regression model applied to data from the Human Connectome Project (HCP) to compare functional brain connectivity between short and conventional sleepers. Before running the ``Rmd`` script, you need to install ``R`` package ``CovRegFC`` and download the data from HCP server. This is supplemnatry material for our paper:

```
Multivariate Heteroscedasticity Models for Functional Brain Connectivity,
C. Seiler and S. Holmes,
bioRxiv 2017
```

To install ``CovRegFC``:

```
R -e "devtools::install_github('ChristofSeiler/CovRegFC')
```

Then clone this repository:

```
git clone git@github.com:ChristofSeiler/CovRegFC_HCP.git
```

The HCP data is available in this repository as a multipart zip file. To unzip:

```
cd CovRegFC_HCP
zip -s 0 HCP_PTN820.zip --out unsplit-HCP_PTN820.zip
unzip unsplit-HCP_PTN820.zip
```

You should see the following files and folders structure:

```
HCP_PTN820/sample_info.csv
HCP_PTN820/node_timeseries/3T_HCP820_MSMAll_d15_ts2/*.txt
HCP_PTN820/groupICA/groupICA_3T_HCP820_MSMAll_d15.ica/melodic_IC_sum.sum/*.png
```

To run low-dimensional model on 40 subjects (this will take about 20 to 25 minutes):

```
R -e "rmarkdown::render('Low_Dimensional.Rmd', \
params = list(num_regions = '15',num_subjects = 40,tp_per_subject = 'Auto',num_samples = '1'))"
```

To run full model (this will take about 50 to 55 minutes):

```
R -e "rmarkdown::render('Full.Rmd',params = list(num_regions = '15'))"
```
