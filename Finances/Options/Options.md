An **FX option** gives the **right** - but **not the obligation** - to the option **buyer** **to buy/sell** a defined currency amount **at an agreed rate** (strike price) **at expiry date.**

The **seller** of an option **has** the **obligation** to **buy/sell** a defined currency amount **at an agreed rate at expiry date.** For this obligation **he** **receives** a **premium**.

A **Call option** gives the right to **buy**, a **Put option** gives the right to **sell** a currency. The **seller** of the option **receives a premium** for giving this right to the buyer. This **premium** has to be **paid** **on the day the option is traded.**

![image](./image%203.png)

### **Dates in the option contract**

- **Trading date** is the day when the option is dealt.
- **Premium payment date** is the day when the option premium has to be paid, usually value 2 days.
- **Exercise or expiration date** is the last day on which the option seller accepts the exercise of the option.
- **The settlement date** is when a trade is final—when the buyer pays the seller, and the seller delivers cleared assets to the buyer.
- **Strike price/rate** the price fixed by the seller of a security after receiving bids in a tender offer, typically for a sale of a new stock market issue.
- **Hedge** is an investment that is selected to reduce the potential for loss in other investments because its price tends to move in the opposite direction. This strategy works as a kind of insurance policy, offsetting any steep losses in other investments.

### **Options**

- **At-the-money (ATM)** the **strike** **rate around the actual market rate**. If the strike price is compared to the spot rate, the option is called at-the-money spot. By comparing the strike with the outright rate, the option is called at-the-money forward.
- **In-the-money (ITM)** the **strike rate is “better”** than the market rate. For a Call this means that the strike is below the market rate. The Put is in-the-money if the strike is higher than the market rate.
- **Out-of-the-money (OTM)**  the **market rate is “better”** than the strike rate. For a Call out-of-the-money this means a lower market rate than the strike. A Put is out-of-the- money if the market rate is higher than the strike.

### **OTC/ exchange traded options**

**FX Options** are almost **exclusively** **traded** **OTC** (“over the counter”). In opposition to **ex-change traded options** where only standardized periods and strikes can be traded there are no restrictions in the OTC market. In practice OTC options have stood up to exchange traded options because the original deals have to be regarded individually.

### **Positions**

![[image 1 3.png|image 1 3.png]]

- By **buying** a **Call** option you acquire the **right** to **buy** an agreed amount of a currency at the expiry date.
- By **selling** a **Call** option the seller has the **obligation** to **sell** the currency at the strike price at expiry, if the buyer decides to exercise. For undertaking this risk the seller receives a **premium**
- By **buying** a **Put** option you buy the **right** to **sell** the currency at the agreed price at expiry. In case of a higher rate at expiry the buyer of the option does not exercise and may sell at the higher market rate. Possible higher rates during the term can be locked in by selling (outright) the currency.
- By **selling** a **Put** option seller takes the **obligation** to **buy** the currency at the agreed price at expiry; if the buyer of the option decides to exercise. For taking this risk the seller receives a **premium**.

**General rule:** the **Call** in the **base** currency is at the same time always the **Put** in the **quote** currency (and vice versa), e.g. a **EUR** **Call** is at the same time a **USD** **Put** (the **right** to **buy EUR equals** the **right** to **sell USD**).

### **Option Styles**

- A **European** style option is an option that can only be **exercised** on the **expiry** **date**.
- An **American** style option is an option that can be **exercised** at **any** **trading** **day** **during** the **life** of the option (usually with exchange traded options).
- A **Bermudan** style option can only be **exercised** at **certain** **dates** during the option period. It is a mixture of European and American style option.
- An **Asian** style option refer to the **average rate of the underlying exchange rate** that exist **during** the **life of the option**. This average will be used to determine the intrinsic value of the option by comparison with the predetermined fixed strike. If the option is a call option and the **average rate exceeds the strike,** the **buyer** will **receive a cash flow** (i.e. the difference between the average rate and the strike). For a put option, the average must be below the strike. It's an option type where the payoff depends on the **average price** of the underlying asset over a certain period of time as opposed to standard options
- A **Compound option** is an option on an option: the buyer has the right to buy a plain vanilla call or put option with a predetermined strike at a predetermined date and at a predetermined price (i.e. the options premium). In other words a compound option is an option for which its underlying security is another option. Therefore, there are two strike prices and two exercise dates. They are available for any combination of calls and puts. For example, a put where the underlying is a call option or a call where the underlying is a put option.
- A **Barrier Options** are standard options with additional barrier level.
    - A **knock-out option** is a standard option, which **expires** worthless **if** a formerly specified **exchange** **rate** (barrier, trigger) **is dealt in the spot market before expiration**. The spot rate moves towards “**out-of-the-money**” in order to reach the “**outstrike**”
        - call strike: 1.2000, outstrike: 1.1500 - the option expires if spot falls below 1.1500
        - put strike: 1.2000 outstrike: 1.2500 - the option expires if spot rises over 1.2500
    - A **reverse knock-out/kick-out option -** spot rate moves towards “**in-the-money**” in order to reach the **outstrike**
        - call strike: 1.2000, outstrike: 1.2500 - the option expires if spot rises above 1.2500
        - put strike 1.2000, outstrike 1.1500 - the option expires if spot falls below 1.1500
    - A **knock-in option** is a standard option, which only **starts** to exist **if** a formerly specified **exchange** **rate** (barrier, trigger) **is dealt in the spot market before expiration**.
        - call strike: 1.2000, instrike: 1.1500 - the option appears if spot falls below 1.1500
        - put strike: 1.2000 instrike: 1.2500 - the option appears if spot rises over 1.2500
    - A **reverse knock-in/kick-in option -** spot rate moves towards “**in-the-money**” in order to reach the **instrike**
        - call strike: 1.2000, instrike: 1.2500 - the option appears if spot rises above 1.2500
        - put strike: 1.2000б instrike: 1.1500 - the option appears if spot falls below 1.1500
    - A **double knock-out/knock-in options** have **two barrier** levels. They **expire** worthless (start to exist) **if** **one** of them is **reached** in the spot market.
