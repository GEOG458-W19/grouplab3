# Group Lab 3

## Comparative Urban Change in US States

IMPORTANT: As mentioned in class Wednesday, Feb. 13th,due to snow cancellations, we will be cutting down the requirements for this lab. You can select either Part 2 or Part 4 to cut from the assignment. You should still be doing Parts 1 and 3, plus one more of your choice. 

This lab will be a little different from previous ones, but the basic idea of narrating your findings to a viewer is the same. Technically, this lab is written to be completed in python, which means that your final product will (probably, see below) not be an R Markdown HTML document, but instead an IPython Notebook (.ipynb) file uploaded to Github. You will need to make use of the IPython Notebook Markdown options, alternating cells of Markdown description with cells containing your code. But in the Notebook for python code, you don’t have to worry about hiding the code itself, everything will be fully visible in an .ipynb file by default, and you will need to make sure to “Run” each cell of code in the document to get the proper output.

Do note that, even though some of this processing might involve arcpy, you are not allowed to use the ArcGIS GUI for any data processing, including spatial analysis, modifying the attribute tables, clipping, etc. That said, you can use the ArcGIS GUI for checking your work, and you can certainly test code in the arcpy window (though it should all ultimately be included in your .ipynb file).

**That said, you may (individually or as a group) decide to do part or all of this lab in R if you feel comfortable doing so. If you choose to do so as a group, you will submit an R Markdown HTML document as we have done for previous labs. If you choose to do your individual work in R, but your group is using python, it is your responsibility to make sure that your completed product is transferrable to the IPython notebook, OR into the a final R Markdown HTML via the reticulate package (Links to an external site.)Links to an external site., which can run python within R Markdown. The crossover material might include maps, graphics, or simply processed data. If you use an .ipynb file as your finished product, you will also need to cite the related R Script in your IPython notebook file and link to its location on Github (if you use reticulate, it’s not necessary, I can read the code in your .rmd file, which allows for python code chunks). In real-life data analysis, you will often just use whatever method is easiest or most familiar to complete a certain task, stitching the end result together at the end in a way that makes your code and overall process transparent.**

### Part 1 - Data Description and Overview

As you’ve done in previous labs, I’d like you to begin with a more theoretical section commenting on your data, potential problems with it, and some broader context. In this case, you’ll be working with population data, some of which will be provided for you, and some of which you will be gathering yourselves. You will then be writing code to determine both the level of urbanization of given areas, as well as change in urbanization over time. So, in this introductory data description section, you need to:

1. Go look up how the US defines an area as “urban” or not (specifically, look at how Census block groups relate to this measurement), and write a summary for the viewer. Here’s a HINT (Links to an external site.)Links to an external site..

2. Go get a couple other examples, at least one from another country, and, if you’d like, you could also mention some other definitions that might differ at a more theoretical

3. Give an overview of the data you’ll be using below, and some potential issues with it (it will mostly be US Census data, so this will be similar to Group Lab 2, but focused on more demographic, rather than economic, data).

4. Give a summary of your findings here, think of it like an executive summary at the front of a report. Note that this will involve doing parts of this written portion last, despite the fact that it should lie at the front/top of your document.

### Part 2 - Basic Processing with Python

First, you’ll be starting with this shapefile (Links to an external site.)Links to an external site., remember to unzip the whole thing into your working directory, since the .shp is related to the others. You’ll also need this table (note that it’s a .dbf file, which might affect how you read it into python **or R**).

The shapefile is from Washington State’s Office of Financial Management—more specifically, their Small Area Estimates Program (SAEP) (Links to an external site.)Links to an external site.—and you can read the metadata for the file here (Links to an external site.)Links to an external site.. It’s a very big file, including all the census block groups in the state. You are all probably familiar with how much of a pain it can be to work with such files in ArcMap, the “drawing” of your layers often crashing the entire program. That’s an ideal time to transfer a file into something like R or python, which can operate on large files without issue (note, however, that when you do map the data, it will still take some time to render, but it shouldn’t crash if you’ve done it correctly).

Just to get acquainted with looping and batch processing in python, we’ll start with some basic aggregation and batch file creation. You’ll frequently be faced with the need to create a series of subsets of a large file/database and to save these subsets systematically, so let’s learn how to do that here, even though we will be producing something that might be easy to find elsewhere:

For each county in Washington, create a shapefile whose filename is the county name and whose contents are the polygons for the block groups within that county. This means that you will end up with as many shapefiles as there are counties in WA. Each such file should be named after a county and contain only the block groups from that county. You can use arcpy to complete any of this process, but the very last part where you save the files may be a bit easier with Geopandas. If you decide to use Geopandas, see this page (Links to an external site.)Links to an external site. for some instructions on reading and writing shapefiles. NOTE: if for some reason you absolutely cannot get it to save as .shp, it’s okay here to save as GeoJSON or another alternate format (like .kml), you don’t necessarily have to actually use these for mapping.

Print out a ranked list, in descending order, of the ten largest total populations in 2017 of counties in Washington, according to this dataset.

TIP 1: When you are writing loops, you’re going to have to start breaking down the process into abstract stages. If using arcpy, also think of how you’d accomplish this task in ArcMap, and see if you can translate that.

TIP 2: The discussion of Cursors from the Zanderbergen book (Chapter 7, but also think of the basic ideas brought up in 1-6) will be incredibly helpful if you are using arcpy. Don’t forget to delete your cursors in your loops though!

TIP 3: It might be helpful to use nested loops, and, in your loops, to use: sum ( my_list ), also refer to the Zanderbergen book, page 141, for an example of how to create a list out of the entries of a particular field in arcpy:

```
my_list = []
cursor = arcpy.da.SearchCursor(my_featureclass, ["MYFIELD"])
for row in cursor:
  my_list.append(row[0])
```

### Part 3 - Urban vs Rural

Using the same data, you need to decide on what makes a block group polygon 'urban' and what makes it 'rural'. This is a conceptual question and there are several ways in which this can be done. You'll have to make a decision on how you want to do it, and you should explain your choice in relation to the broader theory about how “urban” is categorized in the US from Part 1. Once you make this decision, you’ll need to:

1. Add a column which gives each block group a value of “urban” or “rural” (you can also say “non-urban” depending on the definition you use) in the most recent year.

2. Write code that calculates and prints out to the screen what percentage of the population of the state is urbanized in the most recent year.

3. Write code that calculates and prints out to the screen what percentage of the land area of the state is urbanized in the most recent year.

4. Add a column to the shapefile's data table for whether block groups have 'urbanized', 'no change in category', or 'deurbanized' over the previous decade (i.e. if most recent year is ’18, then ’08-’18), using the above classification for urban/rural you decided on. Represent it as a categorical change (a string of text), not in terms of some numerical change. Fill in the field for all rows! 

5. Write code that calculates and prints out how many block groups urbanized and how many deurbanized over the previous decade.

6.Make a map (static or interactive is fine) that portrays an essential perspective on some aspect of your results (e.g., show which areas are urbanized and which are not, or choose a representative area to zoom in on, use the shapefiles created above to show us a particular county of interest). NOTE: you can use GeoPandas here for ease, but it’s also perfectly okay to use the methods already familiar in R, it will just involve some back-and-forth with the shapefile. But if you use R, you’ll need to save the map as an image file, and then figure out how to visualize that in the IPython notebook. You might find this page on working with images in python (Links to an external site.)Links to an external site. You can also make interactive maps in python with folium (Links to an external site.)Links to an external site., here is a page (Links to an external site.)Links to an external site. with some links to examples.

TIP: It will definitely help to read through the GeoPandas and Folium example .ipynb file provided on Canvas. If you read it closely, it can really make Part 2 very easy for you, but it also shows a bunch of basic stuff that will help with Part 3. Note, however, that you’ll have to re-run the code to get the interactive maps to show! That means you have to run it in an environment with all the packages, etc. Also note that some of the references might be off, it’s from last year, so ignore the reference to the “First Arcpy Lab” for example.

~~### Part 4 - How does Washington Compare?

~~HINT: try state government websites, since they may keep better data, in the same way Washington does. For older data, you may want to check some of the pre-joined TIGER shapefiles located here (Links to an external site.)Links to an external site.. Also remember that you *can* use R, which means you could draw from the tigris, acs, and tidycensus libraries if you’d like. You can even just use these to download the correct shapefile if you find it easier, saving that file then opening it in python. Personally, I’m not as familiar with comparable tools in python, though it does appear some have been written, such as this one (Links to an external site.)Links to an external site., which fetches census shapefiles, and this function (Links to an external site.)Links to an external site., which works with the census reporter API. There is a pandas-based library called cenpy (Links to an external site.)Links to an external site. which works similar to the acs package in R, see a run-through here (Links to an external site.)Links to an external site., though note that you’ll still have to join that with spatial data. For a general run-through of using GeoPandas with census data, see this sample IPython Notebook (Links to an external site.)Links to an external site..

~~Now, simply do all the same steps from Part 3 (though step 5 is not necessary to repeat here, since the total number of block groups in a state will vary), and, in your narration, explain how they compare to Washington. Similarly, create a representative map of this new state, or a segment of it.

~~Finally, write a brief summary of the state-to-state comparison. What kinds of conclusions can you draw from this data? 

### Turn In

For this assignment, I just need everyone to give me a link to the github as a submission. Unless: **If you completed this with R Markdown and produced an HTML, please submit that HTML in the same way we have for previous labs: i.e., have one group member give the HTML + a comment w/ the link, everyone else submit the link**
