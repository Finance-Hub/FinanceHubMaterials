# Basic definitions and notation

#### Author: Gustavo Soares

We will try to follow closely the notation in [Cochrane's Asset Pricing book](https://press.princeton.edu/titles/7836.html) with a few differences. Cochrane's focus is on cash equities (or cash instruments) while we will try to acommodate other asset classes and handle unfunded exposures such as futures, forwards and swaps within the same notation.

## Prices

#### Cash assets

**Cash assets** are contractual claims of ownership to goods or capital used as a factor of production in the economy. At any point in time, $t$, that contractal claim of ownership, like a public company stock, or contractual rights to interest income from a bond has a price which we will denote by $P_{t}$.

Cash assets prices reflect the equilibrium supply and demand for their underlying goods or capital as well as their risks. Risks because the value of an asset, $X_s$, for any future date $s>t$, is unknown at time $t$. That is, $X_s$ is a random variable with respect to the set of all information available up to time $t$, $\mathscr{I}_{t}$. For the technically oriented reader $\mathscr{I}_{t}$ is a [$\sigma$-algebra](https://en.wikipedia.org/wiki/%CE%A3-algebra) contained possible past outcomes for prices and other historical data up (and including) to time $t$. 

Unless otherwise stated we will always assume cash asset prices are expressed in USD per their standard quote unit like USD per share, USD per units of another currency (e.g., EUR or JPY), USD per barrels of oil or bushels of corn, etc. Typically, though not always, cash asset prices will be non-negative.

#### Delta-1 derivatives

In quantitative finance it is common to create investment strategies based on derivatives instead of cash assets. A derivative is a financial contractual claim on an underlying cash asset or group of cash assets. The derivative itself is a contract between two or more parties, and the derivative derives its price from fluctuations in the underlying cash asset.

The main advantage of using delta-1 derivatives instead of cash assets in quantitative finance is that entering into a futures, forwards, or swap contract, typically, does not require any disposabal cash. Delta-1 derivatives function more like a "bet" between two or more parties in which cash flow only exchange hands after some event has realized. We will discuss the pricing of delta-1 derivatives later, for now, it sufficies for us to know that when the underlying asset price changes from $P_{t}$ to $P_{s}$ a delta-1 derivative will pay/request (approximately) $P_{s} - P_{t}$ to/from the party that was long/short the asset.

Hence, if a quantitative model suggests the price of Apple stock will rise, we would need to have USD $P_{t}Q_{t}$ at our disposal to buy $Q_{t}$ units of Apple stock. Alternatively, we could enter into a **long** position in a swap contract on Apple stock with notional equal to $P_{t}Q_{t}$ and that swap would give us (approximately) $(P_{s} - P_{t})Q_{t}$ if Apple stock price moves from $P_{t}$ to $P_{s}$. Note that $P_{s} - P_{t}$ may turn out to be negative if Apple stock price falls instead of rising from date $t$ to $s$. In that case, we would have to pay $(P_{s} - P_{t})Q_{t}$ to our counterparty in the transaction.

Because of this major advantage of being **unfunded**, i.e., not requiring disposable cash at the start of the transaction, we will focus our the discussion of quantitative models on the returns of delta-1 derivatives.

## Returns

In quantitative finance, we often talk about prices but, **returns** are really the entire focus of the theory and practice of finance. The whole point of investing is not to be able to price things well but to take advantage of how asset prices move over time.

#### Total returns

In our notation, a cash asset has price $P_{t}$ at time $t$ and it will pay the owner of the asset an yet uncertain amount of $X_{t+h}$ after $h$ time periods. At time $t$, $X_{t+h}$ is a random variable that will realize a value at time $t+h$. The **total return** of this cash asset is also a random variable defined as:

$$
R_{t+h} \equiv \frac{X_{t+h}}{P_{t}}-1
$$

Let's now look at some specific examples to make things more concrete:

###### Stocks

Stocks (or single-name equities) are claims of ownership in a fraction of a company. Their prices at time $t$, $P_{t}$, are quoted in USD per shares. They entitle the owner of the stock to a proportion of the corporation's assets and profits equal to how much stock they own and the supply and demand for those assets and future profits will determine its price at time $t$. Most companies pay a sum of money regularly (e.g., quarterly) to its shareholders in the form of **dividends**. So, at any point in time $t$, the value of a stock is equal to its price, $P_{t}$, plus this a sum of money, $D_{t}$, paid in the form of dividends.

After $h$ time periods, neither the stocks future price $P_{t+h}$ nor how much the company will have paid in dividends $D_{t+h}$ is known with certainty. Hence, the **total return** of this stock between $t$ and $t+h$ is a random variable defined as:

$$
R_{t+h} \equiv \frac{P_{t+h} + D_{t+h}}{P_{t}}-1
$$

It is straightout wrong to calculate the return of a cash equity simply using prices and disregarding dividends. You should never build a quantitative model based on price percentage changes alone.


###### Stock indices

A stock index is an index that measures the value of a **portfolio** of stocks. The index level is computed from the price variation of composing stocks. Typically, though no always, it will be a weighted arithmetic mean of the quantities times the price variation of composing stocks.

Let's say that at time $t$, the stock index is composed of $N$ stocks, $q_{t} \equiv (Q_{1,t},\dots,Q_{N,t})$ where each element of the $N\times1$ vector $q_{t}$, $Q_{i,t}$, represents the number of shares of stock $i$ which one unit of the index entails. The vector of quantities, $q_{t}$, is defined by the index sponsor (e.g. S&P) and tends to be constant over time. From time to time, (e.g. quarterly) the index sponsor will announce a new vector of quantities and this chance index composition is called **index rebalancing**.

After setting an initial arbitrary start value for the stock index, $I_{0}$, we can define the **stock index** recursively as:

$$
I_{t+1} \equiv I_{t} + (p_{t+1} - p_{t})q_{t}
$$ 

where $p_{t} \equiv (P_{1,t},\dots,P_{N,t})$ is the $N\times1$ vector of prices where each of its elements, $P_{i,t}$, represents the price of stock $i$.

Different stock indices treat the payment of dividends in their underlying stocks differently. Since its composing stocks will periodically pay dividends to shareholders, the payment of dividends should be part of the definition of total return of a stock index. Some indices, like the S&P 500, do not use dividends in their calculations. Those types of indices are called **price indices** because they dividends are not included in their return calculations. Some other indices, like the IBOVESPA, use dividends in their calculations. Those types of indices are called **total return indices**.

When dealing with total return indices, like the IBOVESPA, we can define the **total return** of the stock index between $t$ and $t+h$ simply as the random variable: $R_{t+h} \equiv I_{t+h}/I_{t}$. However, when dealing with price indices, like the S&P 500, we typically assume dividends are immediately re-invested into the same composing stocks.

After setting an initial arbitrary start value for the quantities vector, $q_{0}$, and the index level, $TR_{0}$, we can define a **total return index** using the following steps:

1. Let $d_{t+1} \equiv (D_{1,t+1},\dots,D_{N,t+1})$ be the vector of dividends where each element of the $N\times1$ vector $d_{t+1}$ is the amount paid in dividends to shareholders of stock $i$ between $t$ and $t+1$;
2. The total amount collected in dividends for each stock $i$, $D_{i,t+1}Q_{i,t}$, is used to purchase more shares of the company at price $P_{i,t+1}$. So, $D_{i,t+1}Q_{i,t}/P_{i,t+1}$ are purchased and therefore $Q_{i,t+1} \equiv Q_{i,t} + D_{i,t+1}Q_{i,t}/P_{i,t+1}$, defining a new vector of quantities $q_{t+1}$;
3. The new level of the total return index is then defined as $TR_{t+1} \equiv TR_{t} + (p_{t+1} - p_{t})q_{t}$.

Note that in both cases, in the price and total return index case, one unit of the index is worth $p_{t}q_{t}$ at time $t$ and $p_{t+1}q_{t+1}$. However, in the case of the price index, if prices do not change $p_{t+1}=p_{t}$ the unit of the index will maintain its value regardless if the owner of the index unit is entitle to dividends paid betweem $t$ and $t+1$. This does not make sense. In the case of the total return index, if prices do not change $p_{t+1}=p_{t}$ the unit of the index will now be its previous value, $p_{t}q_{t}$, plus all the dividends paid by the index underlying stocks, $d_{t+1}q_{t}$. Hence, the correct way of calculating the the total return of a price index like the S&P between $t$ and $t+h$ is not given by $R_{t+h} \equiv I_{t+h}/I_{t}$ but rather by $TR_{t+h} \equiv TR_{t+h}/TR_{t}$, i.e., it is calculating the returns on the [S&P 500 TR Index](https://finance.yahoo.com/quote/%5ESP500TR/) instead of the actual [S&P 500 Index](https://www.marketwatch.com/investing/index/spx).

Creating and calculating stock indices is a complicated business. New companies get listed, some are included into the index, some are excluded, some companies merger with others, some go private, there are issues like stock splits, spin-offs, rights-issues, etc. For a more comprehensive view on stock indices take a look at the materials from some of the major stock index sponsors like [S&P](https://www.spglobal.com/spdji/en/documents/methodologies/methodology-sp-us-indices.pdf?force_download=true) and [MSCI](https://www.msci.com/index-methodology).

###### Declaration Date, Ex-Dividend Date, Record Date and Pay Date

The **declaration date** is the date on which the company announces the next dividend payment to shareholders. It is simply an announcement no dividends are paid on the declaration date but it is the point in time $s<t$ when $D_{i,t}$ stops being a random variable and it becomes a known pre-determined quantity.

The **record date** is the date on which a company's management looks at the shareholder records to see who is eligible to receive the company’s future dividend payment $D_{i,t}$ announced previously on the declaration date. This date is of little importance to investors however since buying the stock on the record date does not entitle you to receive the dividend. What really matter is to investors is the **ex-dividend date**, which is typically, two two days before the record date.

When an investor purchases shares, typically, it takes three days for the transaction to settle. Hence, in order to have your name on the registry for receiving the company’s future dividend payment $D_{i,t}$, you need to buy the stock at least two days prior to the record date, on the **ex-dividend date**. Buying a stock after the ex-dividend date will not entitle you to the quarter’s dividend (although you will be entitled to future dividends, assuming you  still hold the shares). Investors who purchase shares before the ex-dividend date will be paid that quarter’s dividend $D_{i,t}$.

The **payment date** is the date on which cash is actually paid to shareholders as a dividend $D_{i,t}$.

This terminoly is important to keep in mind when calculating total returns. On any date $t$ prior to the **declaration date** both $D_{i,s}$ and $P_{i,s}$, where $s$ is the **payment date**, are not known with certainty. After the **declaration date**, $D_{i,s}$ is known with certainty and we would expect the price of the stock to fall from $P_{i,t}$ to $P_{i,t}-D_{i,s}$ exactly when $t$ is the **ex-dividend date**. Hence, we should not wait for the **payment date** to prices for dividends. We should add divdidends to prices at time $t$, on the **ex-dividend date**, not at time $s$, the **payment date**. The same applies to the correction of the quantity vector $q_{t}$ in stock indices. It need to happen on the **ex-dividend date**, not at time $s$, the **payment date**.

##### Bonds and bond indices

Total return calculations for bonds and bond indices work exactly the same way as for stocks and stock indices if we think about coupon payments in the same way we think about dividend payments. Analogously, the coupon is paid to the bondholder of record. The **ex-coupon date** is the date by which the trade must occur if the buyer is to receive the upcoming coupon. 

When a bond is transacted between the last coupon payment and the next coupon payment, the buyer will receive the next coupon payment. The amount of interest over this period that will be credited to the buyer is called the accrued interest. Since the seller held the bond for some time between the last coupon payment and the day the trasaction settles, the buyer must pay the bond seller the portion of the interest that the seller earned before selling the bond. The **dirty price** of a bond is its actual price, the **clean price**, plus this accrued interest to which the seller is entitled. The dirty price is the full amount of cash the buyer has to pay the seller for purchasing the bond.


###### Currencies

The Foreing Exchange (Forex or FX) is the largest, most liquid market in the world, with trillions of dollars changing hands every day. It is open 24 hours a day, five days a week, except for holidays. Currencies may still trade on a holiday if at least the country/global market is open for business.

In the cash FX market, at time $t$, market participants buy one unit of currency X by paying $S^{XY}_{t}$ units of the currency Y. This is the so-called XY spot exchange rate. The XY spot exchange rate is the price of currency X in units of currency Y. For example, if I need USD 1.13 to buy 1 EUR, then the EURUSD spot exchange rate is 1.13.

To make things more intereting when trading currencies, not all currencies have the USD as their base currencies. For example, the EUR, the GBP, and the AUD are typically quoted as EURUSD, GBPUSD, and AUDUSD. However, some currencies are inverse quoted. For example, the CAD, the JPY, and the BRL are typically quoted as USDCAD, USDJPY, and USDBRL, i.e., as units of CAD, JPY, and BRL needed to buy one dollar.

When building quantitative models to trade FX, you need to be vary careful in making sure returns are calculated in a single base currency, like the USD, and the data has been processed accordingly. Here, we will assume that all currency data has been processed to have the USD as base currency. So, we will only consider exchange XUSD exchange rates. Since the XUSD spot exchange rate is the number of USD units needed to buy one unit of currency X, we will simply call it the price of currency X, $P_{X,t}$.

Currencies do not pay dividend but you earn interest on them. So, if at any point in time $t$, we purchase currency X in the spot FX market by $P_{X,t}$ and, after $h$ time periods, we sell it for $P_{X,t+h}$, we need to consider interest earned while holding. In FX trading, we typically assume that the position in the currency will generate interest according to the so-called currency **deposit rates**.

The deposit interest rate is paid by financial institutions to certificates of deposit (CD), savings accounts, and other types of accounts. Typically, we will use a "depo rate" which refers to interest paid on the interbank market for that particular currency. The depo rate is a floating rate and it is specific to every currency. Also, each depo rate for each currency has a different. For example, for the USD, deposit rates are typically accrued on an actual/360 day-count convention linearly. In contract, deposit rates on the BRL, for example, accrued on an bus/252 day-count convention exponentially. Regardless, of how the interest will be accrued, we will have to add the amount of earned in interest while holding currency X.

Well, if we purchased one unit of currency X by the price $P_{X,t}$ at time $t$, then this unit of currency X will earn interest from $t$ to $t+h$, in currency X. Since not all deposit rates have the same accrual rules, we will assume that we can always convert the deposit rate to a bus/252 day-count exponential convention. Hence, if periods are measured in business days, at time $t+h$, we will have $(1+r_{X,h,t})^{h/252}$ units of currency X where $r_{X,h,t}$ is the bus/252 day-count exponential rate corresponding to the $h$ period deposit rate in currency X.

Hence, the **total return** of this currency trade between $t$ and $t+h$ is a random variable defined as:

$$
R_{t+h} \equiv \frac{P_{t+h}(1+r_{X,h,t})^{h/252}}{P_{t}}-1 = (1+s_{X,h,t})(1+r_{X,h,t})^{h/252}-1
$$

where $s_{X,h,t}\equiv P_{t+h}/P_{t}-1$ is the rate of appreciation of currency X relative to the USD in the period between $t$ and $t+h$. Obviously, if periods are measured in months, our annualization factor would change from $h/252$ to $h/12$, if periods are measured in quarters, it chanve would from $h/252$ to $h/4$. and if periods are measured in years, it would change from $h/252$ to $h$.

#### Excess returns and the risk-free rate

As we discussed, cash assets are **funded** instruments, i.e., they require disposable cash at the start of the transaction. Hence, the **total return** of an investment has to be at least as large as the interest we would be paid on that cash for the investment over the period to be worthwhile. Hence, the **total return** of an investment is not really relevant for quantitative models what is really relevant are returns achieved above and beyond the return of the disposable cash about to be deployed in the transaction, i.e., the investments' **excess return**.

Technically, the **excess return** of an investment is the difference between the investment total returns and a closely comparable benchmark with similar risk and return characteristics. However, here, we will always compare it with the USD **risk-free rate** over the investment period, which we will typically assume to be the interest generated by USD deposit rates. Again, we will assume that we can always convert the USD deposit rate to a bus/252 day-count exponential convention to **risk-free rate** prevailing from time $t$ to time $t+h$, $r^{f}_{h,t}$.

Hence, if periods are measured in business days, the **excess return** of a funded investments between $t$ and $t+h$ is a random variable defined as:

$$
ER_{t+h} \equiv \frac{1+R_{t+h}}{(1+r^{f}_{h,t})^{h/252}}
$$

where $R_{t+h}$ is the **total return** of the investment in the period and $r^{f}_{h,t}$ is the bus/252 day-count exponential convention of the **risk-free rate** prevailing from time $t$ to time $t+h$. Obviously, if periods are measured in months, our annualization factor of the risk-free rate would change from $h/252$ to $h/12$, if periods are measured in quarters, it would change from $h/252$ to $h/4$ and, if periods are measured in years, it would change from $h/252$ to $h$.


## Delta-1 derivatives

As we discussed above, because they are **unfunded**, i.e., they do not require disposable cash at the start of the transaction, quantitative models are typically built on the returns of delta-1 derivatives, not cash assets. Since unfunded investments typically do not require any (or very little) cash, it does not make sense to think of returns as returns over the cash deployed in the investment. The important concept here is the concept of **return over notional** which is the return over the capital at stake in the transaction, not the capital deployed. Let's discuss the calculation of those returns in some specific examples.

###### Futures

Futures contracts, or *futures* for short, are financial contracts obligating the buyer to purchase an asset or the seller to sell an asset at predetermined future date, $T$, and price, $F_{T}$. A futures contract allows an investor to speculate on the direction of a security, commodity, or a financial instrument wihtout having to buy or sell the actual underlying instrument. This is so, because:

* If the investor thinks the price of the underlying asset at the future date, $T$, will be $S_{T} > F_{T}$, then the investor can buy the underlying using a futures contract. At maturity, $T$, the investor is obliged to buy the underlying from the seller of the futures contract by $F_{T}$. If the investor is right and the underlying asset actually has the predicted price $S_{T}$, then the investor can purchase the asset from the futures sellers by $F_{T}$, sell the asset in the market by $S_{T}$ immediately and profit the difference $F_{T} - S_{T}$.

* If the investor thinks the price of the underlying asset at the future date, $T$, will be $S_{T} < F_{T}$, then the investor can sell the underlying using a futures contract. At maturity, $T$, the investor is obliged to sell the underlying from the buyer of the futures contract by $F_{T}$. If the investor is right and the underlying asset actually has the predicted price $S_{T}$, then the investor can buy the asset in the market by $S_{T}$ and then immediately sell the asset to the futures buyer by $F_{T}$ and profit the difference $S_{T} - F_{T}$.

Note that since the investor is buying and selling the underlying asset on the same day $T$, it actually does not need to own the underlying asset in order to profit from its price movements. You can actually buy and sell very large sums without really having much money in my bank account. This is called **leverage** and is one of the most attractive features of trading futures contracts in quantitative strategies.

Leverage means that the investor does not need to put up 100% of the contract's value amount when entering into a trade. Instead, the broker would require an initial margin amount, which consists of a fraction of the total contract value. The amount held by the broker can vary depending on the size of the contract, the creditworthiness of the investor, and the broker's terms and conditions.

In fact, the investor does not even need to wait for the maturity of the futures contract. If on date $t_{0}$, the investor buy or sells the futures contract with expiration date $T$, it will trade at the price $F_{t,T}$. If in a future date, $t+h \le T$, the investors closes the position selling or buying the futures contract with expiration date $T$ for the prevailing price on that date, $F_{t+h,T}$, it will profit the diffence in the futures contract price $F_{t+h,T} - F_{t,T}$ without ever having to put down any money except for the initial margin amount.

Hence, we say, **return over notional** between $t$ and $t+h$ of this transaction is a random variable defined as:

$$
R_{t+h} \equiv \frac{F_{t+h,T} - F_{t,T}}{F_{t,T}}.
$$

Note that this concept, the **return over notional**, is very different from the concept over the return on investment. Here, we only deployed in cash the initial margin amount which is an ammount very small relative to the ammount at stake in the transaction, i.e., relative to the notional. Here, since we transacted one contract at price $F_{t,T}$, the notional is $F_{t,T}$.

Of course, even when using derivatives, one may still be interested on the return on cash invested. Let's say that for transacting a notional of $F_{t,T}$ we have to set aside cash either in terms of [initial margin](https://www.investopedia.com/terms/i/initialmargin.asp) or just cash sitting aside, not earning any interest, to be able to cope with daily [variation margins](https://www.investopedia.com/terms/v/variationmargin.asp). Let's assume that initial cash amount is a proportion, $\kappa$ of the notional invested, $IA_{t} \equiv \kappa F_{t,T}$. Then, the **return on cash** is given by:

$$
R_{t+h} \equiv \frac{F_{t+h,T} - F_{t,T}}{IA_{t}} = \kappa^{-1} \frac{F_{t+h,T} - F_{t,T}}{F_{t,T}},
$$

which is just a constant times the **return over notional**. So, picking strategies by **return over notional** is more or less the same as picking strategies by **return on cash**, under this assumption. Also, under the same assumption, both returns have the same risk-adjusted returns charecteristics.

###### Forwards

Futures contracts and forward contracts, as well as equity swaps, are very similar contracts. They allow us lock in prices for physical transactions occurring only in the future and that way quantitative strategies can try to benefit from price movements of an underlying security, up until the date of the contract delivery, without the need to actually buy or sell that security, so without the need of having a lot of disposable cash.

Still, because when trading a futures contract you are facing the exchange, every day, as futures prices move up and down, you have to deposit and receive cash reflecting the daily price variation on that future contract. As we discussed, this requires you putting some cash aside to meet potential obligations in terms of initial and variation margins. In contrast, forward contracts are agreements between a buyer and seller to trade an asset at a future date but they do not require, like futures, cash to be exchanged every day as a result of price variation. The only cash flow on the contract is at the end of the contract. So, trading forwards requires even less disposable cash than trading futures. As a result, forwards are often prefered by quantitative strategy as well as other institutional investors over futures when trading.

As it is discussed in [Hull's classic textbook on derivatives](https://www.amazon.com.br/Options-Futures-Other-Derivatives-10th/dp/013447208X), there are some differences in how futures and forwards are priced and how returns on cash would be calculated in each case. For our purposes, we will consider these differences negligible and look into return over notional. The calculation of daily return over notional for futures and forward contracts will then be identical for us.

###### Equity total return swaps

Equity swaps, despite their name, are in fact forward contracts. One leg, the "fixed" side, receive the returns over notional of a pre-agreed-upon price level for a single stock or for an index of stocks. The other leg, the "floating leg", has exactly the opposite cash flow stream. In a **total return swap**, the "fixed" leg investor, the "buyer", also collects dividends and any income generated by the underlying asset and pays the "floating" leg investor floating interest on the notional $N_{t}$ acrrued between $t$ and $T$. Let's see an example:

Suppose two counterparties agree to trade the S&P 500 index (or a single-stock like Apple) at price $F_{t,T}$ in a predetermined future date, $T$. They agree to trade a notional equal to $N_{t}$. At maturity, $T$, if there were no dividends paid by the underlying between $t$ and $T$, the "fixed" leg investor, the "buyer" will receive $N_{t} \times (S_{T}/F_{t,T}-1)$ where $S_{T}$ is the prevailing level of the the S&P 500 index (or of Apple stock) at maturity, $T$ and pay floating interest on the notional $N_{t}$ acrrued between $t$ and $T$. The "floating" leg investor will have to pay $N_{t} \times (S_{T}/F_{t,T}-1)$ and will receive the floating interest on the notional $N_{t}$ acrrued between $t$ and $T$.

Equity swaps are in fact forward contracts becasue in fact the "fixed" leg investor "buys" forward $N_{t}/F_{t,T}$ index units (or Apple shares) at price $F_{t,T}$ on date $t$. Each of those units (or shares) purchased will pay the "fixed" leg investor $S_{T}-F_{t,T}$ which is what an investor would receive if it had bought the underlying at price $F_{t,T}$ through a forward contract on date $t$ and held it to maturity, $T$, when the underlying price would be $S_{T}$. If the stock or index also paid dividends during the swap's life, between $t$ and $T$, in an amount of $D$ per index unit or per share, then the "fixed" leg investor receives $S_{T} - F_{t,T} + D$ for each of those units (or shares) or $N_{t} \times ((S_{T}+D)/F_{t,T}-1)$.

###### Interest rate swaps (IRS)

An interest rate swap (IRS) is a type of contract through which two counterparties agree to exchange one stream of interest payments for another, based on a specified principal amount. In most cases, interest rate swaps include the exchange of a fixed interest rate for a floating rate. The *payer* leg of the swap pays periodically (typically every 6 months) interest accrued at a fixed interest rate whcih is mutually agred by the two counterparties at the contract inception. The *receiver* leg of the contract pays periodically (typically every 3 months) interest accrued at a floating interest rate (e.g. LIBOR, SOFR, SONIA< ESTR, etc.).

Because interest rates are currency specific (so 5% interest in BRL is not the same as in USD), the IRS are also typically currency specific. Because they trade over the counter (OTC), the contract between the two parties can be set according to their desired specifications and can be customized in many different ways and cross-currency swaps are IRS for which the two parties agree to exchange interest payments and/or principal denominated in two different currencies. Here we focus on single-currency IRS and let's look at USD swaps for simplicity.

In USD, there are two common and liquid types of fixed-for-floating IRS:

1. Plain-vanilla semi-annual 30/360
2. IMM semi-annual

The major difference between these two types of swaps are the expire dates. Plain-vanilla swaps trade at constrant maturity, so the expiration date of the wap will typically be one, two, five, etc. years from the trade date. The IMM swaps on the other hand have fixed expiry with expiration dates following the cycle of international money market futures and IMM futures options which are the third Wednesday of Mar/Jun/Sep/Dec.

As it is discussed in [Hull's classic textbook on derivatives](https://www.amazon.com.br/Options-Futures-Other-Derivatives-10th/dp/013447208X), trading and pricing IRS requires some complicated math. So, we will leave the details of how to calculate returns on IRS for another lecture. For now, it sufficies for us to say that bond futures and IRS swaps are unfunded instruments that quantitative strategies, and institutional investors in general, typically use to benefit from interest rate movements.

###### Credit default swaps

A credit default swap (CDS) is type of contract through which two counterparties agree to exchange creidt risk over a specific credit-linked underlying like a pool of mortgages, a sovereing or a corporate bond. The seller of "protection" in the CDS, or simply the CDS seller, will compensate the buyer in the event of a default or other credit event in the underlying asset. In compensation for that protection, the buyer of "protection", or simply the CDS buyer, makes a series of payments (typically quarterly) to the seller. In the event of default, the buyer of the CDS receives cash (usually the face value of the underlying credit-link security), and the seller of the CDS takes possession of the defaulted credit-link security or its market value, which is likely not zero, in cash.

Note that since the investor is buying and selling protection on the underlying asset, it actually does not need to own the underlying asset in order to profit from its credit-worthiness related price movements. You can actually buy and sell very large sums without really having much money in my bank account. In fact, here are more CDS contracts outstanding than bonds in existence.

As it is discussed in [Hull's classic textbook on derivatives](https://www.amazon.com.br/Options-Futures-Other-Derivatives-10th/dp/013447208X), trading and pricing CDS requires some complicated math. So, we will leave the details of how to calculate returns on CDS for another lecture. For now, it sufficies for us to say that CDS are unfunded instruments that quantitative strategies, and institutional investors in general, typically use to benefit from movements in the credit-worthiness of underlying credit-linked underlying securities.