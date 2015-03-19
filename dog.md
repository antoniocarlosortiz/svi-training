#Email to Google Calendar

The main purpose of these scripts is to parse the events stated on subscription emails and automatically create events on the user's google calendar with regards to those.

To that, the whole app was divided into four distinct parts namely:
* Parse the specified emails using an IMAP connection to the mail client.
* Crawl the parsed emails to get only the necessary data in them.
* Place the scraped data inside a database.
* Call those scraped data from the database to serve as input to Google Calendar API.

## How to use
#### Dependendies Required
##### For the IMAP4 connection:
* datetime

##### For crawling the html files and using the database:
* scrapy
* SQLAlchemy
* psycopg2

##### For calling the google calendar api:
* httplib2
* apiclient
* oath2client

####Parsing the Email
Upon running the script in Email Scraping folder, Google will notify the user through email that an unsecured app is trying to access the account. This is because I still havent created the oath2 for this which Google recommends to have for every app that access its services. The user can simply change the setting of his account to allow 'less secure apps' just for this. Once that is done, running the script will get the specified email/s from your account and then save the html part of it to the local directory [out_dir](https://github.com/SiliconValleyInsight/svi-training-a/tree/master/code-samples/week4/SVI%20Email%20to%20Calendar/Email%20Scraping/out_dir).

On running this, simply input the following command in the terminal

    python email_scraper.py
This will then save the html contents of all the emails scraped inside the local directory

####Scrape data out of the Email
Once the html part of the email has been saved, we can now get the specific contents of the email using Scrapy using the app in the [my_scraper](https://github.com/SiliconValleyInsight/svi-training-a/tree/master/code-samples/week4/SVI%20Email%20to%20Calendar/my_scraper) folder.

For this to run, the user should have already created a postgres database and have all its details on the settings.py so the app can access it. Also, the local_url in the spider script should be changed to the user's local

To start this, go inside the my_scraper folder and run the command:
http://newcoder.io/scrape/intro/

    scrapy crawl crunchbaseevents_spider
Scrapy is the the name of the framework we use. Crawl is the command for the spider to start crawling the website, and crunchbaseevents_spider is the name of the spider specified in the spider script. Running the command will place all the scraped data inside the specified database

For a more detailed instructions, please go to the link below:

[Introduction to Web Scraping using Scrapy and Postgres](http://newcoder.io/scrape/intro/)

####Query from the database, edit the result, then use that to post events through the google calendar api.
On querying, simply run the script named  [database_query.py](https://github.com/SiliconValleyInsight/svi-training-a/blob/master/code-samples/week4/SVI%20Email%20to%20Calendar/SVI%20Calendar/database_query.py) inside folder [SVI Calendar](https://github.com/SiliconValleyInsight/svi-training-a/tree/master/code-samples/week4/SVI%20Email%20to%20Calendar/SVI%20Calendar). What this does is it queries all the info inside the database used in Scrapy then formats it in a a way that can be read by the google calendar api. You dont need to run this as this function inside will be called by the final script.

The queried dates are edited to fit the api by calling the script named [stringtodate.py](https://github.com/SiliconValleyInsight/svi-training-a/blob/master/code-samples/week4/SVI%20Email%20to%20Calendar/SVI%20Calendar/stringtodate.py)

The queried data is then used by the script upcoming.py which accesses the api of google. The other scripts named client_secret.json and credentials.dat are used by api for its oath2 authentication method and can be obtained by following the instructions on the google developer site.

To run this, simply call:

        python upcoming.py
This should run properly provided that all the steps from start have been followed.

## To Do list
* Create a function that would check if the email have already been parsed or not.
* Create another spider that can also get the details of smaller events.
* Create a script that could call a database function to remove duplicate entries.
* Create a bash script to run all these automatically
* Place an oath2 on the IMAP4 script
* Check whether it would be better if we just use the Gmail API instead of the IMAP4 Script.
