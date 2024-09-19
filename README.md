# MAST30027 Assignment 3

## MAST30027: Modern Applied Statistics

**Assignment 3, 2024.**

**Due**: 11:59pm Monday September 23rd

- This assignment is worth 12% of your total mark.
- To get full marks, show your working including 1) R commands and outputs you use, 2) mathematics derivation, and 3) rigorous explanation why you reach conclusions or answers. If you just provide final answers, you will get zero mark.
- The assignment you hand in must be typed (except for math formulas), and be submitted using LMS as a single PDF document only (no other formats allowed). For math formulas, you can take a picture of them. Your answers must be clearly numbered and in the same order as the assignment questions.
- The LMS will not accept late submissions. It is your responsibility to ensure that your assignments are submitted correctly and on time, and problems with online submissions are not a valid excuse for submitting a late or incorrect version of an assignment.
- We will mark a selected set of problems. We will select problems worth ≥ 50% of the full marks listed.
- If you need an extension, please contact the lecturer before the due date with appropriate justification and supporting documents. Late assignments will only be accepted if you have obtained an extension from the lecturer before the due date. To ensure that the lecturer responds to your extension request email before the due date, please contact 24h before the due date. Under no circumstances an assignment will be marked if solutions for it have been released.
- Also, please read the “Assessments” section in “Subject Overview” page of the LMS.

### 1. The file `assignment3_2024_prob1.txt` contains 300 observations. We can read the observations and make a histogram as follows:
```R
> X <- scan(file="assignment3_2024_prob1.txt", what=double())
Read 300 items
> length(X)
[1] 300
> hist(X, breaks=0:max(X))
```
We will model the observed data using a mixture of three negative binomial distributions. Specifically, we assume that the observations $X_1,..., X_{300}$ are independent of each other, and each $X_i$ follows the mixture model
$$
Z_i \sim Categorical(\pi_1, \pi_2, 1 - \pi_1 - \pi_2),
$$
$$
X_i|Z_i = 1 \sim NegBinomial(r, p_1),
$$
$$
X_i|Z_i = 2 \sim NegBinomial(r, p_2),
$$
$$
X_i|Z_i = 3 \sim NegBinomial(r, p_3),
$$
where $r$ is some known constant. The negative binomial distribution has the probability mass function
$$
f(x; r, p) = \binom{x + r - 1}{r - 1}p^r(1 - p)^x.
$$
We aim to obtain MLE of the parameters $\theta = (\pi_1, \pi_2, p_1, p_2, p_3)$ using the EM algorithm.

(a) (5 marks) Let $X = (X_1,..., X_{300})$ and $Z = (Z_1,..., Z_{300})$. Derive the expectation of the complete log - likelihood, $Q(\theta, \theta^0) = E_{Z|X, \theta^0}[\log(P(X, Z|\theta))]$.
(b) (3 marks) Derive E - step of the EM algorithm.
(c) (5 marks) Derive M - step of the EM algorithm.
(d) (5 marks) Note: Your answer for this problem should be typed. Answers including screen - captured R codes or figures won’t be marked.
Implement the EM algorithm and obtain MLE of the parameters by applying the implemented algorithm to the observed data, $X_1,..., X_{300}$, assuming a value of $r = 10$. Terminate the algorithm when the number of EM iterations reaches 500, or when the incomplete log - likelihood has changed by less than $10^{-5}$ ($\epsilon = 10^{-5}$). Run the EM algorithm twice with the following two different initial values, and report the parameter estimates corresponding to the highest incomplete log - likelihood.
|$\pi_1$|$\pi_2$|$p_1$|$p_2$|$p_3$|
|---|---|---|---|---|
|1st initial values|0.1|0.3|0.1|0.8|0.5|
|2nd initial values|0.2|0.4|0.3|0.2|0.7|
For each EM run, check that the incomplete log - likelihoods increase at each EM - iteration by plotting them.

### 2. The file `assignment3_2024_prob2.txt` contains 100 additional observations. We can read the 300 obser - vations from the Problem 1 and the new 100 observations and make histograms as follows:
```R
> X <- scan(file="assignment3_2024_prob1.txt", what=double())
Read 300 items
> X.more <- scan(file="assignment3_2024_prob2.txt", what=double())
Read 100 items
> length(X)
[1] 300
> length(X.more)
[1] 100
> xgrid <- 0:max(c(X, X.more))
> par(mfrow=c(2, 2))
> hist(X, breaks=xgrid, ylim=c(0, 21), main="Histogram of X", xlab="X")
> hist(X.more, breaks=xgrid, ylim=c(0, 21), main="Histogram of X.more", xlab="X.more")
> hist(c(X, X.more), breaks=xgrid, ylim=c(0, 21), main="Histogram of X & X.more", xlab="X & X.more")
```
Let $X_1,..., X_{300}$ and $X_{301},..., X_{400}$ denote the 300 observations from `assignment3_2024_prob1.txt` and the 100 observations from `assignment3_2024_prob2.txt`, respectively. We assume that the observations $X_1,..., X_{400}$ are independent to each other. We model $X_1,..., X_{300}$ using a mixture of three negative binomial distributions (as we did in Problem 1), but we model $X_{301},..., X_{400}$ using the first of those three negative binomial distributions. Specifically, for $i = 1,..., 300$, $X_i$ follows the mixture model
$$
Z_i \sim Categorical(\pi_1, \pi_2, 1 - \pi_1 - \pi_2),
$$
$$
X_i|Z_i = 1 \sim NegBinomial(r, p_1),
$$
$$
X_i|Z_i = 2 \sim NegBinomial(r, p_2),
$$
$$
X_i|Z_i = 3 \sim NegBinomial(r, p_3),
$$
whereas for $i = 301,..., 400$, $X_i$ follows
$$
X_i \sim NegBinomial(r, p_1),
$$
where $r$ is some known constant. We aim to obtain MLE of the parameters $\theta = (\pi_1, \pi_2, p_1, p_2, p_3)$ using the EM algorithm.

(a) (5 marks) Let $X = (X_1,..., X_{400})$ and $Z = (Z_1,..., Z_{300})$. Derive the expectation of the com - plete log - likelihood, $Q(\theta, \theta^0) = E_{Z|X, \theta^0}[\log(P(X, Z|\theta))]$.
(b) (5 marks) Derive E - step and M - step of the EM algorithm.
(c) (5 marks) Note: Your answer for this problem should be typed. Answers including screen - captured R codes or figures won’t be marked.
Implement the EM algorithm and obtain MLE of the parameters by applying the implemented algorithm to the observed data, $X_1,..., X_{400}$, assuming a value of $r = 10$. Terminate the algorithm when the number of EM iterations reaches 500, or when the incomplete log - likelihood has changed by less than $10^{-5}$ ($\epsilon = 10^{-5}$). Run the EM algorithm twice with the following two different initial values, and report the parameter estimates corresponding to the highest incomplete log - likelihood.
|$\pi_1$|$\pi_2$|$p_1$|$p_2$|$p_3$|
|---|---|---|---|---|
|1st initial values|0.1|0.3|0.1|0.8|0.5|
|2nd initial values|0.2|0.4|0.3|0.2|0.7|
For each EM run, check that the incomplete log - likelihoods increase at each EM - iteration by plotting them.# MAST30027 Assignment 3

# CS Tutor | 计算机编程辅导 | Code Help | Programming Help

# WeChat: cstutorcs

# Email: tutorcs@163.com

# QQ: 749389476

# 非中介, 直接联系程序员本人
