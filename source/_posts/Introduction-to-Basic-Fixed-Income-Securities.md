# Introduction to Basic Fixed Income Securities

1. Lottery payments

    A major lottery advertises that it pays the winner \$ 10 million. However this prize money is paid at the rate of \$ 500,000 each year (with the first payment being immediate) for a total of 20 payments. What is the present value of this prize at 10% interest compounded annually?
        
    $PV = \sum^N_{k=0}\frac{c_k}{(1+r)^k} = \sum^{20}_{k=0}\frac{500000}{(1+10\%)^k} = 4.2567818598792826 $ 4.68?

2. Sunk Costs (Exercise 2.6 in Luenberger)

    A young couple has made a deposit of the first month's rent (equal to \$ 1,000) on a 6-month apartment lease. The deposit is refundable at the end of six months if they stay until the end of the lease. The next day they find a different apartment that they like just as well,
    but its monthly rent is only \$ 900. And they would again have to put a deposit of \$ 900 refundable at the end of 6 months. They plan to be in the apartment only 6 months. Should they switch to the new apartment? Assume an (admittedly unrealistic!) interest rate of 12% per month compounded monthly.
    
    $ NPV1 = -\sum^5_{i=0}\frac{1000}{1.12^i} + \frac{1000}{1.12^6}$
    $ NPV2 = -1000-\sum^5_{i=0}\frac{900}{1.12^i} + \frac{900}{1.12^6}$

3. Relation between spot and discount rates

    Suppose the spot rates for 1 and 2 years are s1=6.3% and s2=6.9% with annual compounding. Recall that in this course interest rates are always quoted on an annual basis unless otherwise specified. What is the discount rate d(0,2)?
    
    $ d(0,2) = \frac{1}{(1+s_2)^2} = 0.8750736155679097 $

4. Relation between spot and forward rates

    Suppose the spot rates for 1 and 2 years are s1=6.3% and s2=6.9% with annual compounding. Recall that in this course interest rates are always quoted on an annual basis unless otherwise specified. What is the forward rate, f1,2 assuming annual compounding?
    
    $ f(1,2) = (F_2 - F_0)d(2,2) - (F_1-F_0)d(1,2) = (1.069^2 / 1.063 - 1) = 7.503386641580434% $

5. Forward contract on a stock

    The current price of a stock is \$400 per share and it pays no dividends. Assuming a constant interest rate of 8% per year compounded quarterly, what is the stock's theoretical forward price for delivery in 9 months?
    
    $ F = \frac{S}{d(0, T)} = 400*(1+0.02)^3 = 424.48320000000007 $

6. Bounds using different lending and borrowing rate

    Suppose the borrowing rate rB=10% compounded annually. However, the lending rate (or equivalently, the interest rate on deposits) is only 8% compounded annually. Compute the difference between the upper and lower bounds on the price of an perpetuity that pays A=10,000\$ per year.
    
    $ PV = \sum^N_{k=0}\frac{c_k}{(1+r)^k} = 168.3501683501676$ 25000?

7. Value of a Forward contract at an intermediate time

    Suppose we hold a forward contract on a stock with expiration 6 months from now. We entered into this contract 6 months ago so that when we entered into the contract, the expiration was T=1 year. The stock price \$ 6 months ago was S0=100, the current stock price is 125 and the current interest rate is r=10% compounded semi-annually. (This is the same rate that prevailed 6 months ago.) What is the current value of our forward contract?
    
    $ f = S_t - F_0 * e ^{-r * t} = 125 - 100/(e^(-10%*1)) * e^(-10% * 2) = 43.12692469220181 $
