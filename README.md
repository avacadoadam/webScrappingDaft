# Web Scrapping using Python & Scrapy on Daft.ie
## This repo contains the project resulting from [this](https://www.youtube.com/watch?v=cPx621bqgkY&t=14s) Youtube tutorial published by Adam Sever.

It consists basically on fetcing information about rent price, location, size and number of views for each comercial property listed a the daft.ie website. This info is fetched using XPath reference.

The video did not cover how to add the MongoDB pipeline to the project, although the source code can be found on the blog-link listed below.

[Blog page](https://www.cryt.ie/blogs/Scrapy.html) containing further info about the tutorial. 
______________________________________________________________________________________________________________________________

Although the source code for the original project is available [here](https://github.com/avacadoadam/Webscraping-tutorial), I couldn't make my scrapper work.

The output from running the command ```scrapy crawl daft -t csv -o data.csv``` is available at [outputCLI.txt](https://github.com/laisbsc/webScrappingDaft/blob/master/outputCLI.txt) above. This command generates an empty .csv file (available at [data.csv](https://github.com/laisbsc/webScrappingDaft/blob/master/data.csv)) instead of outputting the scrapped data from the website.

Further features to be implemented on this project:
 - use Postman to improve the structure of the website;
 - use MongoDB pipeline to automate the insertion of data into the output file;
 - use matplotlib to visualise the most significant insights from the scrapped data (data science).
 
 
# Possible solutions to no output:

## Checking Command used in log.
(ENV) (base) C:\Users\laisb\python-virtual-environments\scrapyTutorial\scrapyWebTut>scrapy crawl daft -t csv -o data.csv
*The command is correct.*

## Splash reading robots.txt bug
```python
2020-02-02 20:36:01 [scrapy.core.engine] DEBUG: Crawled (200) <GET https://www.daft.ie/robots.txt> (referer: None)
2020-02-02 20:36:01 [scrapy.downloadermiddlewares.retry] DEBUG: Retrying <GET http://0.0.0.0:8050/robots.txt> (failed 1 times): An error occurred while connecting: 10049: The requested address is not valid in its context..
2020-02-02 20:36:02 [scrapy.downloadermiddlewares.retry] DEBUG: Retrying <GET http://0.0.0.0:8050/robots.txt> (failed 2 times): An error occurred while connecting: 10049: The requested address is not valid in its context..
2020-02-02 20:36:02 [scrapy.downloadermiddlewares.retry] DEBUG: Gave up retrying <GET http://0.0.0.0:8050/robots.txt> (failed 3 times): An error occurred while connecting: 10049: The requested address is not valid in its context..
```
It is interesting that the downladermiddlewares tries to access www.daft.ie robots.txt through http://0.0.0.0:8050. This only occurs when using splashRequest. There are workarounds however which can be found in https://github.com/scrapy-plugins/scrapy-splash/issues/180. Which involves changing the build order.
However I do not believe this is the issue you face, but this can be debugged by setting ROBOTSTXT_OBEY to False in settings.py 
and check if the result is differnet.
```
ROBOTSTXT_OBEY = False
```
On my Env I did not need to.


## Possible Splash configuration Error:

When I turned off my splash service my output was similar to yours.
splash uses docker and on linux the command is 
```
sudo docker run -p 8050:8050 scrapinghub/splash
```
https://docs.docker.com/engine/installation/windows/ to setup docker on windows.
*I added a note to add documentation for windows installation on the scrapy-splash repo*

To check if splash is running vist the http://127.0.0.1:8050/ on a browser where you should see a 
webpage that looks like 

![alt text](https://github.com/avacadoadam/webScrappingDaft/blob/master/splashWebpageExample.png)


*I believe this may be the issue*

## Also included a slight code update
The code in parse function in /spiders/daft.py
```python
        yield rent_price
        yield location
        yield size
        yield how_many_times_views
```
Should return a Request, BaseItem, dict or None
So I updated it to 
```python
        yield {
            'rent_price': rent_price,
            'location': location,
            'size': size,
            'how_many_times_views': how_many_times_views
        }
```
hope that helps!
scrapy : 1.8.0
OS: Ubuntu 18.04.2 LTS
Splash v3.3.1

Feel free to ask any questions or another scraping project you would like help with!
