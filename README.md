**Modeling Football League Results (A study on 2000-2001 Serie-A Data)**


**Introduction:**

The surge in interest surrounding football outcome modeling lacks a comprehensive computational tool to seamlessly fit various football models. Addressing this gap, the footBayes package aims to provide a unified solution enabling the fitting, interpretation, and visual exploration of diverse goal-based Bayesian football models. Leveraging the Stan environment (Stan Development Team (2020)), this tool facilitates the seamless execution of goal-based Bayesian models within a singular framework.

I have downloaded the package from the following link:
https://github.com/LeoEgidi/footBayes

I propose to perform the following fits and checks for analysis and posterior predictive checks:

I. **Fitting static maximum likelihood and Bayesian models**

II. **Changing the prior distributions and perform some sensitivity tests**

III. **Fitting dynamic Bayesian models**

IV. **Model Checks**

V. **Predictions and Predictive Accuracy**

VI. **Comparing models**


**Distributions for Modelling:**

We are considering 2 distributions. One being the Double Poisson and the other one being Bivariate Poisson. One major concern with the Double Poisson model is that the goals scored during a match by two competing teams are conditionally independent. But, in team sports, such as football, handball, hockey, and basketball it is reasonable to assume that the two outcome variables are correlated since the two teams interact during the game. Lets say, in the practical football match case of the home team leading with 1-0 or 2-0, when only ten minutes are left to play. The away team can then become more determined and can take more risk in an effort to score and achieve the draw within the end of the match. Or, even when one of the two teams is leading say with 3-0, or 4-0, it is likely that team will be relaxing a bit, and the opposing team could score at least one goal quite easily. For this, goals’ correlation due to a change in the performance of the team or both teams could be captured by a dependence parameter, accounting for positive correlation. Positive parametric goals’ dependence is made possible by using a bivariate Poisson distribution.

**Fitting Static Maximum Likelihood and Bayesian Models**

To fit and interpret the models, and we’ll mainly focus on the bivariate Poisson case.
Classical estimates for BP models are provided, among the others, by Karlis and Ntzoufras (2003) (MLE
through an EM algorithm) and Koopman and Lit (2015).

**STATIC FIT:**

We are currently ignoring any time-dependence in our parameters, considering them to be static across distinct match-
times. Initially, we use a static Bivariate-Poisson distribution along with Monte-Carlo Sampling. A positive influence was
observed for the home, sigma_att, sigma_def.

As we could expect, there is a positive effect from the home-effect (posterior mean about 0.3), and this implies that if
two teams are equally good (meaning that their attack and defence abilities almost coincide), assuming that the
constant intercept
, we get that the average number of goals for the home-team will be λ1=exp{0.3}≈1.35,
against λ2=exp{0}=1
μ≈0
We fit the same model under the MLE approach with Wald-type confidence intervals. We can then print the MLE
estimates.

**Fitting Dynamic Bayesian Models**

Teams’ performance tend to be dynamic and change across different years, if not different weeks. Many factors contribute to this football aspect:
1. Teams act during the summer/winter players’ transfermarket, by dramatically changing their rosters.
2. Some teams’ players could be injured in some periods, by affecting the global quality of the team in some
matches.
3. Coaches could be dismissed from their teams due to some non satisfactory results. 4. Some teams could improve/worsen their attitudes due to the so-called turnover.
We use the dynamic_type argument in the stan_foot function, with possible options 'seasonal' or 'weekly' in order to consider more seasons or more week-times within a single season, respectively. Let’s fit a weekly-dynamic parameters model on the Serie A 2000/2001 season.


**Predictive Intervals for team-specific Football Abilities:**

The function foot_abilities allows to depict posterior/confidence intervals for global attack and defense abilities on the
considered data (attack abilities are plotted in red, whereas defense abilities in blue colors).
higher the attack and the lower the defence for a given team, and the better is the overall team’s strength.
AS Roma, the team winning the Serie A 2000/2001, is associated with the highest attack ability and the lowest defence ability according to both the models. In general, the models seem to well capture the static abilities: AS Roma, Lazio Roma and Juventus (1st, 3rd and 2nd at the end of that season, respectively) are rated as the best teams in terms of their abilities, whereas AS Bari, SSC Napoli and Vicenza Calcio (all relegated at the end of the season) have the worst abilities.
We can also depict the team-specific dynamic plots for the dynamic models.

The aggregated goal difference frequencies seem to be decently captured by the model’s replications: in the first plot, blue horizontal lines denote the observed goal differences frequencies registered in the dataset, whereas yellow jittered points denote the correspondent replications. Goal-difference of 0, corresponding to the draws occurrences, is only slightly underestimated by the model. However, in general there are no particular clues of model’s misfit.In the second plot, the ordered observed goal differences are plotted against their replications (50% and 95% credible intervals), and from this plot also we do not have particular signs of model’s misfits.

**Predictions and Predictive Accuracy**

The hottest feature in sports analytics is to obtain future predictions. By considering the posterior predictive
distribution for future and observable data we acknowledge the whole model’s prediction uncertainty (which propagates from the posterior model’s uncertainty) and we can then generate observable values conditioned on the posterior model’s parameters estimates.
We may predict test matches by using the argument predict of the stan_foot function, for instance considering the last four weeks of the 2000/2001 season as the test set, and then computing posterior-results probabilities using the function foot_prob for two teams belonging to the test set, such as Reggina Calcio and AC Milan:
Darker regions are associated with higher posterior probabilities, whereas the red square corresponds to the actual observed result, 2-1 for Reggina Calcio. This final observed result had about a 5% probability to happen! While other results had good predictions, this match didn’t get the observed result.

**Home Win Probabilities / Rank-League Reconstruction**

We can also use the out-of-sample posterior-results probabilities to compute some aggregated home/draw/loss probabilities for a given match.

Red cells denote more likely home-wins (close to 0.6), such as: Lazio Roma - Fiorentina (observed result: 3-0, home win),
Lazio Roma - Udinese (observed result: 3-1, home win), Juventus - AC Perugia (observed result: 1-0, home win), Brescia
Calcio - AS Bari (observed result: 3-1, home win). Conversely, lighter cells denote more likely away wins (close to 0.6),
such as: AS Bari - AS Roma (observed result: 1-4, away win), AS Bari - Inter (observed result: 1-2, away win).
However, predicting the final rank position (along with the teams’ points) is often assimilated to an oracle, rather than a
coherent statistical procedure:
rank-league reconstruction for the first model fit1_stan by using the in-sample replications (yellow ribbons for the credible intervals, solid blue lines for the observed cumulated points).

Rank-league prediction (aggregated or at team-level) for the fourth model fit4_stan by using the out-of-
sample replications.
credible intervals, solid blue lines for the observed cumulated points).

We can clearly observe that, Fit4_stan has better predictive ability than the Fit1_stan model


**MODEL COMPARISONS**

loo_compare(loo1, loo1_t, loo2, loo3_t)
elpd_diff se_diff

model1  0.0 0.0

model2 -0.4 0.5

model3 -4.5 2.6

model4 -8.8 4.2

According to the above model LOOIC comparisons, the weekly-dynamic double Poisson models attain the lowest LOOIC values and are then the favored models in terms of predictive accuracy.

**CONCLUSION**

The static model’s fit1_stan final looic is suggesting that the assumption of static team-specific parameters is too restrictive and oversimplified to capture teams’ skills over time and make reliable predictions. Anyway, from model checking we have the suggestion that even the static model has a reliable goodness of fit and could be used for some simplified analysis not requiring complex dynamic patterns.

The dynamic model’s fit4-stan, that is, the model 4 has the best predictive capability and its weekly-dynamic fit allows it to do so. It is based out of the double-poisson model and uses Monte Carlo Sampling.
