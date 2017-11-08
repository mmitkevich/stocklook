stocklook
=========

Cryptocurrency exchange analysis & automated trading on Gdax (for now).

Goal: Painless automated spread and target trading.

APIs:
---------

- Coinbase: light wrapper around coinbase client
- Gdax: custom API/database wrapper for managing account/trading and price history
- Poloniex: custom API/wrapper for getting price history.
- Yahoo Finance (broken)

Examples
--------

Accessing Coinbase to view accounts:

    from stocklook.crypto.coinbase_api import CoinbaseClient

    c = CoinbaseClient()

    # method 1 - access accounts via coinbase library
    obj = c.get_accounts()
    accounts = obj.response.json['data']
    for account in accounts:
        print("{}: {}".format(account['currency']: account['id'])

    # method 2 - parses accounts into dictionary upon access.
    usd_account = c.accounts['USD']


Accessing Gdax to buy some coin:

    from stocklook.crypto.gdax import Gdax, GdaxOrder

    g = Gdax()
    g.deposit_from_coinbase('USD', 100)

    o = GdaxOrder('LTC-USD', order_type='market', amount=100)
    o.post()

Market making spreads on Gdax:

    from stocklook.crypto.gdax.market_maker import GdaxMarketMaker

    m = GdaxMarketMaker(product_id='ETH-USD',
                        min_spread=0.10,
                        max_spread=0.30,
                        max_buy_orders=10,
                        max_sell_orders=30,)
    m.run()

Accessing Poloniex chart data:

    # In progress


Configuration:
--------------
Configuration variables are stored in global variable stocklook.config.config(dict). User input may be required
on Object initialization to figure out credentials unless they've been previously cached or added to this dictionary.
Passwords & secrets are always cached safely using the keyring library.

Update os.environ with the following credentials to have them auto-update config:

- coinbase: COINBASE_KEY
- poloniex: POLONIEX_KEY
- GDAX: GDAX_KEY
- GMAIL: STOCKLOOK_EMAIL

You can update global configuration like so:

    from stocklook.config import update_config, config
    my_config = {
        'DATA_DIRECTORY': 'C:/Users/me/stocklook_data'
        'COINBASE_KEY': 'mycoinbasekey',
        'COINBASE_SECRET': 'mycoinbasesecret',
        'GDAX_KEY': 'mygdaxkey',
        'GDAX_SECRET': 'mygdaxsecret',
        'GDAX_PASSPHRASE': 'mygdaxpassphrase',
        'GMAIL_EMAIL': 'mygmailemail@gmail.com',
        'GMAIL_PASSWORD': 'mygmailpassword',
        'LOG_LEVEL': logging.DEBUG,
        'PYTZ_TIMEZONE': 'US/Pacific',

        # SQLAlchemy URL kwargs
        'GDAX_FEED_URL_KWARGS': {
                                'drivername': 'mysql+pymysql',
                                'host': 'localhost',
                                'port': None,
                                'username': 'dbuser',
                                'password': 'dbpass',
                                'database': 'dbname'
                               },
    }

    # method 1
    update_config(my_config)

    # method 2 (same as method 1)
    config.update(my_config)


To-do List:
-----------

- [] Add tests for gdax, coinbase
- [] fix yahoo api
- [] add Poloniex account management code






