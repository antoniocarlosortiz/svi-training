#Email to Google Calendar

The main purpose of these scripts is to parse the events stated on subscription emails and automatically create events on the user's google calendar with regards to those.

To that, the whole app was divided into four distinct parts namely:
* Parse the specified emails using an IMAP connection to the mail client.
* Crawl the parsed emails to get only the necessary data in them.
* Place the scraped data inside a database.
* Call those scraped data from the database to serve as input to Google Calendar API.
