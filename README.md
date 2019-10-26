# f-a-k


Coinlib API v1 BETA
Intro
Feedback on the API is welcome at info+api@coinlib.io. Although this is a BETA we think it will be very stable. Moreover the requests and responses won't change, at least not very much.

License
You can use our API under a Creative Commons Attribution-NonCommercial 3.0 Unported (CC BY-NC 3.0) license. Please make sure you credit us with a link if you use our API on your website or app.

Authentication & rate limits
You need an API key for all API calls. Get yours in your profile page (you need to have an account and be logged-in to access this page).

All API endpoints are under https://coinlib.io/api/v1. Use GET to access the API.

We rate limit the API per endpoint and per hour. All responses include the remaining requests you can do until the beginning of the next hour.

120 requests/hour to /global
60 requests/hour to /coinlist
180 requests/hour to /coin
You should always issue one request at a time, if you try to do parallel requests a 429 will be returned. Finally do not use multiple API keys.

Responses
Responses are in JSON. Whole numbers (number of coins, rank, timestamps) are returned as integers. Decimal numbers are returned as strings, using a . as a decimal point and having up to 10 precision digits.

Error example:
{
    "error": "Unknown pref symbol.",
    "remaining": 53
}
Endpoints

Global market stats
Endpoint:
/global

Required params:
key: API key

Optional params:
pref: symbol to use for prices and other market values. Default is USD.

Example:
https://coinlib.io/api/v1/global?key=82eaa0f06fc22af9&pref=EUR

Result:
{
    "coins": 4329,
    "markets": 13648,
    "total_market_cap": "207058335320.66",
    "total_volume_24h": "10413469137.11",
    "last_updated_timestamp": 1528975469,
    "remaining": 540
}
Coin list
Endpoint:
/coinlist

Required params:
key: API key

Optional params:
pref: symbol to use for prices and other market values. Default is USD.
page: integer, starting from 1. For now we return 100 results per page, but this may change without warning.
order:
For rank (first to last) use rank_asc
For rank (last to first) use rank_desc
For volume 24h (low to high) use volume_asc
For volume 24h (high to low) use volume_desc
For price (low to high) use price_asc
For price (high to low) use price_desc
For date listed (recent to older) use date_inserted_asc
For date listed (older to recent) use date_inserted_desc
Example:
https://coinlib.io/api/v1/coinlist?key=82eaa0f06fc22af9&pref=BTC&page=1&order=volume_desc

Result:
{
    "coins": [
        {
            "symbol": "ETH",
            "show_symbol": "ETH",
            "name": "Ethereum",
            "rank": 2,
            "price": "0.078420138035523",
            "market_cap": "7847729.8474137",
            "volume_24h": "260650.1638446",
            "delta_24h": "5.91"
        },
        {...},
        {...},
    ],
    "last_updated_timestamp": 1565321123,
    "remaining": 84
}
Coin info
Endpoint:
/coin

Required params:
key: API key
symbol: single coin symbol or a comma separated list of symbols

Optional params:
pref: symbol to use for prices and other market values. Default is USD.

Example:
https://coinlib.io/api/v1/coin?key=82eaa0f06fc22af9&pref=EUR&symbol=BTC

Result:
{
    "symbol": "BTC",
    "show_symbol": "BTC",
    "name": "Bitcoin",
    "rank": 1,
    "price": "5524.7112165586",
    "market_cap": "94433817003.39",
    "total_volume_24h": "6378793658.5432",
    "low_24h": "5324.2665427149",
    "high_24h": "5561.0068476948",
    "delta_1h": "0.81",
    "delta_24h": "0.68",
    "delta_7d": "-15.26",
    "delta_30d": "-25.26",
    "markets": [
        {
            "symbol": "EUR",
            "volume_24h": "123707000",
            "price": "5524.7112165586",
            "exchanges": [
                {
                    "name": "Kraken",
                    "volume_24h": "50623900",
                    "price": "5520"
                },
                {
                    "name": "Bitfinex",
                    "volume_24h": "19314700",
                    "price": "5512.6"
                },
                {...}
            ]
        },
        {...},
        {...}
    ],
    "last_updated_timestamp": 1528987416,
    "remaining": 1133
}
Notes:
symbol: Always unique.
show_symbol: The symbol the coin is using. Some coins use the same symbol as other coins.
total_volume_24h: The total volume for all markets this coin participates converted in your pref currency.
markets: We show the top 3 pairs for the selected coin. If your pref currency is traded then it is always included first.
exchanges: The top 3 exchanges by volume for the pair.

Advanced:
You can get info for up to 10 coins with a single call. Each coin is counted against your quota. Give a comma separated list of symbols ie:
https://coinlib.io/api/v1/coin?key=82eaa0f06fc22af9&pref=EUR&symbol=BTC,ETH,XMR
Result in this case looks like:
{
    "coins": [
        {},
        {},
        ...
    ],
    "remaining": 534
}


