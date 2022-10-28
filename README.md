# Mini Data Analysis

## What is this project?

This project involves applying the skills learned in STAT545A on some data from Steam in a semi-guided fashion.

## What files can be found?

    ├── milestone1
    │   ├── mini-project-1_files/figure-gfm       # images generated for milestone 1
    │   ├── README.md
    │   ├── mini-project-1.Rmd                    # source file for milestone 1
    │   ├── mini-project-1.md                     # markdown file for milestone 1
    ├── milestone2
    │   ├── mini-project-2_files/figure-gfm       # images generated for milestone 2
    │   ├── README.md
    │   ├── mini-project-2.Rmd                    # source file for milestone 2
    │   ├── mini-project-2.md                     # markdown file for milestone 2
    └── output                                    # Contains saved files for milestone2
    │   ├── discount_magnitude_count.csv
    │   ├── discount_price_delta_model.rds
    │   ├── README.md

## How to run the code?

There are two ways to run the code in this repository. Please ensure that you have R and the packages used in this codebase installed before running

### Dependencies
 * R
 * datateachr
 * tidyverse
 * scales
 * knitr

Using RStudio:

1.  Press the green "Code"" button and then click the copy button

2.  Open R studio and create a new project by going to "File" and then click "New Project"

3.  Press Version Control then Git. Then paste the link into the url box.

4. The code will then be cloned to your PC via RStudio.

5. To knit your own version of the the Rmd, simply open the Rmd and click on the knit button

Using command line:

1.  `git clone https://github.com/stat545ubc-2022/Johnathan_Wong_Mini_Data-Analysis.git`

2.  `cd milestoneX`

3.  `Rscript -e "rmarkdown::render('mini-project-X.Rmd')"`
