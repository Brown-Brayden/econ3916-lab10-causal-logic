# econ3916-lab10-causal-logic
# Correlation & Causality: Confounders, Colliders and the Limits of Controls

## Objective
I wanted to show that the same correlation can come from causation, confounding or selection, and that the controls I choose decide whether a regression estimate gets closer to the truth or further from it.

## Methodology
- I simulated three worlds with known truths: D causes Y (effect 2.0), a hidden confounder drives both (effect 0), and a selected sample of two independent variables (effect 0). I compared their correlations and scatterplots.
- I drew a confounder, a collider and a mediator as directed graphs with networkx.
- I simulated a confounder and regressed Y on D with and without it as a control.
- I simulated a collider, where D and Y are independent but both cause C, and regressed Y on D with and without C. I also looked at the D–Y correlation inside a narrow band of C to see where the false association comes from.
- I built an M-bias structure, D ← u1 → W ← u2 → Y, in which W is measured before treatment, and regressed Y on D with and without W.
- I wrote a function, `coef_on_d()`, that regresses an outcome on D plus any list of controls and returns the coefficient on D. I used it to control for W and u1 together.
- I reproduced the LaLonde (1986) contrast on the Dehejia and Wahba NSW data. I compared the randomised experiment with a comparison group from the CPS, then added eight covariates and measured how much of the gap they closed.
- For the take-home challenge, I wrote the full DAG in networkx, listed and classified every path between D and Y, and searched for the smallest set of controls that blocks every backdoor path.

## Key Findings
- The three worlds gave correlations of 0.654, 0.664 and 0.632, even though only the first has a causal effect. Nothing in the data or the plots separates them.
- Controlling for the collider gave an estimate of −0.592, with a p-value below 0.0005, where the true effect is 0.
- Controlling for the pre-treatment variable W gave −0.191 where the truth is 0. Also controlling for u1 brought it back to 0.006, because u1 blocks the path that conditioning on W opened. So "control for anything measured before treatment" is not a safe rule.
- In LaLonde, the experiment gave $1,794. The naive CPS comparison gave −$8,498. With controls, the estimate was $699, which closed 89% of the gap. Getting closer to the benchmark does not prove the adjustment is right, and without an experiment I would have had nothing to check it against.
- In the challenge, the smallest valid adjustment set was {U}. The graph made the choice mechanical, but only after I had assumed which arrows exist, and no data can confirm that.
