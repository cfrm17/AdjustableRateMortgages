# Adjustable Rate Mortgages

Adjustable rate mortgages (ARMs) is a mortgage with adjustable mortgage rate. The valuation model has a significant amount of parameter/model risk. In particular, there are many input parameters (many of them market variables) and functions of these parameters that can have add a significant amount of risk. In particular, these include:

1.	Rate indices input into the model (used to evaluate prepayment speeds), and how they evolve under interest rate simulations/scenarios.

2.	Functions of these rate indices that affect the prepayment  (i.e. base function, burnout and seasoning). The specification of these functions occurs at the instrument level and can have a significant affect on the results.

3.	Options prices that are used to calibrate the volatility in the Black-Derman-Toy (BDT) trees. The methodology for how to do this is not necessarily straightforward, and may introduce model risk.

4.	Market prices of the instruments that are used to evaluate Option Adjusted Spread (OAS) that in turn affects the evaluation of the interest rate risk measures of the instruments.

5.	Initial yield curve used to calibrate the term structure of the BDT model. The methodology to do this is straightforward and is therefore not a concern.


The prepayment model used for these instruments is a four-factor model. While the parameters and factors used were supplied to us, it is difficult to assess the accuracy of the parameters since it is a based on extensive statistical analysis of historical data by the vendor. However, the model seemed reasonable and well motivated.

The yield curve implied by the (Monte Carlo) short rates was checked against the input yield curve to check consistency. With these short rates as input (as well as the note rates and prepayment speeds), the cash flows were also checked and the economic value of the instrument verified. 

The various interest rates are modelled using a choice of arbitrage free term structure models. In the current implementation, a Black-Derman-Toy (BDT) short rate model is used. This model is used to generate the future interest rate scenarios (i.e. short rates). These future interest rate scenarios are important because they determine both the cash flows in a particular scenario as well as the discount factors used to discount these cash flows. 

The customer coupon (or note rate) is 5% for the first five years and then resets every year thereafter based on the 12 month LIBOR rate. The compounding frequency is this rate is money market (or simple), and a spread of 2.25% is added to determine the coupon. Furthermore, there are caps on the reset rates as specified above.

Thus, in order to determine the coupon, one needs the future interest rate scenarios from the short rate model to determine possible future 12 month LIBOR rates. The short rates are generated using Monte Carlo (with a BDT process), and a “sub” lattice (binomial) is generated at the reset period to determine the 12 month LIBOR rates and therefore the coupons. 

The specification of a prepayment model is very important for the correct evaluation of these mortgages. In this case, a proprietary prepayment model from Andrew Davidson is going to be used. For comparison purposes, 

The base function describes SMM as a function of refinancing incentive, which is the difference between the mortgage note rate and the current market rate. Generally speaking, the bigger this difference, the larger the prepayment.  We have already discussed how the mortgage note rate is specified (for a given product), but we need to specify what the “current market rate” is. There are a number of ways, but for our purposes this is defined using a “partial response model”.

Clearly, one also needs to specify an initial value for the index in order for this to be fully defined. Thus, one needs to specify an initial rate index, and this value has a significant effect on the value of the instrument. This makes the spread to treasury irrelevant, since we are only concerned with changes in the ten year treasury rate and not the absolute value.

Finally, there is a “time lag” between the time a change in market rates occurs and the time the effects of this change are seen in the cash flows. In the particular case we looked at, the time lag was chosen to be two months.

This factor corresponds to the observed decrease in prepayments as the mortgage (or pool of mortgages) ages. This is due to the fact that when a pool experiences a drop in market rats for the first time, those borrowers who are willing to take advantage and refinance do so, and the borrowers who are left in the pool are less responsive to changes in the market.

As noted earlier, the seasoning factor accounts for the fact that new pools take a certain amount of time before they assume their long-term prepayment behaviour. This is captured by making the factor dependant on the age of the pool, so that the seasoning multiplier is given 

In order to calculate the interest rate risk of the instruments, several interest rate scenarios are considered. These scenarios are most commonly parallel shifts of the initial yield curves discussed earlier. The initial values for the rate indices discussed in the prepayment section of the report are also shifted by the same amount.

To calculate the market value changes under these various scenarios, the interest rate paths are regenerated using BDT and the cash flows are also recalculated. The market value under the new scenario is then calculated using the above formula using the new short rates, cash flows (recalculated to include shocks), and including the OAS calculated previously in the evaluation of the discount factors. 


Reference:

https://finpricing.com/lib/EqCliquet.html

https://zenodo.org/record/6539925/files/zenodo-ARM-analytics.pdf

https://zenodo.org/record/6539925#.YpDurqgpDq4


