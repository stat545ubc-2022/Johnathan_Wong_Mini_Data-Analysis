Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## Warning: package 'purrr' was built under R version 3.6.3

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have &gt;8 variables,
just pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

As my dataset `steam_games` contains more columns than 8, I will first
select 8 columns.

``` r
steam_games_8_cols <- steam_games %>%
  select(id, url, name, release_date, developer, achievements, original_price, discount_price)
glimpse(steam_games_8_cols)
```

    ## Rows: 40,833
    ## Columns: 8
    ## $ id             <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, ~
    ## $ url            <chr> "https://store.steampowered.com/app/379720/DOOM/", "htt~
    ## $ name           <chr> "DOOM", "PLAYERUNKNOWN'S BATTLEGROUNDS", "BATTLETECH", ~
    ## $ release_date   <chr> "May 12, 2016", "Dec 21, 2017", "Apr 24, 2018", "Dec 13~
    ## $ developer      <chr> "id Software", "PUBG Corporation", "Harebrained Schemes~
    ## $ achievements   <dbl> 54, 37, 128, NA, NA, NA, 51, 55, 34, 43, 72, 41, NA, 50~
    ## $ original_price <dbl> 19.99, 29.99, 39.99, 44.99, 0.00, NA, 59.99, 14.99, 29.~
    ## $ discount_price <dbl> 14.99, NA, NA, NA, NA, 35.18, 70.42, 17.58, NA, NA, NA,~

All rows are observations and all column are variables. Each cell
contains only one entry. This data is tidy based on the definition of
tidy above.

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

Untidying Data: I will use the `unite()` combine relevant entries
`original_price` and `discount_price` into one column called `prices`.
While the new column is still appopriate for the dataset, the column is
now unwieldy to use. The cell also no longer contains a single entry.

``` r
steam_games_8_cols_untidy <- steam_games_8_cols %>% unite(col = prices,
                                                          original_price, discount_price,
                                                          sep = ",")
glimpse(steam_games_8_cols_untidy)
```

    ## Rows: 40,833
    ## Columns: 7
    ## $ id           <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17~
    ## $ url          <chr> "https://store.steampowered.com/app/379720/DOOM/", "https~
    ## $ name         <chr> "DOOM", "PLAYERUNKNOWN'S BATTLEGROUNDS", "BATTLETECH", "D~
    ## $ release_date <chr> "May 12, 2016", "Dec 21, 2017", "Apr 24, 2018", "Dec 13, ~
    ## $ developer    <chr> "id Software", "PUBG Corporation", "Harebrained Schemes",~
    ## $ achievements <dbl> 54, 37, 128, NA, NA, NA, 51, 55, 34, 43, 72, 41, NA, 50, ~
    ## $ prices       <chr> "19.99,14.99", "29.99,NA", "39.99,NA", "44.99,NA", "0,NA"~

Tidying data back to original state: I will use the `separate()`
function to undo the `unite()` function.

``` r
steam_games_8_cols_tidy <- steam_games_8_cols_untidy %>% separate(col = prices, 
                                                                  into = c("original_price", "discount_price"),
                                                                  sep = ",")
glimpse(steam_games_8_cols_tidy)
```

    ## Rows: 40,833
    ## Columns: 8
    ## $ id             <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, ~
    ## $ url            <chr> "https://store.steampowered.com/app/379720/DOOM/", "htt~
    ## $ name           <chr> "DOOM", "PLAYERUNKNOWN'S BATTLEGROUNDS", "BATTLETECH", ~
    ## $ release_date   <chr> "May 12, 2016", "Dec 21, 2017", "Apr 24, 2018", "Dec 13~
    ## $ developer      <chr> "id Software", "PUBG Corporation", "Harebrained Schemes~
    ## $ achievements   <dbl> 54, 37, 128, NA, NA, NA, 51, 55, 34, 43, 72, 41, NA, 50~
    ## $ original_price <chr> "19.99", "29.99", "39.99", "44.99", "0", "NA", "59.99",~
    ## $ discount_price <chr> "14.99", "NA", "NA", "NA", "NA", "35.18", "70.42", "17.~

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  Across the games on Steam, what type of discounts can I expect?
2.  For my selected publishers, which of them produce higher quality
    games, and how do reviews relate to price drops?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

For both of these research questions, I have generated results that
provided insight on them. With the things I have learned, I feel like I
can generate more insights on these questions. Furthermore, these
research questions are focused of discount price which is the primary
reason I chose to look into this dataset.

<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

``` r
steam_games_m2 <- #modify steam_games
  steam_games %>%
  select(name, all_reviews, publisher, genre, original_price, discount_price) %>%
  mutate(discount_price_delta = original_price - discount_price) %>% # make new column called discount_price_delta using original_price - discount_price
  filter(discount_price_delta >= 0) %>% # remove entries with negative discount_price_delta
  separate(all_reviews, c("review_sentiment", "num_reviews_and_pct_pos_reviews"), sep = ",", extra = "merge") %>% #separate all_reviews column at the first instance of ,
  separate(num_reviews_and_pct_pos_reviews, c("num_reviews", "pct_pos_reviews"), sep = " ", extra = "merge") %>% #separate num_reviews_and_pct_pos_reviews at the first instance of a space
  separate(pct_pos_reviews, c("pct_pos_reviews", "to_be_dropped"), sep = "%", extra = "merge") %>% #separate num_reviews_and_pct_pos_reviews at the first instance of %
  select(-to_be_dropped) %>% #drop to_be_dropped column
  mutate(pct_pos_reviews = as.numeric(pct_pos_reviews)) %>% #convert pct_pos_review numeric type
  filter(!str_detect(review_sentiment,"reviews")) %>% # remove rows with malformed review_sentiment entries
  mutate(across(c(genre),
                factor))
glimpse(steam_games_m2)
```

    ## Rows: 1,938
    ## Columns: 9
    ## $ name                 <chr> "DOOM", "Neverwinter Nights: Enhanced Edition", "~
    ## $ review_sentiment     <chr> "Very Positive", "Mostly Positive", "NaN", "Very ~
    ## $ num_reviews          <chr> "(42,550),-", "(1,094),-", NA, "(7,855),-", "(7,7~
    ## $ pct_pos_reviews      <dbl> 92, 74, NA, 80, 93, 82, 87, 85, 84, NA, 98, 76, 8~
    ## $ publisher            <chr> "Bethesda Softworks,Bethesda Softworks", "Beamdog~
    ## $ genre                <fct> "Action", "RPG", "Action", "Action", "Action,Adve~
    ## $ original_price       <dbl> 19.99, 57.91, 14.99, 19.99, 6.99, 29.99, 14.99, 9~
    ## $ discount_price       <dbl> 14.99, 46.26, 7.49, 9.99, 5.24, 14.99, 4.49, 1.49~
    ## $ discount_price_delta <dbl> 5.00, 11.65, 7.50, 10.00, 1.75, 15.00, 10.50, 8.5~

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

As I do not have a plot from a milestone that makes use of factors
(there is one generated using names, but it contains only three groups
that does not make use of facetting), I will generate a new plot.For
research question 1, I want to dive into if different genres will have
different discounts. To do that I willgenerate boxplots of
`discount_price_delta` using the `genre` column.

First to make the facetting layout a bit simpler, I genertaed a new
tibble called `steam_games_m2_single_genre` by selecting games that only
has one genre.

``` r
library(stringr) # for str_detect
```

``` r
steam_games_m2_single_genre <- steam_games_m2 %>%
  filter(!str_detect(genre, ",")) # remove genre entries with more than 1 genre

glimpse(steam_games_m2_single_genre)
```

    ## Rows: 318
    ## Columns: 9
    ## $ name                 <chr> "DOOM", "Neverwinter Nights: Enhanced Edition", "~
    ## $ review_sentiment     <chr> "Very Positive", "Mostly Positive", "NaN", "Very ~
    ## $ num_reviews          <chr> "(42,550),-", "(1,094),-", NA, "(7,855),-", "(2,3~
    ## $ pct_pos_reviews      <dbl> 92, 74, NA, 80, 82, 84, NA, 98, 93, 84, 77, 88, 7~
    ## $ publisher            <chr> "Bethesda Softworks,Bethesda Softworks", "Beamdog~
    ## $ genre                <fct> "Action", "RPG", "Action", "Action", "Action", "R~
    ## $ original_price       <dbl> 19.99, 57.91, 14.99, 19.99, 29.99, 59.98, 69.99, ~
    ## $ discount_price       <dbl> 14.99, 46.26, 7.49, 9.99, 14.99, 40.48, 59.99, 53~
    ## $ discount_price_delta <dbl> 5.00, 11.65, 7.50, 10.00, 15.00, 19.50, 10.00, 23~

Now we plot a box plot of `discount_price` with facetting on `genres`. I
also removed observations with `discount_price_delta`greater than 100 to
make the plots more readable

``` r
steam_games_m2_single_genre %>%
  filter(discount_price_delta < 100) %>% # filter out discount prices greater than 100
  ggplot(aes(genre, discount_price_delta)) + 
  geom_boxplot(width = 0.2) +
  facet_wrap(~genre, scales = "free_x") # rescale x-axis by removing unused labels 
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 1

``` r
steam_games_m2_single_genre %>%
  filter(discount_price_delta < 100) %>% # filter out discount prices greater than 100
  mutate(genre = fct_infreq(genre)) %>% # reorder factor based on frequency of genre
  ggplot(aes(genre, discount_price_delta)) + 
  geom_boxplot(width = 0.2) +
  facet_wrap(~genre, scales = "free_x") # rescale x-axis by removing unused labels 
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

I reoreder the genre factor level by the frequency of each genre. The
reason for this ordering is some genres have few entries and I want to
make judgement on genres of games with more entries, as these genre will
have more statistical power. Now I can see that the box plot of the
Action game uses more datapoints to generate the summary stats
(quartiles) used in boxplot.

<!----------------------------------------------------------------------------->
<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

``` r
genre_levels <- unique(steam_games_m2_single_genre$genre)

steam_games_m2_single_genre %>%
  filter(discount_price_delta < 100) %>% # filter out discount prices greater than 100
  mutate(genre = fct_collapse(genre, # manually collapse factors
                              Action = "Action",
                              Other = as.character(genre_levels[! genre_levels %in% "Action"]))) %>% # generate char vector of all factors except for Action
  ggplot(aes(genre, discount_price_delta)) + 
  geom_boxplot(width = 0.2) +
  facet_wrap(~genre, scales = "free_x") # rescale x-axis by removing unused labels 
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

I am interested in action games discount versus the rest. To do that, I
collapsed all other factors into “Other” using `fct_collapse`. By
plotting the “Action” genre against the “Other”, I can compare action
games discounts against the other discounts on steam, and I can see that
action games have a higher median discount on average so action games
are probably more econmoical to play.

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: \~For my selected publishers,\~ Which of them
produce higher quality games, and how do reviews relate to price drops?

Since “my selected publishers”" is too small of a scope and I want more
data to train the model, I removed “For my selected publishers” from the
research question.

**Variable of interest**: `discount_price_delta`

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
discount_price_delta_model <- lm(discount_price_delta ~ pct_pos_reviews, steam_games_m2)
```

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
broom::tidy(discount_price_delta_model)
```

    ## # A tibble: 2 x 5
    ##   term            estimate std.error statistic    p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>      <dbl>
    ## 1 (Intercept)       54.4      11.9       4.59  0.00000477
    ## 2 pct_pos_reviews   -0.122     0.158    -0.771 0.441

I want to see the regression coefficient of my model so I used
`broom::tidy()` to output summarize the model. Row 2 and column 2
contains the regression coefficient which is “-0.122”. The intercept is
found in Row 1 and column 1.
<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

``` r
dir.create(here::here("output")) #create output dir and ensure Rmd won't fail if ran elsewhere
```

    ## Warning in dir.create(here::here("output")): 'C:
    ## \Users\johna\OneDrive\Documents\Johnathan_Wong_Mini_Data-Analysis\output'
    ## already exists

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

Reproduce summary table from milestone 1

``` r
discount_magnitude_count_summary_table <- 
  steam_games %>%
  mutate(discount_price_delta = original_price - discount_price) %>% # make new column called discount_price_delta using original_price - discount_price
  filter(discount_price_delta >= 0) %>% # remove entries with negative discount_price_delta
  mutate(discount_magnitude = ifelse(discount_price_delta < 20, "small", # add new variables based on description above
                                     ifelse(discount_price_delta >= 20 & discount_price_delta < 40, "medium",
                                            "large"))) %>%
  count(discount_magnitude) # count how many discounts
```

``` r
glimpse(discount_magnitude_count_summary_table) # view table
```

    ## Rows: 3
    ## Columns: 2
    ## $ discount_magnitude <chr> "large", "medium", "small"
    ## $ n                  <int> 579, 411, 2144

``` r
write_csv(discount_magnitude_count_summary_table, here::here("output", "discount_magnitude_count.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
saveRDS(discount_price_delta_model, here::here("output","discount_price_delta_model.rds"))
saved_model <- readRDS(here::here("output","discount_price_delta_model.rds"))
broom::tidy(saved_model) # check model has the same result as above
```

    ## # A tibble: 2 x 5
    ##   term            estimate std.error statistic    p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>      <dbl>
    ## 1 (Intercept)       54.4      11.9       4.59  0.00000477
    ## 2 pct_pos_reviews   -0.122     0.158    -0.771 0.441

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

Plots failing to show up on Github in the .md counts as an error here.
So does the entire .md failing to show up on Github in the .md (“Sorry
about that, but we can’t show files that are this big right now”).

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
