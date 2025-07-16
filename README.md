# The CEPH R project template

This is a github template for a CEPH R project repository. This template can
be loaded when creating a new project in github. As such you pre-populate a
project with a standard set of directories. This ensures a project-oriented 
and consistent workflow across all group members, and removes some of the mental
overhead in making decisions on a project structure.

This template was created based on the [template](https://github.com/geco-bern/R_proj_template) by the [geco-bern](https://github.com/geco-bern) group.

## Use

### A github project from the template

To use the template create a new repository on github, as you otherwise would
using the big green button. If you are in the project on github you can hit
the green button top right (Use this template).

![](https://github.com/bluegreen-labs/environmental_data_science_101/raw/main/images/green_button.png)

Otherwise you can select the repository from the template dropdown menu, select `geco-bern/R-project-template`.

![](https://github.com/bluegreen-labs/environmental_data_science_101/raw/main/images/new_repo_1.png)

Proceed as usual by naming your repository. However, be careful to select the 
correct owner of the project if you have multiple identities.
![](https://github.com/bluegreen-labs/environmental_data_science_101/raw/main/images/new_repo_2.png)

### Clone to your local computer

The new repository will be created and populated with the files of the template.
You can then clone the project to your local computer to add files.

Although optional it is wise to rename the `*.Proj` file as this will not
automatically change to your repository name. However, retaining the original
name should not change any of the functionality.

```bash
git clone git@github.com:USER/YOUR_PROJECT.git
```

<br>

## Structure

The reasons for this folder structure.

- Avoids top level aggregation of data, code and reporting files
- Splits pre-processing of data from working / included data (`data-proessing`vs. `data-raw` vs. `data`)
- Splits R functions from R analysis scripts (`functions` vs `analysis`)
- Splits the dynamic reporting from analysis output (`vignettes` vs. `output`)


Below you find a comprehensive list of what goes where an why, as well as some
best practices on how to structure further data within these folders.

### The functions folder

The `functions` folder contains R functions, not scripts. This means code wrapped in a
structure as such

```R
# A demo function
#
# This function demonstrates the general layout
# of a function

my_function <- function(parameter) {
  some_actions
}
```

Functions are actions you need more than once, which can not be generated
easily with external packages and are tailored to your project.

These functions should stand on their own with limited links to additional
custom functions. Ideally you provide a brief title and description on the 
function's purpose before.

Writing functions seems an initial waste of time, you could easily just copy and
paste some code in your analysis scripts. However, this means that if you
decide certain aspects of this workflow you might have to hunt down these
changes in all analysis scripts. Failing to do so will result in corrupted 
analysis. In addition, writing functions will make it easy to re-use the code
within the context of a new project, and if proven to be generally useful
outside a single research project it can be integrated in a formal package.

### The data-processing folder

The `data-processing` folder contains scripts and code which describe your processing routine. 
Following this routine, you convert the data from the `data-raw` folder into analysis ready 
data that is stored in the `data` folder. To ensure reproducibility, structure your processing 
steps indicating both the order of the processing routine and the content or variables being processed.

```
data-processing/
├─ 01_processing_step1.R
├─ 02_processing_step2.R
├─ 03_processing_step3.R
├─ 04_processing_step4.R
├─ 05_processing_step5.R
```

### The data-raw folder

The `data-raw` folder contains, as the name suggests, raw data and the scripts
to download the data. This is data which requires significant
pre-processing to be of use in analysis. In other words, this data is not 
analysis ready (within the context of the project).

To create full transparency in terms of the source of this raw data it is best
to include (numbered) scripts to download the data. Either in
these scripts, or in a separate README, include the source of the data (reference)

It is best practice to store various raw data products in their own sub-folder,
with data downloading in the main `data-raw` folder.

```
data-raw/
├─ raw_data_product/
├─ 00_download_raw_data.R
```

Where possible it is good practice to store output data (in `output`) either as human 
readable CSV files, or as R serialized files 
(generated using with the `saveRDS()` function).

It is common that raw data is large in size, which limits the option of storing
the data in a git repository. If this isn't possible this data can be excluded
from the git repository by explicitly adding directories to `.gitignore` to
avoid accidentally adding them.

When dealing with heterogeneous systems dynamic paths can be set to (soft) link
to raw-data outside the project directory.

### The data folder

The `data` folder contains analysis ready data. This is data which you can use,
as is. This often contains the output of a `data--processing` pre-processing workflow,
but can also include data which doesn't require any intervention, e.g. a land
cover map which is used as-is. Output from `data-raw` often undergoes a
dramatic dimensionality reduction and will often fit github file size limits. In
some cases however some data products will still be too large, it is recommended
to use similar practices as describe for `data-raw` to ensure transparency
on the sourcing of this data (and reproducible acquisition).

It is best to store data in transparently named sub-folders according to the
product type, once more including references to the source of the data where
possible. Once more, download scripts can be used to ensure this transparency
as well.

```
data/
├─ data_product/
├─ 00_download_data.R
```

### The analysis folder

The `analysis` folder contains, *surprise*, R scripts covering analysis of your
analysis ready data (in the `data` folder). These are R scripts with output
which is limited to numbers, tables and figures, which you will store in the 
`output` folder. It should not include R markdown code!

Scripts can have a numbered prefix to indicate an order of execution, but this
is generally less important as you will work on analysis ready data. If there
is carry over between analysis, either merge the two files or use numbered
prefixes.

```
analysis/
├─ 00_crossbasis_parameters.R
├─ 01_research_question1.R
```


### The output folder

The `output` folder contains, your results in the form of figures and tables which 
were generated in your analysis. This output should be saved in subfolders by category of 
the output.

```
output/
├─ figures/
├─ ├─ figure1.png
├─ ├─ figure2.png
├─ tables/
├─ ├─ table1.csv
├─ ├─ table2.csv
```

### The vignettes folder

The `vignettes` folder contains dynamic notebooks, i.e. R markdown files. These
might serve a dual use between analysis and manuscript. They can be used to present
small, educational, examples, and preliminary output to peers with code examples, 
but should not be used as a integral part of the analysis. 

In short, R markdown files have their function in reporting results, once
generated (through functions or analysis scripts) but should be avoided to
develop code / ideas!

### Sensitive data

Much of the data we are using cannot be made public due to sensitivity  reasons. For this we need to specify in the `.gitignore` file in this repository that the contents of the this file are not seen by git and GitHub. You can also specify other files to not be seen by git/GitHub, e.g. shapefiles which can be very big and are not needed on GitHub. Large files take up unnecessary space and should be kept from GitHub.

By default, the content of all data folders: `data`, `data-raw` will not be seen by git/GitHub. 

### Capturing your session state

If you want to ensure full reproducibility you will need to capture the state of the system and libraries with which you ran the original analysis. Note that you will have to execute all code and required libraries for `renv` to correctly capture all used libraries.

When setting up your project you can run:

``` r
# Initiate a {renv} environment
renv::init()
```

To initiate your static R environment. Whenever you want to save the state of your project (and its packages) you can call:

``` r
# Save the current state of the environment / project
renv::snapshot()
```

To save any changes made to your environment. All data will be saved in a project description file called a lock file (i.e. `renv.lock`). It is advised to update the state of your project regularly, and in particular before closing a project.

When you move your project to a new system, or share a project on github with collaborators, you can revert to the original state of the analysis by calling:

``` r
# On a new system, or when inheriting a project
# from a collaborator you can use a lock file
# to restore the session/project state using
renv::restore()
```

> NOTE: As mentioned in the {renv} documentation: "For development and collaboration, the `.Rprofile`, `renv.lock` and `renv/activate.R` files should be committed to your version control system. But the `renv/library` directory should normally be ignored. Note that `renv::init()` will attempt to write the requisite ignore statements to the project `.gitignore`." We refer to \@ref(learning-objectives-6) for details on github and its use.

