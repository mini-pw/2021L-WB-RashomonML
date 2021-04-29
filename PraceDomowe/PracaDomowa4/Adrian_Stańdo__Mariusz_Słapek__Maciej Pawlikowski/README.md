## Reproduction summary
We weren't able to reproduce the same results mainly because of few reasons:
- Hyperparameters stated in the raport didn't correspond to those in attached code. Some models had completely different values, others weren't even mentioned in the report.
- Creators of original raport didn't set random_state, what might have caused models to learn on different sets.
- We encountered few problems during preprocessing phase, which required us to modify the code before running it.
