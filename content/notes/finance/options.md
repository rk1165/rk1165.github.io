+++
title = 'Option Volatility and Pricing'
date = 2024-02-11

+++

### Chapter 1

- Option premium can be separated into two components:
  1. _Intrinsic value_
  2. _Time value_
- Intrinsic value is the amount which would be credited to the option holder's account if he were to exercise the option and close out the position against the underlying contract at the current market price.
- Call will only have intrinsic value if its exercise price is less than CMP
- Put will only have intrinsic value if its exercise price is greater than the CMP
- It is possible for an European option to have a negative time value.
- The price of an _out-of-the-money_ option consists solely of time value.
- long call implies _long market position_
- long put implies _short market position_

### Forward Pricing

- `forward price = current cash price + costs of buying now - benefits of buying now`
- Traders in forward or futures contracts sometimes refer to the _basis_ - `cash price - forward price`
- In most cases, the basis will be a negative number.
- A normal or _contango_ commodity market is one in which long-term futures contracts trade at a premium to short-term contracts.
- If the cash price of a commodity is greater than a futures price, the market is _backward_ or in _backwardation_.
- The benefit of being able to obtain a commodity right now is sometimes referred to as a _convenience yield_.
- The _forward_ rate is the rate of interest that is applicable beginning on some future date for a specified period of time. Forward rates are often expressed in months
- 1 × 5 forward rate: A four-month rate beginning in one month
- A _forward-rate agreement (FRA)_ is an agreement to borrow or lend money for a fixed period, beginning on some future date. A 3 × 9 FRA is an agreement to borrow money for six months, but beginning three months from now.
- If we treat the coupon payments as if they were dividends, we can evaluate bond and note forward contracts in a similar manner to stock forwards
- With foreign-currency forward contracts, we must deal with two different rates:
  1. the domestic interest rate we must pay on the domestic currency to buy the foreign currency
  2. the foreign interest rate we earn if we hold the foreign currency
- To calculate a foreign-currency forward price, we must first express the spot exchange rate `S` as a fraction - the cost of one foreign-currency unit in terms of domestic-currency units `Cd` divided by one foreign-currency unit `Cf`.
- For both stock options and futures options, the value of the option will depend on the forward price for the underlying contract
- In a foreign-currency market, a trader may attempt to profit by borrowing a low-interest-rate domestic currency and using this to purchase a high-interest-rate foreign currency. The trader hopes to pay a low interest rate and simultaneously earn a high interest rate. However, this type of _carry trade_ is not without risk.
- _cash-and-carry arbitrage_ involves buying in the cash market, selling in the futures market, and carrying the position to maturity.
- If a trader believes that a contract is fairly priced, the implied value must represent the marketplace’s consensus estimate of the missing value
- **Declared Date**. The date on which a company announces both the amount of the dividend and the date on which the dividend will be paid.
- **Record Date**. The date on which the stock must be owned in order to receive the dividend.
- **Ex-Dividend Date (Ex-Date)**. The first day on which a stock is trading without the rights to the dividend.
- **Payable Date**. The date on which the dividend will be paid to qualifying shareholders
- If, however, the date on which the dividend will be paid is expected to fall close to the maturity date of a derivative contract, a slight miscalculation of the dividend date can significantly alter the value of the derivative.
- The rate that the trader receives on the short sale of stock is sometimes referred to as the _short-stock rebate_.
- We can make a distinction between the long rate `rl` that applies to ordinary borrowing and lending and the short rate `rs` that applies to the short sale of stock. The difference between the long and short rates represents the borrowing costs `rbc`.
- we always apply the ordinary long rate to the cash flow resulting from either the purchase or sale of an option.

### Contract Specification and Option Terminology

- Short-term options on long-term futures are listed as _midcurve options_.
- Many derivative strategies require carrying an offsetting stock position to expiration, at which time the stock position is liquidated. Consequently, stock exchanges found that as the close of trading approached on expiration day, they were faced with large orders to buy or sell stock. These large orders often had the effect of disrupting trading or distorting prices at expiration.

### Elemenetary Strategies

- Options that have either unlimited reward or unlimited risk because they are either net long or net short positions.
- If we purchase and sell equal numbers of options of the same type we can create positions which have both limited risk and limited reward.
- buy call and put : Such a position might be sensible if we thought a large move in the underlying contract would take place in the near future, but were uncertain as to the direction of that move.
- sell call and put: If we believe the underlying contract is unlikely to make a big move in either direction. (i.e it remains in a range)
- buy 90 call and sell 100 call : Bullish. We are willing to give up the unlimited upside profit potential associated with the outright purchase of call, in return for the partial downside protection afforded by the sale of the call.
- buy 100 call and sell 90 call: bearish.
- buy higher K put and sell lower K put: bearish
- **Constructing an expiration graph**
  1. If the graph bends, it will do so at an exercise price. Therfore, we can calculate profit or loss at each exercise price involved and simply connect these points with straight lines.
  2. If the position is long and short equal numbers of calls (puts), the potential downside (upside) risk or reward will be equal to the total debit or credit required to establish the position.
  3. Above the highest `K`, all calls will go into-the-money, so the entire position will act like an underlying position which is either long or short underlying contracts equal to the number of net long or short calls. Below the lowest `K` all puts will go into-the-money, so the entire position will act like an underlying position which is either long or short underlying contracts equal to the number of net long or short puts.

### Chapter 3

- While the option trader is sensitive to directional considerations, he must also give careful consideration to how fast the market is likely to move.
- The concept of speed is vital in trading options.
- There are many option strategies which depend only on the speed of the underlying market and not at all on its direction.
- If a trader believes that a strategy has a very high probability of profit and a very low probability of loss, he will be satisfied with a small potential profit because the profit is likely to be quite secure.
- If the probability of profit is very low, the trader will demand a large profit when market conditions develop favorably.
- The _theoretical value_ of a proposition is the price one would expect to pay in order to just break even in the long run. The _theoretical value_ of the bet is really the present value of its expected value.
- The two most common considerations in a financial investment are the expected return and carrying costs.
- The sensible approach is to make use of a model but with a full awareness of what it can and cannot do.
- Necessary steps in developing a theoretical pricing model might be:
  1. Propose a series of possible prices at expiration for the underlying contract.
  2. Assign a probability to each possible price with the restriction that the underlying market is arbitrage-free — the expected value for the underlying contract must be equal to the forward price.
  3. From the prices and probabilities in steps 1 and 2, and from the chosen exercise price, calculate the expected value of the option.
  4. Lastly, depending on the option’s settlement procedure, calculate the present value of the expected value.
- With the movement in price of the underlying, the probability will change dynamically and thus the expected value
- For a long stock position to be profitable, the stock must appreciate by at least the amount of carrying costs over the holding period. (take inflation also into account)
- In we assume an arbitrage-free market, we must necessarily assume that the _forward price_, the avg. price of the contract at the end of the holding period, is the current price, plus an expected return whill will exactly offset all other credits and debits.
- _Riskless hedge_. For every option posn. there is a theoretically equivalent position in the underlying contract such that, for small price changes in the underlying contract, the option position will gain or lose value at exactly the same rate as the underlying position.
- To take advantage of a theoretically mispriced option, it is necessary to establish a hedge by offsetting the option position with this theoretically equivalent underlying position i.e, whatever option position we take, we must take an opposing market position in the underlying contract.
- The correct proportion of underlying contracts needed to establish this riskless hedge is known as the _hedge ratio_.
- we are always doing the opposite with calls and the underlying (i.e., buy calls, sell the underlying; sell calls, buy the underlying) and doing the same with puts and the underlying (i.e., buy puts, buy the underlying; sell puts, sell the underlying)
- **Time to expiration**, is entered as an annualized number.
- feed actual number of days remaining to expiration knowing that the model will interpret the number correctly.
- Most traders have learned through experience that as expiration approaches, the use of a theoretical pricing model becomes less reliable because the inputs become less reliable. Indeed, very close to expiration, many traders stop using model-generated values altogether.
- Therefore, the underlying price that he feeds into the theoretical pricing model ought to be the price at which he believes he can make the opposing trade.
- Interest rates play two roles in the theoretical evaluation of options
  1. First, they may affect the forward price of the underlying contract.
  2. Second, interest rates may affect the present value of the option.
- If the underlying contract is subject to stock-type settlement, as we raise interest rates, we raise the forward price, increasing the value of calls and decreasing the value of puts
- **How interest rates affect options**
  1. If the underlying contract is subject to stock-type settlement, as we raise interest rates, we raise the forward price, increasing the value of calls and decreasing the value of puts
  2. If the option is subject to stock-type settlement, as we raise interest rates, we reduce the present value of the option.
- To determine a more realistic rate, a trader might look to a freely traded market in interest-rate contracts
- the correct interest rate will, in theory, depend on whether the trade will create a credit or a debit. In the former case the trader will be interested in the borrowing rate, while in the latter case he will be interested in the lending rate.
- In practice, rather than using the date of dividend payment, an option trader is likely to focus on the _ex-dividend_ date, the date on which the stock is trading without the rights to the dividend.

### Chapter 4 Volatility

- think of the ball’s sideways movement as the up and down price movement of an underlying contract and the ball’s downward movement as the passage of time.
- In low volatility market, price movement is severly restricted, and consequently options will command relatively low premiums. In a high-volatility markets the chances for extreme price movement is greatly increased, and options will command high premiums.
- The various forms of the Black-Scholes model differ primarily in how they calculate the forward price. Depending on the type of underlying contract, whether a stock, a futures contract, or a foreign currency, the model takes the current underlying price, the time to expiration, interest rates, and, in the case of stocks, dividends to calculate the forward price. It then makes this the mean of the distribution.
- We can define the volatility number associated with an underlying instrument as a one S.D price change, in percent, at the end of a one-year period.
- volatility is proportional to the square root of time. To calculate a volatility, or standard deviation, over some period of time other than one year, we must multiply the annual volatility by the square root of time, where the time period is expressed in years
- with the advent of electronic trading, it may become more difficult to determine exactly what fraction of a year one day represents. Depending on the contract and exchange, in some cases it may be sensible to look at prices every day, 365 days per year
- To approximate a daily standard deviation, we can divide the annual volatility by 16.
- To approximate a weekly standard deviation, we can divide the annual volatility by 7.2.
- most traders prefer to see larger price samplings, perhaps 20 days, or 50 days, or 100 days, before drawing any dramatic conclusions about volatility.
- A price change greater than two standard deviations will occur about 1 time in 20. Because there are approximately 20 trading days in a month, as an additional benchmark, most traders expect to see a daily two standard deviation occurrence about once a month.
- it is the settlement-to-settlement price change on which we focus
- For some interest-rate products, primarily Eurocurrency interest-rate futures, the listed contract price represents the interest rate associated with that contract, expressed as a whole number, subtracted from 100.
- Volatility calculations for these contracts are done using the rate associated with the contract (_the rate volatility_) rather than the price of the contract (_the price volatility_).
- The B-S-M is a _continuous time_ model. It assumes that the volatility of an underlying instrument is constant over the life of the option, but that this volatility is continuously compounded.
- When the percent price changes are normally distributed, the continuous compounding of these price changes will result in a lognormal distribution of prices at expiration.
- **Future volatility**, the volatility that describes the future distribution of prices for an underlying contract. It is this number to which we are referring when we speak of the volatility input
- If there is a discrepancy between our theoretical option price and the market price. if we assume that everyone in the marketplace is using the same model - the B-S-M model, then the discerpancy must be due to a difference of opinion concerning one or more of the inputs into the model.
- It is often more useful to consider an option's price in terms of implied volatility rather than in terms of its total dollar price. Say by calculating the implied volatility we say the option is x% overpriced or underpiced in terms of volatility.

### Chapter 5 Using an Option's Theoretical Value

- The delta of a call option is always somewhere between 0 and 1
- The delta of an option can change as market conditions change.
- An underlying contract always has a delta of 1.0
- To make correct use of the option's theoretical value, we need to know the delta.
- The correct procedure in using an option's theoretical value:
  1. Purchase (sell) undervalued (overvalued) options.
  2. Establish a delta neutral hedge against the underlying contract.
  3. Adjust the hedge at regular intervals to remain delta neutral.
- Volatility is assumed to be constant over the life of the option.
- According to the model, the option's cash flow can be replicated through an adjustment process in the underlying contract.

### Chapter 6

- Learning how changing market conditions are likely to change an option's value and the risks associated with an option position.
- If interest rates are high, we will prefer the call since outright purchase of stock requires a much larger cash outlay and greater carrying cost.
- For gauging the effect of interest rates, ask yourself, whether a call (put) is a better or worse substitute for the outright purchase (sale) of the underlying contract.
- `Hedge ratio = 100 / option's delta`
- The **gamma** is usually expressed in deltas gained or lost per one point change in the underlying, with the delta increasing by the amount of the gamma when the underlying rise, and falling by the amount of the gamma when the underlying falls.
- Both calls and puts must have positive gammas
- We always add the gamma to the old delta as the underlying rise, and subtract the gamma from the old delta as the underlying falls.
- The gamma is measure of how fast an option changes its directional characteristics, acting more or less like an actual underlying position.
- A large gamma number, + or -, indicates a high degree of risk.
- large negative gamma risk
- The **theta**, or time decay factor, is the rate at which an option loses value as time passes.
- It is usually expressed in points lost per day, when all other conditions remain the same.
- A long option will always have a negative theta. A short option will always have a positive theta.
- An option position will have a gamma and theta of opposite signs.
- The relative size of the gamma and theta will correlate.
- large positive gamma goes hand in hand with a large negative theta
- As we get closer to expiration the rate at which an option decays begins to accelerate.
- Either a trader wants the market to move or stand still.
- A large negative theta represents a high degree of risk wrt. the passage of time.
- The **vega** of an option is usually given in point change in theoretical value for each one percentage point change in vol.
- The vega for both calls and puts is positive.
- Changes in the amount of time remaining to expiration and changes in volatility often have similar effects on an option's value.
- The sensitivity of an option's theoretical value to a change in interest rates is given by its **rho**.
- Dividend considerations play their greatest role in arbitrage strategies and early exercise.
- An option's _elasticity_, sometimes denoted with the _omega_ or _lambda_ is the relative percent change in an option's value for a given percent change in the price of the underlying contract.
- The elasticity is sometimes referred to as the option's _leverage value_. The greater an option's elasticity, the more highly leveraged the option.
- `elasticity = underlying price * delta / theoretical value`

### Chapter 6 Introduction to Spreading

- A spread is a strategy which involves taking simultaneous but opposing positions in different instruments.
- A spread trader makes the assumption that there is an identifiable price relationship between different instruments and, although he may not know in which direction the market will move, the price relationship ought to remain relatively constant.
- Among futures traders the most common type of spread involves taking opposing positions in different delivery months for the same commodity.
- A trader might purchase Oct. crude oil and sell Nov. crude oil.
- The price of a more distant delivery month ought to be greater than the price of a nearby delivery month. this is known as _contango_ relationship.
- Markets where the near-by delivery month is trading at a premium to the more distant months are said to be in _backwardation_.
- Much of the most sophisticated trading in derivative markets involves identifying and following spread relationships.
- Spread relationships are sort of like pairs trade.
- Volatility spreads form one of the most important classes of option trading strategies.
- Option spreading strategies enable traders to profit over a wide variety of market conditions by giving them an increased margin for error in estimating the inputs into a theoretical pricing model.

### Chapter 8 Volatility Spread

- **BACKSPREAD** (also referred to as a _ratio backspread_ or _long ratio spread_).
- A backspread is a delta neutral spread which consists of more long (purchased) options than short (sold) options where all options expire at the same time.
- In order to achieve this, options with smaller deltas must be purchased and options with larger deltas must be sold.
- A call backspread consists of long calls at a higher exercise price and short calls at a lower exercise price.
- A put backspread consists of long puts at a lower exercise price and short puts at a higher exercise price.
- The primary consideration is that some movement will occur.
- A trader will tend to choose the type of backspread which reflects his opinion about market direction. If he foresees a market with great upside potential, he will tend to choose a call backspread; if with great downside potential he will tend to choose a put backspread.
- **RATIO VERTICAL SPREAD** (also referrred to as a _ratio spread_, _short ratio spread_, _vertical spread_, or _front spread_)
- The opposite of a backspread i.e short more contracts than long, with all options expiring at the same time is called ratio vertical spread.
- A ratio vertical spreader expects the market to remain relatively stable.
- **STRADDLE**
  - A straddle consists of either a long call and a long put, or a short call and a short put, where both options have the same exercise price and expire at the same time.
  - If both the call and put are purchased, the trader is long the straddle; if both, options are sold, the trader is short the straddle.
  - A straddle can also be _ratioed_.
- **STRANGLE**
  - A strangle consists of a long call and a long put, or a short call and a short put, where both options expire at the same time.
  - In a strangle, however, the options have different exercise prices.
  - A long strangle needs movement to be profitable.
- **BUTTERFLY**
  - A butterfly consists of options at three equally spaced exercise prices, where all options are of the same type (either calls or all puts ) and expire at the same time.
  - In a long butterfly the outside exercise prices are purchased and the inside exercise price is sold, and vice versa for a short butterfly.
  - The ratio is always 1 X 2 X 1
  - At expiration a butterfly will always have a value somewhere between zero and the amount between exercise prices.
  - If a trader feels that the underlying contract will remain within a narrow range until expiration, he can buy a butterfly where the inside exercise price is at-the-money.
  - The trader who sells a butterfly wants the underlying market to move as far away from the inside exercise price as possible.
- A spread which requires an outlay of capital is referred as long.
- **TIME SPREAD** (also referred to as a _calendar spread_ or _horizontal spread_)
  - Time spreads, consist of opposing positions which expire in different months.
  - The most common type of time spread consists of opposing positions in two options of the same type (either both calls or both puts) where both options have the same exercise price.
  - When the long-term option is purchased and the short term option is sold, a trader is long the time spread.
  - When the short-term option is purchase and the long-term option is sold, the trader is short the time spread.
  - A long time spread always wants the underlying market to sit still.
  - A long time spread always benefits from an increase in implied volatility
  - When implied volatility rises, time spreads tend to widen; when implied volatility falls, time spreads tend to narrow.
  - The trader who is long a time spread wants two apparently contradictory conditions in the marketplace.
  - He wants the underlying market to sit still so that time decay will have a beneficial effect on the spread.
  - He wants everyone to think the market is going to move so that implied volatility will rise.
- A swift move in the underlying market or an increased in implied volatility will help a backspread (long straddles, short strangles, and short butterflies)
- A quiet market or a decrease in implied volatility will help a ratio vertial spreads( including short straddles, short strangles, long butterflies)
- **DIAGONAL SPREADS**
  - A diagonal spread is similar to a time spread, except that the options have different exercise prices.
- **OTHER VARIATIONS**
  - A _Christmas tree_ (also referred to as a _ladder_) is a term which can be applied to a variety of spreads.
    - The spread usually consists of three different exercise prices where all options are of the same type and expire at the same time.
    - In a long (short) call Christmas tree, one call is purchased (sold) at the lowest exercise price, and one call is sold (purchased) at each of the higher exercise prices.
    - In a long (short) put Christmas tree, one put is sold (purchased) at the highest exercise price, and one put is sold (purchased) at each of the higher exercise prices.
