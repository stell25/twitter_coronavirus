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

<img src="reduced.country%23coronavirus.png" />

<img src="reduced.country%23코로나바이러스.png" />

<img src="reduced.lang%23coronavirus.png" />

<img src="reduced.lang%23코로나바이러스.png" />

**Task 4: Alternative Reduce**

I also used `alternative_reduce.py` to generate a plot that shows the daily trends of the use of certain hashtags over 2020, running
```
$ python3 alternative_reduce.py "HASHTAG" --data_dir=OUTPUT_FOLDER --output_path=OUTPUT_NAME
```

<img src=corona_plot.png />

