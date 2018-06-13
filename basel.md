# IRB(Internal Rating Based Method)
## A. Overview
## B. Mechanics of the IRB
1. Categorisation of exposures
    - Corporate
        - SL(Special Lending)
            - project finance
            - object finance
            - commodities
            - income-producing real estate
            - high-volatiltiy commercial real estate(HVCRE)
    - Sovereign
    - Bank
    - Retail
    - Qualifying revolving retail
    - Equity
    - Eligible purchased receivables
2. Fundation and advanced approches
    - Corporate and Bank exposures
        - Foundation: only estimate PD
        - Advanced : PD, LGD, EAD, M (SL in particular HVCRE is exception)
    - Retail exposures
    - Equity exposures
    - Elligible purchased receivables
## C. Rules for corporate and bank exposures
1. RWA for corporate bank exposure
    - RWA function for corporate
    - SME adjustment
    - RW for Special Lending(SL)
        - RW for PF, OF, CF, IPRE
        - RW for HVCRE
2. Risk components
- PD
    - 1Y PD for corporate and bank exposure associated with internal borrower grade.
    - PD >= 0.05.
- LGD
    - Fundation:
        LGD = LGD_U (E_U/{E(1+H_E)}) + LGD_S (E_S/{E(1+H_E)}) 
        - E : current exposure
        - E_S : collaterarl 
        - E_U : E_U = E * (1 + H_E) - E_s.
        - LGD_U : unsecured LGD 
        - LGD_S : secured LGD
    - Advanced:
        - Floor = LGD_U,Floor (E_U/{E(1+H_E)}) + LGD_S,Floor (E_S/{E(1+H_E)}) 
    - Advanced is forbidden for some entity
- EAD
    - Foudation:
    - Advanced
- M
    - Foundation: Repo 0.5Y, Other 2.5Y
    - 1Y < M < 5Y
    - M= \sum_t t CF_t / \sum CF_t
    - Cosevative: max remaing time(in years)
    - For derivatives in netting set: weighed averate matruity (inner product with notional)
## D. Rules for retail exposures
1. RWA for retail exposure
    - mortgate: corr = 0.15, K: no adjust
    - Qualyfing revolving retail: cor = 0.04, no adj
    - Other retail: cor = 0.03 A + 0.16 (1 - A), A = (1-e^-35PD) / (1-e^-35)
2. Risk components

## F. Rules for Purchased receivables
1. RWA for default risk
    - Purchased retail receivables
    - Purchased corporate receivables
        - FIRB: RW function with LGD = 40%
        - AIRB
2. RWA for dilution risk
## G. Treatment of expected losses and recognition of provision
## H. Minimum requirements for IRB


