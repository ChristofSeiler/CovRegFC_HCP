# Multivariate Heteroscedasticity Models for FC

Low-dimensional and full covariance regression model applied to data from the Human Connectome Project (HCP) to compare functional brain connectivity between short and conventional sleepers. Before running the ``Rmd`` script, you need to install ``R`` package ``CovRegFC`` and download the data from HCP server. This is supplemnatry material for our paper:

```
Multivariate Heteroscedasticity Models for Functional Brain Connectivity,
C. Seiler and S. Holmes,
bioRxiv 2017
```

## Installation

To install ``CovRegFC``:

```
R -e "devtools::install_github('ChristofSeiler/CovRegFC')"
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

## Low-Dimensional Model

To run low-dimensional model on 80 subjects (this will take about 1 to 2 hour):

```
R -e "rmarkdown::render('Low_Dimensional.Rmd', \
params = list(num_regions = '15',num_subjects = '80',tp_per_subject = 'Auto',num_samples = '1'))"
```

## Full Model

To run full model (this will take about 1 to 2 hour):

```
R -e "rmarkdown::render('Full.Rmd',params = list(num_regions = '15'))"
```

## Power Analysis

To run the power analysis:

```
R -e "rmarkdown::render('Low_Dimensional.Rmd', \
params = list(num_regions = '15',num_subjects = 'Inf',tp_per_subject = 'Auto',num_samples = '1'))"

for num_subjects in `seq 20 20 160`
do
  echo num_subjects = $num_subjects
  R -e "rmarkdown::render('Low_Dimensional.Rmd', \
  params = list(num_regions = '15',num_subjects = '${num_subjects}',tp_per_subject = 'Auto',num_samples = '100'))"
done
```

This will take a long time (1 to 2 days), so it might be good to run in it on a computing cluster. We use the R package ``BatchJobs`` to parallelize. You can use it on your cluster by specifiying ``.BatchJobs.R`` and ``.BatchJobs.slurm.brew`` inside your cloned folder.

In ``.BatchJobs.R`` you specify your cluster system:

```
cluster.functions = makeClusterFunctionsSLURM(".BatchJobs.slurm.brew")
```

In ``.BatchJobs.slurm.brew`` you specify required job resources:

```
#!/bin/bash

#SBATCH --job-name=<%= job.name %>
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=8GB
#SBATCH --time=24:00:00
#SBATCH --partition=normal

## Run R:
module load R/3.3.0
R CMD BATCH --no-save --no-restore "<%= rscript %>" /dev/stdout
```

This will produce 9 ``.Rdata`` files. To plot the results:

```
R -e "rmarkdown::render('Power.Rmd',params = list(num_regions = '15'))"
```
