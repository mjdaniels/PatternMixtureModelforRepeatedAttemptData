
<!-- README.md is generated from README.Rmd. Please edit that file -->
RAMPMM package
==============

The goal of the package is to perform the nonignorable analyses through the Pattern Mixture approach for the Analysis of Repeated Attempt Data. In our pattern mixture formulation of the Repeated Attempts Model, denoted as RAMPMM, the patterns are defined by the values of R ( the number of attempts until the outcome is successfully collected; Maximum up to K attempts where R = K + 1 corresponds to the outcome not being collected after the maximum number of attempts). We use the hazard model for the conditional distribution of the pattern indicator.

The Covariates (Xs) are the baseline response and indicators of the centers. The conditional distribution of the Response ( for the pattern R = k , where k = 1, . . . , K + 1) is a normal distribution with mean and variance depending on covariates and patterns.

The Quantity of interest '/theta' is the 'treatment arm specific' effect on the mean of the Response ( conditional on treatment arm). /theta can be calcualted via WINBUGS by doing a Monte Carlo integration over distibution of the covariates. We obtain posterior summaries as well via WINBUGS. See example for details. The model allows for sensitivity analysis and is more transparent and flexible towards parameter identifibility.

Main Components of the package
------------------------------

The packages needs,

1.  a data .txt file.

A data file ( txt/csv) having coulmns that are named and placed in a specific order.

For the column having values for 'Intervention’, users must either have name 'Z' for this column in their data or when calling the function, they should provide the name for this column as per name in their dataset. For the column having values for 'Response’, users must either have name ‘Y2’ for this column in their data or when calling the function, they should provide the name for this column as per name in their dataset For the column having values for 'Baseline Response’, users must either have name ‘Y1’ for this column in their data or when calling the function, they should provide the name for this column as per name in their dataset For the column values for 'Number of Attempts’, users must either have name ‘R’ for this column in their data or when calling the function, they should provide the name for this column as per name in their dataset For the Indicator Centers (Xs), the User dataset must have Xs starting from 5th column and ending as the last column of the dataset.

1.  a JAGS code file that specifies a model in it. WINBUGS (Windows version of Bayesian inference Using Gibbs Sampling) is a MCMC software to implement Gibbs sampling, which generates samples from the posterior distribution. Alternative to WINBUGS is JAGS that is accessed in R via rjags to perform the Gibbs sampling. JAGS, written in C++, gets installed outide R and the user of this package must have it installed on their system. For help and instructions on how to install JAGS, user can visit <https://sourceforge.net/projects/mcmc-jags/files/> and download the latest version, 4.2.0 based user's Operating System.

The model file has deterministic relations, stochastic relations through distributions suported in JAGS, and priors. The code is written in a dialect of the BUGS language that specifies the model. A model file Model\_JAGSfile.txt' is provided with this pacakge inside the inst/ folder.

Rjags' jags.model() function sends the model in the file to JAGS for parsing and compiling. On compilation, the code syntax is turned into a set of sequential instructions and the compiler resolves the names and links in the model. The function generates 1000 samples (updates) of the MCMC algorithm.

Finally, rjags' coda.samples() function generates posterior samples. CODA (COnvergence Diagnostic and Analysis) describes a set of functions for analyzing outputs generated from MCMC. It continues updating the chain for the number of iterations specified by n.iter, and it saves them if the parameter is listed in the variable.names argument.

The object returned is of class 'mcmc.list'. The summary() method on this object is run to obtain a summary of the posterior distribution. The output has the characteristics of MCMC and then the statistics of the samples from the posterior distribution.

Installation
------------

You can install Q6 from github with:

``` r
# install.packages("devtools")
devtools::install_github("mjdaniels/PatternMixtureModelforRepeatedAttemptData")
```

Example
-------

This is a basic example which shows you how to solve a common problem:

``` r
# Run the function by following command

# You will have to provide two files, 1. the dataset file 2. the JAGS model file. ( NOTE: if they are not in the same working directory, then provide the full path to the files.)
# Also, the dataset must have the column names either as default names for each column in the argument or provide the respective names while calling this function.

PatternMixtureModel::PMM("/Users/as82986/Datasetfile.txt", system.file("extdata", "Model_JAGSfile.txt", package = "PatternMixtureModel"))
#> Compiling model graph
#>    Resolving undeclared variables
#>    Allocating nodes
#> Graph information:
#>    Observed stochastic nodes: 1571
#>    Unobserved stochastic nodes: 8090
#>    Total graph size: 273368
#> 
#> Initializing model
#> 
#> [1] "The posterior summaries are stored in the 'Results' Variable (Object type 'list'). Examples Usage: Result$statistics"
```
