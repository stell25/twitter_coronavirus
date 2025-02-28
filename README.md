# Coronavirus twitter analysis

I scaned all geotagged tweets sent in 2020 to monitor for the spread of the coronavirus on social media. This project utilizes the mapreduce divide-and-conquer paradigm process to analyze trends in the usage of coronavirus related hashtags by country and language. 

## Background

**About the Data:**
The data was stored in our class's lambda server's `/data/Twitter dataset` folder, which contained all geotagged tweets that were sent in 2020. In total, there are about 1.1 billion tweets in this dataset.

**About MapReduce:**

I followed the [MapReduce](https://en.wikipedia.org/wiki/MapReduce) procedure to analyze these tweets.

**1. Mapping**
   The file `./src/map.py` processes each zip file with geotagged tweets, counting language and country of origin of tweets that utilize the specific hashtags. The output of `map.py` are two files, one that ends in `.lang` for the language dictionary, and one that ends in `.country` for the country dictionary.
   The file `./src/reduce.py` takes as input the outputs from the `map.py` file above and reduces them together.
    I created a shell script `run_maps.sh` that loops over each file in the dataset and runs `map.py` on that file, and used `reduce.py` to combine the outputs.
```
$ python reduce.py --input_paths output_folder/geoTwitter*.lang --output_path=reduced.lang
```

**2. Visualize**

I then used `visualize.py` to generate bar graphs of the results, running
```
$ python visualize.py --input_path=PATH --key=HASHTAG
```
I generated these four plots below.

<img src=corona_plot.png />

**Task 4: Alternative Reduce**

Create a new file `alternative_reduce.py`.
This file should take as input on the command line a list of hashtags,
and output a line plot where:
1. There is one line per input hashtag.
1. The x-axis is the day of the year.
1. The y-axis is the number of tweets that use that hashtag during the year.

Your `alternative_reduce.py` file have to follow a similar structure to a combined version of the `reduce.py` and `visualize.py` files.
First, you will scan through all of the data in the `outputs` folder created by the mapping step.
In this scan, you will construct a dataset that contains the information that you need to plot.
Then, after you have extracted this information,
you should call the appropriate matplotlib functions to plot the data.

> **HINT:**
> The specifications for this program and plot are intentionally underspecified
> (similar to how many real-world problems are underspecified).
> Feel free to ask clarifying questions.

**Task 5: Uploading**

Commit all of your code and images output files to your github repo and push the results to github.
You must:
1. Delete the current contents of the `README.md` file
1. Insert into the `README.md` file a brief explanation of your project, including the 4 generated png files.
    This explanation should be suitable for a future employer to look at while they are interviewing you to get a rough idea of what you accomplished.
    (And you should tell them about this in your interviews!)

## Submission

Upload a link to you github repository on sakai.
I will look at your code and visualization to determine your grade.

**Grading:**

The assignment is worth 32 points:

1. 8 points for getting the map/reduce to work
1. 8 points for your repo/readme file
1. 8 points for Task 3 plots
1. 8 points for Task 4 plots

The most common ways to miss points are:
1. having incorrect data plotted (because the map program didn't finish running on all of the inputs)
1. having illegible plots that are not "reasonably" formatted

Notice that we are not using CI to grade this assignment.
There's two reasons:

1. You can get slightly different numbers depending on some of the design choices you make in your code.
    For example, should the term `corona` count tweets that contain `coronavirus` as well as tweets that contain just `corona`?
    These are relatively insignificant decisions.
    I'm more concerned with your ability to write a shell script and use `nohup`, `&`, and other process control tools effectively.

1. The dataset is too large to upload to github actions.
    In general, writing test cases for large data analysis tasks is tricky and rarely done.
    Writing correct code without test cases is hard,
    and so many (most?) analysis of large datasets contain lots of bugs.
