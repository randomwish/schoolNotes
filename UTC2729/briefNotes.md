//Very scuffed, brief notes
//Will probably clean it up after finals or something

**Causality (UTC 2729) AY23/24 S2**

**Heavily adopted from Causal Inference: The Mixtape (Cunningham, 2021) https://mixtape.scunning.com/**

**Directed Acyclic graphs**

Describe all causal relationships relevant to the effect of D on Y
Graphical representation of a chain of causal effects
Arrows -> causal effect b between two random variables
D -> Y (direct)
D -> X -> Y (mediator), capturing a sequence of events originating with D
Backdoor path - D <- X -> Y (confounder), as X jointly determines D and Y, 

If there are no colliders along backdoor paths, then it is an open backdoor path (introduces bias and creates non causal correlation)
To close open backdoor path: 1) condition on confounder 2) create a collider along the backdoor path
No backdoor paths -> satisfies the backdoor criterion that isolated a causal effect


D -> X <- Y (Collider) -> closes a backdoor path
Should not control for colliders, as it may open up a backdoor path

Collider bias -> bad controls and selection bias
Bad controls -> situation in which the outcome had been a collider linking treatment to the outcome of interest
eg D → O ← A → Y

5 methods for causation:
1. Method of agreement
2. Method of difference (similar to idea of causation and comparison among counterfactuals)
3. Joint method
4. Method of concomitant variation
5. Method of residuals

Potential outcomes -> causal effect defined as a comparison between two states of the world: actual vs counterfactual; expresses causality in terms of counterfactuals

Treat i for a unit Y
Y^0 -> potential outcome of unit not receiving treatment
Y^1 -> potential outcome of unit I receiving treatment
Switchinng equation - Y_i = D_iY_i^1 + (1 - D_i)y_i^0
Where D_i is 1 if unit receives treatment and 0 if it does not
Y_i = Y_i^1 if unit receives treatment
Y_i = Y_i^0 if unit does not receive treatment
Causal effect = Y_i^1 - Y_i^0

Average treatment effect ATE -> expected value of causal effect (E[Y_i^1 - Y_i^0]), can be estimated
Can also be ATE = p * ATT + (1 - p) * ATU
Average treatment effect for the treatment group (ATT) -> population mean treatment effect for the group of units assigned the treatment in the first place
ATT = E[theta_i | D_i = 1] = E[Y_i^1 | D_i = 1] - E[Y_i^0 | D_i = 1]
Average treatment effect for the control group ATU -> population mean treatment effect for units assigned into the control group
ATT = E[theta_i | D_i = 0] = E[Y_i^1 | D_i = 0] - E[Y_i^0 | D_i = 0]

Simple difference in means -> compare the average actualised outcomes between two groups
E[Y^1 | D = 1] - E[Y^= | D = 0]

Selection bias -> inherent bias between the treated and untreated groups if both groups did not receive a treatment

Heterogenous treatment effect bias - different returns one can obtained from the treatment group for the 2 groups * share of population that is in the untreated group

Independence assumption -> treatment was given randomly to two parties with no regard to potential outcomes for an individual
Check for independence -> mean potential outcome for both treated and untreated group is the same for both groups, if so, can eliminate both selection bias and heterogenous treatment effect bias
Independence -> 2 groups of units have the same potential outcome on average in the population
E[Y^1 | D=1] - E[Y^1 | D = 0] = 0
E[Y^0 | D = 1] - E[Y^0 | D = 0] = 0

Stable unit treatment value assumption -> assumption underlying the potential outcome framework, such as each treated unit getting the same homogenous doses 

Randomization inference -> standard errors are justified, uncomfortable appealing to large sample properties of an estimator, “aesthetic prefernec” for placebo-based inference
Fisher’s sharp null -> no unit in the data had a causal effect


**Matching**

**Conditional independence assumption** -> randomisation occured only conditional on observable characteristics (expected VALUES OF y^1 AND y^0 are equal for treatment and control group for each value of X)
CIA -> condition strategy satisfies backdoor criterion and is a situation of selection on observables
Covariate -> a variable that satisfies the backdoor criterion, and is usually a random variable assigned to individual units prior to treatment (MUST NOT BE a collider)

To estimate a causal effect, need 
1) CIA and
2) probability of treatment to be between 0 and 1 for each strata (notion of common support)

Example: Whether or not being in first class made someone more likely to survive
Potential confounder: women and children are more likely to be in firs class
Solution: use age and gender to close backdoor paths and satisfy backdoor criterion

Curse of dimensionality -> if we condition on too specific a strata, may not have the specific examples required (specific age, gender and class of room)

Matching -> estimate outcome by inputting potential outcomes by conditioning on coufnding, observed covariate (filling in missing potential outcome for each treatment unit using the closest control group unit to the treatment group unit for a confounder variable -> extrapolation from ATT)
Notion of exchangeable -> covariate is balanced (mean of the covariates are the same)


**Regression Discontinuity**

Eliminate selection bias with an assignment variable X, an observable confounder
Estimate potential outcomes change smoothly as a function of the running variable through cutoff; only thing that causes the outcome to change abruptly at the cutoff variable is the treatment
Causal effects can be identified through units whose score is in a close neighbourhood around some cutoff
Compare people above the cuts off and below the cut off, then estimate the local average treatment effect as we do not have common support (extrapolation required)
Running variable -> refers to the assignment variable itself (has to be quantitative)

Sharp RDD - probability of treatment goes from 0 to 1 at the cutoff; treatment is deterministic
Sharp RDD estimation can be interpreted as the average causal effect of the treatment as the running variable approaches cutoff in the limit

Fuzzy RDD - probability of treatment discontinuously increases at cutoff; treatment is probabilistic 

Local Average Treatment Effect -> local because it is only for running variables near the cutoff

Continuity assumption -> expected outcomes would be continuous even across the cutoff threshold if there had been no treatment (rules out omitted variable bias at the cut off)

Challenges -> 
violations to RDD include:

1. Assignment rule known in advance
2. Agents are interested in adjusting
3. Agents have time to adjust
4. Cutoff is endogenous to factors that independently cause potential outcomes to shift
5. Non random distribution of the running variable

**Instrumental Variables**

Typically used to address omitted variable bias, measurement error and simultaneity
Given a backdoor path between D and Y, U, which is assumed to be unobserved
D <- U -> Y and Z -> D -> Y
When Z varies, D varies, which causes Y to change. 
“Y is only varying because D has varied -> Z affects Y only through D

Hypothetical situation -> D consists of people making choices, and sometimes these choices affect Y and sometimes these are merely correlated with changes in Y due to unobserved changes in U. Z then induces a shock to some people in D to make different decisions. Then, we can observe the causal effect of D on Y for this subgroup of people (“compliers”)
Ideas: 1) if treatment effects are heterogenous, Z shock would only identify some causal effect of D on Y (only know effect of compliers)
2. If Z is inducing some of the change in Y via a portion, we have lesser data to identify the causal effect (larger standard errors)
Requirements:

1. Exclusion restriction path -> instrumental variable only affects exposure of interest
2. Z is correlated with D and thus correlated to Y. (Z to D is known as first stage, D to Y is second stage, Z to Y is known as reduced form)

Homogenous treatment effect - treatment has same causal effect for everyone
Heterogeneous treatment effect - treatment effects can differ across the population

To satisfy a valid IV strategy:
Stable unit treatment value assumption -> potential outcomes for each person are unrelated to treatment status of other individuals
Independence assumption -> “as good as random assignment” assumption (IV is independent of potential outcomes and potential treatment assignments)
Exclusion restriction -> any effect of Z on Y must be via effect of Z on D
First stage -> Z has to have a statistically significant effect on average probability of treatment
Monotoicity assumption -> Instrument variable operate in same direction on all individual unit (so as to guarantee a weighted average of the underlying causal effects of the affected group

Local average treatment effect - average causal effect of D on Y for those whose treatment status was changed by instrument Z (only measures causal effect of compliers in heterogenous treatment effects)
For homogenous treatment effects, compliers would have had the same treatment effects as non compliers

Popular IV design: Lottery, Judge fixed effects design, Bartik Instruments
Lottery - randomised trials - whether one was offered treatments


**Differences in differences design**

Idea is to find an instance where a consequential treatment was given to some people but denied to others -> also known as a natural experiment because it is based on naturally occurring variation in a treatment variable that affects only some units over timem

Why not ATE -> ATE only works if treatment had been randomised, but choices made are endogenous to potential outcomes (people would choose what is most beneficial for them)

Works on 2 pertinent differences: simple before-and-after difference (eliminates unit-specific fixed effects), and then difference the differences from the two groups (hence the term, difference in differences)

Only works with parallel trends assumption -> idea that there are no unobserved factors that could also lead to a change in the outcome, and that the only change is in the intervention to a variable

Why parallel -> DID would simplify to ATT iff the expected difference in outcome for both groups (treated and non treated) over a period of time is zero had both groups not been treated (arisen from selection bias)

Triple differences -> inclusion of variable-specific time change to the original “generic” intervention change

Twoway fixed effects with differential timing -> units receive treatments at different points in time
