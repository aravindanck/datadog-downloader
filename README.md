# DataDog log downloader

Download logs from DataDog via the API.
This allows you to download a large number of logs matching a particular query rather than being bound by the 5000 limit imposed on the export button in the UI.

## Differences from parent repository

- Logs are streamed directly to output file instead of being written as a single action at the end from memory. This avoid OOM issues for large logs
- Logs are output as log lines instead of JSON.


## Usage

```
DD_API_KEY=... DD_APP_KEY=... npx github:wegift/datadog-downloader --query '"Redeem failed"'
```

## Authentication

You will need an API key and an app key to access the DataDog api.
These should be provided in environment variables as seen above.

API keys are global for a DataDog account and can be found in organization settings.
App keys are personal to your profile and can be generated in personal settings.

## Options

```
--query     The filter query (aka search term). Take care when quoting on the command line, single quote the entire query for best results.

--index     Which index to read from, default 'main'

--from      Start date/time defaults to 1y ago
--to        End date/time, omit for results up to the current time

--pageSize  How many results to download at a time, default 1000 limit of 5000

--output    Path of log file to write results to, default 'results.log'
```

Note: Date/times are parsed by JS `Date` constructor, e.g. 2022-01-01

## Local Dev

Run `npm install`.

Copy `.env.example` to `.env` and add a valid DataDog API key and app key.

### Run

```
node index.mjs --query '"Redeem token failure" -@redeem_failure_reason:"Invalid token"'
```

## Caveats

Be watchful of 429 HTTP response code when you paginate over large numbers in a short period of time. DataDog rate limits API calls, for more details refer https://docs.datadoghq.com/api/latest/rate-limits/

## Contributing

**Pull requests welcome!**
