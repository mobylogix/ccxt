# Overview

The ccxt library is a collection of available crypto *exchanges* or exchange classes. Each class implements the public and private API for a particular crypto exchange. All exchanges are derived from the base Exchange class and share a set of common methods. To access a particular exchange from ccxt library you need to create an instance of corresponding exchange class. Supported exchanges are updated frequently and new exchanges are added regularly.

The structure of the library can be outlined as follows:

```
                                 User
    +-------------------------------------------------------------+
    |                            CCXT                             |
    +------------------------------+------------------------------+
    |            Public            |           Private            |
    +=============================================================+
    │                              .                              |
    │                    The Unified CCXT API                     |
    │                              .                              |
    |       loadMarkets            .           fetchBalance       |
    |       fetchMarkets           .            createOrder       |
    |       fetchTicker            .            cancelOrder       |
    |       fetchTickers           .             fetchOrder       |
    |       fetchOrderBook         .            fetchOrders       |
    |       fetchOHLCV             .        fetchOpenOrders       |
    |       fetchTrades            .      fetchClosedOrders       |
    |                              .          fetchMyTrades       |
    |                              .                deposit       |
    |                              .               withdraw       |
    │                              .                              |
    +=============================================================+
    │                              .                              |
    |                     Custom Exchange API                     |
    |                      (Derived Classes)                      |
    │                              .                              |
    |       publicGet...           .          privateGet...       |
    |       publicPost...          .         privatePost...       |
    |                              .          privatePut...       |
    |                              .       privateDelete...       |
    |                              .                   sign       |
    │                              .                              |
    +=============================================================+
    │                              .                              |
    |                      Base Exchange Class                    |
    │                              .                              |
    +=============================================================+
```

Full public and private HTTP REST APIs for all exchanges are implemented. WebSocket and FIX implementations in JavaScript, PHP, Python and other languages coming soon.

- [Exchanges](#exchanges)
- [Markets](#markets)
- [API Methods / Endpoints](#api-methods--endpoints)
- [Market Data](#market-data)
- [Trading](#trading)

# Exchanges

The ccxt library currently supports the following 102 cryptocurrency exchange markets and trading APIs:

|                                                                                                                           | id                 | name                                                      | ver | doc                                                                                          | countries                               |
|---------------------------------------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------|:---:|:--------------------------------------------------------------------------------------------:|-----------------------------------------|
|![_1broker](https://user-images.githubusercontent.com/1294454/27766021-420bd9fc-5ecb-11e7-8ed6-56d0081efed2.jpg)           | _1broker           | [1Broker](https://1broker.com)                            | 2   | [API](https://1broker.com/?c=en/content/api-documentation)                                   | US                                      |
|![_1btcxe](https://user-images.githubusercontent.com/1294454/27766049-2b294408-5ecc-11e7-85cc-adaff013dc1a.jpg)            | _1btcxe            | [1BTCXE](https://1btcxe.com)                              | *   | [API](https://1btcxe.com/api-docs.php)                                                       | Panama                                  |
|![acx](https://user-images.githubusercontent.com/1294454/30247614-1fe61c74-9621-11e7-9e8c-f1a627afa279.jpg)                | acx                | [ACX](https://acx.io)                                     | 2   | [API](https://acx.io/documents/api_v2)                                                       | Australia                               |
|![allcoin](https://user-images.githubusercontent.com/1294454/31561809-c316b37c-b061-11e7-8d5a-b547b4d730eb.jpg)            | allcoin            | [Allcoin](https://www.allcoin.com)                        | 1   | [API](https://www.allcoin.com/About/APIReference)                                            | Canada                                  |
|![anxpro](https://user-images.githubusercontent.com/1294454/27765983-fd8595da-5ec9-11e7-82e3-adb3ab8c2612.jpg)             | anxpro             | [ANXPro](https://anxpro.com)                              | 2   | [API](http://docs.anxv2.apiary.io)                                                           | Japan, Singapore, Hong Kong, New Zealand|
|![bibox](https://user-images.githubusercontent.com/1294454/34902611-2be8bf1a-f830-11e7-91a2-11b2f292e750.jpg)              | bibox              | [Bibox](https://www.bibox.com)                            | 1   | [API](https://github.com/Biboxcom/api_reference/wiki/home_en)                                | China, US, South Korea                  |
|![binance](https://user-images.githubusercontent.com/1294454/29604020-d5483cdc-87ee-11e7-94c7-d1a8d9169293.jpg)            | binance            | [Binance](https://www.binance.com)                        | *   | [API](https://github.com/binance-exchange/binance-official-api-docs/blob/master/rest-api.md) | Japan                                   |
|![bit2c](https://user-images.githubusercontent.com/1294454/27766119-3593220e-5ece-11e7-8b3a-5a041f6bcc3f.jpg)              | bit2c              | [Bit2C](https://www.bit2c.co.il)                          | *   | [API](https://www.bit2c.co.il/home/api)                                                      | Israel                                  |
|![bitbay](https://user-images.githubusercontent.com/1294454/27766132-978a7bd8-5ece-11e7-9540-bc96d1e9bbb8.jpg)             | bitbay             | [BitBay](https://bitbay.net)                              | *   | [API](https://bitbay.net/public-api)                                                         | Poland, EU                              |
|![bitcoincoid](https://user-images.githubusercontent.com/1294454/27766138-043c7786-5ecf-11e7-882b-809c14f38b53.jpg)        | bitcoincoid        | [Bitcoin.co.id](https://www.bitcoin.co.id)                | 1.7 | [API](https://vip.bitcoin.co.id/downloads/BITCOINCOID-API-DOCUMENTATION.pdf)                 | Indonesia                               |
|![bitfinex](https://user-images.githubusercontent.com/1294454/27766244-e328a50c-5ed2-11e7-947b-041416579bb3.jpg)           | bitfinex           | [Bitfinex](https://www.bitfinex.com)                      | 1   | [API](https://bitfinex.readme.io/v1/docs)                                                    | British Virgin Islands                  |
|![bitfinex2](https://user-images.githubusercontent.com/1294454/27766244-e328a50c-5ed2-11e7-947b-041416579bb3.jpg)          | bitfinex2          | [Bitfinex v2](https://www.bitfinex.com)                   | 2   | [API](https://bitfinex.readme.io/v2/docs)                                                    | British Virgin Islands                  |
|![bitflyer](https://user-images.githubusercontent.com/1294454/28051642-56154182-660e-11e7-9b0d-6042d1e6edd8.jpg)           | bitflyer           | [bitFlyer](https://bitflyer.jp)                           | 1   | [API](https://bitflyer.jp/API)                                                               | Japan                                   |
|![bithumb](https://user-images.githubusercontent.com/1294454/30597177-ea800172-9d5e-11e7-804c-b9d4fa9b56b0.jpg)            | bithumb            | [Bithumb](https://www.bithumb.com)                        | *   | [API](https://www.bithumb.com/u1/US127)                                                      | South Korea                             |
|![bitlish](https://user-images.githubusercontent.com/1294454/27766275-dcfc6c30-5ed3-11e7-839d-00a846385d0b.jpg)            | bitlish            | [Bitlish](https://bitlish.com)                            | 1   | [API](https://bitlish.com/api)                                                               | UK, EU, Russia                          |
|![bitmarket](https://user-images.githubusercontent.com/1294454/27767256-a8555200-5ef9-11e7-96fd-469a65e2b0bd.jpg)          | bitmarket          | [BitMarket](https://www.bitmarket.pl)                     | *   | [API](https://www.bitmarket.net/docs.php?file=api_public.html)                               | Poland, EU                              |
|![bitmex](https://user-images.githubusercontent.com/1294454/27766319-f653c6e6-5ed4-11e7-933d-f0bc3699ae8f.jpg)             | bitmex             | [BitMEX](https://www.bitmex.com)                          | 1   | [API](https://www.bitmex.com/app/apiOverview)                                                | Seychelles                              |
|![bitso](https://user-images.githubusercontent.com/1294454/27766335-715ce7aa-5ed5-11e7-88a8-173a27bb30fe.jpg)              | bitso              | [Bitso](https://bitso.com)                                | 3   | [API](https://bitso.com/api_info)                                                            | Mexico                                  |
|![bitstamp](https://user-images.githubusercontent.com/1294454/27786377-8c8ab57e-5fe9-11e7-8ea4-2b05b6bcceec.jpg)           | bitstamp           | [Bitstamp](https://www.bitstamp.net)                      | 2   | [API](https://www.bitstamp.net/api)                                                          | UK                                      |
|![bitstamp1](https://user-images.githubusercontent.com/1294454/27786377-8c8ab57e-5fe9-11e7-8ea4-2b05b6bcceec.jpg)          | bitstamp1          | [Bitstamp v1](https://www.bitstamp.net)                   | 1   | [API](https://www.bitstamp.net/api)                                                          | UK                                      |
|![bittrex](https://user-images.githubusercontent.com/1294454/27766352-cf0b3c26-5ed5-11e7-82b7-f3826b7a97d8.jpg)            | bittrex            | [Bittrex](https://bittrex.com)                            | 1.1 | [API](https://bittrex.com/Home/Api)                                                          | US                                      |
|![bitz](https://user-images.githubusercontent.com/1294454/35862606-4f554f14-0b5d-11e8-957d-35058c504b6f.jpg)               | bitz               | [Bit-Z](https://www.bit-z.com)                            | 1   | [API](https://www.bit-z.com/api.html)                                                        | Hong Kong                               |
|![bl3p](https://user-images.githubusercontent.com/1294454/28501752-60c21b82-6feb-11e7-818b-055ee6d0e754.jpg)               | bl3p               | [BL3P](https://bl3p.eu)                                   | 1   | [API](https://github.com/BitonicNL/bl3p-api/tree/master/docs)                                | Netherlands, EU                         |
|![bleutrade](https://user-images.githubusercontent.com/1294454/30303000-b602dbe6-976d-11e7-956d-36c5049c01e7.jpg)          | bleutrade          | [Bleutrade](https://bleutrade.com)                        | 2   | [API](https://bleutrade.com/help/API)                                                        | Brazil                                  |
|![braziliex](https://user-images.githubusercontent.com/1294454/34703593-c4498674-f504-11e7-8d14-ff8e44fb78c1.jpg)          | braziliex          | [Braziliex](https://braziliex.com/)                       | *   | [API](https://braziliex.com/exchange/api.php)                                                | Brazil                                  |
|![btcbox](https://user-images.githubusercontent.com/1294454/31275803-4df755a8-aaa1-11e7-9abb-11ec2fad9f2d.jpg)             | btcbox             | [BtcBox](https://www.btcbox.co.jp/)                       | 1   | [API](https://www.btcbox.co.jp/help/asm)                                                     | Japan                                   |
|![btcchina](https://user-images.githubusercontent.com/1294454/27766368-465b3286-5ed6-11e7-9a11-0f6467e1d82b.jpg)           | btcchina           | [BTCChina](https://www.btcchina.com)                      | 1   | [API](https://www.btcchina.com/apidocs)                                                      | China                                   |
|![btcexchange](https://user-images.githubusercontent.com/1294454/27993052-4c92911a-64aa-11e7-96d8-ec6ac3435757.jpg)        | btcexchange        | [BTCExchange](https://www.btcexchange.ph)                 | *   | [API](https://github.com/BTCTrader/broker-api-docs)                                          | Philippines                             |
|![btcmarkets](https://user-images.githubusercontent.com/1294454/29142911-0e1acfc2-7d5c-11e7-98c4-07d9532b29d7.jpg)         | btcmarkets         | [BTC Markets](https://btcmarkets.net/)                    | *   | [API](https://github.com/BTCMarkets/API)                                                     | Australia                               |
|![btctradeim](https://user-images.githubusercontent.com/1294454/36770531-c2142444-1c5b-11e8-91e2-a4d90dc85fe8.jpg)         | btctradeim         | [BtcTrade.im](https://www.btctrade.im)                    | *   | [API](https://www.btctrade.im/help.api.html)                                                 | Hong Kong                               |
|![btctradeua](https://user-images.githubusercontent.com/1294454/27941483-79fc7350-62d9-11e7-9f61-ac47f28fcd96.jpg)         | btctradeua         | [BTC Trade UA](https://btc-trade.com.ua)                  | *   | [API](https://docs.google.com/document/d/1ocYA0yMy_RXd561sfG3qEPZ80kyll36HUxvCRe5GbhE/edit)  | Ukraine                                 |
|![btcturk](https://user-images.githubusercontent.com/1294454/27992709-18e15646-64a3-11e7-9fa2-b0950ec7712f.jpg)            | btcturk            | [BTCTurk](https://www.btcturk.com)                        | *   | [API](https://github.com/BTCTrader/broker-api-docs)                                          | Turkey                                  |
|![btcx](https://user-images.githubusercontent.com/1294454/27766385-9fdcc98c-5ed6-11e7-8f14-66d5e5cd47e6.jpg)               | btcx               | [BTCX](https://btc-x.is)                                  | 1   | [API](https://btc-x.is/custom/api-document.html)                                             | Iceland, US, EU                         |
|![bxinth](https://user-images.githubusercontent.com/1294454/27766412-567b1eb4-5ed7-11e7-94a8-ff6a3884f6c5.jpg)             | bxinth             | [BX.in.th](https://bx.in.th)                              | *   | [API](https://bx.in.th/info/api)                                                             | Thailand                                |
|![ccex](https://user-images.githubusercontent.com/1294454/27766433-16881f90-5ed8-11e7-92f8-3d92cc747a6c.jpg)               | ccex               | [C-CEX](https://c-cex.com)                                | *   | [API](https://c-cex.com/?id=api)                                                             | Germany, EU                             |
|![cex](https://user-images.githubusercontent.com/1294454/27766442-8ddc33b0-5ed8-11e7-8b98-f786aef0f3c9.jpg)                | cex                | [CEX.IO](https://cex.io)                                  | *   | [API](https://cex.io/cex-api)                                                                | UK, EU, Cyprus, Russia                  |
|![chbtc](https://user-images.githubusercontent.com/1294454/28555659-f0040dc2-7109-11e7-9d99-688a438bf9f4.jpg)              | chbtc              | [CHBTC](https://trade.chbtc.com/api)                      | 1   | [API](https://www.chbtc.com/i/developer)                                                     | China                                   |
|![chilebit](https://user-images.githubusercontent.com/1294454/27991414-1298f0d8-647f-11e7-9c40-d56409266336.jpg)           | chilebit           | [ChileBit](https://chilebit.net)                          | 1   | [API](https://blinktrade.com/docs)                                                           | Chile                                   |
|![cobinhood](https://user-images.githubusercontent.com/1294454/35755576-dee02e5c-0878-11e8-989f-1595d80ba47f.jpg)          | cobinhood          | [COBINHOOD](https://cobinhood.com)                        | *   | [API](https://cobinhood.github.io/api-public)                                                | Taiwan                                  |
|![coincheck](https://user-images.githubusercontent.com/1294454/27766464-3b5c3c74-5ed9-11e7-840e-31b32968e1da.jpg)          | coincheck          | [coincheck](https://coincheck.com)                        | *   | [API](https://coincheck.com/documents/exchange/api)                                          | Japan, Indonesia                        |
|![coinegg](https://user-images.githubusercontent.com/1294454/36770310-adfa764e-1c5a-11e8-8e09-449daac3d2fb.jpg)            | coinegg            | [CoinEgg](https://www.coinegg.com)                        | *   | [API](https://www.coinegg.com/explain.api.html)                                              | China, UK                               |
|![coinexchange](https://user-images.githubusercontent.com/1294454/34842303-29c99fca-f71c-11e7-83c1-09d900cb2334.jpg)       | coinexchange       | [CoinExchange](https://www.coinexchange.io)               | *   | [API](https://coinexchangeio.github.io/slate/)                                               | India, Japan, South Korea, Vietnam, US  |
|![coinfloor](https://user-images.githubusercontent.com/1294454/28246081-623fc164-6a1c-11e7-913f-bac0d5576c90.jpg)          | coinfloor          | [coinfloor](https://www.coinfloor.co.uk)                  | *   | [API](https://github.com/coinfloor/api)                                                      | UK                                      |
|![coingi](https://user-images.githubusercontent.com/1294454/28619707-5c9232a8-7212-11e7-86d6-98fe5d15cc6e.jpg)             | coingi             | [Coingi](https://coingi.com)                              | *   | [API](http://docs.coingi.apiary.io/)                                                         | Panama, Bulgaria, China, US             |
|![coinmarketcap](https://user-images.githubusercontent.com/1294454/28244244-9be6312a-69ed-11e7-99c1-7c1797275265.jpg)      | coinmarketcap      | [CoinMarketCap](https://coinmarketcap.com)                | 1   | [API](https://coinmarketcap.com/api)                                                         | US                                      |
|![coinmate](https://user-images.githubusercontent.com/1294454/27811229-c1efb510-606c-11e7-9a36-84ba2ce412d8.jpg)           | coinmate           | [CoinMate](https://coinmate.io)                           | *   | [API](http://docs.coinmate.apiary.io)                                                        | UK, Czech Republic, EU                  |
|![coinsecure](https://user-images.githubusercontent.com/1294454/27766472-9cbd200a-5ed9-11e7-9551-2267ad7bac08.jpg)         | coinsecure         | [Coinsecure](https://coinsecure.in)                       | 1   | [API](https://api.coinsecure.in)                                                             | India                                   |
|![coinspot](https://user-images.githubusercontent.com/1294454/28208429-3cacdf9a-6896-11e7-854e-4c79a772a30f.jpg)           | coinspot           | [CoinSpot](https://www.coinspot.com.au)                   | *   | [API](https://www.coinspot.com.au/api)                                                       | Australia                               |
|![coolcoin](https://user-images.githubusercontent.com/1294454/36770529-be7b1a04-1c5b-11e8-9600-d11f1996b539.jpg)           | coolcoin           | [CoolCoin](https://www.coolcoin.com)                      | *   | [API](https://www.coolcoin.com/help.api.html)                                                | Hong Kong                               |
|![cryptopia](https://user-images.githubusercontent.com/1294454/29484394-7b4ea6e2-84c6-11e7-83e5-1fccf4b2dc81.jpg)          | cryptopia          | [Cryptopia](https://www.cryptopia.co.nz)                  | *   | [API](https://www.cryptopia.co.nz/Forum/Category/45)                                         | New Zealand                             |
|![dsx](https://user-images.githubusercontent.com/1294454/27990275-1413158a-645a-11e7-931c-94717f7510e3.jpg)                | dsx                | [DSX](https://dsx.uk)                                     | 3   | [API](https://api.dsx.uk)                                                                    | UK                                      |
|![exmo](https://user-images.githubusercontent.com/1294454/27766491-1b0ea956-5eda-11e7-9225-40d67b481b8d.jpg)               | exmo               | [EXMO](https://exmo.me)                                   | 1   | [API](https://exmo.me/en/api_doc)                                                            | Spain, Russia                           |
|![flowbtc](https://user-images.githubusercontent.com/1294454/28162465-cd815d4c-67cf-11e7-8e57-438bea0523a2.jpg)            | flowbtc            | [flowBTC](https://trader.flowbtc.com)                     | 1   | [API](http://www.flowbtc.com.br/api/)                                                        | Brazil                                  |
|![foxbit](https://user-images.githubusercontent.com/1294454/27991413-11b40d42-647f-11e7-91ee-78ced874dd09.jpg)             | foxbit             | [FoxBit](https://foxbit.exchange)                         | 1   | [API](https://blinktrade.com/docs)                                                           | Brazil                                  |
|![fybse](https://user-images.githubusercontent.com/1294454/27766512-31019772-5edb-11e7-8241-2e675e6797f1.jpg)              | fybse              | [FYB-SE](https://www.fybse.se)                            | *   | [API](http://docs.fyb.apiary.io)                                                             | Sweden                                  |
|![fybsg](https://user-images.githubusercontent.com/1294454/27766513-3364d56a-5edb-11e7-9e6b-d5898bb89c81.jpg)              | fybsg              | [FYB-SG](https://www.fybsg.com)                           | *   | [API](http://docs.fyb.apiary.io)                                                             | Singapore                               |
|![gatecoin](https://user-images.githubusercontent.com/1294454/28646817-508457f2-726c-11e7-9eeb-3528d2413a58.jpg)           | gatecoin           | [Gatecoin](https://gatecoin.com)                          | *   | [API](https://gatecoin.com/api)                                                              | Hong Kong                               |
|![gateio](https://user-images.githubusercontent.com/1294454/31784029-0313c702-b509-11e7-9ccc-bc0da6a0e435.jpg)             | gateio             | [Gate.io](https://gate.io/)                               | 2   | [API](https://gate.io/api2)                                                                  | China                                   |
|![gdax](https://user-images.githubusercontent.com/1294454/27766527-b1be41c6-5edb-11e7-95f6-5b496c469e2c.jpg)               | gdax               | [GDAX](https://www.gdax.com)                              | *   | [API](https://docs.gdax.com)                                                                 | US                                      |
|![gemini](https://user-images.githubusercontent.com/1294454/27816857-ce7be644-6096-11e7-82d6-3c257263229c.jpg)             | gemini             | [Gemini](https://gemini.com)                              | 1   | [API](https://docs.gemini.com/rest-api)                                                      | US                                      |
|![getbtc](https://user-images.githubusercontent.com/1294454/33801902-03c43462-dd7b-11e7-992e-077e4cd015b9.jpg)             | getbtc             | [GetBTC](https://getbtc.org)                              | *   | [API](https://getbtc.org/api-docs.php)                                                       | St. Vincent & Grenadines, Russia        |
|![hitbtc](https://user-images.githubusercontent.com/1294454/27766555-8eaec20e-5edc-11e7-9c5b-6dc69fc42f5e.jpg)             | hitbtc             | [HitBTC](https://hitbtc.com)                              | 1   | [API](https://github.com/hitbtc-com/hitbtc-api/blob/master/APIv1.md)                         | UK                                      |
|![hitbtc2](https://user-images.githubusercontent.com/1294454/27766555-8eaec20e-5edc-11e7-9c5b-6dc69fc42f5e.jpg)            | hitbtc2            | [HitBTC v2](https://hitbtc.com)                           | 2   | [API](https://api.hitbtc.com)                                                                | UK                                      |
|![huobi](https://user-images.githubusercontent.com/1294454/27766569-15aa7b9a-5edd-11e7-9e7f-44791f4ee49c.jpg)              | huobi              | [Huobi](https://www.huobi.com)                            | 3   | [API](https://github.com/huobiapi/API_Docs_en/wiki)                                          | China                                   |
|![huobicny](https://user-images.githubusercontent.com/1294454/27766569-15aa7b9a-5edd-11e7-9e7f-44791f4ee49c.jpg)           | huobicny           | [Huobi CNY](https://www.huobi.com)                        | 1   | [API](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference)                          | China                                   |
|![huobipro](https://user-images.githubusercontent.com/1294454/27766569-15aa7b9a-5edd-11e7-9e7f-44791f4ee49c.jpg)           | huobipro           | [Huobi Pro](https://www.huobi.pro)                        | 1   | [API](https://github.com/huobiapi/API_Docs/wiki/REST_api_reference)                          | China                                   |
|![independentreserve](https://user-images.githubusercontent.com/1294454/30521662-cf3f477c-9bcb-11e7-89bc-d1ac85012eda.jpg) | independentreserve | [Independent Reserve](https://www.independentreserve.com) | *   | [API](https://www.independentreserve.com/API)                                                | Australia, New Zealand                  |
|![itbit](https://user-images.githubusercontent.com/1294454/27822159-66153620-60ad-11e7-89e7-005f6d7f3de0.jpg)              | itbit              | [itBit](https://www.itbit.com)                            | 1   | [API](https://api.itbit.com/docs)                                                            | US                                      |
|![jubi](https://user-images.githubusercontent.com/1294454/27766581-9d397d9a-5edd-11e7-8fb9-5d8236c0e692.jpg)               | jubi               | [jubi.com](https://www.jubi.com)                          | 1   | [API](https://www.jubi.com/help/api.html)                                                    | China                                   |
|![kraken](https://user-images.githubusercontent.com/1294454/27766599-22709304-5ede-11e7-9de1-9f33732e1509.jpg)             | kraken             | [Kraken](https://www.kraken.com)                          | 0   | [API](https://www.kraken.com/en-us/help/api)                                                 | US                                      |
|![kucoin](https://user-images.githubusercontent.com/1294454/33795655-b3c46e48-dcf6-11e7-8abe-dc4588ba7901.jpg)             | kucoin             | [Kucoin](https://kucoin.com)                              | 1   | [API](https://kucoinapidocs.docs.apiary.io)                                                  | Hong Kong                               |
|![kuna](https://user-images.githubusercontent.com/1294454/31697638-912824fa-b3c1-11e7-8c36-cf9606eb94ac.jpg)               | kuna               | [Kuna](https://kuna.io)                                   | 2   | [API](https://kuna.io/documents/api)                                                         | Ukraine                                 |
|![lakebtc](https://user-images.githubusercontent.com/1294454/28074120-72b7c38a-6660-11e7-92d9-d9027502281d.jpg)            | lakebtc            | [LakeBTC](https://www.lakebtc.com)                        | 2   | [API](https://www.lakebtc.com/s/api_v2)                                                      | US                                      |
|![liqui](https://user-images.githubusercontent.com/1294454/27982022-75aea828-63a0-11e7-9511-ca584a8edd74.jpg)              | liqui              | [Liqui](https://liqui.io)                                 | 3   | [API](https://liqui.io/api)                                                                  | Ukraine                                 |
|![livecoin](https://user-images.githubusercontent.com/1294454/27980768-f22fc424-638a-11e7-89c9-6010a54ff9be.jpg)           | livecoin           | [LiveCoin](https://www.livecoin.net)                      | *   | [API](https://www.livecoin.net/api?lang=en)                                                  | US, UK, Russia                          |
|![luno](https://user-images.githubusercontent.com/1294454/27766607-8c1a69d8-5ede-11e7-930c-540b5eb9be24.jpg)               | luno               | [luno](https://www.luno.com)                              | 1   | [API](https://www.luno.com/en/api)                                                           | UK, Singapore, South Africa             |
|![lykke](https://user-images.githubusercontent.com/1294454/34487620-3139a7b0-efe6-11e7-90f5-e520cef74451.jpg)              | lykke              | [Lykke](https://www.lykke.com)                            | 1   | [API](https://hft-api.lykke.com/swagger/ui/)                                                 | Switzerland                             |
|![mercado](https://user-images.githubusercontent.com/1294454/27837060-e7c58714-60ea-11e7-9192-f05e86adb83f.jpg)            | mercado            | [Mercado Bitcoin](https://www.mercadobitcoin.com.br)      | 3   | [API](https://www.mercadobitcoin.com.br/api-doc)                                             | Brazil                                  |
|![mixcoins](https://user-images.githubusercontent.com/1294454/30237212-ed29303c-9535-11e7-8af8-fcd381cfa20c.jpg)           | mixcoins           | [MixCoins](https://mixcoins.com)                          | 1   | [API](https://mixcoins.com/help/api/)                                                        | UK, Hong Kong                           |
|![nova](https://user-images.githubusercontent.com/1294454/30518571-78ca0bca-9b8a-11e7-8840-64b83a4a94b2.jpg)               | nova               | [Novaexchange](https://novaexchange.com)                  | 2   | [API](https://novaexchange.com/remote/faq)                                                   | Tanzania                                |
|![okcoincny](https://user-images.githubusercontent.com/1294454/27766792-8be9157a-5ee5-11e7-926c-6d69b8d3378d.jpg)          | okcoincny          | [OKCoin CNY](https://www.okcoin.cn)                       | 1   | [API](https://www.okcoin.cn/rest_getStarted.html)                                            | China                                   |
|![okcoinusd](https://user-images.githubusercontent.com/1294454/27766791-89ffb502-5ee5-11e7-8a5b-c5950b68ac65.jpg)          | okcoinusd          | [OKCoin USD](https://www.okcoin.com)                      | 1   | [API](https://www.okcoin.com/rest_getStarted.html)                                           | China, US                               |
|![okex](https://user-images.githubusercontent.com/1294454/32552768-0d6dd3c6-c4a6-11e7-90f8-c043b64756a7.jpg)               | okex               | [OKEX](https://www.okex.com)                              | 1   | [API](https://www.okex.com/rest_getStarted.html)                                             | China, US                               |
|![paymium](https://user-images.githubusercontent.com/1294454/27790564-a945a9d4-5ff9-11e7-9d2d-b635763f2f24.jpg)            | paymium            | [Paymium](https://www.paymium.com)                        | 1   | [API](https://github.com/Paymium/api-documentation)                                          | France, EU                              |
|![poloniex](https://user-images.githubusercontent.com/1294454/27766817-e9456312-5ee6-11e7-9b3c-b628ca5626a5.jpg)           | poloniex           | [Poloniex](https://poloniex.com)                          | *   | [API](https://poloniex.com/support/api/)                                                     | US                                      |
|![qryptos](https://user-images.githubusercontent.com/1294454/30953915-b1611dc0-a436-11e7-8947-c95bd5a42086.jpg)            | qryptos            | [QRYPTOS](https://www.qryptos.com)                        | 2   | [API](https://developers.quoine.com)                                                         | China, Taiwan                           |
|![quadrigacx](https://user-images.githubusercontent.com/1294454/27766825-98a6d0de-5ee7-11e7-9fa4-38e11a2c6f52.jpg)         | quadrigacx         | [QuadrigaCX](https://www.quadrigacx.com)                  | 2   | [API](https://www.quadrigacx.com/api_info)                                                   | Canada                                  |
|![quoinex](https://user-images.githubusercontent.com/1294454/35047114-0e24ad4a-fbaa-11e7-96a9-69c1a756083b.jpg)            | quoinex            | [QUOINEX](https://quoinex.com/)                           | 2   | [API](https://developers.quoine.com)                                                         | Japan, Singapore, Vietnam               |
|![southxchange](https://user-images.githubusercontent.com/1294454/27838912-4f94ec8a-60f6-11e7-9e5d-bbf9bd50a559.jpg)       | southxchange       | [SouthXchange](https://www.southxchange.com)              | *   | [API](https://www.southxchange.com/Home/Api)                                                 | Argentina                               |
|![surbitcoin](https://user-images.githubusercontent.com/1294454/27991511-f0a50194-6481-11e7-99b5-8f02932424cc.jpg)         | surbitcoin         | [SurBitcoin](https://surbitcoin.com)                      | 1   | [API](https://blinktrade.com/docs)                                                           | Venezuela                               |
|![therock](https://user-images.githubusercontent.com/1294454/27766869-75057fa2-5ee9-11e7-9a6f-13e641fa4707.jpg)            | therock            | [TheRockTrading](https://therocktrading.com)              | 1   | [API](https://api.therocktrading.com/doc/v1/index.html)                                      | Malta                                   |
|![tidex](https://user-images.githubusercontent.com/1294454/30781780-03149dc4-a12e-11e7-82bb-313b269d24d4.jpg)              | tidex              | [Tidex](https://tidex.com)                                | 3   | [API](https://tidex.com/exchange/public-api)                                                 | UK                                      |
|![urdubit](https://user-images.githubusercontent.com/1294454/27991453-156bf3ae-6480-11e7-82eb-7295fe1b5bb4.jpg)            | urdubit            | [UrduBit](https://urdubit.com)                            | 1   | [API](https://blinktrade.com/docs)                                                           | Pakistan                                |
|![vaultoro](https://user-images.githubusercontent.com/1294454/27766880-f205e870-5ee9-11e7-8fe2-0d5b15880752.jpg)           | vaultoro           | [Vaultoro](https://www.vaultoro.com)                      | 1   | [API](https://api.vaultoro.com)                                                              | Switzerland                             |
|![vbtc](https://user-images.githubusercontent.com/1294454/27991481-1f53d1d8-6481-11e7-884e-21d17e7939db.jpg)               | vbtc               | [VBTC](https://vbtc.exchange)                             | 1   | [API](https://blinktrade.com/docs)                                                           | Vietnam                                 |
|![virwox](https://user-images.githubusercontent.com/1294454/27766894-6da9d360-5eea-11e7-90aa-41f2711b7405.jpg)             | virwox             | [VirWoX](https://www.virwox.com)                          | *   | [API](https://www.virwox.com/developers.php)                                                 | Austria, EU                             |
|![wex](https://user-images.githubusercontent.com/1294454/30652751-d74ec8f8-9e31-11e7-98c5-71469fcef03e.jpg)                | wex                | [WEX](https://wex.nz)                                     | 3   | [API](https://wex.nz/api/3/docs)                                                             | New Zealand                             |
|![xbtce](https://user-images.githubusercontent.com/1294454/28059414-e235970c-662c-11e7-8c3a-08e31f78684b.jpg)              | xbtce              | [xBTCe](https://www.xbtce.com)                            | 1   | [API](https://www.xbtce.com/tradeapi)                                                        | Russia                                  |
|![yobit](https://user-images.githubusercontent.com/1294454/27766910-cdcbfdae-5eea-11e7-9859-03fea873272d.jpg)              | yobit              | [YoBit](https://www.yobit.net)                            | 3   | [API](https://www.yobit.net/en/api/)                                                         | Russia                                  |
|![yunbi](https://user-images.githubusercontent.com/1294454/28570548-4d646c40-7147-11e7-9cf6-839b93e6d622.jpg)              | yunbi              | [YUNBI](https://yunbi.com)                                | 2   | [API](https://yunbi.com/documents/api/guide)                                                 | China                                   |
|![zaif](https://user-images.githubusercontent.com/1294454/27766927-39ca2ada-5eeb-11e7-972f-1b4199518ca6.jpg)               | zaif               | [Zaif](https://zaif.jp)                                   | 1   | [API](http://techbureau-api-document.readthedocs.io/ja/latest/index.html)                    | Japan                                   |
|![zb](https://user-images.githubusercontent.com/1294454/32859187-cd5214f0-ca5e-11e7-967d-96568e2e2bd1.jpg)                 | zb                 | [ZB](https://trade.zb.com/api)                            | 1   | [API](https://www.zb.com/i/developer)                                                        | China                                   |

Besides making basic market and limit orders, some exchanges offer margin trading (leverage), various derivatives (like futures contracts and options) and also have [dark pools](https://en.wikipedia.org/wiki/Dark_pool), [OTC](https://en.wikipedia.org/wiki/Over-the-counter_(finance)) (over-the-counter trading), merchant APIs and much more.

## Instantiation

To connect to an exchange and start trading you need to instantiate an exchange class from ccxt library.

To get the full list of ids of supported exchanges programmatically:

```JavaScript
// JavaScript
const ccxt = require ('ccxt')
console.log (ccxt.exchanges)
```

```Python
# Python
import ccxt
print (ccxt.exchanges)
```

```PHP
// PHP
include 'ccxt.php';
var_dump (\ccxt\Exchange::$exchanges);
```

An exchange can be instantiated like shown in the examples below:

```JavaScript
// JavaScript
const ccxt = require ('ccxt')
let exchange = new ccxt.kraken () // default id
let kraken1 = new ccxt.kraken ({ id: 'kraken1' })
let kraken2 = new ccxt.kraken ({ id: 'kraken2' })
let id = 'gdax'
let gdax = new ccxt[id] ();
```

```Python
# Python
import ccxt
exchange = ccxt.okcoinusd () # default id
okcoin1 = ccxt.okcoinusd ({ 'id': 'okcoin1' })
okcoin2 = ccxt.okcoinusd ({ 'id': 'okcoin2' })
id = 'btcchina'
btcchina = eval ('ccxt.%s ()' % id)
gdax = getattr (ccxt, 'gdax') ()
```

The ccxt library in PHP uses builtin UTC/GMT time functions, therefore you are required to set date.timezone in your php.ini or call [date_default_timezone_set ()](http://php.net/manual/en/function.date-default-timezone-set.php) function before using the PHP version of the library. The recommended timezone setting is `"UTC"`.

```PHP
// PHP
date_default_timezone_set ('UTC');
include 'ccxt.php';
$bitfinex = new \ccxt\bitfinex (); // default id
$bitfinex1 = new \ccxt\bitfinex (array ('id' => 'bitfinex1'));
$bitfinex2 = new \ccxt\bitfinex (array ('id' => 'bitfinex2'));
$id = 'kraken';
$kraken = new \ccxt\$id ();
```

## Exchange Structure

Every exchange has a set of properties and methods, most of which you can override by passing an associative array of params to an exchange constructor. You can also make a subclass and override everything.

Here's an overview of base exchange properties with values added for example:

```JavaScript
{
    'id':   'exchange'                  // lowercase string exchange id
    'name': 'Exchange'                  // human-readable string
    'countries': [ 'US', 'CN', 'EU' ],  // string or array of ISO country codes
    'urls': {
        'api': 'https://api.example.com/data',  // string or dictionary of base API URLs
        'www': 'https://www.example.com'        // string website URL
        'doc': 'https://docs.example.com/api',  // string URL or array of URLs
    },
    'version':         'v1',            // string ending with digits
    'api':             { ... },         // dictionary of api endpoints
    'has': {                            // exchange capabilities
        'CORS': false,
        'publicAPI': true,
        'privateAPI': true,
        'cancelOrder': true,
        'createDepositAddress': false,
        'createOrder': true,
        'deposit': false,
        'fetchBalance': true,
        'fetchClosedOrders': false,
        'fetchCurrencies': false,
        'fetchDepositAddress': false,
        'fetchMarkets': true,
        'fetchMyTrades': false,
        'fetchOHLCV': false,
        'fetchOpenOrders': false,
        'fetchOrder': false,
        'fetchOrderBook': true,
        'fetchOrders': false,
        'fetchTicker': true,
        'fetchTickers': false,
        'fetchBidsAsks': false,
        'fetchTrades': true,
        'withdraw': false,
    },
    'timeframes': {                     // empty if the exchange !has.fetchOHLCV
        '1m': '1minute',
        '1h': '1hour',
        '1d': '1day',
        '1M': '1month',
        '1y': '1year',
    },
    'timeout':          10000,          // number in milliseconds
    'rateLimit':        2000,           // number in milliseconds
    'userAgent':       'ccxt/1.1.1 ...' // string, HTTP User-Agent header
    'verbose':          false,          // boolean, output error details
    'markets':         { ... }          // dictionary of markets/pairs by symbol
    'symbols':         [ ... ]          // sorted list of string symbols (traded pairs)
    'currencies':      { ... }          // dictionary of currencies by currency code
    'markets_by_id':   { ... },         // dictionary of dictionaries (markets) by id
    'proxy': 'https://crossorigin.me/', // string URL
    'apiKey':   '92560ffae9b8a0421...', // string public apiKey (ASCII, hex, Base64, ...)
    'secret':   '9aHjPmW+EtRRKN/Oi...'  // string private secret key
    'password': '6kszf4aci8r',          // string password
    'uid':      '123456',               // string user id
}
```

### Exchange Properties

Below is a detailed description of each of the base exchange properties:

- `id`: Each exchange has a default id. The id is not used for anything, it's a string literal for user-land exchange instance identification purposes. You can have multiple links to the same exchange and differentiate them by ids. Default ids are all lowercase and correspond to exchange names.

- `name`: This is a string literal containing the human-readable exchange name.

- `countries`: A string literal or an array of string literals of 2-symbol ISO country codes, where the exchange is operating from.

- `urls['api']`: The single string literal base URL for API calls or an associative array of separate URLs for private and public APIs.

- `urls['www']`: The main HTTP website URL.

- `urls['doc']`: A single string URL link to original documentation for exchange API on their website or an array of links to docs.

- `version`: A string literal containing version identifier for current exchange API. The ccxt library will append this version string to the API Base URL upon each request. You don't have to modify it, unless you are implementing a new exchange API. The version identifier is a usually a numeric string starting with a letter 'v' in some cases, like v1.1. Do not override it unless you are implementing your own new crypto exchange class.

- `api`: An associative array containing a definition of all API endpoints exposed by a crypto exchange. The API definition is used by ccxt to automatically construct callable instance methods for each available endpoint.

- `has`: This is an associative array of exchange capabilities (e.g `fetchTickers`, `fetchOHLCV` or `CORS`).

- `timeframes`: An associative array of timeframes, supported by the fetchOHLCV method of the exchange. This is only populated when `has['fetchTickers']` property is true.

- `timeout`: A timeout in milliseconds for a request-response roundtrip (default timeout is 10000 ms = 10 seconds). You should always set it to a reasonable value, hanging forever with no timeout is not your option, for sure.

- `rateLimit`: A request rate limit in milliseconds. Specifies the required minimal delay between two consequent HTTP requests to the same exchange. This parameter is not used for now (reserved for future).

- `userAgent`: An object to set HTTP User-Agent header to. The ccxt library will set its User-Agent by default. Some exchanges may not like it. If you are having difficulties getting a reply from an exchange and want to turn User-Agent off or use the default one, set this value to false, undefined, or an empty string.

- `verbose`: A boolean flag indicating whether to log HTTP requests to stdout (verbose flag is false by default). Python people have an alternative way of DEBUG logging with a standard pythonic logger, which is enabled by adding these two lines to the beginning of their code:
  ```Python
  import logging
  logging.basicConfig(level=logging.DEBUG)
  ```

- `markets`: An associative array of markets indexed by common trading pairs or symbols. Markets should be loaded prior to accessing this property. Markets are unavailable until you call the `loadMarkets() / load_markets()` method on exchange instance.

- `symbols`: A non-associative array (a list) of symbols available with an exchange, sorted in alphabetical order. These are the keys of the `markets` property. Symbols are loaded and reloaded from markets. This property is a convenient shorthand for all market keys.

- `currencies`: An associative array (a dict) of currencies by codes (usually 3 or 4 letters) available with an exchange. Currencies are loaded and reloaded from markets.

- `markets_by_id`: An associative array of markets indexed by exchange-specific ids. Markets should be loaded prior to accessing this property.

- `proxy`: A string literal containing base URL of http(s) proxy, `''` by default. For use with web browsers and from blocked locations. An example of a proxy string is `'http://crossorigin.me/'`. The absolute exchange endpoint URL is appended to this string before sending the HTTP request.

- `apiKey`: This is your public API key string literal. Most exchanges require this for trading ([see below](https://github.com/ccxt/ccxt/wiki/Manual#api-keys-setup)).

- `secret`: Your private secret API key string literal. Most exchanges require this as well together with the apiKey.

- `password`: A string literal with your password/phrase. Some exchanges require this parameter for trading, but most of them don't.

- `uid`: A unique id of your account. This can be a string literal or a number. Some exchanges also require this for trading, but most of them don't.

- `has`: An assoc-array containing flags for exchange capabilities, including the following:

    ```JavaScript
    'has': {

        'CORS': false,  // has Cross-Origin Resource Sharing enabled (works from browser) or not

        'publicAPI': true,  // has public API available and implemented, true/false
        'privateAPI': true, // has private API available and implemented, true/false

        // unified methods availability flags (can be true, false, or 'emulated'):

        'cancelOrder': true,
        'createDepositAddress': false,
        'createOrder': true,
        'deposit': false,
        'fetchBalance': true,
        'fetchClosedOrders': false,
        'fetchCurrencies': false,
        'fetchDepositAddress': false,
        'fetchMarkets': true,
        'fetchMyTrades': false,
        'fetchOHLCV': false,
        'fetchOpenOrders': false,
        'fetchOrder': false,
        'fetchOrderBook': true,
        'fetchOrders': false,
        'fetchTicker': true,
        'fetchTickers': false,
        'fetchBidsAsks': false,
        'fetchTrades': true,
        'withdraw': false,
    }
    ```

    The meaning of each flag showing availability of this or that method is:

      - boolean `true` means the method is natively available from the exchange API and unified in the ccxt library
      - boolean `false` means the method isn't natively available from the exchange API or not unified in the ccxt library yet
      - an `'emulated'` string means the endpoint isn't natively available from the exchange API but reconstructed by the ccxt library from available true-methods

## Rate Limit

Exchanges usually impose what is called a *rate limit*. Exchanges will remember and track your user credentials and your IP address and will not allow you to query the API too frequently. They balance their load and control traffic congestion to protect API servers from (D)DoS and misuse.

**WARNING: Stay under the rate limit to avoid ban!**

Most exchanges allow **up to 1 or 2 requests per second**. Exchanges may temporarily restrict your access to their API or ban you for some period of time if you are too aggressive with your requests.

### DDoS Protection By Cloudflare / Incapsula

Some exchanges are [DDoS](https://en.wikipedia.org/wiki/Denial-of-service_attack)-protected by [Cloudflare](https://www.cloudflare.com) or [Incapsula](https://www.incapsula.com). Your IP can get temporarily blocked during periods of high load. Sometimes they even restrict whole countries and regions. In that case their servers usually return a page that states a HTTP 40x error or runs an AJAX test of your browser / captcha test and delays the reload of the page for several seconds. Then your browser/fingerprint is granted access temporarily and gets added to a whitelist or receives a HTTP cookie for further use.

If you encounter DDoS protection errors and cannot reach a particular exchange then:

- try using a cloudscraper:
  - https://github.com/ccxt/ccxt/blob/master/examples/js/bypass-cloudflare.js
  - https://github.com/ccxt/ccxt/blob/master/examples/py/bypass-cloudflare.py
- use a proxy (this is less responsive, though)
- ask the exchange support to add you to a whitelist
- run your software in close proximity to the exchange (same country, same city, same datacenter, same server rack, same server)
- try an alternative IP within a different geographic region
- run your software in a distributed network of servers
- ...

In case your calls hit a rate limit or get nonce errors, the ccxt library will throw an exception of one of the following types:

- DDoSProtectionError
- ExchangeNotAvailable
- ExchangeError

A later retry is usually enough to handle that. More on that here:

- [Authentication](https://github.com/ccxt/ccxt/wiki/Manual#authentication)
- [Troubleshooting](https://github.com/ccxt/ccxt/wiki/Manual#troubleshooting)
- [Overriding The Nonce](https://github.com/ccxt/ccxt/wiki/Manual#overriding-the-nonce)

# Markets

Each exchange is a place for trading some kinds of valuables. Sometimes they are called with various different terms like instruments, symbols, trading pairs, currencies, tokens, stocks, commodities, contracts, etc, but they all mean the same – a trading pair, a symbol or a financial instrument.

In terms of the ccxt library, every exchange offers multiple markets within itself. The set of markets differs from exchange to exchange opening possibilities for cross-exchange and cross-market arbitrage. A market is usually a pair of traded crypto/fiat currencies.

## Market Structure

```JavaScript
{
    'id':     'btcusd',   // string literal for referencing within an exchange
    'symbol': 'BTC/USD',  // uppercase string literal of a pair of currencies
    'base':   'BTC',      // uppercase string, base currency, 3 or more letters
    'quote':  'USD',      // uppercase string, quote currency, 3 or more letters
    'active': true,       // boolean, market status
    'precision': {        // number of decimal digits "after the dot"
        'price': 8,       // integer
        'amount': 8,      // integer
        'cost': 8,        // integer
    },
    'limits': {           // value limits when placing orders on this market
        'amount': {
            'min': 0.01,  // order amount should be > min
            'max': 1000,  // order amount should be < max
        },
        'price': { ... }, // same min/max limits for the price of the order
        'cost':  { ... }, // same limits for order cost = price * amount
    }
    'info':      { ... }, // the original unparsed market info from the exchange
}
```

Each market is an associative array (aka dictionary) with the following keys:

- `id`. The string or numeric ID of the market or trade instrument within the exchange. Market ids are used inside exchanges internally to identify trading pairs during the request/response process.
- `symbol`. An uppercase string code representation of a particular trading pair or instrument. This is usually written as `BaseCurrency/QuoteCurrency` with a slash as in `BTC/USD`, `LTC/CNY` or `ETH/EUR`, etc. Symbols are used to reference markets within the ccxt library (explained below).
- `base`. An uppercase string code of base fiat or crypto currency.
- `quote`. An uppercase string code of quoted fiat or crypto currency.
- `active`. A boolean indicating whether or not trading this market is currently possible.
- `info`. An associative array of non-common market properties, including fees, rates, limits and other general market information. The internal info array is different for each particular market, its contents depend on the exchange.
- `precision`. The amounts of decimal digits accepted in order values by exchanges upon order placement for price, amount and cost.
- `limits`. The minimums and maximums for prices, amounts (volumes) and costs (where cost = price * amount).

### Precision And Limits

**Do not confuse `limits` with `precision`!** Precision has nothing to do with min limits. A precision of 8 digits does not necessarily mean a min limit of 0.00000001. The opposite is also true: a min limit of 0.0001 does not necessarily mean a precision of 4.

Examples:

1. `(market['limits']['amount']['min'] == 0.05) && (market['precision']['amount'] == 4)`

  In the first example the **amount** of any order placed on the market **must satisfy both conditions**:

  - The *amount value* should be >= 0.05:
    ```diff
    + good: 0.05, 0.051, 0.0501, 0.0502, ..., 0.0599, 0.06, 0.0601, ...
    - bad: 0.04, 0.049, 0.0499
    ```
  - *Precision of the amount* should up to 4 decimal digits:
    ```diff
    + good: 0.05, 0.051, 0.052, ..., 0.0531, ..., 0.06, ... 0.0719, ...
    - bad: 0.05001, 0.05000, 0.06001
    ```

2. `(market['limits']['price']['min'] == 0.0019) && (market['precision']['price'] == 5)`

  In the second example the **price** of any order placed on the market **must satisfy both conditions**:

  - The *price value* should be >= 0.019:
    ```diff
    + good: 0.019, ... 0.0191, ... 0.01911, 0.01912, ...
    - bad: 0.016, ..., 0.01699
    ```
  - *Precision of price* should be 5 decimal digits or less:
    ```diff
    + good: 0.02, 0.021, 0.0212, 0.02123, 0.02124, 0.02125, ...
    - bad: 0.017000, 0.017001, ...
    ```

3. `(market['limits']['amount']['min'] == 50) && (market['precision']['amount'] == -1)`

  - The *amount value* should be greater than 50:
    ```diff
    + good: 50, 60, 70, 80, 90, 100, ... 2000, ...
    - bad: 1, 2, 3, ..., 9
    ```
  - A negative *amount precision* means that the amount should be an integer multiple of 10:
    ```diff
    + good: 50, ..., 110, ... 1230, ..., 1000000, ..., 1234560, ...
    - bad: 9.5, ... 10.1, ..., 11, ... 200.71, ...
    ```

*The `precision` and `limits` params are currently under heavy development, some of these fields may be missing here and there until the unification process is complete. This does not influence most of the orders but can be significant in extreme cases of very large or very small orders. The `active` flag is not yet supported and/or implemented by all markets.*

## Loading Markets

In most cases you are required to load the list of markets and trading symbols for a particular exchange prior to accessing other API methods. If you forget to load markets the ccxt library will do that automatically upon your first call to the unified API. It will send two HTTP requests, first for markets and then the second one for other data, sequentially.

In order to load markets manually beforehand call the `loadMarkets ()` / `load_markets ()` method on an exchange instance. It returns an associative array of markets indexed by trading symbol. If you want more control over the execution of your logic, preloading markets by hand is recommended.

```JavaScript
// JavaScript
(async () => {
    let kraken = new ccxt.kraken ()
    let markets = await kraken.load_markets ()
    console.log (kraken.id, markets)
}) ()
```

```Python
# Python
okcoin = ccxt.okcoinusd ()
markets = okcoin.load_markets ()
print (okcoin.id, markets)
```

```PHP
// PHP
$id = 'huobi';
$huobi = new \ccxt\$id ();
$markets = $huobi.load_markets ();
var_dump ($huobi->id, $markets);
```

## Symbols And Market Ids

Market ids are used during the REST request-response process to reference trading pairs within exchanges. The set of market ids is unique per exchange and cannot be used across exchanges. For example, the BTC/USD pair/market may have different ids on various popular exchanges, like `btcusd`, `BTCUSD`, `XBTUSD`, `btc/usd`, `42` (numeric id), `BTC/USD`, `Btc/Usd`, `tBTCUSD`, `XXBTZUSD`. You don't need to remember or use market ids, they are there for internal HTTP request-response purposes inside exchange implementations.

The ccxt library abstracts uncommon market ids to symbols, standardized to a common format. Symbols aren't the same as market ids. Every market is referenced by a corresponding symbol. Symbols are common across exchanges which makes them suitable for arbitrage and many other things.

A symbol is an uppercase string literal name for a pair of traded currencies with a slash in between. A currency is a code of three or four uppercase letters, like `BTC`, `ETH`, `USD`, `GBP`, `CNY`, `LTC`, `JPY`, `DOGE`, `RUB`, `ZEC`, `XRP`, `XMR`, etc. Some exchanges have exotic currencies with longer names. The first currency before the slash is usually called *base currency*, and the one after the slash is called *quote currency*.  Examples of a symbol are: `BTC/USD`, `DOGE/LTC`, `ETH/EUR`, `DASH/XRP`, `BTC/CNY`, `ZEC/XMR`, `ETH/JPY`.

Market structures are indexed by symbols and ids. The base exchange class also has builtin methods for accessing markets by symbols. Most API methods require a symbol to be passed in their first argument. You are often required to specify a symbol when querying current prices, making orders, etc.

Most of the time users will be working with market symbols. You will get a standard userland exception if you access non-existent keys in these dicts.

```JavaScript
// JavaScript

(async () => {

    console.log (await exchange.loadMarkets ())

    let btcusd1 = exchange.markets['BTC/USD']     // get market structure by symbol
    let btcusd2 = exchange.market ('BTC/USD')     // same result in a slightly different way

    let btcusdId = exchange.marketId ('BTC/USD')  // get market id by symbol

    let symbols = exchange.symbols                // get an array of symbols
    let symbols2 = Object.keys (exchange.markets) // same as previous line

    console.log (exchange.id, symbols)            // print all symbols

    let currencies = exchange.currencies          // a list of currencies

    let bitfinex = new ccxt.bitfinex ()
    await bitfinex.loadMarkets ()

    bitfinex.markets['BTC/USD']                   // symbol → market (get market by symbol)
    bitfinex.markets_by_id['XRPBTC']              // id → market (get market by id)

    bitfinex.markets['BTC/USD']['id']             // symbol → id (get id by symbol)
    bitfinex.markets_by_id['XRPBTC']['symbol']    // id → symbol (get symbol by id)

})
```

```Python
# Python

print (exchange.load_markets ())

etheur1 = exchange.markets['ETH/EUR']      # get market structure by symbol
etheur2 = exchange.market ('ETH/EUR')      # same result in a slightly different way

etheurId = exchange.market_id ('BTC/USD')  # get market id by symbol

symbols = exchange.symbols                 # get a list of symbols
symbols2 = list (exchange.markets.keys ()) # same as previous line

print (exchange.id, symbols)               # print all symbols

currencies = exchange.currencies           # a list of currencies

kraken = ccxt.kraken ()
kraken.load_markets ()

kraken.markets['BTC/USD']                  # symbol → market (get market by symbol)
kraken.markets_by_id['XXRPZUSD']           # id → market (get market by id)

kraken.markets['BTC/USD']['id']            # symbol → id (get id by symbol)
kraken.markets_by_id['XXRPZUSD']['symbol'] # id → symbol (get symbol by id)
```

```PHP
// PHP

$var_dump ($exchange->load_markets ());

$dashcny1 = $exchange->markets['DASH/CNY'];     // get market structure by symbol
$dashcny2 = $exchange->market ('DASH/CNY');     // same result in a slightly different way

$dashcnyId = $exchange->market_id ('DASH/CNY'); // get market id by symbol

$symbols = $exchange->symbols;                  // get an array of symbols
$symbols2 = array_keys ($exchange->markets);    // same as previous line

var_dump ($exchange->id, $symbols);             // print all symbols

$currencies = $exchange->currencies;            // a list of currencies

$okcoinusd = '\\ccxt\\okcoinusd';
$okcoinusd = new $okcoinusd ();

$okcoinusd->load_markets ();

$okcoinusd->markets['BTC/USD'];                 // symbol → market (get market by symbol)
$okcoinusd->markets_by_id['btc_usd'];           // id → market (get market by id)

$okcoinusd->markets['BTC/USD']['id'];           // symbol → id (get id by symbol)
$okcoinusd->markets_by_id['btc_usd']['symbol']; // id → symbol (get symbol by id)
```

### Naming Consistency

There is a bit of term ambiguity across various exchanges that may cause confusion among newcoming traders. Some exchanges call markets as *pairs*, whereas other exchanges call symbols as *products*. In terms of the ccxt library, each exchange contains one or more trading markets. Each market has an id and a symbol. Most symbols are pairs of base currency and quote currency.

```Exchanges → Markets → Symbols → Currencies```

Historically various symbolic names have been used to designate same trading pairs. Some cryptocurrencies (like Dash) even changed their names more than once during their ongoing lifetime. For consistency across exchanges the ccxt library will perform the following known substitutions for symbols and currencies:

- `XBT → BTC`: `XBT` is newer but `BTC` is more common among exchanges and sounds more like bitcoin ([read more](https://www.google.ru/search?q=xbt+vs+btc)).
- `BCC → BCH`: The Bitcoin Cash fork is often called with two different symbolic names: `BCC` and `BCH`. The name `BCC` is ambiguous for Bitcoin Cash, it is confused with BitConnect. The ccxt library will convert `BCC` to `BCH` where it is appropriate (some exchanges and aggregators confuse them).
- `DRK → DASH`: `DASH` was Darkcoin then became Dash ([read more](https://minergate.com/blog/dashcoin-and-dash/)).
- `DSH → DASH`: Try not to confuse symbols and currencies. The `DSH` (Dashcoin) is not the same as `DASH` (Dash). Some exchanges have `DASH` labelled inconsistently as `DSH`, the ccxt library does a correction for that as well (`DSH → DASH`), but only on certain exchanges that have these two currencies confused, whereas most exchanges have them both correct. Just remember that `DASH/BTC` is not the same as `DSH/BTC`.

### Consistency Of Base And Quote Currencies

It depends on which exchange you are using, but some of them have a reversed (inconsistent) pairing of `base` and `quote`. They actually have base and quote misplaced  (switched/reversed sides). In that case you'll see a difference of parsed `base` and `quote` currency values with the unparsed `info` in the market substructure.

For those exchanges the ccxt will do a correction, switching and normalizing sides of base and quote currencies when parsing exchange replies. This logic is financially and terminologically correct. If you want less confusion, remember the following rule: **base is always before the slash, quote is always after the slash in any symbol and with any market**.

```
base currency ↓
             BTC / USDT
             ETH / BTC
            DASH / ETH
                    ↑ quote currency
```

## Market Cache Force Reload

The `loadMarkets () / load_markets ()` is also a dirty method with a side effect of saving the array of markets on the exchange instance. You only need to call it once per exchange. All subsequent calls to the same method will return the locally saved (cached) array of markets.

When exchange markets are loaded, you can then access market information any time via the `markets` property. This property contains an associative array of markets indexed by symbol. If you need to force reload the list of markets after you have them loaded already, pass the reload = true flag to the same method again.

```JavaScript
// JavaScript
(async () => {
    let kraken = new ccxt.kraken ({ verbose: true }) // log HTTP requests
    await kraken.load_markets () // request markets
    console.log (kraken.id, kraken.markets)    // output a full list of all loaded markets
    console.log (Object.keys (kraken.markets)) // output a short list of market symbols
    console.log (kraken.markets['BTC/USD'])    // output single market details
    await kraken.load_markets () // return a locally cached version, no reload
    let reloadedMarkets = await kraken.load_markets (true) // force HTTP reload = true
    console.log (reloadedMarkets['ETH/BTC'])
}) ()
```

```Python
# Python
poloniex = ccxt.poloniex({'verbose': True}) # log HTTP requests
poloniex.load_markets() # request markets
print(poloniex.id, poloniex.markets)   # output a full list of all loaded markets
print(list(poloniex.markets.keys())) # output a short list of market symbols
print(poloniex.markets['BTC/ETH'])     # output single market details
poloniex.load_markets() # return a locally cached version, no reload
reloadedMarkets = poloniex.load_markets(True) # force HTTP reload = True
print(reloadedMarkets['ETH/ZEC'])
```

```PHP
// PHP
$bitfinex = new \ccxt\bitfinex (array ('verbose' => true)); // log HTTP requests
$bitfinex.load_markets (); // request markets
var_dump ($bitfinex->id, $bitfinex->markets); // output a full list of all loaded markets
var_dump (array_keys ($bitfinex->markets));   // output a short list of market symbols
var_dump ($bitfinex->markets['XRP/USD']);     // output single market details
$bitfinex->load_markets (); // return a locally cached version, no reload
$reloadedMarkets = $bitfinex->load_markets (true); // force HTTP reload = true
var_dump ($bitfinex->markets['XRP/BTC']);
```

# API Methods / Endpoints

Each exchange offers a set of API methods. Each method of the API is called an *endpoint*. Endpoints are HTTP URLs for querying various types of information. All endpoints return JSON in response to client requests.

Usually, there is an endpoint for getting a list of markets from an exchange, an endpoint for retrieving an order book for a particular market, an endpoint for retrieving trade history, endpoints for placing and canceling orders, for money deposit and withdrawal, etc... Basically every kind of action you could perform within a particular exchange has a separate endpoint URL offered by the API.

Because the set of methods differs from exchange to exchange, the ccxt library implements the following:
- a public and private API for all possible URLs and methods
- a unified API supporting a subset of common methods

The endpoint URLs are predefined in the `api` property for each exchange. You don't have to override it, unless you are implementing a new exchange API (at least you should know what you're doing).

## Implicit API Methods

Most of exchange-specific API methods are implicit, meaning that they aren't defined explicitly anywhere in code. The library implements a declarative approach for defining implicit (non-unified) exchanges' API methods.

Each method of the API usually has its own endpoint. The library defines all endpoints for each particular exchange in the `.api` property. Upon exchange construction an implicit *magic* method (aka *partial function* or *closure*) will be created inside `defineRestApi()/define_rest_api()` on the exchange instance for each endpoint from the list of `.api` endpoints. This is performed for all exchanges universally. Each generated method will be accessible in both `camelCase` and `under_score` notations.

The endpoints definition is a **full list of ALL API URLs** exposed by an exchange. This list gets converted to callable methods upon exchange instantiation. Each URL in the API endpoint list gets a corresponding callable method. This is done automatically for all exchanges, therefore the ccxt library supports **all possible URLs** offered by crypto exchanges.

Each implicit method gets a unique name which is constructed from the `.api` definition. For example, a private HTTPS PUT `https://api.exchange.com/order/{id}/cancel` endpoint will have a corresponding exchange method named `.privatePutOrderIdCancel()`/`.private_put_order_id_cancel()`. A public HTTPS GET `https://api.exchange.com/market/ticker/{pair}` endpoint would result in the corresponding method named `.publicGetTickerPair()`/`.public_get_ticker_pair()`, and so on.

An implicit method takes a dictionary of parameters, sends the request to the exchange and returns an exchange-specific JSON result from the API **as is, unparsed**. To pass a parameter, add it to the dictionary explicitly under a key equal to the parameter's name. For the examples above, this would look like `.privatePutOrderIdCancel ({ id: '41987a2b-...' })` and `.publicGetTickerPair ({ pair: 'BTC/USD' })`.

The recommended way of working with exchanges is not using exchange-specific implicit methods but using the unified ccxt methods instead. The exchange-specific methods should be used as a fallback in cases when a corresponding unified method isn't available (yet).

To get a list of all available methods with an exchange instance, including implicit methods and unified methods you can simply do the following:

```
console.log (new ccxt.kraken ())   // JavaScript
print (dir (ccxt.hitbtc ()))        # Python
var_dump (new \ccxt\okcoinusd ()); // PHP
```

## Public/Private API

API URLs are often grouped into two sets of methods called a *public API* for market data and a *private API* for trading and account access. These groups of API methods are usually prefixed with a word 'public' or 'private'.

A public API is used to access market data and does not require any authentication whatsoever. Most exchanges provide market data openly to all (under their rate limit). With the ccxt library anyone can access market data out of the box without having to register with the exchanges and without setting up account keys and passwords.

Public APIs include the following:

- instruments/trading pairs
- price feeds (exchange rates)
- order books (L1, L2, L3...)
- trade history (closed orders, transactions, executions)
- tickers (spot / 24h price)
- OHLCV series for charting
- other public endpoints

For trading with private API you need to obtain API keys from/to exchanges. It often means registering with exchanges and creating API keys with your account. Most exchanges require personal info or identification. Some kind of verification may be necessary as well.

If you want to trade you need to register yourself, this library will not create accounts or API keys for you. Some exchange APIs expose interface methods for registering an account from within the code itself, but most of exchanges don't. You have to sign up and create API keys with their websites.

Private APIs allow the following:

- manage personal account info
- query account balances
- trade by making market and limit orders
- create deposit addresses and fund accounts
- request withdrawal of fiat and crypto funds
- query personal open / closed orders
- query positions in margin/leverage trading
- get ledger history
- transfer funds between accounts
- use merchant services

Some exchanges offer the same logic under different names. For example, a public API is also often called *market data*, *basic*, *market*, *mapi*, *api*, *price*, etc... All of them mean a set of methods for accessing data available to public. A private API is also often called *trading*, *trade*, *tapi*, *exchange*, *account*, etc...

A few exchanges also expose a merchant API which allows you to create invoices and accept crypto and fiat payments from your clients. This kind of API is often called *merchant*, *wallet*, *payment*, *ecapi* (for e-commerce).

To get a list of all available methods with an exchange instance, you can simply do the following:

```
console.log (new ccxt.kraken ())   // JavaScript
print (dir (ccxt.hitbtc ()))        # Python
var_dump (new \ccxt\okcoinusd ()); // PHP
```

## Synchronous vs Asynchronous Calls

In the JavaScript version of CCXT all methods are asynchronous and return [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that resolve with a decoded JSON object. In CCXT we use the modern *async/await* syntax to work with Promises. If you're not familiar with that syntax, you can read more about it [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function).

```JavaScript
// JavaScript

(async () => {
    let pairs = await kraken.publicGetSymbolsDetails ()
    let marketIds = Object.keys (pairs['result'])
    let marketId = marketIds[0]
    let ticker = await kraken.publicGetTicker ({ pair: marketId })
    console.log (kraken.id, marketId, ticker)
}) ()
```

The ccxt library supports asynchronous concurrency mode in Python 3.5+ with async/await syntax. The asynchronous Python version uses pure [asyncio](https://docs.python.org/3/library/asyncio.html) with [aiohttp](http://aiohttp.readthedocs.io). In async mode you have all the same properties and methods, but most methods are decorated with an async keyword. If you want to use async mode, you should link against the `ccxt.async` subpackage, like in the following example:

```Python
# Python

import asyncio
import ccxt.async as ccxt

async def print_poloniex_ethbtc_ticker():
    poloniex = ccxt.poloniex()
    print(await poloniex.fetch_ticker('ETH/BTC'))

asyncio.get_event_loop().run_until_complete(print_poloniex_ethbtc_ticker())
```

In PHP all API methods are synchronous.

## Returned JSON Objects

All public and private API methods return raw decoded JSON objects in response from the exchanges, as is, untouched. The unified API returns JSON-decoded objects in a common format and structured uniformly across all exchanges.

## Passing Parameters To API Methods

The set of all possible API endpoints differs from exchange to exchange. Most of methods accept a single associative array (or a Python dict) of key-value parameters. The params are passed as follows:

```
bitso.publicGetTicker ({ book: 'eth_mxn' })                 // JavaScript
ccxt.zaif().public_get_ticker_pair ({ 'pair': 'btc_jpy' })  # Python
$luno->public_get_ticker (array ('pair' => 'XBTIDR'));      // PHP
```

For a full list of accepted method parameters for each exchange, please consult [API docs](#exchanges).

### API Method Naming Conventions

An exchange method name is a concatenated string consisting of type (public or private), HTTP method (GET, POST, PUT, DELETE) and endpoint URL path like in the following examples:

| Method Name                  | Base API URL                   | Endpoint URL                   |
|------------------------------|--------------------------------|--------------------------------|
| publicGetIdOrderbook         | https://bitbay.net/API/Public  | {id}/orderbook                 |
| publicGetPairs               | https://bitlish.com/api        | pairs                          |
| publicGetJsonMarketTicker    | https://www.bitmarket.net      | json/{market}/ticker           |
| privateGetUserMargin         | https://bitmex.com             | user/margin                    |
| privatePostTrade             | https://btc-x.is/api           | trade                          |
| tapiCancelOrder              | https://yobit.net              | tapi/CancelOrder               |
| ...                          | ...                            | ...                            |

The ccxt library supports both camelcase notation (preferred in JavaScript) and underscore notation (preferred in Python and PHP), therefore all methods can be called in either notation or coding style in any language. Both of these notations work in JavaScript, Python and PHP:

```
exchange.methodName ()  // camelcase pseudocode
exchange.method_name () // underscore pseudocode
```

To get a list of all available methods with an exchange instance, you can simply do the following:

```
console.log (new ccxt.kraken ())   // JavaScript
print (dir (ccxt.hitbtc ()))        # Python
var_dump (new \ccxt\okcoinusd ()); // PHP
```

## Unified API

The unified ccxt API is a subset of methods common among the exchanges. It currently contains the following methods:

- `fetchMarkets ()`: Fetches a list of all available markets from an exchange and returns an array of markets (objects with properties such as `symbol`, `base`, `quote` etc.). Some exchanges do not have means for obtaining a list of markets via their online API. For those, the list of markets is hardcoded.
- `loadMarkets ([reload])`: Returns the list of markets as an object indexed by symbol and caches it with the exchange instance. Returns cached markets if loaded already, unless the `reload = true` flag is forced.
- `fetchOrderBook (symbol[, limit = undefined[, params = {}]])`: Fetch L2/L3 order book for a particular market trading symbol.
- `fetchL2OrderBook (symbol[, limit = undefined[, params]])`: Level 2 (price-aggregated) order book for a particular symbol.
- `fetchTrades (symbol[, since[, [limit, [params]]]])`: Fetch recent trades for a particular trading symbol.
- `fetchTicker (symbol)`: Fetch latest ticker data by trading symbol.
- `fetchBalance ()`: Fetch Balance.
- `createOrder (symbol, type, side, amount[, price[, params]])`
- `createLimitBuyOrder (symbol, amount, price[, params])`
- `createLimitSellOrder (symbol, amount, price[, params])`
- `createMarketBuyOrder (symbol, amount[, params])`
- `createMarketSellOrder (symbol, amount[, params])`
- `cancelOrder (id[, symbol[, params]])`
- `fetchOrder (id[, symbol[, params]])`
- `fetchOrders ([symbol[, params]])`
- `fetchOpenOrders ([symbol[, params]])`
- `fetchClosedOrders ([symbol[, params]])`
- ...

Note, that most of methods of the unified API accept an optional `params` parameter. It is an associative array (a dictionary, empty by default) containing the params you want to override. Use the `params` dictionary if you need to pass a custom setting or an optional parameter to your unified query.

# Market Data

- [Order Book / Market Depth](https://github.com/ccxt/ccxt/wiki/Manual#order-book--market-depth)
  - [Market Price](https://github.com/ccxt/ccxt/wiki/Manual#market-price)
- [Price Tickers](https://github.com/ccxt/ccxt/wiki/Manual#price-tickers)
  - [Individually By Symbol](https://github.com/ccxt/ccxt/wiki/Manual#individually-by-symbol)
  - [All At Once](https://github.com/ccxt/ccxt/wiki/Manual#all-at-once)
- [OHLCV Candlestick Charts](https://github.com/ccxt/ccxt/wiki/Manual#ohlcv-candlestick-charts)
- [Public Trades](https://github.com/ccxt/ccxt/wiki/Manual#public-trades)

## Order Book

Exchanges expose information on open orders with bid (buy) and ask (sell) prices, volumes and other data. Usually there is a separate endpoint for querying current state (stack frame) of the *order book* for a particular market. An order book is also often called *market depth*. The order book information is used in the trading decision making process.

The method for fetching an order book for a particular symbol is named `fetchOrderBook` or `fetch_order_book`. It accepts a symbol and an optional dictionary with extra params (if supported by a particular exchange). The method for fetching the order book is called like shown below:

```JavaScript
// JavaScript
delay = 2000 // milliseconds = seconds * 1000
(async () => {
    for (symbol in exchange.markets) {
        console.log (await exchange.fetchOrderBook (symbol))
        await new Promise (resolve => setTimeout (resolve, delay)) // rate limit
    }
}) ()
```

```Python
# Python
import time
delay = 2 # seconds
for symbol in exchange.markets:
    print (exchange.fetch_order_book (symbol))
    time.sleep (delay) # rate limit
```

```PHP
// PHP
$delay = 2000000; // microseconds = seconds * 1000000
foreach ($exchange->markets as $symbol => $market) {
    var_dump ($exchange->fetch_order_book ($symbol));
    usleep ($delay); // rate limit
}
```

The structure of a returned order book is as follows:

```JavaScript
{
    'bids': [
        [ price, amount ], // [ float, float ]
        [ price, amount ],
        ...
    ],
    'asks': [
        [ price, amount ],
        [ price, amount ],
        ...
    ],
    'timestamp': 1499280391811, // Unix Timestamp in milliseconds (seconds * 1000)
    'datetime': '2017-07-05T18:47:14.692Z', // ISO8601 datetime string with milliseconds
}
```

Prices and amounts are floats. The bids array is sorted by price in descending order. The best (highest) bid price is the first element and the worst (lowest) bid price is the last element. The asks array is sorted by price in ascending order. The best (lowest) ask price is the first element and the worst (highest) ask price is the last element. Bid/ask arrays can be empty if there are no corresponding orders in the order book of an exchange.

Exchanges may return the stack of orders in various levels of details for analysis. It is either in full detail containing each and every order, or it is aggregated having slightly less detail where orders are grouped and merged by price and volume. Having greater detail requires more traffic and bandwidth and is slower in general but gives a benefit of higher precision. Having less detail is usually faster, but may not be  enough in some very specific cases.

### Market Depth

Some exchanges accept a dictionary of extra parameters to the `fetchOrderBook () / fetch_order_book ()` function. **All extra `params` are exchange-specific (non-unified)**. You will need to consult exchanges docs if you want to override a particular param, like the depth of the order book. You can get a limited count of returned orders or a desired level of aggregation (aka *market depth*) by specifying an limit argument and exchange-specific extra `params` like so:

```JavaScript
// JavaScript

(async function test () {
    const ccxt = require ('ccxt')
    const exchange = new ccxt.bitfinex ()
    const limit = 5
    const orders = await exchange.fetchOrderBook ('BTC/USD', limit, {
        // this parameter is exchange-specific, all extra params have unique names per exchange
        'group': 1, // 1 = orders are grouped by price, 0 = orders are separate
    })
}) ()
```

```Python
# Python

import ccxt
# return up to ten bidasks on each side of the order book stack
limit = 10
ccxt.cex().fetch_order_book('BTC/USD', limit)
```

```PHP
// PHP

// instantiate the exchange by id
$exchange = '\\ccxt\\kraken';
$exchange = new $exchange ();
// up to ten orders on each side, for example
$limit = 20;
var_dump ($exchange->fetch_order_book ('BTC/USD', $limit));
```

The levels of detail or levels of order book aggregation are often number-labelled like L1, L2, L3...

- **L1**: less detail for quickly obtaining very basic info, namely, the market price only. It appears to look like just one order in the order book.
- **L2**: most common level of aggregation where order volumes are grouped by price. If two orders have the same price, they appear as one single order for a volume equal to their total sum. This is most likely the level of aggregation you need for the majority of purposes.
- **L3**: most detailed level with no aggregation where each order is separate from other orders. This LOD naturally contains duplicates in the output. So, if two orders have equal prices they are **not** merged together and it's up to the exchange's matching engine to decide on their priority in the stack. You don't really need L3 detail for successful trading. In fact, you most probably don't need it at all. Therefore some exchanges don't support it and always return aggregated order books.

If you want to get an L2 order book, whatever the exchange returns, use the `fetchL2OrderBook(symbol, limit, params)` or `fetch_l2_order_book(symbol, limit, params)` unified method for that.

### Market Price

In order to get current best price (query market price) and calculate bidask spread take first elements from bid and ask, like so:

```JavaScript
// JavaScript
let orderbook = exchange.fetchOrderBook (exchange.symbols[0])
let bid = orderbook.bids.length ? orderbook.bids[0][0] : undefined
let ask = orderbook.asks.length ? orderbook.asks[0][0] : undefined
let spread = (bid && ask) ? ask - bid : undefined
console.log (exchange.id, 'market price', { bid, ask, spread })
```

```Python
# Python
orderbook = exchange.fetch_order_book (exchange.symbols[0])
bid = orderbook['bids'][0][0] if len (orderbook['bids']) > 0 else None
ask = orderbook['asks'][0][0] if len (orderbook['asks']) > 0 else None
spread = (ask - bid) if (bid and ask) else None
print (exchange.id, 'market price', { 'bid': bid, 'ask': ask, 'spread': spread })
```

```PHP
// PHP
$orderbook = $exchange->fetch_order_book ($exchange->symbols[0]);
$bid = count ($orderbook['bids']) ? $orderbook['bids'][0][0] : null;
$ask = count ($orderbook['asks']) ? $orderbook['asks'][0][0] : null;
$spread = ($bid && $ask) ? $ask - $bid : null;
$result = array ('bid' => $bid, 'ask' => $ask, 'spread' => $spread);
var_dump ($exchange->id, 'market price', $result);
```

## Price Tickers

A price ticker contains statistics for a particular market/symbol for some period of time in recent past, usually last 24 hours. The structure of a ticker is as follows:

```JavaScript
{
    'symbol':        string symbol of the market ('BTC/USD', 'ETH/BTC', ...)
    'info':        { the original non-modified unparsed reply from exchange API },
    'timestamp':     int (64-bit Unix Timestamp in milliseconds since Epoch 1 Jan 1970)
    'datetime':      ISO8601 datetime string with milliseconds
    'high':          float, // highest price
    'low':           float, // lowest price
    'bid':           float, // current best bid (buy) price
    'bidVolume':     float, // current best bid (buy) amount
    'ask':           float, // current best ask (sell) price
    'askVolume':     float, // current best ask (sell) amount
    'vwap':          float, // volume weighed average price
    'open':          float, // opening price
    'close':         float, // price of last trade (closing price for current period)
    'last':          float, // same as `close`, duplicated for convenience
    'previousClose': float, // closing price for the previous period
    'change':        float, // absolute change, `last - open`
    'percentage':    float, // relative change, `(change/open) * 100`
    'average':       float, // average price, `(last + open) / 2`
    'baseVolume':    float, // volume of base currency
    'quoteVolume':   float, // volume of quote currency
}
```

Timestamp and datetime are both Universal Time Coordinated (UTC).

Although some exchanges do mix-in orderbook's top bid/ask prices into their tickers (and some even top bid/asks volumes) you should not treat ticker as a `fetchOrderBook` replacement. The main purpose of a ticker is to serve statistical data, as such, treat it as "live 24h OHLCV". It is known that exchanges discourage frequent `fetchTicker` requests by imposing stricter rate limits on these queries. If you need a unified way to access bid/asks you should use `fetchL[123]OrderBook` family instead.

### Individually By Symbol

To get the individual ticker data from an exchange for each particular trading pair or symbol call the `fetchTicker (symbol)`:

```JavaScript
// JavaScript
(async () => {
    console.log (await (exchange.fetchTicker ('BTC/USD'))) // ticker for BTC/USD
    let symbols = Object.keys (exchange.markets)
    let random = Math.floor ((Math.random () * symbols.length)) - 1
    console.log (exchange.fetchTicker (symbols[random])) // ticker for a random symbol
}) ()
```

```Python
# Python
import random
print(exchange.fetch_ticker('LTC/ZEC')) # ticker for LTC/ZEC
symbols = list(exchange.markets.keys())
print(exchange.fetch_ticker(random.choice(symbols))) # ticker for a random symbol
```

```PHP
// PHP (don't forget to set your timezone properly!)
var_dump ($exchange->fetch_ticker ('ETH/CNY')); // ticker for ETH/CNY
$symbols = array_keys ($exchange->markets);
$random = rand () % count ($symbols);
var_dump ($exchange->fetch_ticker ($symbols[$random])); // ticker for a random symbol
```

### All At Once

Some exchanges (not all of them) also support fetching all tickers at once. See [their docs](https://github.com/ccxt/ccxt/wiki/Manual#exchanges) for details. You can fetch all tickers with a single call like so:

```JavaScript
// JavaScript
(async () => {
    console.log (await (exchange.fetchTickers ())) // all tickers indexed by their symbols
}) ()
```

```Python
# Python
print(exchange.fetch_tickers()) # all tickers indexed by their symbols
```

```PHP
// PHP
var_dump ($exchange->fetch_tickers ()); // all tickers indexed by their symbols
```

Fetching all tickers requires more traffic than fetching a single ticker. If you only need one ticker, fetching by a particular symbol is faster in general. You probably want to fetch all tickers only if you really need all of them.

The structure of returned value is as follows:

```JavaScript
{
    'info':    { ... }, // the original JSON response from the exchange as is
    'BTC/USD': { ... }, // a single ticker for BTC/USD
    'ETH/BTC': { ... }, // a ticker for ETH/BTC
    ...
}
```

A general solution for fetching all tickers from all exchanges (even the ones that don't have a corresponding API endpoint) is on the way, this section will be updated soon.

```
UNDER CONSTRUCTION
```

#### Async Mode / Concurrency

```
UNDER CONSTRUCTION
```

## OHLCV Candlestick Charts

```diff
- this is under heavy development right now, contributions appreciated
```

Most exchanges have endpoints for fetching OHLCV data, but some of them don't. The exchange boolean (true/false) property named `has['fetchOHLCV']` indicates whether the exchange supports candlestick data series or not.

The `fetchOHLCV` method is declared in the following way:

```
fetchOHLCV (symbol, timeframe = '1m', since = undefined, limit = undefined, params = {})
```

You can call the unified `fetchOHLCV` / `fetch_ohlcv` method to get the list of most recent OHLCV candles for a particular symbol like so:

```JavaScript
// JavaScript
let sleep = (ms) => new Promise (resolve => setTimeout (resolve, ms));
if (exchange.has.fetchOHLCV) {
    (async () => {
        for (symbol in exchange.markets) {
            await sleep (exchange.rateLimit) // milliseconds
            console.log (await exchange.fetchOHLCV (symbol, '1m')) // one minute
        }
    }) ()
}
```

```Python
# Python
import time
if exchange.has['fetchOHLCV']:
    for symbol in exchange.markets:
        time.sleep (exchange.rateLimit / 1000) # time.sleep wants seconds
        print (symbol, exchange.fetch_ohlcv (symbol, '1d')) # one day
```

```PHP
// PHP
if ($exchange->has['fetchOHLCV'])
    foreach ($exchange->markets as $symbol => $market) {
        usleep ($exchange.rateLimit * 1000); // usleep wants microseconds
        var_dump ($exchange->fetch_ohlcv ($symbol, '1M')); // one month
    }
```

To get the list of available timeframes for your exchange see the `timeframes` property. Note that it is only populated when `has['fetchTickers']`  is true as well.

 **There's a limit on how far back in time your requests can go.** Most of exchanges will not allow to query detailed candlestick history (like those for 1-minute and 5-minute timeframes) too far in the past. They usually keep a reasonable amount of most recent candles, like 1000 last candles for any timeframe is more than enough for most of needs. You can work around that limitation by continuously fetching (aka *REST polling*) latest OHLCVs and storing them in a CSV file or in a database.

The fetchOHLCV method shown above returns a list (a flat array) of OHLCV candles represented by the following structure:

```JavaScript
[
    [
        1504541580000, // UTC timestamp in milliseconds, integer
        4235.4,        // (O)pen price, float
        4240.6,        // (H)ighest price, float
        4230.0,        // (L)owest price, float
        4230.7,        // (C)losing price, float
        37.72941911    // (V)olume (in terms of the base currency), float
    ],
    ...
]
```

### OHLCV Emulation

Some exchanges don't offer any OHLCV method, and for those, the ccxt library will emulate OHLCV candles from [Public Trades](https://github.com/ccxt/ccxt/wiki/Manual#trades-executions-transactions). In that case you will see `exchange.has['fetchOHLCV'] = 'emulated'`. However, because the trade history is usually very limited, the emulated fetchOHLCV methods cover most recent info only and should only be used as a fallback, when no other option is available.

**WARNING: the fetchOHLCV emulations is experimental!**

## Trades, Executions, Transactions

```diff
- this is under heavy development right now, contributions appreciated
```

You can call the unified `fetchTrades` / `fetch_trades` method to get the list of most recent trades for a particular symbol. The `fetchTrades` method is declared in the following way:

```
async fetchTrades (symbol, since = undefined, limit = undefined, params = {})
```

For example, if you want to print recent trades for all symbols one by one sequentially (mind the rateLimit!) you would do it like so:

```JavaScript
// JavaScript
let sleep = (ms) => new Promise (resolve => setTimeout (resolve, ms));
(async () => {
    for (symbol in exchange.markets) {
        await sleep (exchange.rateLimit) // milliseconds
        console.log (await exchange.fetchTrades (symbol))
    }
}) ()
```

```Python
# Python
import time
for symbol in exchange.markets:                    # ensure you have called loadMarkets() or load_markets() method.
    time.sleep (exchange.rateLimit / 1000)         # time.sleep wants seconds
    print (symbol, exchange.fetch_trades (symbol))
```

```PHP
// PHP
foreach ($exchange->markets as $symbol => $market) {
    usleep ($exchange.rateLimit * 1000); // usleep wants microseconds
    var_dump ($exchange->fetch_trades ($symbol));
}
```

The fetchTrades method shown above returns an ordered list of trades (a flat array, sorted by timestamp in ascending order, most recent trade last) represented by the following structure:

```JavaScript
[
    {
        'info':       { ... },                  // the original decoded JSON as is
        'id':        '12345-67890:09876/54321', // string trade id
        'timestamp':  1502962946216,            // Unix timestamp in milliseconds
        'datetime':  '2017-08-17 12:42:48.000', // ISO8601 datetime with milliseconds
        'symbol':    'ETH/BTC',                 // symbol
        'order':     '12345-67890:09876/54321', // string order id or undefined/None/null
        'type':      'limit',                   // order type, 'market', 'limit' or undefined/None/null
        'side':      'buy',                     // direction of the trade, 'buy' or 'sell'
        'price':      0.06917684,               // float price in quote currency
        'amount':     1.5,                      // amount of base currency
    },
    ...
]
```

Most exchanges return most of the above fields for each trade, though there are exchanges that don't return the type, the side, the trade id or the order id of the trade. Most of the time you are guaranteed to have the timestamp, the datetime, the symbol, the price and the amount of each trade.

The second optional argument `since` reduces the array by timestamp, the third `limit` argument reduces by number (count) of returned items.

The `fetchTrades ()` / `fetch_trades()` method also accepts an optional `params` (assoc-key array/dict, empty by default) as its fourth argument. You can use it to pass extra params to method calls or to override a particular default value (where supported by the exchange). See the API docs for your exchange for more details.

```
UNDER CONSTRUCTION
```

# Trading

In order to be able to access your user account, perform algorithmic trading by placing market and limit orders, query balances, deposit and withdraw funds and so on, you need to obtain your API keys for authentication from each exchange you want to trade with. They usually have it available on a separate tab or page within your user account settings. API keys are exchange-specific and cannnot be interchanged under any circumstances.

## Authentication

Authentication with all exchanges is handled automatically if provided with proper API keys. The process of authentication usually goes through the following pattern:

1. Generate new nonce. A nonce is an integer, often a Unix Timestamp in seconds or milliseconds (since epoch January 1, 1970). The nonce should be unique to a particular request and constantly increasing, so that no two requests share the same nonce. Each next request should have greater nonce than the previous request. **The default nonce is a 32-bit Unix Timestamp in seconds.**
2. Append public apiKey and nonce to other endpoint params, if any, then serialize the whole thing for signing.
3. Sign the serialized params using HMAC-SHA256/384/512 or MD5 with your secret key.
4. Append the signature in Hex or Base64 and nonce to HTTP headers or body.

This process may differ from exchange to exchange. Some exchanges may want the signature in a different encoding, some of them vary in header and body param names and formats, but the general pattern is the same for all of them.

**You should not share the same API keypair across multiple instances of an exchange running simultaneously, in separate scripts or in multiple threads. Using the same keypair from different instances simultaneously may cause all sorts of unexpected behaviour.**

The authentication is already handled for you, so you don't need to perform any of those steps manually unless you are implementing a new exchange class. The only thing you need for trading is the actual API key pair.

## API Keys Setup

The API credentials usually include the following:

- `apiKey`. This is your public API Key and/or Token. This part is *non-secret*, it is included in your request header or body and sent over HTTPS in open text to identify your request. It is often a string in Hex or Base64 encoding or an UUID identifier.
- `secret`. This is your private key. Keep it secret, don't tell it to anybody. It is used to sign your requests locally before sending them to exchanges. The secret key does not get sent over the internet in the request-response process and should not be published or emailed. It is used together with the nonce to generate a cryptographically strong signature. That signature is sent with your public key to authenticate your identity. Each request has a unique nonce and therefore a unique cryptographic signature.
- `uid`. Some exchanges (not all of them) also generate a user id or *uid* for short. It can be a string or numeric literal. You should set it, if that is explicitly required by your exchange. See [their docs](https://github.com/ccxt/ccxt/wiki/Manual#exchanges) for details.
- `password`. Some exchanges (not all of them) also require your password/phrase for trading. You should set this string, if that is explicitly required by your exchange. See [their docs](https://github.com/ccxt/ccxt/wiki/Manual#exchanges) for details.

In order to create API keys find the API tab or button in your user settings on the exchange website. Then create your keys and copy-paste them to your config file. Your config file permissions should be set appropriately, unreadable to anyone except the owner.

**Remember to keep your apiKey and secret key safe from unauthorized use, do not send or tell it to anybody. A leak of the secret key or a breach in security can cost you a fund loss.**

To set up an exchange for trading just assign the API credentials to an existing exchange instance or pass them to exchange constructor upon instantiation, like so:

```JavaScript
// JavaScript

const ccxt = require ('ccxt')

// any time
let kraken = new ccxt.kraken ()
kraken.apiKey = 'YOUR_KRAKEN_API_KEY'
kraken.secret = 'YOUR_KRAKEN_SECRET_KEY'

// upon instantiation
let okcoinusd = new ccxt.okcoinusd ({
    apiKey: 'YOUR_OKCOIN_API_KEY',
    secret: 'YOUR_OKCOIN_SECRET_KEY',
})
```

```Python
# Python

import ccxt

# any time
bitfinex = ccxt.bitfinex ()
bitfinex.apiKey = 'YOUR_BFX_API_KEY'
bitfinex.secret = 'YOUR_BFX_SECRET'

# upon instantiation
hitbtc = ccxt.hitbtc ({
    'apiKey': 'YOUR_HITBTC_API_KEY',
    'secret': 'YOUR_HITBTC_SECRET_KEY',
})
```

```PHP
// PHP

include 'ccxt.php'

// any time
$quoine = new \ccxt\quoine ();
$quoine->apiKey = 'YOUR_QUOINE_API_KEY';
$quoine->secret = 'YOUR_QUOINE_SECRET_KEY';

// upon instantiation
$zaif = new \ccxt\zaif (array (
    'apiKey' => 'YOUR_ZAIF_API_KEY',
    'secret' => 'YOUR_ZAIF_SECRET_KEY'
));

```

Note that your private requests will fail with an exception or error if you don't set up your API credentials before you start trading. To avoid character escaping **always write your credentials in single quotes**, not double quotes (`'VERY_GOOD'`, `"VERY_BAD"`).

## Querying Account Balance

The returned balance structure is as follows:

```JavaScript
{
    'info':  { ... },    // the original untouched non-parsed reply with details

    //-------------------------------------------------------------------------
    // indexed by availability of funds first, then by currency

    'free':  {           // money, available for trading, by currency
        'BTC': 321.00,   // floats...
        'USD': 123.00,
        ...
    },

    'used':  { ... },    // money on hold, locked, frozen, or pending, by currency

    'total': { ... },    // total (free + used), by currency

    //-------------------------------------------------------------------------
    // indexed by currency first, then by availability of funds

    'BTC':   {           // string, three-letter currency code, uppercase
        'free': 321.00   // float, money available for trading
        'used': 234.00,  // float, money on hold, locked, frozen or pending
        'total': 555.00, // float, total balance (free + used)
    },

    'USD':   {           // ...
        'free': 123.00   // ...
        'used': 456.00,
        'total': 579.00,
    },

    ...
}
```

Some exchanges may not return full balance info. Many exchanges do not return balances for your empty or unused accounts. In that case some currencies may be missing in returned balance structure.

```JavaScript
// JavaScript
(async () => {
    console.log (await exchange.fetchBalance ())
}) ()
```

```Python
# Python
print (exchange.fetch_balance ())
```

```PHP
// PHP
var_dump ($exchange->fetch_balance ());
```

### Balance inference

Some exchanges do not return the full set of balance information from their API. Those will only return just the `free` or just the `total` funds, i.e. funds `used` on orders unknown. In such cases ccxt will try to obtain the missing data from [.orders cache](#orders-cache) and will guess complete balance info from what is known for sure. However, in rare cases the available info may not be enough to deduce the missing part, thus, **the user shoud be aware of the possibility of not getting complete balance info from less sophisticated exchanges**.

## Orders

```diff
- this part of the unified API is currenty a work in progress
- there may be some issues and missing implementations here and there
- contributions, pull requests and feedback appreciated
```

### Querying Orders

Most of the time you can query orders by an id or by a symbol, though not all exchanges offer a full and flexible set of endpoints for querying orders. Some exchanges might not have a method for fetching recently closed orders, the other can lack a method for getting an order by id, etc. The ccxt library will target those cases by making workarounds where possible.

The list of methods for querying orders consists of the following:

- `fetchOrder (id, symbol = undefined, params = {})`
- `fetchOrders (symbol = undefined, since = undefined, limit = undefined, params = {})`
- `fetchOpenOrders (symbol = undefined, since = undefined, limit = undefined, params = {})`
- `fetchClosedOrders (symbol = undefined, since = undefined, limit = undefined, params = {})`

Note that the naming of those methods indicates if the method returns a single order or multiple orders (an array/list of orders). The `fetchOrder()` method requires a mandatory order id argument (a string). Some exchanges also require a symbol to fetch an order by id, where order ids can intersect with various trading pairs. Also, note that all other methods above return an array (a list) of orders. Most of them will require a symbol argument as well, however, some exchanges allow querying with a symbol unspecified (meaning *all symbols*).

The library will throw a NotSupported exception if a user calls a method that is not available from the exchange or is not implemented in ccxt.

To check if any of the above methods are available, look into the `.has` property of the exchange:

```JavaScript
// JavaScript
'use strict';

const ccxt = require ('ccxt')
const id = 'poloniex'
exchange = new ccxt[id] ()
console.log (exchange.has)
```

```Python
# Python
import ccxt
id = 'cryptopia'
exchange = getattr(ccxt, 'id') ()
print(exchange.has)
```

```PHP
// PHP
$exchange = new \ccxt\liqui ();
print_r ($exchange->has); // or var_dump
```

A typical structure of the `.has` property usually contains the following flags corresponding to order API methods for querying orders:

```JavaScript
exchange.has = {

    // ... other flags ...

    'fetchOrder': true, // available from the exchange directly and implemented in ccxt
    'fetchOrders': false, // not available from the exchange or not implemented in ccxt
    'fetchOpenOrders': true,
    'fetchClosedOrders': 'emulated', // not available from the exchange, but emulated in ccxt

    // ... other flags ...

}
```

The meanings of boolean `true` and `false` are obvious. A string value of `emulated` means that particular method is missing in the exchange API and ccxt will workaround that where possible by adding a caching layer, the `.orders` cache. The next section describes the inner workings of the `.orders` cache, one has to understand it to do order management with ccxt effectively.

#### `.orders` cache

Some exchanges do not have a method for fetching closed orders or all orders. They will offer just the `fetchOpenOrders` endpoint, sometimes they are also generous to offer a `fetchOrder` endpoint as well. This means that they don't have any methods for fetching the order history. The ccxt library will try to emulate the order history for the user by keeping the cached .orders property containing all orders issued within a particular exchange class instance.

Whenever a user creates a new order or cancels an existing open order or does some other action that would alter the order status, the ccxt library will remember the entire order info in its cache. Upon a subsequent call to an emulated `fetchOrder`, `fetchOrders` or `fetchClosedOrders` method, the exchange instance will send a single request to **`fetchOpenOrders`** and will compare currently fetched open orders with the orders stored in cache previously. The ccxt library will check each cached order and will try to match it with a corresponding fetched open order. When the cached order isn't present in the open orders fetched from the exchange anymore, the library marks the cached order as `closed` (filled). The call to a `fetchOrder`, `fetchOrders`, `fetchClosedOrders` will then return the updated orders from `.orders` cache to the user.

The same logic can be put shortly: *if a cached order is not found in fetched open orders it isn't open anymore, therefore, closed*. This makes the library capable of tracking the order status and order history even with exchanges that don't have that functionality in their API natively. This is true for all methods that query orders or manipulate (place, cancel or edit) orders in any way.

In most cases the `.orders` cache will work transparently for the user. Most often the exchanges themselves have a sufficient set of methods. However, with some exchanges not having a complete API, the `.orders` cache has the following known limitations:

- If the user does not save the `.orders` cache between program runs and does not restore it upon launching a new run, the `.orders` cache will be lost, for obvious reasons. Therefore upon a call to fetchClosedOrders later on a different run, the exchange instance will return an empty list of orders. Without a properly restored cache a fresh new instance of the exchange won't be able to know anything about the orders that were closed and canceled (no history of orders).
- If the API keypair is shared across multiple exchange instances (e.g. when the user accesses the same exchange account in a multithreaded environment or in simultaneously launched separate scripts). Each particular instance would not be able to know anything about the orders created or canceled by other instances. This means that the order cache is not shared, and, in general, the same API keypair should not be shared across multiple instances accessing the private API. Otherwise it will cause side-effects with nonces and cached data falling out of sync.
- If the order was placed or canceled from outside of ccxt (on the exchange's website or by other means), the new order status won't arrive to the cache and ccxt won't be able to return it properly later.
- If an order's cancelation request bypasses ccxt then the library will not be able to find the order in the list of open orders returned from a subsequent call to `fetchOpenOrders()`. Thus the library will mark the cached order with a `'closed'` status.
- When `fetchOrder(id)` is emulated, the library will not be able to return a specific order, if it was not cached previously or if a change of the order' status was done bypassing ccxt. In that case the library will throw an `OrderNotFound` exception.
- If an unhandled error leads to a crash of the application and the `.orders` cache isn't saved and restored upon restart, the cache will be lost. Handling the exceptions properly is the responsibility of the user. One has to pay **extra care** when implementing proper [error handling](#error-handling), otherwise the `.orders` cache may fall out of sync.

*Note: the order cache functionality is to be reworked soon to obtain the order statuses from private trades history, where available. This is a work in progress, aimed at adding full-featured support for order fees, costs and other info. More about it here: https://github.com/ccxt/ccxt/issues/569*.

#### Purging Cached Orders

With some long-running instances it might be critical to free up used resources when they aren't needed anymore. Because in active trading the `.orders` cache can grow pretty big, the ccxt library offers the `purgeCachedOrders/purge_cached_orders` method for clearing old non-open orders from cache where `(order['timestamp'] < before) && (order['status'] != 'open')` and freeing used memory for other purposes. The purging method accepts one single argument named `before`:

```JavaScript
// JavaScript

// keep last 24 hours of history in cache
before = exchange.milliseconds () - 24 * 60 * 60 * 1000

// purge all closed and canceled orders "older" or issued "before" that time
exchange.purgeCachedOrders (before)
```

```Python
# Python

# keep last hour of history in cache
before = exchange.milliseconds () - 1 * 60 * 60 * 1000

# purge all closed and canceled orders "older" or issued "before" that time
exchange.purge_cached_orders (before)
```

```PHP
// PHP

// keep last 24 hours of history in cache
$before = $exchange->milliseconds () - 24 * 60 * 60 * 1000;

// purge all closed and canceled orders "older" or issued "before" that time
$exchange->purge_cached_orders ($before);

```

#### By Order Id

To get the details of a particular order by its id, use the fetchOrder / fetch_order method. Some exchanges also require a symbol even when fetching a particular order by id.

The signature of the fetchOrder/fetch_order method is as follows:

```JavaScript
if (exchange.has['fetchOrder']) {
    //  you can use the params argument for custom overrides
    let order = await exchange.fetchOrder (id, symbol = undefined, params = {})
}
```

**Some exchanges don't have an endpoint for fetching an order by id, ccxt will emulate it where possible.** For now it may still be missing here and there, as this is a work in progress.

You can pass custom overrided key-values in the additional params argument to supply a specific order type, or some other setting if needed.

Below are examples of using the fetchOrder method to get order info from an authenticated exchange instance:

```JavaScript
// JavaScript
(async function () {
    const order = await exchange.fetchOrder (id)
    console.log (order)
}) ()
```

```Python
# Python 2/3 (synchronous)
if exchange.has['fetchOrder']:
    order = exchange.fetch_order(id)
    print(order)

# Python 3.5+ asyncio (asynchronous)
import asyncio
import ccxt.async as ccxt
if exchange.has['fetchOrder']:
    order = asyncio.get_event_loop().run_until_complete(exchange.fetch_order(id))
    print(order)
```

```PHP
// PHP
if ($exchange->has['fetchOrder']) {
    $order = $exchange->fetch_order ($id);
    var_dump ($order);
}
```

#### All Orders

```JavaScript
if (exchange.has['fetchOrders'])
    exchange.fetchOrders (symbol = undefined, since = undefined, limit = undefined, params = {})
```

**Some exchanges don't have an endpoint for fetching all orders, ccxt will emulate it where possible.** For now it may still be missing here and there, as this is a work in progress.

#### Open Orders

```JavaScript
if (exchange.has['fetchOpenOrders'])
    exchange.fetchOpenOrders (symbol = undefined, since = undefined, limit = undefined, params = {})
```

#### Closed Orders

Do not confuse *closed orders* with *trades* aka *fills* ! An order can be closed (filled) with multiple opposing trades! So, a *closed order* is not the same as a *trade*. In general, the order does not have a `fee` at all, but each particular user trade does have `fee`, `cost` and other properties. However, many exchanges propagate those properties to the orders as well.

**Some exchanges don't have an endpoint for fetching closed orders, ccxt will emulate it where possible.** For now it may still be missing here and there, as this is a work in progress.

```JavaScript
if (exchange.has['fetchClosedOrders'])
    exchange.fetchClosedOrders (symbol = undefined, since = undefined, limit = undefined, params = {})
```

### Order Structure

Most of methods returning orders within ccxt unified API will usually yield an order structure as described below:

```JavaScript
{
    'id':        '12345-67890:09876/54321', // string
    'datetime':  '2017-08-17 12:42:48.000', // ISO8601 datetime with milliseconds
    'timestamp':  1502962946216, // order placing/opening Unix timestamp in milliseconds
    'status':    'open',         // 'open', 'closed', 'canceled'
    'symbol':    'ETH/BTC',      // symbol
    'type':      'limit',        // 'market', 'limit'
    'side':      'buy',          // 'buy', 'sell'
    'price':      0.06917684,    // float price in quote currency
    'amount':     1.5,           // ordered amount of base currency
    'filled':     1.1,           // filled amount of base currency
    'remaining':  0.4,           // remaining amount to fill
    'cost':       0.076094524,   // 'filled' * 'price'
    'trades':   [ ... ],         // a list of order trades/executions
    'fee':      {                // fee info, if available
        'currency': 'BTC',       // which currency the fee is (usually quote)
        'cost': 0.0009,          // the fee amount in that currency
    },
    'info':     { ... },         // the original unparsed order structure as is
}
```

### Placing Orders

To place an order you will need the following information:

- `symbol`, a string literal symbol of the market you wish to trade on, like `BTC/USD`, `ZEC/ETH`, `DOGE/DASH`, etc...
- `side`, a string literal for the direction of your order, `buy` or `sell`. When you place a buy order you give quote currency and receive base currency. For example, buying `BTC/USD` means that you will receive bitcoins for your dollars. When you are selling `BTC/USD` the outcome is the opposite and you receive dollars for your bitcoins.
- `type`, a string literal type of order, ccxt currently supports `market` and `limit` orders
- `amount`, how much of currency you want to trade. This usually refers to base currency of the trading pair symbol, though some exchanges require the amount in quote currency and a few of them require base or quote amount depending on the side of the order. See their API docs for details.
- `price`, how much quote currency you are willing to pay for a trade lot of base currency (for limit orders only)

A successful call to a unified method for placing market or limit orders returns the following structure:

```JavaScript
{
    'id': 'string',  // order id
    'info': { ... }, // decoded original JSON response from the exchange as is
}
```

**Some exchanges will allow to trade with limit orders only.** See [their docs](https://github.com/ccxt/ccxt/wiki/Manual#exchanges) for details.

#### Market Orders

Market price orders are also known as *spot price orders*, *instant orders* or simply *market orders*. A market order gets executed immediately. The matching engine of the exchange closes the order (fulfills it) with one or more transactions from the top of the order book stack.

The exchange will close your market order for the best price available. You are not guaranteed though, that the order will be executed for the price you observe prior to placing your order. There can be a slight change of the price for the traded market while your order is being executed, also known as *price slippage*. The price can slip because of networking roundtrip latency, high loads on the exchange, price volatility and other factors. When placing a market order you don't need to specify the price of the order.

Note, that some exchanges will not accept market orders (they allow limit orders only).

```
// camelCaseNotation
exchange.createMarketBuyOrder (symbol, amount[, params])
exchange.createMarketSellOrder (symbol, amount[, params])

// underscore_notation
exchange.create_market_buy_order (symbol, amount[, params])
exchange.create_market_sell_order (symbol, amount[, params])
```

#### Limit Orders

Limit price orders are also known as *limit orders*. Some exchanges accept limit orders only. Limit orders require a price (rate per unit) to be submitted with the order. The exchange will close limit orders if and only if market price reaches the desired level.

```
// camelCaseStyle
exchange.createLimitBuyOrder (symbol, amount, price[, params])
exchange.createLimitSellOrder (symbol, amount, price[, params])

// underscore_style
exchange.create_limit_buy_order (symbol, amount, price[, params])
exchange.create_limit_sell_order (symbol, amount, price[, params])
```

#### Custom Order Params

Some exchanges allow you to specify optional parameters for your order. You can pass your optional parameters and override your query with an associative array using the `params` argument to your unified API call.

```JavaScript
// JavaScript
// use a custom order type
bitfinex.createLimitSellOrder ('BTC/USD', 1, 10, { 'type': 'trailing-stop' })
```

```Python
# Python
# add a custom order flag
kraken.create_market_buy_order('BTC/USD', 1, {'trading_agreement': 'agree'})
```

```PHP
// PHP
// add custom user id to your order
$hitbtc->create_order ('BTC/USD', 'limit', 'buy', 1, 3000, array ('clientOrderId' => '123'));
```

### Canceling Orders

To cancel an existing order pass the order id to `cancelOrder (id, symbol, params) / cancel_order (id, symbol, params)` method. Note, that some exchanges require a second symbol parameter even to cancel a known order by id. The usage is shown in the following examples:

```JavaScript
// JavaScript
exchange.cancelOrder ('1234567890') // replace with your order id here (a string)
```

```Python
# Python
exchange.cancel_order ('1234567890') # replace with your order id here (a string)
```

```PHP
// PHP
$exchange->cancel_order ('1234567890'); // replace with your order id here (a string)
```

#### Exceptions on order canceling

The `cancelOrder()` is usually used on open orders only. However, it may happen that your order gets executed (filled and closed)
before your cancel-request comes in, so a cancel-request might hit an already-closed order.

A cancel-request might also throw a `NetworkError` indicating that the order might or might not have been canceled successfully and whether you need to retry or not. Consecutive calls to `cancelOrder()` may hit an already canceled order as well.

As such, `cancelOrder()` can throw an `OrderNotFound` exception in these cases:
- canceling an already-closed order
- canceling an already-canceled order

## Personal Trades

```
- this part of the unified API is currenty a work in progress
- there may be some issues and missing implementations here and there
- contributions, pull requests and feedback appreciated
```

### How Orders Are Related To Trades

A trade is also often called `a fill`. Each trade is a result of order execution. Note, that orders and trades have a one-to-many relationship: an execution of one order may result in several trades. However, when one order matches another opposing order, the pair of two matching orders yields one trade. Thus, when an order matches multiple opposing orders, this yields multiple trades, one trade per each pair of matched orders.

To put it shortly, an order can contain *one or more* trades. Or, in other words, an order can be *filled* with one or more trades.

For example, an orderbook can have the following orders (whatever trading symbol or pair it is):

```
    | price | amount
----|----------------
  a |  1.200 | 200
  s |  1.100 | 300
  k |  0.900 | 100
----|----------------
  b |  0.800 | 100
  i |  0.700 | 200
  d |  0.500 | 100
```

All specific numbers above aren't real, this is just to illustrate the way orders and trades are related in general.

A seller decides to place a sell limit order on the ask side for a price of 0.700 and an amount of 150.

```
    | price | amount
----|----------------  ↓
  a |  1.200 | 200     ↓
  s |  1.100 | 300     ↓
  k |  0.900 | 100     ↓
----|----------------  ↓
  b |  0.800 | 100     ↓ sell 150 for 0.700
  i |  0.700 | 200     --------------------
  d |  0.500 | 100
```

As the price and amount of the incoming sell (ask) order cover more than one bid order (orders `b` and `i`), the following sequence of events usually happens within an exchange engine very quickly, but not immediately:

1. Order `b` is matched against the incoming sell because their prices intersect. Their volumes *"mutually annihilate"* each other, so, the bidder gets 100 for a price of 0.800. The seller (asker) will have his sell order partially filled by bid volume of 100.

2. A trade is generated for the order `b` against the incoming sell order. That trade *"fills"* the entire order `b` and most of the sell order. One trade is generated pear each pair of matched orders, whether the amount was filled completely or partially. In this example the amount of 100 fills order `b` completely (closed the order `b`) and also fills the selling order partially (leaves it open in the orderbook).

3. Order `b` now has a status of `closed` and a filled volume of 100. It contains one trade against the selling order. The selling order has `open` status and a filled volume of 100. It contains one trade against order `b`. Thus each order has just one fill-trade so far.

3. The incoming sell order has a filled amount of 100 and has yet to fill the reamining amount of 50 from its initial amount of 150 in total.

4. Order `i` is matched against the remaining part of incoming sell, because their prices intersect. The amount of buying order `i` which is 200 completely annihilates the reamining sell amount of 50. The order `i` is filled partially by 50, but the rest of its volume, namely the remaining amount of 150 will stay in the orderbook. The selling order, however, is filled completely by this second match.

5. A trade is generated for the order `i` against the incoming sell order. That trade partially fills order `i`. And completes the filling of the sell order. Again, this is just one trade for a pair of matched orders.

6. Order `i` now has a status of `open`, a filled amount of 50, and a remaining amount of 150. It contains one filling trade against the selling order. The selling order has a `closed` status now, as it was completely filled its total initial amount of 150. However, it contains two trades, the first against order `b` and the second against order `i`. Thus each order can have one or more filling trades, depending on how their volumes were matched by the exchange engine.

After the above sequence takes place, the updated orderbook will look like this.

```
    | price | amount
----|----------------
  a |  1.200 | 200
  s |  1.100 | 300
  k |  0.900 | 100
----|----------------
  i |  0.700 | 150
  d |  0.500 | 100
```

Notice that the order `b` has disappeared, the selling order also isn't there. All closed and fully-filled orders disappear from the orderbook. The order `i` which was filled partially and still has a remaining volume and an `open` status, is still there.


### Recent Trades

```JavaScript
exchange.fetchMyTrades (symbol = undefined, since = undefined, limit = undefined, params = {})
```

Returns ordered array of trades (most recent trade first).

### Trades By Order Id

```UNDER CONSTRUCTION```


## Funding Your Account

```diff
- this part of the unified API is currenty a work in progress
- there may be some issues and missing implementations here and there
- contributions, pull requests and feedback appreciated
```

### Deposit

```
fetchDepositAddress (code, params = {})
createDepositAddress (code, params = {})
```

- `code` is the currency code (uppercase string)
- `params` contains optional extra overrides

```JavaScript
{
    'currency': currency, // currency code
    'address': address,   // address in terms of requested currency
    'tag': tag,           // tag / memo / paymentId for particular currencies (XRP, XMR, ...)
    'status': status,     // 'ok' or other
    'info': response,     // raw unparsed data as returned from the exchange
}
```

With certain currencies, like AEON, BTS, GXS, NXT, SBD, STEEM, STR, XEM, XLM, XMR, XRP, an additional argument `tag` is usually required by exchanges. The tag is a memo or a message or a payment id that is attached to a withdrawal transaction. The tag is mandatory for those currencies and it identifies the recipient user account.

### Withdraw

```
exchange.withdraw (currency, amount, address, tag = undefined, params = {})
```

The withdraw method returns a dictionary containing the withdrawal id, which is usually the txid of the onchain transaction itself, or an internal *withdrawal request id* registered within the exchange. The returned value looks as follows:

```JavaScript
{
    'info' { ... },      // unparsed reply from the exchange, as is
    'id': '12345567890', // string withdrawal id, if any
}
```

Some exchanges require a manual approval of each withdrawal by means of 2FA (2-factor authentication). In order to approve your withdrawal you usually have to either click their secret link in your email inbox or enter a Google Authenticator code or an Authy code on their website to verify that withdrawal transaction was requested intentionally.

In some cases you can also use the withdrawal id to check withdrawal status later (whether it succeeded or not) and to submit 2FA confirmation codes, where this is supported by the exchange. See [their docs](https://github.com/ccxt/ccxt/wiki/Manual#exchanges) for details.

### Ledger

```UNDER CONSTRUCTION```

## Overriding The Nonce

**The default nonce is a 32-bit Unix Timestamp in seconds. You should override it with a milliseconds-nonce if you want to make private requests more frequently than once per second! Most exchanges will throttle your requests if you hit their rate limits, read [API docs for your exchange](https://github.com/ccxt/ccxt/wiki/Exchanges) carefully!**

In case you need to reset the nonce it is much easier to create another pair of keys for using with private APIs. Creating new keys and setting up a fresh unused keypair in your config is usually enough for that.

In some cases you are unable to create new keys due to lack of permissions or whatever. If that happens you can still override the nonce. Base market class has the following methods for convenience:

- `seconds ()`: returns a Unix Timestamp in seconds.
- `milliseconds ()`: same in milliseconds (ms = 1000 * s, thousandths of a second).
- `microseconds ()`: same in microseconds (μs = 1000 * ms, millionths of a second).

There are exchanges that confuse milliseconds with microseconds in their API docs, let's all forgive them for that, folks. You can use methods listed above to override the nonce value. If you need to use the same keypair from multiple instances simultaneously use closures or a common function to avoid nonce conflicts. In Javascript you can override the nonce by providing a `nonce` parameter to the exchange constructor or by setting it explicitly on exchange object:

```JavaScript
// JavaScript

// A: custom nonce redefined in constructor parameters
let nonce = 1
let kraken1 = new ccxt.kraken ({ nonce: () => nonce++ })

// B: nonce redefined explicitly
let kraken2 = new ccxt.kraken ()
kraken2.nonce = function () { return nonce++ } // uses same nonce as kraken1

// C: milliseconds nonce
let kraken3 = new ccxt.kraken ({
    nonce: function () { return this.milliseconds () },
})

// D: newer ES syntax
let kraken4 = new ccxt.kraken ({
    nonce () { return this.milliseconds () },
})
```

In Python and PHP you can do the same by subclassing and overriding nonce function of a particular exchange class:

```Python
# Python

# A: the shortest
gdax = ccxt.gdax({'nonce': ccxt.Exchange.milliseconds})

# B: custom nonce
class MyKraken(ccxt.kraken):
    n = 1
    def nonce(self):
        return self.n += 1

# C: milliseconds nonce
class MyBitfinex(ccxt.bitfinex):
    def nonce(self):
        return self.milliseconds()

# D: milliseconds nonce inline
hitbtc = ccxt.hitbtc({
    'nonce': lambda: int(time.time() * 1000)
})

# E: milliseconds nonce
acx = ccxt.acx({'nonce': lambda: ccxt.Exchange.milliseconds()})
```

```PHP
// PHP

// A: custom nonce value
class MyOKCoinUSD extends \ccxt\okcoinusd {
    public function __construct ($options = array ()) {
        parent::__construct (array_merge (array ('i' => 1), $options));
    }
    public function nonce () {
        return $this->i++;
    }
}

// B: milliseconds nonce
class MyZaif extends \ccxt\zaif {
    public function __construct ($options = array ()) {
        parent::__construct (array_merge (array ('i' => 1), $options));
    }
    public function nonce () {
        return $this->milliseconds ();
    }
}
```

# Error Handling

All exceptions are derived from the base BaseError exception, which, in its turn, is defined in the ccxt library like so:

```JavaScript
// JavaScript
class BaseError extends Error {
    constructor () {
        super ()
        // a workaround to make `instanceof BaseError` work in ES5
        this.constructor = BaseError
        this.__proto__   = BaseError.prototype
    }
}
```

```Python
# Python
class BaseError (Exception):
    pass
```

```PHP
// PHP
class BaseError extends \Exception {}
```

Below is an outline of exception inheritance hierarchy:

```
+ BaseError
|
+---+ ExchangeError
|   |
|   +---+ NotSupported
|   |
|   +---+ AuthenticationError
|   |
|   +---+ InsufficientFunds
|   |
|   +---+ InvalidAddress
|   |
|   +---+ InvalidOrder
|       |
|       +---+ OrderNotFound
|
+---+ NetworkError (recoverable)
    |
    +---+ DDoSProtection
    |
    +---+ RequestTimeout
    |
    +---+ ExchangeNotAvailable
    |
    +---+ InvalidNonce
```

The `BaseError` class is a generic error class for all sorts of errors, including accessibility and request/response mismatch. Users should catch this exception at the very least, if no error differentiation is required.

## ExchangeError

This exception is thrown when an exchange server replies with an error in JSON. Possible reasons:

  - endpoint is switched off by the exchange
  - symbol not found on the exchange
  - required parameter is missing
  - the format of parameters is incorrect
  - an exchange replies with an unclear answer

Other exceptions derived from `ExchangeError`:

  - `NotSupported`: This exception is raised if the endpoint is not offered/not supported by the exchange API.
  - `AuthenticationError`: Raised when an exchange requires one of the API credentials that you've missed to specify, or when there's a mistake in the keypair or an outdated nonce. Most of the time you need `apiKey` and `secret`, sometimes you also need `uid` and/or `password`.
  - `InsufficientFunds`: This exception is raised when you don't have enough currency on your account balance to place an order.
  - `InvalidAddress`: This exception is raised upon encountering a bad funding address or a funding address shorter than `.minFundingAddressLength` (10 characters by default) in a call to `fetchDepositAddress`, `createDepositAddress` or `withdraw`.
  - `InvalidOrder`: This exception is the base class for all exceptions related to the unified order API.
  - `OrderNotFound`: Raised when you are trying to fetch or cancel a non-existent order.

## NetworkError

All errors related to networking are usually recoverable, meaning that networking problems, traffic congestion, unavailability is usually time-dependent. Making a retry later is usually enough to recover from a NetworkError, but if it doesn't go away, then it may indicate some persistent problem with the exchange or with your connection.

### DDoSProtection

This exception is thrown whenever Cloudflare or Incapsula rate limiter restrictions are enforced per user or region/location. The ccxt library does a case-insensitive search in the response received from the exchange for one of the following keywords:

  - `cloudflare`
  - `incapsula`
  - `overload`
  - `ddos`

### RequestTimeout

This exception is raised when the connection with the exchange fails or data is not fully received in a specified amount of time. This is controlled by the `timeout` option. When a `RequestTimeout` is raised, the user doesn't know the outcome of a request (whether it was accepted by the exchange server or not).

Thus it's advised to handle this type of exception in the following manner:

- for fetching requests it is safe to retry the call
- for a request to `cancelOrder` a user is required to retry the same call the second time. If instead of a retry a user calls a `fetchOrder`, `fetchOrders`, `fetchOpenOrders` or `fetchClosedOrders` right away without a retry to call `cancelOrder`, this may cause the [`.orders` cache](#orders-cache) to fall out of sync. A subsequent retry to `cancelOrder` will return one of the following possible results:
  - a request is completed successfully, meaning the order has been properly canceled now
  - an `OrderNotFound` exception is raised, which means the order was either already canceled on the first attempt or has been executed (filled and closed) in the meantime between the two attempts. Note, that the order will still have an `'open'` status in the `.orders` cache. To determine the actual order status you'll need to call `fetchOrder` to update the cache properly (where available from the exchange). If the `fetchOrder` method is `'emulated'` the ccxt library will mark the order as `'closed'`. The user has to call `fetchBalance` and set the order status to `'canceled'` manually if the balance hasn't changed (a trade didn't not occur).
- if a request to `createOrder` fails with a `RequestTimeout` the user should:
  - update the `.orders` cache with a call to `fetchOrders`, `fetchOpenOrders`, `fetchClosedOrders` to check if the request to place the order has succeeded and the order is now open
  - if the order is not `'open'` the user should `fetchBalance` to check if the balance has changed since the order was created on the first run and then was filled and closed by the time of the second check. Note that `fetchBalance` relies on the `.orders` cache for [balance inference](#balance-inference) and thus should only be called after updating the cache!

### ExchangeNotAvailable

The ccxt library throws this error if it detects any of the following keywords in response:

  - `offline`
  - `unavailable`
  - `busy`
  - `retry`
  - `wait`
  - `maintain`
  - `maintenance`
  - `maintenancing`

### InvalidNonce

Raised when your nonce is less than the previous nonce used with your keypair, as described in the [Authentication](https://github.com/ccxt/ccxt/wiki/Manual#authentication) section. This type of exception is thrown in these cases (in order of precedence for checking):

  - You are not rate-limiting your requests or sending too many of them too often.
  - Your API keys are not fresh and new (have been used with some different software or script already).
  - The same keypair is shared across multiple instances of the exchange class (for example, in a multithreaded environment or in separate processes).
  - Your system clock is out of synch. System time should be synched with UTC in a non-DST timezone at a rate of once every ten minutes or even more frequently because of the clock drifting. **Enabling time synch in Windows is usually not enough!** You have to set it up with the OS Registry (Google *"time synch frequency"* for your OS).

# Troubleshooting

In case you experience any difficulty connecting to a particular exchange, do the following in order of precedence:

- Make sure that you have the most recent version of ccxt.
- Check the [CHANGELOG](https://github.com/ccxt/ccxt/blob/master/CHANGELOG.md) for recent updates.
- Turn `verbose = true` to get more detail about it.
- Python people can turn on DEBUG logging level with a standard pythonic logger, by adding these two lines to the beginning of their code:
  ```Python
  import logging
  logging.basicConfig(level=logging.DEBUG)
  ```
- Check your API credentials. Try a fresh new keypair if possible.
- If it is a Cloudflare protection error, try these examples:
  - https://github.com/ccxt/ccxt/blob/master/examples/js/bypass-cloudflare.js
  - https://github.com/ccxt/ccxt/blob/master/examples/py/bypass-cloudflare.py
- Check your nonce. If you used your API keys with other software, you most likely should [override your nonce function](#overriding-the-nonce) to match your previous nonce value. A nonce usually can be easily reset by generating a new unused keypair. If you are getting nonce errors with an existing key, try with a new API key that hasn't been used yet.
- Check your request rate if you are getting nonce errors. Your private requests should not follow one another quickly. You should not send them one after another in a split second or in short time. The exchange will most likely ban you if you don't make a delay before sending each new request. In other words, you should not hit their rate limit by sending unlimited private requests too frequently. Add a delay to your subsequent requests, like show in the long-poller [examples](https://github.com/ccxt/ccxt/tree/master/examples), also [here](https://github.com/ccxt/ccxt/wiki/Manual#order-book--market-depth).
- Read the [docs for your exchange](https://github.com/ccxt/ccxt/wiki/Exchanges) and compare your verbose output to the docs.
- Check your connectivity with the exchange by accessing it with your browser.
- Check your connection with the exchange through a proxy. Read the [Proxy](https://github.com/ccxt/ccxt/wiki/Install#proxy) section for more details.
- Try accesing the exchange from a different computer or a remote server, to see if this is a local or global issue with the exchange.
- Check if there were any news from the exchange recently regarding downtime for maintenance. Some exchanges go offline for updates regularly (like once a week).

## Notes

- Use the `verbose = true` option or instantiate your troublesome exchange with `new ccxt.exchange ({ 'verbose': true })` to see the HTTP requests and responses in details. The verbose output will also be of use for us to debug it if you submit an issue on GitHub.
- Use DEBUG logging in Python!
- As written above, some exchanges are not available in certain countries. You should use a proxy or get a server somewhere closer to the exchange.
- If you are getting authentication errors or *'invalid keys'* errors, those are most likely due to a nonce issue.
- Some exchanges do not state it clearly if they fail to authenticate your request. In those circumstances they might respond with an exotic error code, like HTTP 502 Bad Gateway Error or something that's even less related to the actual cause of the error.
- ...

```UNDER CONSTRUCTION```
