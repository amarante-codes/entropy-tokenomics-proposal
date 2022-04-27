# Entropy Tokenomics + Keeper Incentives

## Entropy Keeper Rewards Program
__How to get rewarded for decentralizing Entropy infrastructure__

Keepers who are approved to participate in the Entropy Keeper Rewards Program are eligible to receive an airdrop from a pool of tokens held by Friktion DAO as part of the Keeper Airdrop. In order to receive this airdrop, each eligible Keeper must meet certain criteria, which are polled regularly.

## Keeper Airdrop

The Keeper Airdrop is a one-time grant of Entropy Tokens to Entropy Keeper Rewards Program participants. The Keeper Airdrop will distribute 2% (two-percent) of the Total Entropy Token Treasury. The Total Entropy Token Treasury is 100,000,000 Entropy Tokens. The total tokens for the Keeper Airdrop are 2,000,000 Entropy Tokens. In the event the Total Entropy Token Treasury is greater or smaller than the above 100,000,000 Entropy Tokens, the Keeper Airdrop shall remain 2% of the final Total Entropy Token Treasury.

| Element                      | Value        | Denomination    |
|------------------------------|--------------|-----------------|
| Total Entropy Token Treasury | 100,000,000  | Entropy Tokens  |
| Keeper Airdrop Share         | 2%           |   |
| Keeper Airdrop               | 2,000,000    |  Entropy Tokens |

The Keeper Airdrop will occur with the Entropy Token Airdrop occurs.

## Rewards Eligibility

A Keeper must meet all Rewards Participation Criteria to receive a pro-rata Keeper Airdrop from the Friktion DAO of Entropy Token. If for any polling period, any of the criteria are no longer met, the keeper will be excluded from the Reward Calculation until the keeper meets all the criteria, at which point the keeper will be included in the Reward Calculation again.

A Keeper that meets the Rewards Participation Criteria and the Bonus Award Criteria will also receive a bonus in the Rewards Calculation. If for any polling period, the Bonus Award Criteria is met, the keeper will receive a bonus in the Rewards Calculation.

## Rewards Participation Criteria

The polling period for Rewards Participation Criteria evaluation is every 10 minutes. In each polling period a Keeper must confirm transactions of various intruction types. Each instruction type has required volumes to meet the criteria per the following table:

| instruction type | Minimum Transactions Per Polling Period |
|------------------|-----------------------------------------|
| ConsumeEvents    | 200 |
| CachePrices      |  60 |
| CacheRootBanks   |  60 |
| CachePerpMarkets |  60 |
| UpdateFunding    |  60 |  
| UpdateRootBank   |  60 |

## Bonus Award Critera

In any polling period where a Keeper confirms the Bonus Minimum Transactions Per Polling Period for the required instruction types.

| instruction type | Bonus Minimum Transactions Per Polling Period |
|------------------|-----------------------------------------|
| ConsumeEvents    | 541 |

## Rewards Calculation

```%python

from datetime import date
from dateutil.relativedelta import *

ENTROPY_TOKEN_AIRDROP = 100000000

KEEPER_AIRDROP_SHARE = 0.02

PROGRAM_LENGTH_IN_MONTHS = 24
PROGRAM_START_DATE = date(2022, 4, 1)

PROGRAM_MONTH = {keys=list(range(PROGRAM_LENGTH_IN_MONTHS)),
                 values=[PROGRAM_START_DATE + relativedelta(months=x) for x in range(PROGRAM_LENGTH_IN_MONTHS)]
                }

# {month : rate}
EMISSION_RATE = { 0: 0.12,
                  1: 0.12,
                  2: 0.10,
                  3: 0.10,
                  4: 0.08,
                  5: 0.07,
                  6: 0.06,
                  7: 0.05,
                  8: 0.04,
                  9: 0.03,
                  10: 0.03,
                  11: 0.03,
                  12: 0.03,
                  13: 0.02,
                  14: 0.02,
                  15: 0.02,
                  16: 0.01,
                  17: 0.01,
                  18: 0.01,
                  19: 0.01,
                  20: 0.01,
                  21: 0.01,
                  22: 0.01,
                  23: 0.01
}

def days_in_month(date):
    import calendar
    calendar.monthrange(date.year, date.month)[1]

rewards_per_day = [(EMISSION_RATE[x] * KEEPER_AIRDROP_SHARE * ENTROPY_TOKEN_AIRDROP)/days_in_month(PROGRAM_MONTH[x]) for x in range(PROGRAM_LENGTH_IN_MONTHS)]

# Rewards scale with time since last confirmed txn of the same instruction type
#
# time 1: ConsumeEvents
#
# time 3: ConsumeEvents
#

rewards = (time_confirmed - time_previous_confirmed) / (minutes_in_a_day) * rewards_per_day[current_program_month] * participation_criteria_flag

```
