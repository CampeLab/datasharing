> Hi! This is a heavily edited copy of _The Leek group guide to data sharing_, by Jeff Leek and his group from Johns Hopkin's University. If you're interested in the original document, go check it in the _jtleek/datasharing_ repository on GitHub.  
> Cheers,  
> Felipe

Guidelines for data sharing
===========

This is a guide for anyone who needs to share data with me (Prof. Felipe Campelo, ORCS Lab). The goals of this guide are to provide some instruction on the best way to share data to avoid the most common pitfalls and sources of delay in the transition from data collection to data analysis. We tend to work collaboratively (partcularly when it comes to the analysis of experimental results) and the number one source of variation in the speed to results is the status of the data when they arrive for the analyst to work.

My strong feeling is that the data analyst should be able to handle the data in whatever state they arrive. It is important
to see the raw data, understand the steps in the processing pipeline, and be able to incorporate hidden sources of
variability in one's data analysis. On the other hand, our research generally involves the outcomes of computational algorithms, so reproducibility of the results is ideally perfect (assuming you controlled your PRNG), and the process of calculating performance indicators from the raw data (e.g., hypervolume, convergence rate, etc.) is quite straightforward, so the work of converting the data from raw form to directly analyzable form can be performed before sending your data to me. This can dramatically speed up the turnaround time, since the I won't have to work through all the pre-processing steps first. 


What you should deliver
====================

For maximum speed in the analysis this is the information you absolutely MUST send me:

- (1) A [tidy data set](http://vita.had.co.nz/papers/tidy-data.pdf) 
- (2) A code book describing each variable and its values in the tidy data set.  

If there is any chance that the raw data might be useful, you should also send:

- (3) The raw data
- (4) An explicit and exact recipe you used to go from (3) -> (1), (2)

Let's look at each part of the data package you will transfer. 

### The raw data

It is critical that you include the rawest form of the data that you have access to. Some examples are: 
- The **.mat** file(s) with the final populations of your EA runs;
- The text output of your java algorithm;

You know the raw data is in the right format if you: 

1. Ran no software on the data
1. Did not manipulate any of the numbers in the data
1. You did not remove any data from the data set
1. You did not summarize the data in any way

If you did any manipulation of the data at all it is not the raw form of the data. Reporting manipulated data
as raw data is a very common way to slow down the analysis process, since the analyst will often have to do a
forensic study of your data to figure out why the raw data looks weird. 

### The tidy data set

The general principles of tidy data are laid out by [Hadley Wickham](http://had.co.nz/) in [this paper](http://vita.had.co.nz/papers/tidy-data.pdf)
and [this video](http://vimeo.com/33727555). The paper and the video are both focused on the [R](http://www.r-project.org/) package, which you may or may not know how to use, but which **I** use for data analysis. The most important ideas here are: 

1. Each variable you measure should be in one column;
1. Each different observation of that variable should be in a different row;
1. All columns should be named, with NO spaces (use dots instead) and NO special characters (using a column name _Time.s_ is OK, but _Time (s)_ is not);
1. If you are sending me an updated dataset, make sure that you did not change the names of columns that represent the same variable (if you originally called a column _time.s_, don't change to _Time.s_ or _time.seconds_ in the next file)
1. Your tidy data should be sent as **.csv** file.

### The code book

For almost any data set, the measurements you calculate will need to be described in more detail than you will sneak
into the spreadsheet. The code book contains this information. At minimum it should contain:

1. Information about the variables (including units!) in the data set
1. Information about the summary choices you made
1. Information about the experimental study design you used


### How to code variables

There are several main [data types](http://en.wikipedia.org/wiki/Statistical_data_type) that we usually encounter:

1. Continuous
1. Ordinal
1. Categorical
1. Missing 
1. Censored

Continuous variables are anything measured on a quantitative scale that could be any fractional number. An example
would be something like weight measured in kg. [Ordinal data](http://en.wikipedia.org/wiki/Ordinal_data) are data that have a fixed, small (< 100) number of levels but are ordered. 
This could be for example ordinal categories such as quality, with levels _poor_, _fair_, _good_. [Categorical data](http://en.wikipedia.org/wiki/Categorical_variable) are data where there
are multiple categories, but they aren't ordered. One example would be algorithm: A or B. [Missing data](http://en.wikipedia.org/wiki/Missing_data) are data
that are missing and you don't know the mechanism. You should code missing values as `NA`. [Censored data](http://en.wikipedia.org/wiki/Censoring_(statistics\)) are data
where you know the missingness mechanism on some level. Common examples are a measurement being below a detection limit
or a patient being lost to follow-up. They should also be coded as `NA` when you don't have the data. But you should
also add a new column to your tidy data called, "VariableNameCensored" which should have values of `TRUE` if censored 
and `FALSE` if not. In the code book you should explain why those values are missing. It is absolutely critical to report
to the analyst if there is a reason you know about that some of the data are missing. You should also not [impute](http://en.wikipedia.org/wiki/Imputation_(statistics\))/make up/
throw away missing observations.

In general, try to avoid coding categorical or ordinal variables as numbers. When you enter the value for a column _Problem.Instance_ in the tidy data, it should be entered as "Problem.1" or "DTLZ3", and not simply "1" or "3". This will avoid potential mixups about which direction effects go and will help identify coding errors. 

### The instruction list/script

You may have heard this before, but [reproducibility is kind of a big deal in computational science](http://www.sciencemag.org/content/334/6060/1226).
That means, when you submit your paper, the reviewers and the rest of the world should be able to exactly replicate
the analyses from raw data all the way to final results. If you are trying to be efficient, you will likely perform
some summarization/data analysis steps before the data can be considered tidy. 

The ideal thing for you to do when performing summarization is to create a computer script (in `R`, `Python`, `Matlab` or something else) that takes the raw data as input and produces the tidy data you are sharing as output. You can try running your script a couple of times and see if the code produces the same output. 

Even if the summarization script is not formalized (e.g., if some data is entered by hand), a list of steps should be provided. An example list would be:

1. Step 1 - take the raw file, run version 3.1.2 of summarize software with parameters a=1, b=2, c=3
1. Step 2 - run the software separately for each sample
1. Step 3 - take column three of outputfile.out for each sample and record it as the corresponding row in the output data set.

You should also include information about which system (Mac/Windows/Linux) you used the software on and whether you 
tried it more than once to confirm it gave the same results. Ideally, you will run this by a fellow student/labmate
to confirm that they can obtain the same output file you did. 

What you should expect from me
====================

When you turn over a properly tidied data set it dramatically decreases the data analysis workload for me. So hopefully
your results will be ready much sooner. If I have the time I will check your recipe, but this is not guaranteed. Also, I may need to ask questions about the steps you performed, and try to confirm that they can obtain the same tidy data that you did with, at minimum, spot checks.

You should then expect from me:

1. An analysis script that performs the analysis, with the computer code used and any other supplementary material;
1. All output files/figures they generated. 
1. Depending on what we agreed upon, the _Experimental Results_ section of our paper.

About this document
====================

Original document by [Jeff Leek](http://biostat.jhsph.edu/~jleek/), with contributions from [L. Collado-Torres](http://bit.ly/LColladoTorres) and [Nick Reich](http://people.umass.edu/nick/).

Edited version by [Felipe Campelo](http://github.com/fcampelo)


