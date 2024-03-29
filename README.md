# TT-001

Test Task 001 - Telegram change bot


Implement a small exchange rate telegram bot which does the following:
Uses exchange rate data from this web service:
[exchangeratesapi.io](https://exchangeratesapi.io) or similar.
Returns the latest exchange rates in a listview.
USD should be used as base currency and converts currency from the list.

## Requirements.

1. Add the following commands:
    `/list` or `/lst` - returns list of all available rates from: `https://api.exchangeratesapi.io/latest?base=USD`.
    Item of the listview should have 2 rows: the currency name in the first row
    and the latest exchange rate (with two decimal precision) in the second row:
    Ex.
    ```
    DKK: 6.74
    HUF: 299.56
    ```

    Once the currency data is loaded from the service, save it in a local database too.
    Also, save the timestamp of the last request somewhere.
    Next time user requests anything the app you should check whether
    10 minutes elapsed since the last request:
    * If yes, you should load new data from web service.
    * If no, you should load previously saved data from the local database.

1. `/exchange $10 to CAD` or `/exchange 10 USD to CAD` - converts to the second currency with two decimal precision and return.
    Ex.:
    ```
    $15.55
    ```
1. `/history USD/CAD for 7 days` - return an image graph chart which shows the exchange rate graph/chart
    of the selected currency for the last 7 days. Here it is not necessary to save anything in the
    local database, you should request every time the currency data for the last 7 days.
    Example request for getting currency history in a given period between USD and CAD:
    `https://api.exchangeratesapi.io/history?start_at=2019-11-27&end_at=2019-12-03&base=USD&symbols=CAD`
1. Results must be given as sources and short instruction how to run it.

You can use any third party library for drawing currency graph/chart.
If web service doesnt return data for the last 7 days,
please show a warning popup with the following message:
_No exchange rate data is available for the selected currency_.

# How to run


## 1. Run using polling _(TESTED)_
For testing purposes use local ttl cache instead redis

```sh
export IS_LOCAL=1
export TELEGRAM_TOKEN=*********
./run_polling.py
```

## 2 Run using webhooks 

```
export TELEGRAM_TOKEN=*********
export WEBHOOK_URL=https://bot.****.****/
export PORT=1234
./run_webhook.py
```

## 2.1 Ngrok _(TESTED)_

First run `ngrok`
```
ngrok http 4480
```

Run bot

```
export TELEGRAM_TOKEN=*********
export WEBHOOK_URL=https://$(curl --silent --show-error http://127.0.0.1:4040/api/tunnels | sed -nE 's/.*public_url":"https:..([^"]*).*/\1/p')
export PORT=4480
./run_webhook.py
```

