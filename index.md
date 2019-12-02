---
title: "Creating time series plots"
subtitle: Creating and customising time series plots 
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

An important thing about time series data is that new measurements don't overwrite previous ones, so data is accumulated over time which means when plotting time series data, you might be dealing with some very large datasets! But don't worry, in this tutorial we'll learn how to plot a graph from a large dataset - and if you want to go into more detail check out <a href="https://ourcodingclub.github.io/2017/03/20/seecc.html" target="_blank">this tutorial</a> on working efficiently with large datasets!


## <a name="section2"> 2. Uploading data </a>
Once you've cloned or downloaded the repository, open up RStudio (R and RStudio are different so be careful!) 
If you've cloned the repo, make sure you have the relevant project opened up (add pic). 

If you've downloaded, just make sure you save any files into the same folder. 

At the start of your script make note of: 
```
# Main objective
# Your name
# Email
# Date
```

Now you need to load the relevant packages for this tutorial - we are installing multiple packages at once so we use `c()` to link the list of packages in a chain so the `install.packages()` function is applied to all the packages. It just saves us having to write out separate lines of code for each package!
```R
install.packages(c("dplyr", "ggplot2", "ggthemes", "gridExtra", "colourpicker"))
```


Now we will load the packages and set the working directory:
```R
library(dplyr) # To manipulate data
library(ggplot2) # To plot graphs
library(ggthemes) # To use custom themes
library(gridExtra) # To arrange mutiple plots on one grid
library(colourpicker) # To choose custom colours

# Set the working directory (skip this step if you cloned)
setwd("C:/folder_name") # It's best to save the repo folder in a root folder - somewhere easy to access
```

And now time to load the data!
```R
FAO_data <- read.csv("Tutorial/Datasets/FAO_data.csv")
```




## <a name="section3"> 3. Tidying data </a>


Viewing the data

We can view the first 6 rows of data by using the `head()` function. 

```R
head(FAO_data)
```

We can see it is already in long format so we don't have to edit it. One way we know this is because each row represents one time point, e.g. each row is a different year, whereas in wide format each year would be a different column. 

See section 2 of<a href="https://ourcodingclub.github.io/2017/01/06/data-manip-intro.html" target="_blank">this tutorial</a> on how to convert data from a wide to long format! 


```R
# Here we can check the structure of the data is like, we can see what types of data we have
str(FAO_data)

```
What can you learn from using `str()` function? 

### Data types
In data frames, there are several different modes which data can be stored as (modes are essentially the different types of data.)

<center><img src="/Tutorial/Images/data_frame.png" width="900;"/><center>

##### - __Character (chr)__ - strings (which are made of letters/words)
##### - __Numeric (num)__ - numbers with decimal values 
##### - __Integer (int)__ - whole numbers
##### - __Factor__ - categorical variables

Each column in a data frame can hold a different mode of data, but each column can only contain one data mode. So, even if there's more than one data mode in a column, the whole column will be stored as a single data mode. 
```R
# Example of a data frame with multiple modes
x <- c(2, 4, "a", 5, 6, 3, 9)               # There's a character and integer values
str(x)                                      # Checking the data mode
chr [1:7] "2" "4" "a" "5" "6" "3" "9"       # The data has been stored as a character
```

When plotting time series data, it is important that the data in the time/date column is stored as an integer or as a date. (To make sure the data points are plotted in correct time order?)

You can check the data type with the class() function:
```R
class(FAO_data$Year)                        # $ lets you choose which column you want to apply the function to (dataset_name$column_name)


[1] "integer"                               # The year data is stored as an integer
```

If the data in the time/date column is not stored as an integer or as a date you can change it by: 
```R
FAO_data$Year <- as.integer(FAO_data$Year)  # Using the as.integer() function to convert the data in the Year column to integer
```

If the data in your time/date column is written in this form (or something similar): `"28/01/2008", "21/05/2011"`
You can store the data as a 'date' class. 
You need to tell R what the format of the date is - specify which value is the day, month and the year. 

```R
FAO_data$Year <- as.date(FAO_data%Year, format = %d/%m/%Y) # We used '/' between the components of the date because the dates are written with '/' (21/05/2011) 
                                                           # If our dates were written with '-' e.g. 21-05-2011, the format would have been = %d-%m-%Y

                                                           # When writing the format, we use a capital letter, Y, if the year has 4 digits (e.g. 2011), and a lowercase, y, if it has two digits (e.g. 11)

# Check the data type:
class(FAO_data$Year)
## [1] "Date"

# Check the data:
head(FAO_data$Year)
## [1] "2008-01-28" "2011-05-21"
```

Looking at the data, we can see a column called "Item" which represents the type of land use. You could rename this column to give it a more relevant name such as "land_type"
```R
FAO_data <- rename(FAO_data, Land_type = Item)
```


## <a name="section4"> 4. Creating plots </a>

We're interested in seeing the change in land cover types in a given country, and as the dataset has data from many countries you can choose one or simply use the country in the example below! 

We'll be plotting data from North Macedonia and we'll start by creating basic plots of change in forest and agricultural land cover over time. 

```R
# The filter() function lets you choose which rows you want to column in a specified column and the select() function lets you choose which columns to keep
macedonia_forest <- FAO_data %>%                                                # Filtering out forest data
  filter(Area == "North Macedonia", Land_type == "Forest land") %>%             # Filtering for forest cover data from North Macedonia
  dplyr::select(Year, Value)                                                    # We are keeping the year and value columns
macedonia_forest                                                                # Checking everything filtered correctly! 

# Basic ggplot for forest
# Hint: put brackets around the whole chunk of code to print the plot automatically
(forest_plot <- (ggplot(macedonia_forest, aes(x=Year)) + 
   geom_line(aes(y=Value)) +                                                    # geom_line() function is for line plots
   labs(y = "Land cover area (%)", title = "Forest cover change over time")))   # Changing the y axis and adding a title to distinguish the plots



# We'll repeat this for agricultural land data!

# Filtering out agricultural data
macedonia_agri <- FAO_data %>%                                                  # We use "agri" as it's more concise 
  filter(Area == "North Macedonia", Land_type == "Agricultural land") %>% 
  dplyr::select(Year, Value)
macedonia_agri

# Basic ggplot for agricultural land
(agri_plot <- (ggplot(macedonia_agri, aes(x=Year)) + 
   geom_line(aes(y=Value))+
   labs(y = "Land cover area (%)", title = "Agricultural land cover change over time")))
   
   
   
# Make a panel of the two plots (allows us to compare them side by side)
grid.arrange(forest_plot, agri_plot, ncol = 2)
```



<img src="/Tutorial/Images/basic_panel_plot.png" width="900;"/>


These plots can be improved significantly by customisation, and in the next section you'll learn how to do this! 



## <a name="section5"> 5. Customising the plots </a>

### Formatting titles, subtitles, captions, and axis labels

Below you'll learn how to add extra detail to your plot and customise the font type, size, colour and alignment!

```R
(ggplot(macedonia_forest, aes(x=Year)) + 
   geom_line(aes(y=Value)) +
   labs(title = "Forest cover change over time",              # labs() function is for adding titles  and axis labels to your plot
        subtitle = "North Macedonia 1992 - 2016",             # The subtitle provides greater detail on the data being plotted
        caption = "Data from the FAO",                        # Adding a caption
        x = "\n Year", y = "Land cover area (%) \n") +        # The '\n' adds a space between the axis label and the axis
   theme(axis.title = element_text(size = 12, face = "plain"),# 'size' = size of axis titles & 'face' = text type e.g. "plain", "bold", "italic", or "bold.italic"
         plot.title = element_text(hjust = 0.5, face = "bold"),  # 'hjust' = to centre the graph title 
         plot.subtitle = element_text(hjust = 0.5, colour = "darkblue"),  # Aligning subtitle in the centre & 'colour' = changes text colour
         axis.text.x = element_text(size = 10, face = "italic"), # To edit size of x axis numbers & change text type
         axis.text.y = element_text(size = 10, face = "italic"))) # To edit y axis numbers
```

The argument, `hjust` allows you to change the horizontal position of something, in this case we're editing the title and subtitle. If the `hjust` value is = 0, the titles will be aligned on the left. The higher the value, the more the titles will move to the right. 
You could also use `vjust` in your code to change the vertical alignment of plot elements! 

<centre><img src="/Tutorial/Images/titles_plot.png" width="650;"/><centre>



### Formatting grid lines and background

There are set themes available from ggplot2 which you can use to change the colour and style of the grid. Some of the more distinct themes are `theme_light()`, `theme_bw()`, `theme_minimal()` and `theme_classic()`. 

- `theme_light()`: creates a grid with a white background and grey grid lines
- `theme_bw()`: white background, light grey grid lines and a black outline around the grid.  
- `theme_minimal()`: white background, light grey lines and no outline around the grid. 
- `theme_classic()`: white background, no grid lines but has the x and y axis. 


Now we'll generate plots with each of the themes to compare them!

```R
# theme_light:
(agri_light <- (ggplot(macedonia_agri, aes(x=Year)) +                         # Making an object for each plot in order to make a panel of the plots
                 geom_line(aes(y=Value))+
                 theme_light()+                                               # Specifying the theme! 
                 theme(plot.title = element_text(hjust = 0.5)) +              # Centering the title
                 labs(y = "Land cover area (%)", title = "theme_light ()")))

# theme_bw: 
(agri_bw <- (ggplot(macedonia_agri, aes(x=Year)) + 
                 geom_line(aes(y=Value))+
                 theme_bw()+
                 theme(plot.title = element_text(hjust = 0.5)) +
                 labs(y = "Land cover area (%)", title = "theme_bw ()")))

# theme_minimal:
(agri_minimal <- (ggplot(macedonia_agri, aes(x=Year)) + 
                 geom_line(aes(y=Value))+
                 theme_minimal()+
                 theme(plot.title = element_text(hjust = 0.5)) +
                 labs(y = "Land cover area (%)", title = "theme_minimal ()")))

# theme_classic:
(agri_classic <- (ggplot(macedonia_agri, aes(x=Year)) + 
                 geom_line(aes(y=Value))+
                 theme_classic()+
                 theme(plot.title = element_text(hjust = 0.5)) +
                 labs(y = "Land cover area (%)", title = "theme_classic ()")))

# Make a panel of the plots
grid.arrange(agri_light, agri_bw, agri_minimal, agri_classic, ncol = 2)       # List the objects 
                                                                              # 'ncol' = how many columns you want in your panel
```

<img src="/Tutorial/Images/theme_facet.png" width="800;"/>

As you can see, `theme_light()` and `theme_bw()` are quite similar, however theme_bw() draws more attention to the plot because the grid lines are more faded.

The lack of grid lines on the `theme_classic` plot might make it harder to accurately read values from points on the plot, however if you're going for a more simple theme this one is probably most suitable! 
* Talk briefly about theme_minimal()!!

Each theme has different pros and cons, so choose your theme based on what you want to focus on and the purpose of the figure.


It's also possible to customise elements of the grid manually! 

```R
# Custom format:
(ggplot(macedonia_agri, aes(x=Year)) + 
             geom_line(aes(y=Value))+
             theme(panel.background = element_rect(fill = "#D4E9FF", colour = "gray"),                 # 'fill' = background colour & 'colour' = grid outline colour
               panel.grid.major = element_line(size = 0.9, linetype = "dashed", colour = "white"),     # Change size of major grid lines & remove minor lines & change line type & colour' 
               panel.grid.minor = element_blank()) +                                                   # 'element_blank()' = removes grid lines 
             labs(y = "Land cover area (%)", title = "Agricultural land cover change over time")))
```

<img src="/Tutorial/Images/grid_plot.png" width="650;"/>

`panel.grid.major` and `panel.grid.minor` lets you change the size, colour and line type of grid lines. You can even choose to remove grid lines with the `element_blank()` function. 

You can use the colour picker addin to choose custom colours - just make sure you installed and loaded the `colourpicker` package! 

<img src="/Tutorial/Images/colourpicker.png" width="650;"/>


### Formatting the plot line
You can customise your plot even further by editing the colour, size and type of line of your plot! 

```R
# Formatting plot line colour + size
(ggplot(macedonia_forest, aes(x=Year)) + 
   geom_line(aes(y=Value), colour = "#40E0D0",                                   # You can use the colour picker to choose colours
                           size = 1.0, linetype = "longdash") +                  # Editing the size and type of line
   labs(y = "Land cover area (%)", title = "Forest cover change over time"))
```

<img src="/Tutorial/Images/lines_plot.png" width="650;"/>

There are several types of lines you can choose from which are illustrated below:


<img src="/Tutorial/Images/line_types.png" width="650;"/>

###### Image from Cookbook for R by Winston Chang. License: https://creativecommons.org/licenses/by-sa/3.0/legalcode 


### Formatting axis labels 

You can change how frequently tick marks appear and the range of values that are displayed along the axes. 
```R
(ggplot(macedonia_agri, aes(x=Year)) + 
                 geom_line(aes(y=Value))+
                scale_x_continuous(breaks=seq(1990, 2015, 2)) +                                         # To edit x axis labels
                theme(axis.text.x = element_text(angle = 45, vjust = 0.8))                              # 'angle' = to rotate the labels by 45 degrees & 'vjust' = move labels down so they're not overlapping each other and the grid
               labs(y = "Land cover area (%)", title = "Agricultural land cover change over time")) 
```

When editing the axis labels, the first and second numbers from the code `(1990, 2015, 2)` are the start and end year that you want to display. The 3rd number is how often you want the years to appear (e.g. so every 2 years). 

<img src="/Tutorial/Images/xaxis_labels.png" width="650;"/>


### Using ggthemes()

You can also customise your plots by using themes from the `ggthemes` package! There are themes available which replicate the style of plots used in The Wall Street Journal, The Economist and Excel (just to name a few!)

```R
# The Wall Street Journal theme
(wsj <- (ggplot(macedonia_forest, aes(x=Year)) + 
                   geom_line(aes(y=Value)) +
                   theme_wsj() +                                                 # Specifying the theme
                   labs(y = "Land cover area (%)", title = "theme_wsj()")))


# Theme based on the Economist
(economist <- (ggplot(macedonia_forest, aes(x=Year)) + 
                   geom_line(aes(y=Value)) +
                   theme_economist() +
                   labs(y = "Land cover area (%)", title = "theme_economist()")))


# Google docs default plot theme
(google <- (ggplot(macedonia_forest, aes(x=Year)) + 
                   geom_line(aes(y=Value)) +
                   theme_gdocs() +
                   labs(y = "Land cover area (%)", title = "theme_gdocs()")))


# Tufte theme - based on the work of Edward Tufte
(tufte <- (ggplot(macedonia_forest, aes(x=Year)) + 
                   geom_line(aes(y=Value)) +
                   theme_tufte() +
                   labs(y = "Land cover area (%)", title = "theme_tufte()")))



# Stata
(stata <- (ggplot(macedonia_forest, aes(x=Year)) + 
                   geom_line(aes(y=Value)) +
                   theme_stata() +
                   labs(y = "Land cover area (%)", title = "theme_stata()")))


# Fivethirtyeight
(fte <- (ggplot(macedonia_forest, aes(x=Year)) + 
             geom_line(aes(y=Value)) +
             theme_fivethirtyeight()+
             labs(y = "Land cover area (%)", title = "theme_fivethirtyeight()")))




# Make a panel of the plots
grid.arrange(wsj, economist, google, tufte, stata, fte, ncol = 3)
```

<img src="/Tutorial/Images/ggtheme_plots.png" width="800;"/>

These themes give...

### Multiple line plots
If you want to compare how different types of land cover vary over time, you can create a multiple line plot. For example, we could plot forest and agricultural land cover over time on the same plot! 

We would create a new object which only filters for data from North Macedonia and we'd distinguish the lines by what the land type is.

```R
# The only land types in the dataset are forest and agricultural land, so we 
# only have to filter for North Macedonia 
macedonia_both <- FAO_data %>% 
  filter(Area == "North Macedonia") %>% 
  dplyr::select(Land_type, Year, Value)

# Multiple line plot
(macedonia_plot <- ggplot(macedonia_data, aes(Year, Value, colour = Land_type)) +
    geom_line() +
    theme_bw() +
    labs(title = "Land cover change over time in North Macedonia", 
         x = "\n Year", y = "Percentage of land type (of total land area) \n"))
```

Further reading:
- http://www.sthda.com/english/wiki/installing-and-using-r-packages 

