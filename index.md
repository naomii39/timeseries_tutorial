---
title: "Plotting time series data"
subtitle: Creating and customising plots!
author: "Naomi Gunasekara"
date: 2 December 2019
layout: default
---

<center> <img src="https://github.com/EdDataScienceEES/tutorial-naomii39/blob/master/Tutorial/Images/banner.png" /> <center>

## Tutorial aims and steps

#### <a href="#section1"> 1. What is time series data?</a>

#### <a href="#section2"> 2. Upload data</a>

#### <a href="#section3"> 3. Tidy data</a>

#### <a href="#section4"> 4. Create a plot with ggplot2</a>

#### <a href="#section5"> 5. Customising the plots</a>

#### <a href="#section6"> 6. Challenge yourself!</a>

### All the files you'll need for this tutorial can be found in <a href="https://github.com/EdDataScienceEES/tutorial-naomii39" target="_blank">this repository!</a>

#### Unsure about how to access the files? 

##### 1) Click on the link above
##### 2) Click the green button (see image below)
##### 3) You can either download the repo onto your computer _or_ you can clone the repo. Either way works fine! 
##### - The button in the red circle copies the link to the repo, which you can paste in RStudio to clone the repo!
##### - See this this <a href="https://ourcodingclub.github.io/2017/02/27/git.html" target="_blank">tutorial!</a> for how to clone a repo. 
##### - The folder named "Tutorial" has all the relevant files for this tutorial!
###### Downloading will give you a copy of the most recent version of the code - but with no history of past commits. Cloning will create a local copy (a copy on your laptop) of the GitHub repo - you _can_ see commit history, and can make and push changes to the GitHub repo. 

<center> <img src="https://github.com/EdDataScienceEES/tutorial-naomii39/blob/master/Tutorial/Images/clone_download.png" /> <center>

## What are the aims, what code do you need to achieve them?
There is more than one way to plot time series data in R. One way is to use the plot.ts() function, however this produces a very basic graph, so this tutorial will explain how to use ggplot2 which is more common and allows for greater plot customisation. 


## <a name="section1"> 1. What is time series data? </a>
Time series data has many definitions, but the most common one is that it is a sequence of data points in time order which measure how something changes over time. The gaps between measurements can be regular (e.g. measuring abundance of Arctic fox once every 6 months) or irregular (measuring ... whenever the river level is not too high). 
