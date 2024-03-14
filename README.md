# ⛓💰 bdex: an amm-toolkit

<br>

<p align="center">
<img src="https://github.com/go-outside-labs/bdex-amm-arb-toolkit-py/assets/138340846/4f91482e-b721-4d19-9086-a726df19ab3c" width="50%" align="center" style="padding:1px;border:1px solid black;"/>
 </p>




<br>

### tl; dr

#### 💰 my package and CLI tool to extract data and arbitrage for specified tokens/exchange pools.

#### 📚 for more details, check **[my mirror post about this tool](https://mirror.xyz/go-outside.eth/EwRYWHcjsic4lPbFLRcPhakxKyfGuOKS_TL4YgsoaOk)**.


<br>

<br>

<p align="center">
<img width="400" src="https://github.com/go-outside-labs/bdex-amm-arb-toolkit-py/assets/138340846/6de77a51-8b99-4f33-a6d2-5cb6b5903c88">
 </p>


<br>

---

<br>

### available token pairs:

* **WETH/DAI**

<br>

### available exchanges:

* **Uniswap** ([0xa478c2975ab1ea89e8196811f51a7b7ade33eb11](https://etherscan.io/address/0xa478c2975ab1ea89e8196811f51a7b7ade33eb11))
* **Sushiswap** ([0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f](https://etherscan.io/address/0xc3d03e4f041fd4cd388c549ee2a29a9e5075882f))
* **Shebaswap** ([0x8faf958e36c6970497386118030e6297fff8d275](https://etherscan.io/address/0x8faf958e36c6970497386118030e6297fff8d275))
* **Sakeswap** ([0x2ad95483ac838e2884563ad278e933fba96bc242](https://etherscan.io/address/0x2ad95483ac838e2884563ad278e933fba96bc242))
* **Croswap** ([0x60a26d69263ef43e9a68964ba141263f19d71d51)](https://etherscan.io/address/0x60a26d69263ef43e9a68964ba141263f19d71d51)

<br>

---

<br>

## installation

Add your [Alchemy API key and endpoint](https://dashboard.alchemyapi.io/apps) to a file named `.env`:

```bash
cp .env_example .env
vim .env
```

Create a virtual environment:

```bash
virtualenv venv
source venv/bin/activate
```

Install dependencies:

```bash
make install_deps
```

Install the CLI:

```bash
make install
```

<br>

---

<br>

## running

You can run the CLI with:

```
bdex
```

<br>

<img width="807" alt="Screen Shot 2022-04-04 at 1 35 46 AM" src="https://user-images.githubusercontent.com/1130416/161449784-3342293b-e75f-4788-aecb-9a653e363917.png">



<br>

### checking the latest block


We leverage [Alchemy's API endpoint `eth_blockNumber_hex`](https://docs.alchemy.com/alchemy/apis/ethereum/eth_blockNumber_hex) to get the latest block:

```bash
bdex -c
```

<br>

<img width="406" alt="Screen Shot 2022-03-31 at 1 15 25 PM" src="https://user-images.githubusercontent.com/1130416/161032451-685dee8b-8ed3-40c2-9391-191fa2abce35.png">

<br>

<br>

💡 The block number can be checked against [ETHstat](https://ethstats.net/).


<br>

<img width="293" alt="Screen Shot 2022-03-31 at 1 15 19 PM" src="https://user-images.githubusercontent.com/1130416/161032358-86969275-7a72-406d-93bc-73906303a0cb.png">


<br>

💡 We are crafting the checksum address string by hand without directly [Keccak-256 hashing the methods and parameters](https://docs.soliditylang.org/en/develop/abi-spec.html).

<br>

### getting the token balance for an exchange

We leverage [Alchemy API endpoint `eth_call`](https://docs.alchemy.com/alchemy/apis/ethereum/eth_call) to retrieve the current token balance for a specific exchange:

```bash
bdex -b TOKEN EXCHANGE
```
<br>

<img width="481" alt="Screen Shot 2022-03-31 at 10 26 21 PM" src="https://user-images.githubusercontent.com/1130416/161125262-4a623e23-adc9-4928-98c5-16dd67d3302b.png">



<br>


### getting all token balances for all the exchanges

 We loop over the previous method for a list of tokens and exchanges:

```bash
bdex -a
```

<br>

<img width="407" alt="Screen Shot 2022-03-31 at 10 26 51 PM" src="https://user-images.githubusercontent.com/1130416/161125283-9a320c9b-a89b-4efa-832e-e0c315e2adf6.png">




<br>


### [extra] getting all token balances for all exchanges with Python's web3 library

To be able to compare our results from the previous steps, we implemented an alternative way to fetch pair balances utilizing the [Python web3 library](https://web3py.readthedocs.io/en/stable/):

```bash
bdex -w
```
<br>

<img width="401" alt="Screen Shot 2022-03-31 at 10 27 25 PM" src="https://user-images.githubusercontent.com/1130416/161125363-ced644b9-8011-4f9f-b470-324e5f7e4079.png">


<br>

<br>

💡 For this library, it's necessary to supply the contracts' ABI (in our case, for [DAI](https://api.etherscan.io/api?module=contract&action=getabi&address=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2) and [WETH](https://api.etherscan.io/api?module=contract&action=getabi&address=0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2), located at `./docs`)


💡 A third option to verify token balances is through [Etherscan tokenholdings dashboard](https://etherscan.io/tokenholdings?a=0xa478c2975ab1ea89e8196811f51a7b7ade33eb11).


<br>


### getting trading prices for all the exchanges

To get the current price for `WETH/DAI` in all exchanges (e.g., as shown in [the projects' dashboards](https://v2.info.uniswap.org/pair/0xa478c2975ab1ea89e8196811f51a7b7ade33eb11)), run:


```bash
bdex -p QUANTITY TOKEN1 TOKEN2
```

<br>

Quote for trading 1 `WETH`:

<br>

<img width="533" alt="Screen Shot 2022-04-03 at 8 27 55 PM" src="https://user-images.githubusercontent.com/1130416/161438209-6386b71d-94c8-45aa-a60a-ac7a13b2de26.png">

<br>

Quote for trading 10 `WETH`:

<br>

<img width="544" alt="Screen Shot 2022-04-03 at 8 28 20 PM" src="https://user-images.githubusercontent.com/1130416/161438257-5397cd0d-535c-49b8-8db2-3499e04b22bb.png">

<br>

Quote for trading 100 `WETH`:

<br>


<img width="542" alt="Screen Shot 2022-04-03 at 8 28 46 PM" src="https://user-images.githubusercontent.com/1130416/161438283-4e65810f-d297-4331-9ce2-aa9ef23b3c87.png">


<br>

#### how the price is calculated

An AMM replaces the buy and sell orders in an order book market with a liquidity pool of two assets, both valued relative to each other. As one asset is traded for the other, the relative prices of the two assets shift, and the new market rate for both is determined.

The constant product is:

```
token_a_pool_size * token_b_pool_size = constant_product
```

All the exchanges are forks from [UniswapV2](https://uniswap.org/blog/uniswap-v2), so they all use the same price formula for trading:

```
market_price_token1 = token2_balance / token1_balance
```

For example, in a pool with `2,000,000 DAI` and `1,000 WETH`, the constant product is `2,000,000,000` and the market price for `WETH` is `$2,000`.

<br>

#### Buy price (e.g., buying `WETH` in a `WETH/DAI` pool)


To find the buy price for a certain quantity, first, we calculate how much `WETH` needs to remain in balance to keep the constant product unchanged:

```
token1_balance_buy = constant_product / (token2_balance + quantity)
```

Then we calculate how much `WETH` goes out to keep this constant:

```
 t1_amount_out_buy = token1_balance - token1_balance_buy
```

The buy price to reflect this ratio is:

```
buy_price = quantity / t1_amount_out_buy
```

<br>

#### sell price (e.g., selling `WETH` in a `WETH/DAI` pool)

To find how much we can sell a certain quantity of `WETH` for `DAI`, first, we calculate the ratio of `DAI` in the new pool, as we add `WETH`:

```
token2_balance_buy = constant_product / (token1_balance + quantity)
```

We then calculate how much `DAI` will go out:

```
t2_amount_out_buy = token2_balance + token2_balance_buy
```

We calculate the `DAI` balance reflected with the income `WETH`:

```
token1_balance_sell = constant_product / (token2_balance - quantity)
```

And what's the proportion of `WETH` in the new balance:

```
t1_amount_in_sell = token1_balance + token1_balance_sell
```

We can now calculate the sell price to reflect the balance change, keeping the constant:

```
sell_price = t2_amount_out_buy / t1_amount_in_sell
```


<br>

---

### getting arbitrages

Run an algorithm to search for arbitrage in the supported exchanges for a certain buy quantity:

```bash
bdex -x QUANTITY
```


<br>

Arbitrage opportunities for 10 `WETH`:

<br>
<img width="495" alt="Screen Shot 2022-04-04 at 6 02 09 PM" src="https://user-images.githubusercontent.com/1130416/161561281-c49beaec-c0fe-4022-b323-a795b0ebb92a.png">


<br>

Arbitrage opportunities for 1 `WETH`:

<br>

<img width="464" alt="Screen Shot 2022-04-04 at 6 02 31 PM" src="https://user-images.githubusercontent.com/1130416/161561271-94c2c75f-fc27-48bc-a29f-a8b60a692585.png">

<br>


Arbitrage opportunities for 0.01 `WETH`:

<br>
<img width="507" alt="Screen Shot 2022-04-04 at 6 03 20 PM" src="https://user-images.githubusercontent.com/1130416/161561257-aab365b9-d04b-4754-948b-4f2894aeb03e.png">



<br>


<br>


<br>

### running arbitrage algorithm in a loop

To run the arbitrage algorithm for a certain amount of minutes:

```bash
bdex -r MIN
```

<br>

<img width="499" alt="Screen Shot 2022-04-04 at 1 05 07 PM" src="https://user-images.githubusercontent.com/1130416/161511273-cebe71dd-863a-4a5d-a13b-5e85f9b99bef.png">



<br>

<br>

Results are saved into `results/<arbitrage_TIMESTAMP>.txt`.

<br>

Here is a sample of the results running this algorithm for 100 minutes for trading 1 `WETH`:

```
{'buy_exchange': 'SUSHISWAP', 'sell_exchange': 'UNISWAP', 'arbitrage': '7.01', 'buy_price': 3475.14, 'sell_price': 3482.15}
{'buy_exchange': 'SUSHISWAP', 'sell_exchange': 'SHEBASWAP', 'arbitrage': '4.27', 'buy_price': 3475.14, 'sell_price': 3479.41}
{'buy_exchange': 'SHEBASWAP', 'sell_exchange': 'UNISWAP', 'arbitrage': '2.06', 'buy_price': 3480.09, 'sell_price': 3482.15}
{'buy_exchange': 'CROSWAP', 'sell_exchange': 'UNISWAP', 'arbitrage': '13.06', 'buy_price': 3469.09, 'sell_price': 3482.15}
{'buy_exchange': 'CROSWAP', 'sell_exchange': 'SUSHISWAP', 'arbitrage': '5.79', 'buy_price': 3469.09, 'sell_price': 3474.88}
{'buy_exchange': 'CROSWAP', 'sell_exchange': 'SHEBASWAP', 'arbitrage': '10.32', 'buy_price': 3469.09, 'sell_price': 3479.41}
...
```

<br>

### running arbitrage algorithm in a loop in a Docker container

To run the algorithm in a separated container, first [install Docker](https://docs.docker.com/get-docker/), then build the Docker image:

```bash
docker build -t bdex .
```

Finally, run the container (in a separate terminal tab):

```
docker run -v $(pwd):/results -it bdex sleep infinity
```

Results are available at `results/<arbitrage_TIMESTAMP>.txt`.

💡 You can inspect your container at any time with these commands:

```
docker ps
docker exec -it <container_id> /bin/bash
docker inspect bdex
```

Cleaning up:

```
docker volumes prune
```

<br>


---

<br>

## development


Install dependencies (in a virtual env):

```
make install
```

Useful commands:

```
make lint
make test
```


<br>

