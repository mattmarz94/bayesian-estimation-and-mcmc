# bayesian-estimation-and-mcmc
Applying Bayesian thinking, conjugate priors, Bayesian estimation, credible intervals, and MCMC methods

Overview
Applying Bayesian thinking, conjugate priors, Bayesian estimation, credible intervals, and MCMC methods. In this assignment, I will:

- Apply Bayes' theorem to simple problems.

- Use conjugate priors for binomial and Poisson likelihoods.

- Calculate posterior distributions and credible intervals.

- Implement Metropolis-Hastings sampling from scratch.

I will also explain the results of specific code output, and how I interpreted these results below:

1 - A brief explanation of the Bayesian interpretation of the results.

In Bayesian terms, we began with explicit priors about unknown quantities and updated those beliefs with the likelihood of the observed data to obtain the posterior, then interpreted all summaries (point estimates, credible intervals, and probabilities) with respect to that posterior. In part 1, we framed 'spam given keyword' as P(spam|keyword). estimating it directly from the data and when appropriate, using laplacian smoothing as a way to use priors. The returned value is the posterior probability than an email is spam given the keyword, not a long-run frequency.  In Part Two, we modeled the number of daily spam emails using a Poisson model with an unknown rate and placed a Gamma prior on that rate. We then updated our belief day by day which let us compute the probability that the daily rate falls between any two numbers using the final posterior. In part 3, we formed an equal tailed credible interval by choosing bounds so that a small and equal amount of probability lies below the lower bound and above the upper bound. We noted that this Bay Interval gives a direct probability statement about the parameter given the data, and the prior, while the frequentist intervals concern long run coverage under repeated sampling. In part four we approximated the same posterior using the Metropolis-Hastings algorithm. We checked trace plots,  examined autocorrelation to see that dependence between draws decayed quickly, used a burn-in period to discard early transients, and reported acceptance rates to judge step size. With our results we were given a more clear picture on what to act on.

2 - A brief interpretation for the posterior distributions.

The posteriors in prts 2-4 consistently concentrate around a daily spam rate in the mod 30s, showing that the data quickly dominate the prior while still reflecting it. In part 2 the sequential gamma-poisson updating which was th e 40 cays of counts with each day's posterior becoming the next day's prior, produces curves that increasingly sharpen around a common region. You can view this with the plot labeled Sequential Posteriors to make it visible. Part 3 builds on that and turns the final posterior into a probabilistic statement. The 95% credible interval for the daily rate is roughly 31.8 to 35.4 (and in one run 32.6 to 36.4), meaning given the observed data and prior, there’s a 95% probability the true rate lies in that range. In Part 4, Metropolis–Hastings sampling (with acceptance ~0.47) yields a posterior histogram centered near the same mean ~34.4, and the trace/autocorrelation checks indicate the chain is mixing reasonably with the chosen proposals, reinforcing that the MCMC posterior agrees with the conjugate-update result

3 - Comparing the Bayesian credible intervals with frequentist confidence intervals for the same data. Explaining briefly any differences observed.

On this dataset, the frequentist 95% confidence intervals for the daily spam rate centered on the sample mean of 34.41 emails/day, with a normal-approximation CI of 32.57 to 36.25 and a chi-square CI of 32.59 to 36.30. These describe long-run coverage of the estimation procedure, not the probability of the parameter in this specific run. The Bayesian 95% credible interval from the final posterior was 31.84 to 35.44, which is a direct probability statement about where the rate lies given the data and prior. It is slightly shifted lower and a touch narrower than the frequentist bands, reflecting the prior and the asymmetric, non-negative shape appropriate for a rate. A separate Bayesian check via MCMC produced a posterior mean of 34.44 with a 95% credible interval of 32.64 to 36.37, broadly agreeing in location and width with the analytic results while confirming posterior shape through simulation. In short, all intervals are comparable when data are strong, but the Bayesian intervals encode prior information and give probability statements about the parameter, whereas the frequentist intervals quantify a procedure’s long-run coverage.

4 - Stationarity and mixing

After burn-in, both samplers’ trace plots settle into the fuzzy caterpillar band with no visible drift or long trends, which indicates stationarity. The chain oscillates around a stable level and repeatedly visits the high-probability region rather than creeping in one direction. In the autocorrelation plots, dependence drops as the lag increases, which indicates decent mixing. Faster decay means more effective independent information per draw. In our runs, the Normal random-walk proposal with standard deviation five shows a tighter and more uniform cloud in the trace and a quicker drop in autocorrelation at small lags, which signals better mixing for a given number of iterations. The Uniform proposal with a fixed half-width of three targets the same posterior but moves in bounded steps, which can produce a slightly stickier trace and a slower decay in autocorrelation if the window is small relative to the posterior spread. Acceptance rates sit in a reasonable middle range for both proposals, which is consistent with healthy traces. Very low acceptance would appear as long flat segments, while very high acceptance with tiny moves would imply slow exploration.

5 - repeating task 4.1 and now describing what I observe.
