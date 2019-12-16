# Quantitative Finance Lectures

Here you will find a set of notebooks to help get started with implementing some basic quantitative strategies described in the academic literature of factors and commonly employed in practice. You can see these notebooks as a follow up on our Python Lectures covering fundamentals of the Python programming language.

## Trackers

Financial time series are the "atoms" of quantitative finance. Hence, it is important that these financial time series realistically reflect the cumulative **total returns** of those asssets and not simply their prices over time. Total return is the actual rate of return of a position, portfolios or strategy over a given evaluation period, typically a business day. Total return of an asset has to include interest gained or paid when holding the position, capital gains coming from price or exchange rate variations in the period as well as dividends, coupons, borrow costs, and other distributions or payments realized over the period of time the asset was held. For example, check out our Jupyter Notebook on how to create a financial time series for trading [currencies](https://github.com/Finance-Hub/FinanceHubMaterials/blob/master/Quantitative%20Finance%20Lectures/creating_fx_time_series_fh.ipynb), [futures](https://github.com/Finance-Hub/FinanceHubMaterials/blob/master/Quantitative%20Finance%20Lectures/rolling_futures_time_series.ipynb), and [interest rates swaps](https://github.com/Finance-Hub/FinanceHubMaterials/blob/master/Quantitative%20Finance%20Lectures/swap_historical_returns.ipynb)!

## Pricing

Sometimes constructing these financial time series can be very difficult as the valuation of the underlying instruments can be a very challenging thing to do based on pulically available data. Check out our notebook that shows how to price and calculate the returns of a plain-vanilla interest rate swap [here]()
