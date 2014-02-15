Title: Using PyMC to Analyze A/B Testing Data
Date: 2013-12-24 22:23
Author: amygdalama
Category: Projects
Tags: ab testing, bayesian methods for hackers, matplotlib, numpy, pymc, python
Slug: using-pymc-to-analyze-ab-testing-data

In Chapter 2 of [Bayesian Methods for Hackers][], there's an example of
Bayesian analysis of an A/B test using simulated data. I decided to play
around with this analysis method with real A/B landing page test data
from one of my clients.

This method uses PyMC to estimate the real conversion rate for each page
and Matplotlib to visually interpret the results.

First, I import the relevant packages:

[code language="python"]import numpy as np  
import pymc as pm  
import matplotlib.pyplot as plt[/code]

My client ran a landing page test with the following results:

[code language="python" firstline="5"]clicks\_A = 1135  
orders\_A = 5  
clicks\_B = 1149  
orders\_B = 17[/code]

The observed conversion rates are 44% and 1.48% for pages A and B,
respectively, but I'd like to be confident that the true conversion rate
of page B is higher than page A.

To format this data for the analysis, I create a numpy array for each
page with 1s representing orders and 0s representing clicks without an
order:

[code language="python" firstline="10"]data\_A = np.r\_[ [0] \*
(clicks\_A - orders\_A), [1] \* orders\_A ]  
data\_B = np.r\_[ [0] \* (clicks\_B - orders\_B), [1] \* orders\_B
][/code]

Next I assign distributions to my prior beliefs of \$latex
p\_{A}\$ and \$latex p\_{B}\$, the unknown, true conversion rates. I
assume, for simplicity, that the distributions are uniform (i.e. I have
no prior knowledge of what \$latex p\_{A}\$ and \$latex p\_{B}\$ are).
[Note: the rest of the code in blog post is taken from [the book][].]

[code language="python" firstline="13"][/code]

 

[code language="python" firstline="13"]p\_A = pm.Uniform('p\_A',
lower=0, upper=1)  
p\_B = pm.Uniform('p\_B', lower=0, upper=1)[/code]

Since I want to estimate the difference in true conversion rates, I need
to define a variable `delta`, which equals `p_B - p_A`. Since, if I know
both `p_A` and `p_B`, I can calculate `delta`, it's a deterministic
variable. In PyMC, deterministic variables are created using a function
with a `pymc.deterministic` wrapper:

[code language="python" firstline="16"]  
@pm.deterministic  
def delta(p\_A=p\_A, p\_B=p\_B):  
return p\_B - p\_A  
[/code]

Next I add the observed data to PyMC variables and run an inference
algorithm (I don't understand what this code is actually doing yet - an
explanation is coming up in Chapter 3):

[code language="python" firstline="20"]obs\_A = pm.Bernoulli("obs\_A",
p\_A, value = data\_A, observed = True)  
obs\_B = pm.Bernoulli("obs\_B", p\_B, value = data\_B, observed = True)

mcmc = pm.MCMC([p\_A, p\_B, delta, obs\_A, obs\_B])  
mcmc.sample(20000, 1000)[/code]

Then I plot the posterior distributions for the three unknowns:

[code language="python" firstline="26"]  
p\_A\_samples = mcmc.trace("p\_A")[:]  
p\_B\_samples = mcmc.trace("p\_B")[:]  
delta\_samples = mcmc.trace("delta")[:]

ax = plt.subplot(311)

plt.xlim(0, .035)  
plt.hist(p\_A\_samples, histtype='stepfilled', bins=25, alpha=0.85,  
label="posterior of \$p\_A\$", color="\#A60628", normed=True, edgecolor
= "none")  
plt.legend(loc="upper right")  
plt.title("Posterior distributions of \$p\_A\$, \$p\_B\$, and delta
unknowns")

ax = plt.subplot(312)

plt.xlim(0, .035)  
plt.hist(p\_B\_samples, histtype='stepfilled', bins=25, alpha=0.85,  
label="posterior of \$p\_B\$", color="\#467821", normed=True, edgecolor
= "none")  
plt.legend(loc="upper right")

ax = plt.subplot(313)  
plt.ylim(0,120)  
plt.hist(delta\_samples, histtype='stepfilled', bins=50, alpha=0.85,  
label="posterior of \$p\_B\$ - \$p\_A\$", color="\#7A68A6",
normed=True, edgecolor = "none")  
plt.legend(loc="upper right")  
plt.vlines(0, 0, 120, color="black", alpha = .5)

plt.show()  
[/code]

[![figure\_1][]][figure\_1]

I can also compute the probability that the true conversion rate of page
A, \$latex p\_{A}\$, is better than the true conversion rate of page
B, \$latex p\_{B}\$:

[code language="python" firstline="54"]  
print "Probability site A is BETTER than site B: %.3f" % \\  
(delta\_samples \< 0).mean()  
print "Probability site A is WORSE than site B: %.3f" % \\  
(delta\_samples \> 0).mean()  
[/code]

`Probability page A is BETTER than page B: 0.006 Probability page A is WORSE than page B: 0.994`

It's very safe to say (as long as our data was collected properly) that
page B is better than page A, and these results come very intuitively
from looking at the graphs.

The full code can be found on my [GitHub][].

  [Bayesian Methods for Hackers]: https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers
  [the book]: http://nbviewer.ipython.org/github/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/blob/master/Chapter2_MorePyMC/MorePyMC.ipynb
  [figure\_1]: http://mathamy.files.wordpress.com/2013/12/figure_11.png
  [GitHub]: https://github.com/amygdalama/tutorials/blob/master/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/MySourceFiles/Chapter2/ab-real-data.py
