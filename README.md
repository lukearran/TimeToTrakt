# TV Time to Trakt - Import Script

A Python script to import TV Time tracked episode and movie data into Trakt.TV - using data export provided by TV Time through a GDPR request.

This script was made possible by the following contributors.

<a href="https://github.com/lukearran/TvTimeToTrakt/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=lukearran/TvTimeToTrakt" />
</a>

# Notes

1. The script is using limited data provided from a GDPR request - so the accuracy isn't 100%. But you will be prompted to manually pick the Trakt show/movie, when it can't be determined automatically.
2. A delay of 1 second is added between each episode/movie to ensure fair use of Trakt's API server. You can adjust this for your own import, but make sure it's at least 0.75 second to remain within the rate limit: https://trakt.docs.apiary.io/#introduction/rate-limiting
3. Episodes which have been processed will be saved to a TinyDB file `localStorage.json` - when you restart the script, the program will skip those episodes which have been marked 'imported'. Processed movies are also stored in the same file.

# Setup

## Get your Data

TV Time's API is not open. In order to get access to your personal data, you will have to request it from TV Time's support via a GDPR request - or maybe just ask for it, whatever works, it's your data.

1. Copy the template provided by [www.datarequests.org](https://www.datarequests.org/blog/sample-letter-gdpr-access-request/) into an email
2. Send it to support@tvtime.com
3. Wait a few working days for their team to process your request
4. Extract the data somewhere safe on your local system

## Register API Access at Trakt

1. Go to "Settings" under your profile
2. Select ["Your API Applications"](https://trakt.tv/oauth/applications)
3. Select "New Application"
4. Provide a name into "Name" e.g. John Smith Import from TV Time
5. Paste "urn:ietf:wg:oauth:2.0:oob" into "Redirect uri:"
6. Click "Save App"
7. Make note of your details to be used later.

## Setup Script

### Install Required Libraries

Install the following frameworks via Pip:

```
python -m pip install -r requirements.txt
```

### Setup Configuration

Create a new file named `config.json` in the same directory of `TimeToTrakt.py`, using the below JSON contents (replace the values with your own).

Use forward slash or double backslash for `GDPR_WORKSPACE_PATH` if you encounter `json.decoder.JSONDecodeError: Invalid \escape: line 4 column 31 (char 206)`, as seen [here](https://github.com/lukearran/TvTimeToTrakt/issues/18) and [here](https://github.com/lukearran/TvTimeToTrakt/issues/39).

The movie and show data is usually in "tracking-prod-records.csv" and "tracking-prod-records-v2.csv" respectively, however please check this is where your data is actually stored.

Date format can be left as the default value unless you receive an error. If you receive an error about the time data not matching format, update this value using the [docs](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes).
```
{
    "CLIENT_ID": "YOUR_CLIENT_ID",
    "CLIENT_SECRET": "YOUR_CLIENT_SECRET",
    "MOVIE_DATA_PATH": "DIRECTORY_OF_YOUR_GDPR_REQUEST_MOVIE_DATA",
    "SHOW_DATA_PATH": "DIRECTORY_OF_YOUR_GDPR_REQUEST_SHOW_DATA",
    "TRAKT_USERNAME": "YOUR_TRAKT_USERNAME",
    "DATE_FORMAT": "%Y-%m-%d %H:%M:%S"
}
```

Once the config is in place, execute the program using `python TimeToTrakt.py`. The process isn't 100% automated - you will need to pop back, especially with large imports, to check if the script requires a manual user input.
