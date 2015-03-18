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

## To Do list
* Create a function that would check if the email have already been parsed or not.
* Create another spider that can also get the details of smaller events.
* Create a script that could call a database function to remove duplicate entries.
* Create a bash script to run all these automatically
* Place an oath2 on the IMAP4 script
* Check whether it would be better if we just use the Gmail API instead of the IMAP4 Script.
