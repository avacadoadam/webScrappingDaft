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
