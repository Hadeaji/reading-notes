# Web Scraping

Web scraping, web harvesting, or web data extraction

is data scraping used for extracting data from websites. Web scraping software may access the World Wide Web directly using the Hypertext Transfer Protocol, or through a web browser. While web scraping can be done manually by a software user, the term typically refers to automated processes implemented using a bot or web crawler. It is a form of copying, in which specific data is gathered and copied from the web, typically into a central local database or spreadsheet, for later retrieval or analysis.

Web scraping is a technique to automatically access and extract large amounts of information from a website, which can save a huge amount of time and effort.

## New York MTA Data

turnstile data from this site:

<http://web.mta.info/developers/turnstile.html>

Turnstile data is compiled every week from May 2010 to present, so hundreds of .txt files exist on the site. Below is a snippet of what some of the data looks like.

Important notes about web scraping:

1. Read through the website’s Terms and Conditions to understand how you can legally use the data. Most sites prohibit you from using the data for commercial purposes.
2. Make sure you are not downloading data at too rapid a rate because this may break the website. You may potentially be blocked from the site as well.

## Inspecting the Website

The first thing that we need to do is to figure out where we can locate the links to the files we want to download inside the multiple levels of HTML tags. Simply put, there is a lot of code on a website page and we want to find the relevant pieces of code that contains our data

- On the website, right click and click on “Inspect”. This allows you to see the raw code behind the site.
- Once you’ve clicked on “Inspect”, you should see console pop up.

If you click on this arrow and then click on an area of the site itself, the code for that particular item will be highlighted in the console

Now that we’ve identified the location of the links, let’s get started on coding!

## Python Code

We start by importing the following libraries

```
import requests
import urllib.request
import time
from bs4 import BeautifulSoup
```

Next, we set the url to the website and access the site with our requests library.

```
url = 'http://web.mta.info/developers/turnstile.html'
response = requests.get(url)
```

If the access was successful, you should see the following output

`<Response [200]>`

Next we parse the html with BeautifulSoup so that we can work with a nicer, nested BeautifulSoup data structure

```
soup = BeautifulSoup(response.text, “html.parser”)
```

We use the method .findAll to locate all of our `<a>` tags.

```
soup.findAll('a')
```

This code gives us every line of code that has an `<a>` tag

The information that we are interested in starts on line [x] as seen below. That is, the very first text file is located in line [x], so we want to grab the rest of the text files located below.

Next, let’s extract the actual link that we want. Let’s test out the first link.

```
one_a_tag = soup.findAll(‘a’)[x]
link = one_a_tag[‘href’]
```

This code saves the first text file, ‘data/nyct/turnstile/turnstile_180922.txt’ to our variable link.

```
download_url = 'http://web.mta.info/developers/'+ link
urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:])
```

Last but not least, we should include this line of code so that we can pause our code for a second so that we are not spamming the website with requests. This helps us avoid getting flagged as a spammer.

`time.sleep(1)`

Now that we understand how to download a file, let’s try downloading the entire set of data files with a for loop

```
# Import libraries
import requests
import urllib.request
import time
from bs4 import BeautifulSoup

# Set the URL you want to webscrape from
url = 'http://web.mta.info/developers/turnstile.html'

# Connect to the URL
response = requests.get(url)

# Parse HTML and save to BeautifulSoup object¶
soup = BeautifulSoup(response.text, "html.parser")

# To download the whole data set, let's do a for loop through all a tags
line_count = 1 #variable to track what line you are on
for one_a_tag in soup.findAll('a'):  #'a' tags are for links
    if line_count >= 36: #code for text files starts at line 36
        link = one_a_tag['href']
        download_url = 'http://web.mta.info/developers/'+ link
        urllib.request.urlretrieve(download_url,'./'+link[link.find('/turnstile_')+1:]) 
        time.sleep(1) #pause the code for a sec
    #add 1 for next line
    line_count +=1
```
