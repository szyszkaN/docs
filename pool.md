**Innovative Liquidity Pool with Impermanent Loss protection and Yield generation.**

![LP graphic](.gitbook/assets/lp_ann.png)


The TPRO Network has developed a innovative liquidity pool system designed to maximize support for all network participants. This innovative approach optimizes earnings for liquidity providers, reduces impermanent loss, and enhances the system’s resilience to market fluctuations.

The **80/20 Weighted Liquidity Pool**, pioneered by Balancer, lowers entry barriers for liquidity providers, driving higher Total Value Locked (TVL) while minimizing impermanent loss. The **Dynamic Fee Mechanism** further boosts profitability by optimizing fees generated from the pool, and an **Impermanent Loss Protection** mechanism ensures additional security for liquidity providers, significantly enhancing the overall yield from investments. 


All of the information presented is at the in progress stage, so the final technical implementation could change. 

<a name="balancer_pool"></a>
# Liquidity Pool - Balancer
The TPRO Network team evaluated four different types of Balancer pools to optimize liquidity management and ensure economic efficiency. The pools under consideration were as follows:

- Standard 50/50 Pool - A classic Balancer pool where liquidity is equally distributed between two assets, providing balanced exposure and straightforward liquidity provisioning.
- 80/20 Weighted Pool - A pool with an asymmetric weight distribution, designed to minimize impermanent loss for a primary asset while still enabling efficient trading and liquidity. This structure is particularly suitable for projects aiming to support a dominant asset.
- Multi-Asset Pool - A flexible pool that can support multiple tokens with custom weightings. It offers a diversified approach to liquidity management, allowing for broader asset exposure within a single pool.
- Boosted Pool - An advanced pool that integrates with external yield protocols (e.g., Aave) to generate additional returns on idle liquidity while maintaining trading efficiency. This approach combines liquidity provisioning with yield optimization.

The TPRO Network team carefully evaluated four different types of Balancer pools to optimize liquidity management and ensure economic efficiency. After thorough analysis and simulations, we selected the 80/20 Weighted Pool as the optimal solution.

This pool offers a modern and secure approach to liquidity provisioning, addressing key challenges faced by liquidity providers:
- **Reduced Impermanent Loss**: The asymmetric 80/20 weight distribution minimizes the impact of price volatility on the primary asset, ensuring a safer environment for liquidity providers.
- **Lower Entry Barrier**: The reduced requirement for reserve contributions makes it easier for participants to become liquidity providers, increasing accessibility.
- **Increased Attractiveness**: By offering enhanced security and efficiency, the 80/20 Weighted Pool provides a compelling alternative to traditional liquidity models.

Furthermore, the 80/20 Weighted Pool is designed to increase the Total Value Locked (TVL) within the liquidity pool, surpassing the current performance on Uniswap. This makes it an innovative and appealing choice for both the project and its community of liquidity providers.

The implementation of this pool underscores our commitment to fostering economic efficiency, accessibility, and safety in liquidity management.

<a name="dynamic_fee"></a>
# Dynamic Fee
The dynamic fee mechanism in the liquidity pool aims to create a more efficient, fair and secure trading environment. Unlike static fee models, this mechanism adapts to changing market conditions, providing flexibility to the system. 

The primary objectives of the dynamic fee mechanism include:

- Fairness in Fee Structure: By calculating fees separately for buy and sell transactions, the mechanism addresses the distinct dynamics of each trade type. This ensures fees are proportional to the market impact of individual transactions.
- Adaptability to Market Volatility: The system incorporates market volatility into its calculations, adjusting fees based on the volatility of Ethereum. During periods of heightened volatility, fees increase to mitigate risks, while in stable conditions, fees decrease to encourage trading activity.
- Protection for Liquidity Providers: The dynamic fee mechanism shields all liquidity providers by increasing their income when impermanent loss rises due to volatility. Additionally, part of the collected fees funds a dedicated protection mechanism for providers who are most concerned about impermanent loss, further reducing their risk.
- Support for Ecosystem Stability and Growth: By balancing the interests of traders and liquidity providers, the dynamic fee system fosters a sustainable ecosystem. Traders benefit from context-aware fees, while liquidity providers gain assurance of fair compensation and additional protection.

This dynamic approach ensures that the fee structure evolves with market conditions and participant behavior, creating a fair and adaptive framework. The hedging mechanism for impermanent loss, which complements the dynamic fee system, is discussed in detail in a separate section.
<a name="mechanism_parameters_dynamic_fee"></a>
## Mechanism parameters
The table below shows the key parameters of the dynamic fee mechanism.
Table 1. Dynamic Fee Parameters


| **Parameter**                | **Description**                                                                 | **Value**    |
|------------------------------|-------------------------------------------------------------------------------|-------------|
| `min_fee_buy`                | Minimum fee on $TPRO token purchase transaction.                              | 0.01 (1%)   |
| `max_fee_buy`                | Maximum fee at the transaction of purchase of $TPRO.                          | 0.05 (5%)   |
| `min_fee_sell`               | Minimum fee at $TPRO sale transaction.                                        | 0.01 (1%)   |
| `max_fee_sell`               | Maximum fee at the $TPRO sale transaction.                                    | 0.1 (10%)   |
| `min_price_tolerance_buy`    | The percentage change in the $TPRO price that triggers the raising of the fee on purchase transactions. | 0.2 (20%)   |
| `max_price_tolerance_buy`    | Percentage change in the price of the $TPRO token, beyond which the maximum fee on purchase transactions. | 0.5 (50%)   |
| `eth_price_min_tolerance`    | Percentage change in the price of ETH that triggers the mechanism for raising fees on sales transactions. | 0.07 (7%)   |
| `eth_price_max_tolerance`    | The percentage change in the price of ETH, after which the maximum sales transaction fee is charged. | 0.15 (15%)  |
| `eth_component_weight`       | The weight of the component reflects the volatility of the ETH price throughout the fee for sale transactions. | 0.5 (50%)   |
| `min_price_tolerance_sell`   | Percentage change in the price of the $TPRO that triggers fee raising on sales transactions. | 0.01 (1%)   |
| `max_price_tolerance_sell`   | The percentage change in the price of the $TPRO, beyond which the maximum fee on sales transactions. | 0.05 (5%)   |
| `protection_fee_rate`        | The portion of the fee that is charged on from the collected fee on protection mechanism. | 0.2 (20%)   |

Based on the above parameters, the mathematical specification of the mechanism is defined.
<a name="mechanism_mathematical_specification_dynamic_fee"></a>
## Mechanisms mathematical specification

The mechanism for dynamic fee adjustment is divided into several components. The first component handles the fee for TPRO purchase transactions. Two key metrics are defined to support the mechanism, calculated using the following formulas:

$$
price_-buy_{min} = avg_-price_-24h_{before} \cdot (1 + min_-price_-tolerance_-buy) 
$$

$$
price_-buy_{max} = avg_-price_-24h_{before} \cdot (1 + max_-price_-tolerance_-buy)
$$

<p align="center"><b>Formula 1. Dynamic Price Thresholds for TPRO Purchase Transactions</b></p>

where:  

- $$price_-buy_{min}$$ is the TPRO price that triggers the raising of the fee on purchase transactions,  
- $$price_-buy_{max}$$ is the price of the TPRO, beyond which the maximum fee on purchase transactions,  
- $$avg_-price_-24h_{before}$$ is the average TPRO price over the last 24 hours preceding the update fee,
- $$min_-price_-tolerance_-buy$$ is a parameter describing the percentage change in the TPRO price that triggers the raising of the fee on purchase transactions,  
- $$max_-price_-tolerance_-buy$$ is a parameter describing the percentage change in the price of the TPRO, beyond which the maximum fee on purchase transactions.  

Based on these key metrics, the fee for TPRO purchase transactions is calculated as follows:

$$
Fee_{buy} = \begin{cases} 
min_-fee_-buy, &\text{if } price_{current} \leq price_-buy_{min} \\ 
max_-fee_-buy, &\text{if } price_{current} \geq price_-buy_{max} \\ 
min_-fee_-buy + \frac{max_-fee_-buy - min_-fee_-buy}{price_-buy_{max} - price_-buy_{min}} \cdot (price_{current} - price_-buy_{min}), &\text{otherwise}.
\end{cases}
$$

<p align="center"><b>Formula 2. Dynamic Fee for TPRO Buying Transactions</b></p>

where:

- $$Fee_{buy}$$ is the fee applied to a TPRO buying transaction,  
- $$min_-fee_-buy$$ is a parameter describing the minimum fee applied to TPRO buying transactions,  
- $$max_-fee_-buy$$ is a parameter describing the maximum fee applied to TPRO buying transactions,  
- $$price_{current}$$ is the current price of the TPRO,  
- $$price_-buy_{min}$$ is the TPRO price that triggers the raising of the fee on purchase transactions,  
- $$price_-buy_{max}$$ is the price of the TPRO, beyond which the maximum fee on purchase transactions.  

The chart below illustrates an example of how the fee value varies with the TPRO price.

![fee_buy_price_component.png](.gitbook%2Fassets%2Ffee_buy_price_component.png)

<p align="center"><b>Figure 1. Buy transaction fee.</b></p>

In the case of the fee for TPRO sales, the mechanism is more elaborate. Its first component works on a similar basis. Based on the parameters and the average price of the previous day, the following metrics are determined:

$$
price_-sell_{min} = avg_-price_-24h_{before} \cdot (1 - min_-price_-tolerance_-sell)
$$

$$
price_-sell_{max} = avg_-price_-24h_{before} \cdot (1 - max_-price_-tolerance_-sell)
$$

<p align="center"><b>Formula 3. Dynamic Price Thresholds for TPRO Sale Transactions</b></p>

where:

- $$price_-sell_{min}$$ is the TPRO price that triggers the raising of the fee on sale transactions,  
- $$price_-sell_{max}$$ is the price of the TPRO, beyond which the maximum fee on purchase transactions,  
- $$avg_-price_-24h_{before}$$ is the average TPRO price over the last 24 hours preceding the update fee,  
- $$min_-price_-tolerance_-sell$$ is a parameter describing percentage change in the price of the TPRO that triggers fee raising on sales transactions,  
- $$max_-price_-tolerance_-sell$$ is a parameter describing the percentage change in the price of the TPRO, below which the maximum fee on sales transactions.  

These key metrics are used to determine the first component of fee.

$$
Fee_{sell}^{price} =
\begin{cases} 
min_-fee_-sell, & \text{if } price_{current} \geq price_-sell_{min} \\ 
max_-fee_-sell, & \text{if } price_{current} \leq price_-sell_{max} \\ 
min_-fee_-sell + \frac{max_-fee_-sell - min_-fee_-sell}{price_-sell_{min} - price_-sell_{max}} \cdot (price_-sell_{min} - price_{current}), & \text{otherwise.}
\end{cases}
$$

<p align="center"><b>Formula 4. Dynamic Fee for TPRO Sale Transactions Based on $TPRO Price</b></p>

where:

- $$Fee_{sell}^{price}$$ is the fee component for TPRO selling transactions based on price,  
- $$min_-fee_-sell$$ is a parameter describing the minimum fee applied to TPRO selling transactions,  
- $$max_-fee_-sell$$ is a parameter describing the maximum fee applied to TPRO selling transactions,  
- $$price_{current}$$ is the current price of the TPRO,  
- $$price_-sell_{min}$$ is the TPRO price that triggers the raising of the fee on sale transactions,  
- $$price_-sell_{max}$$ is the price of the TPRO, beyond which the maximum fee on purchase transactions.  

![fee_sell_price_component.png](.gitbook%2Fassets%2Ffee_sell_price_component.png)

<p align="center"><b>Figure 2. Sale transactions fee (price component)</b></p>

The second component of the fee for selling TPRO is influenced by fluctuations in the ETH price over the past 7 days. To account for this, the fee is calculated based on these price changes as follows:

$$
eth_-price_-adjusted_-fee = 
$$
$$
\left( \frac{average_-eth_-price_-24h}{average_-eth_-price_-7d} - 1 - eth_-price_-max_-tolerance \right) \cdot \frac{max_-fee_-sell - min_-fee_-sell}{eth_-price_-max_-tolerance - eth_-price_-min_-tolerance} + max_-fee_-sell
$$

<p align="center"><b>Formula 5. Fee Adjustment Based on ETH Price</b></p>

where:

- $$eth_-price_-adjusted_-fee$$ is the fee adjusted for TPRO selling transactions based on changes in ETH price,  
- $$average_-eth_-price_-24h$$ is the average ETH price over the last 24 hours,  
- $$average_-eth_-price_-7d$$ is the average ETH price over the last 7 days,  
- $$eth_-price_-max_-tolerance$$ is a parameter describing the maximum tolerance for changes in ETH price,  
- $$eth_-price_-min_-tolerance$$ is a parameter describing the minimum tolerance for changes in ETH price,  
- $$max_-fee_-sell$$ is a parameter describing the maximum fee applied to TPRO selling transactions,  
- $$min_-fee_-sell$$ is a parameter describing the minimum fee applied to TPRO selling transactions.  

This component is subject to specific limitations, outlined in the formula below:

$$
Fee_{sell}^{eth} =
\begin{cases} 
min_-fee_-sell, & \text{if } \frac{average_-eth_-price_-24h}{average_-eth_-price_-7d} - 1 \leq eth_-price_-min_-tolerance, \\  
max_-fee_-sell, & \text{if } \frac{average_-eth_-price_-24h}{average_-eth_-price_-7d} - 1 \geq eth_-price_-max_-tolerance, \\  
eth_-price_-adjusted_-fee, & \text{otherwise.}
\end{cases}
$$

<p align="center"><b>Formula 6. ETH Price Component of the Fee for TPRO Sale Transactions</b></p>

where:

- $$Fee_{sell}^{eth}$$ is the ETH price component of the fee applied to TPRO selling transactions,  
- $$max_-fee_-sell$$ is a parameter describing the maximum fee applied to TPRO selling transactions,  
- $$min_-fee_-sell$$ is a parameter describing the minimum fee applied to TPRO selling transactions,  
- $$average_-eth_-price_-24h$$ is the average ETH price over the last 24 hours,  
- $$average_-eth_-price_-7d$$ is the average ETH price over the last 7 days,  
- $$eth_-price_-max_-tolerance$$ is a parameter describing the maximum tolerance for changes in ETH price,  
- $$eth_-price_-min_-tolerance$$ is a parameter describing the minimum tolerance for changes in ETH price,  
- $$eth_-price_-adjusted_-fee$$ is the fee adjusted for TPRO selling transactions based on changes in ETH price.  

![eth_price_sell_component.png](.gitbook%2Fassets%2Feth_price_sell_component.png)

<p align="center"><b>Figure 3. Sale transactions fee (ETH component)</b></p>

Considering both components, the final fee for TPRO token sales is determined using the following formula:

$$
Fee_{sell} = (1 - eth_-weight) \cdot Fee_{sell}^{price} + eth_-weight \cdot Fee_{sell}^{eth}
$$

<p align="center"><b>Formula 7. Dynamic Fee for TPRO Sale Transactions</b></p>

where:

- $$Fee_{sell}$$ is the fee applied to TPRO selling transactions,  
- $$eth_-component_-weight$$ is a parameter describing the weight of the ETH component throughout the fee for sale transactions,  
- $$Fee_{sell}^{price}$$ is the fee component based on TPRO price changes,  
- $$Fee_{sell}^{eth}$$ is the fee component based on ETH price changes.  

A primary objective of the dynamic fee mechanism is to allocate fees toward the impermanent loss protection mechanism. At a given moment $$t$$, when a purchase transaction occurs, the collected fee is distributed as follows:

$$
fee_-protection_-eth_{t} = protection_-fee_-rate \cdot fee_-collected_-eth_{t}
$$

$$
fee_-lp_-eth_{t} = fee_-collected_-eth_{t} - fee_-protection_-eth_{t}
$$

<p align="center"><b>Formula 8. Fee Allocation for ETH pool in the Protection Mechanism</b></p>

where:

- $$fee_-protection_-eth_{t}$$ is the portion of ETH fees allocated to the protection mechanism pool at time $$t$$,  
- $$protection_-fee_-rate$$ is a parameter describing the portion of the fee that is charged on from the collected fee on protection mechanism,  
- $$fee_-collected_-eth_{t}$$ is the total ETH fees collected at time $$t$$,  
- $$fee_-lp_-eth_{t}$$ is the portion of ETH fees allocated to liquidity providers at time $$t$$.  

A similar mechanism occurs for sales transactions, where a TPRO fee is charged. This division is determined by the following formula:

$$
fee_-protection_-token_{t} = protection_-fee_-rate \cdot fee_-collected_-token_{t}
$$

$$
fee_-lp_-token_{t} = fee_-collected_-token_{t} - fee_-protection_-token_{t}
$$

<p align="center"><b>Formula 9. Fee Allocation for TPRO tokens pool in the Protection Mechanism</b></p>

where:

- $$fee_-protection_-token_{t}$$ is the portion of TPRO fees allocated to the protection mechanism pool at time $$t$$,  
- $$protection_-fee_-rate$$ is a parameter describing the portion of the fee that is charged on from the collected fee on protection mechanism,  
- $$fee_-collected_-token_{t}$$ is the total TPRO fees collected at time $$t$$,  
- $$fee_-lp_-token_{t}$$ is the portion of TPRO fees allocated to liquidity providers at time $$t$$.
  
<a name="impermanent_loss_protection"></a>
# Impermanent Loss Protection Mechanism

The protection mechanism is a key feature of the liquidity pool, offering additional safeguards against impermanent loss for liquidity providers.

**Key aspects of the mechanism include:**

- **Fee-Based Funding**: A portion of the fees collected from the dynamic fee mechanism is allocated to the protection mechanism, ensuring consistent funding for collateral payouts.  
- **Token-Based Participation**: Liquidity providers can deposit additional TPRO tokens into the collateral pool. These deposits represent shares in the pool, and the provider’s participation is determined by the amount of TPRO deposited, regardless of the liquidity they initially contributed.  
- **Flexible Withdrawal with Lock-In Period**: Deposited TPRO tokens can be withdrawn at any time but are subject to a **14-day lock** before they can be collected. Previously accumulated collateral remains intact even after TPRO tokens are withdrawn.  
- **Coverage of Impermanent Loss**: When liquidity is removed, the mechanism calculates impermanent loss as the difference between the value of the liquidity pool and a hold strategy. The collateral is used to offset all or part of the loss, compensating liquidity providers.  
- **Management of Excess Collateral**: If the collateral collected is greater than the calculated loss, the excess is redistributed. A portion of the excess is allocated as a success fee for the creators of the mechanism, while the remainder is returned to the collateral pool to benefit other participants in the program.  

This protection mechanism minimizes exposure to impermanent loss while fostering a collaborative and incentivized environment for liquidity providers. Its efficient handling of surplus collateral and performance-based fee structure ensures alignment between creators and participants, promoting trust and long-term engagement.


<a name="impermanent_loss_protection_machnism_parameters"></a>
## Mechanism parameters

**Table 2. Impermanent Loss Protection Mechanism Parameters**

| **Parameter**  | **Description**                                                                                         | **Value**    |
|-----------------|-------------------------------------------------------------------------------------------------------|-------------|
| `success_fee`  | Part of the unrealized protection for the liquidity provider charged as remuneration on effective protection against impermanent loss. | 0.1 (10%)   |

Based on the above parameters, the mathematical specification of the mechanism is defined.

<a name="impermanent_loss_protection_mathematical_specification"></a>
## Mechanisms mathematical specification

Fees collected by the liquidity pool for the protection mechanism are collected in separate pools of ETH and TPRO tokens. Each time a liquidity provider joins, leaves the protection program, or increases its deposit, the collected funds are redistributed to users to ensure a fair allocation of collateral. We define $$t_i$$ as the moments when the distribution of pool shares changes due to user actions. Let $$t_{i-1}$$ represent the previous moment of such a change, and $$t_0$$ denote the initial moment of the pool's creation. The amount of collected assets is calculated using the following formulas:

$$
protection_-eth_-pool_{t_{i}} = \sum_{t = t_{i-1}}^{t_{i}} fee_-protection_-eth_{t}
$$

$$
protection_-token_-pool_{t_{i}} = \sum_{t = t_{i-1}}^{t_{i}} fee_-protection_-token_{t}
$$

<p align="center"><b>Formula 10. Accumulated Protection Fees in ETH and TPRO</b></p>

where:
- $$protection_-eth_-pool_{t}$$ is the accumulated ETH fees in the protection pool by time $$t$$,  
- $$protection_-token_-pool_{t}$$ is the accumulated TPRO fees in the protection pool by time $$t$$,  
- $$fee_-protection_-eth_{t}$$ is the ETH fee allocated to the protection mechanism pool at time $$t$$,  
- $$fee_-protection_-token_{t}$$ is the TPRO fee allocated to the protection mechanism pool at time $$t$$.  

Both TPRO and ETH are distributed following the same principles. Let $$lp$$ describe a specific liquidity provider, the amount of ETH and TPRO reserved for liquidity provider $$lp$$, at time $$t_{i}$$ describes the formula:

$$
protection_-eth_{t_{i}}^{lp} = \frac{tokens_-deposited^{lp}}{\sum_{k=1}^{N} tokens_-deposited^{k}} \cdot protection_-eth_-pool_{t_{i}}
$$

$$
protection_-token_{t_{i}}^{lp} = \frac{tokens_-deposited^{lp}}{\sum_{k=1}^{N} tokens_-deposited^{k}} \cdot protection_-token_-pool_{t_{i}}
$$

<p align="center"><b>Formula 11. Allocation of Protection Pool to Liquidity Providers</b></p>

where:
- $$protection_-eth_{t}^{lp}$$ is the portion of the ETH protection mechanism pool allocated to the liquidity provider $$lp$$ at time $$t_{i}$$,  
- $$protection_-token_{t}^{lp}$$ is the portion of the TPRO protection mechanism pool allocated to the liquidity provider $$lp$$ at time $$t_{i}$$,  
- $$tokens_-deposited^{lp}$$ is the number of TPRO deposited by the specific liquidity provider,  
- $$tokens_-deposited^{k}$$ is the total number of TPRO deposited by liquidity provider $$k$$,  
- $$protection_-eth_-pool_{t}$$ is the total ETH in the protection pool by time $$t_{i}$$,  
- $$protection_-token_-pool_{t_{i}}$$ is the total TPRO in the protection pool by time $$t_{i}$$,  
- $$N$$ is the total number of liquidity providers with some deposit in the protection mechanism.  

As long as the liquidity provider remains active and the collateral is not utilized, these funds remain secured for the provider and continue to accumulate throughout the entire period their TPRO are locked in deposit.

The withdrawal of TPRO and ETH designated for liquidity providers within the protection mechanism occurs only when the user removes liquidity.

Initially, the portion of liquidity being withdrawn by the user is determined. To achieve this, a variable is calculated to represent the proportion of the user's total liquidity being withdrawn:

$$
withdrawal_-ratio = \frac{lp_-tokens_-withdrawn}{sum_-lp_-tokens}
$$

<p align="center"><b>Formula 12. Withdrawal Ratio for Liquidity Provider</b></p>

where:
- $$withdrawal_-ratio$$ is the proportion of liquidity withdrawn by the liquidity provider,  
- $$lp_-tokens_-withdrawn$$ is the number of liquidity provider tokens withdrawn by the specific liquidity provider,  
- $$sum_-lp_-tokens$$ is the total number of liquidity provider tokens in the pool.
- 
A key step in calculating impermanent loss is estimating the hold position of a given liquidity provider. This involves tracking the assets contributed by the user to the liquidity pool at the time of adding liquidity. When liquidity is withdrawn, the hold position is adjusted by reducing it proportionally to the fraction of liquidity removed. This process is expressed mathematically as follows. Let $$t_{i}$$ denote the moment at which liquidity is added or withdrawn. By $$t_{0}$$ is meant the first addition of liquidity by a liquidity provider, then:

$$
hold_-position_{t_{i}} = 
\begin{cases} 
hold_-position_{t_{i-1}} + eth_-provided_{t_{i}} + tokens_-provided_{t_{i}} \cdot spot_-price_{t_{i}} & \text{if liquidity is added} \\  
(1 - withdrawal_-ratio) \cdot hold_-position_{t_{i-1}} & \text{if liquidity is withdrawn}
\end{cases}
$$

<p align="center"><b>Formula 13. Liquidity Provider's Estimated Hold Position</b></p>

where:
- $$hold_-position_{t}$$ is the total hold position value of the liquidity provider at time $$t_{i}$$, expressed in ETH,  
- $$eth_-provided_{t_{i}}$$ is the amount of ETH provided by the liquidity provider at time $$t_{i}$$,  
- $$tokens_-provided_{t_{i}}$$ is the amount of TPRO provided by the liquidity provider at time $$t_{i}$$,  
- $$spot_-price_{t_{i}}$$ is the price of the TPRO at time $$t_{i}$$, expressed in ETH/TPRO,  
- $$withdrawal_-ratio$$ is the proportion of liquidity withdrawn by the liquidity provider.  

**Note:** the $$hold\_-position$$ is updated after the entire withdrawal mechanism is completed, as its value before the withdrawal is required to determine the amount of funds to be withdrawn by the user.

The next step is to calculate the liquidity provider's position, which takes into account the assets withdrawn from the pool and the fees charged. During the liquidity withdrawal process, the entire fee charged to the liquidity provider is allocated. In addition, the system tracks the total fees paid since the user last withdrew liquidity. The lp position is calculated as follows:

$$
lp_-position = (withdrawn_-tokens + fee_-collected_-tokens) \cdot spot_-price + withdrawn_-eth + fee_-collected_-eth
$$

<p align="center"><b>Formula 14. Liquidity Provider's Position</b></p>

where:

- $$lp_-position$$ is the total value of the liquidity provider's position, expressed in ETH,  
- $$withdrawn_-tokens$$ is the number of TPRO withdrawn by the liquidity provider in current withdrawal,  
- $$fee_-collected_-tokens$$ is the number of TPRO collected as fees by the liquidity provider since the last liquidity withdrawal,  
- $$spot_-price$$ is the current TPRO price, expressed in ETH/TPRO,  
- $$withdrawn_-eth$$ is the amount of ETH withdrawn by the liquidity provider in current withdrawal,  
- $$fee_-collected_-eth$$ is the amount of ETH collected as fees by the liquidity provider since the last liquidity withdrawal.  

Using the estimated hold position and the liquidity provider's position, the loss incurred from providing liquidity, as opposed to simply holding TPRO and ETH, is calculated. This calculation is expressed by the following formula:

$$
loss = 
\begin{cases}
hold_-position * withdrawal_-ratio - lp_-position, & \text{if } lp_-position < hold_-position * withdrawal_-ratio \\
0, & \text{otherwise.}
\end{cases}
$$

<p align="center"><b>Formula 15. Liquidity Provider's Loss</b></p>

where:

- $$loss$$ is the loss incurred by the liquidity provider compared to the hold strategy, expressed in ETH,  
- $$lp_-position$$ is the total value of the liquidity provider's position, expressed in ETH,  
- $$hold_-position$$ is the total hold position value of the liquidity provider, expressed in ETH,  
- $$withdrawal_-ratio$$ is the proportion of liquidity withdrawn by the liquidity provider.  

Once the user's loss is determined, the process of collateral payout begins. This payout occurs only if the calculated loss is greater than zero. A negative loss value indicates that the liquidity provider has not experienced any loss relative to their hold position, and therefore, no compensation is issued. In order to disburse funds, the following metrics are defined:

$$
full_-value_-protection = protection_-eth + protection_-token \cdot spot_-price
$$

$$
value_-protection = full_-value_-protection \cdot withdrawal_-ratio
$$

<p align="center"><b>Formula 16. Value of Protection for Liquidity Provider</b></p>

where:

- $$full_-value_-protection$$ is the total value of protection available for a liquidity provider, expressed in ETH,  
- $$protection_-eth$$ is the ETH allocated as protection,  
- $$protection_-token$$ is the TPRO allocated as protection,  
- $$spot_-price$$ is the price of the TPRO, expressed in ETH/TPRO,  
- $$value_-protection$$ is the final value of protection applied to the liquidity provider's withdrawn liquidity.  

The liquidity provider receives both TPRO and ETH, with the amounts of each asset calculated using the following formula:

$$
received_-protection_-eth =
\begin{cases}
protection_-eth & \text{if } loss \geq full_-value_-protection, \\  
withdrawal_-ratio \cdot protection_-eth \cdot \frac{loss}{value_-protection} & \text{if } loss \leq value_-protection, \\  
protection_-eth \cdot \frac{loss}{full_-value_-protection} & \text{otherwise.}
\end{cases}
$$

$$
received_-protection_-token =
\begin{cases}
protection_-token & \text{if } loss \geq full_-value_-protection, \\  
withdrawal_-ratio \cdot protection_-token \cdot \frac{loss}{value_-protection} & \text{if } loss \leq value_-protection, \\  
protection_-token \cdot \frac{loss}{full_-value_-protection} & \text{otherwise.}
\end{cases}
$$

<p align="center"><b>Formula 17. Received Protection for Liquidity Provider</b></p>
where:

- $$received_-protection_-eth$$ is the amount of ETH received as protection by the liquidity provider,  
- $$received_-protection_-token$$ is the amount of TPRO received as protection by the liquidity provider,  
- $$protection_-eth$$ is the ETH allocated to the liquidity provider for protection,  
- $$protection_-token$$ is the TPRO allocated to the liquidity provider for protection,  
- $$loss$$ is the loss incurred by the liquidity provider compared to the hold strategy, expressed in ETH,  
- $$full_-value_-protection$$ is the total value of protection available for a liquidity provider, expressed in ETH,  
- $$value_-protection$$ is the value of part of protection applied based on the withdrawal ratio.   

Since the mechanism aims to offset the loss relative to the hold position, the amount of TPRO and ETH received may exceed the loss incurred. In such cases, a surplus is calculated, which is then split into a fee for the creators and a contribution to the protection pool. The surplus for both TPRO and ETH is determined using the following formula:

$$
protection_-surplus_-eth =
\begin{cases}
withdrawal_-ratio \cdot protection_-eth & \text{if } loss = 0, \\
withdrawal_-ratio \cdot protection_-eth \cdot \frac{1 - loss}{value_-protection} & \text{if } loss \leq value_-protection, \\
0 & \text{otherwise.}
\end{cases}
$$

$$
protection_-surplus_-token =
\begin{cases}
withdrawal_-ratio \cdot protection_-token & \text{if } loss = 0, \\
withdrawal_-ratio \cdot protection_-token \cdot \frac{1 - loss}{value_-protection} & \text{if } loss \leq value_-protection, \\
0 & \text{otherwise.}
\end{cases}
$$

<p align="center"><b>Formula 18. Protection Surplus</b></p>

where:

- $$protection_-surplus_-eth$$ is the surplus ETH that remains after covering the loss,  
- $$protection_-surplus_-token$$ is the surplus TPRO that remains after covering the loss,  
- $$protection_-eth$$ is the ETH allocated to the liquidity provider for protection,  
- $$protection_-token$$ is the TPRO allocated to the liquidity provider for protection,  
- $$loss$$ is the loss incurred relative to the hold position,  
- $$value_-protection$$ is the value of protection applied based on the withdrawal ratio,  
- $$full_-value_-protection$$ is the total value of protection available for a liquidity provider, expressed in ETH,  
- $$withdrawal_-ratio$$ is the proportion of liquidity withdrawn by the liquidity provider.  

Based on the surplus, a fee for the TPRO Network is calculated. The calculation is performed as follows:
$$
success_-fee_-eth = protection_-surplus_-eth \cdot success_-fee
$$

$$
success_-fee_-token = protection_-surplus_-token \cdot success_-fee
$$

<p align="center"><b>Formula 19. Success Fee</b></p>

where:

- $$success_-fee_-eth$$ is the success fee in ETH deducted from the protection surplus,  
- $$success_-fee_-token$$ is the success fee in TPRO deducted from the protection surplus,  
- $$protection_-surplus_-eth$$ is the surplus ETH that remains after covering the loss,  
- $$protection_-surplus_-token$$ is the surplus TPRO that remains after covering the loss,  
- $$success_-fee$$ is the parameter describing the percentage of the protection surplus taken as a fee.

The remaining surplus is allocated to two pools within the protection mechanism and will be distributed among all liquidity providers in the future. Fee for protection pools from surplus is calculated as follows:
$$
fee_-protection_-eth = protection_-surplus_-eth - success_-fee_-eth
$$

$$
fee_-protection_-token = protection_-surplus_-token - success_-fee_-token
$$

<p align="center"><b>Formula 20. Surplus Protection Fee</b></p>

where:
- $$fee_-protection_-eth$$ is the ETH fee allocated to the protection mechanism pool,  
- $$fee_-protection_-token$$ is the TPRO fee allocated to the protection mechanism pool,  
- $$protection_-surplus_-eth$$ is the surplus ETH that remains after covering the loss,  
- $$protection_-surplus_-token$$ is the surplus TPRO that remains after covering the loss,  
- $$success_-fee_-eth$$ is the success fee in ETH deducted from the protection surplus,  
- $$success_-fee_-token$$ is the success fee in TPRO deducted from the protection surplus.  

From the funds allocated to the liquidity provider in the protection mechanism are deducted $$received_-protection_-eth, received_-protection_-token, protection_-surplus_-eth, protection_-surplus_-token$$.
