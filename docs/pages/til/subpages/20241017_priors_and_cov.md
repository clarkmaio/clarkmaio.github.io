---
hide:
    - navigation
    - toc
---

# Priors and CoV
One of the most difficult aspect when dealing with Bayesian models is the choice of priors.

Infact it is never clear which priors you should adopt and different priors can lead to very different results.

I've found out that one can actually follow a simple procedure to find the "best" priors given (if any) the informations one has about them.

<br>
**But what does it mean that a prior is the best one?**

By **best** I mean that it is rapresentative exactly of the informations you have, nothing more, nothing less.

Namely one would inject in the priors a precise information and avoi choosing a distribution that, implicitly, bring additional, not wanted informations.

For example if I am looking for a prior to rapresent a standard deviation and I only know that it must be a positive value then choosing as prior an uniform distribution $U(0,10)$ means that I am implicitly injecting an additional costraint $\sigma<10$

## Entropy



## Minimization problem