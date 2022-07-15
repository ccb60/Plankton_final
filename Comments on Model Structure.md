# Stepsize Model Selection Methods
I've not been using stepwise model selection for the GAM models, largely to simplify presentation in the manuscript (following Erin's original strategy).  There's simplicity in presenting all analyses  based on the same model.  There are also technical reasons why AIC model selection does not work with GAM models, but I'd have to go back and reread both my notes and the related "help files" to remember why.  We achieve something similar (for the smoothed terms) by using "shrinkage estimators" in GAM.  Shrinkage estimators allow model terms to be "shrunk" out of the model if they are unimportant.  It would be very easy to refit simplified GAM models that omit the smooth terms that are unimportant.  We would have to figure out some way to decide whether to retain both Season and Station in the models.

# Random Effects Versus Fixed Effects in Model Selection
The GAM models I am using have THREE categories of model components to think about.  The include:
*  Random Effects (Year and Sample Event)
*  Fixed Effects fit as linear terms (Station and Season) 
*  Fixed Effects fit as Smoothers (environmental variables)

Classically, "random effects" were used to account for anticipated correlations in the data imposed by the experimental design.  I sometimes think of them as a generalization of the "paired t-test" to more complex experimental designs (groups that consist of more than a pair) and more complex models (more than one predictor).  For example, agricultural experiments used (precursors of) hierarchical models to account for different productivity of different fields or plots.  Medical experiments used hierarchical models to account for "repeated measures" designs where each  patient was measured repeatedly over time.  Including the random effects improves estimation of experimental error, by properly identifying groups of observations expected to be "more similar" than a randomly selected observation.  Usually (but not always) they reduce "experimental error" or model residuals and improve statistical power, just as the paired t-test does.

Any experimental design component can be fit either as a random factor or as a fixed effect.   If you fit a term as a random factor, you don't later get to look under the hood and interpret differences among groups defined by the factor.  By choosing to treat that variable as a random factor, you are saying "this matters, and creates a group of correlated observations that I am aware of, but uninterested in for their own sake". So, a key decision is which experimental design features do you want to study, and which are largely uninteresting to you? If you are interested, you fit them as a fixed effect.  if you are uninterested, you fit a random effect (or omit the design component from the model entirely.

We have at least four experiments "design" features we could fit either way.  They include:
*  Station
*  Season
*  Year
*  Sampling Event (Date).

I believe we are interested in understanding both Season and Station, as part of understanding dynamics within the estuary, so I would retain them in the model as fixed (not random) effects.  I would be interested in understanding why the other papers you have been reading treat Season as a random effect.  I would treat it as a random effect if the purpose of the study was focused on understanding long-term trends, in which case seasonal patterns could be distracting. Similarly, I would treat Station as a random effect only  if we were uninterested in understanding differences between stations.  For example, when I analyze Friends of Casco Bay's long-term water quality monitoring data, I treat their monitoring Stations as a random factor when modelling long-term trends in Casco Bay water quality.  In that setting, the Stations being monitored can be thought of as a selection of all the possible places we COULD have been monitoring.

Handling of Year takes a little more thought.  Are we interested in interpreting year to year variation in plankton density?  Not especially.  Do we think observations from within one year are likely to be more similar than other observations?  Probably.  (We did want to look at the impact of river restoration, which might manifest as a long-term trend or a difference between years  "before restoration" and "after restoration".  But that analysis fell by the wayside, as it is quite clear there was no pattern. It's not obvious whether including Year as a random facto increases or decreases sensitivity to a long-term trend.)  So, we probably want to include Year as a random factor.

But notice that the reason we should include Year in the model rests on an assumption of temporal autocorrelation.  Observations within the same year are more similar than not.  It is possible that annual variation is the result of looking at a "slice" of a time series with high temporal autocorrelation.  Maybe the correlation is not year to year, but it is day to day (or hour to hour) so observations gathered closer in time tend to be similar.  Likewise, it is not unreasonable to imagine that all samples collected on the same day (at different locations) are "more similar" than expected. For whatever reason, plankton is just more abundant some days than others.

I examined those possibilities, in a notebook titled "Random Factors and Autocorrelation in Plankton Models".  I believe I already shared that with you.  If not, let me know and I can send it to you.  In that notebook, I explored ways of modeling temporal autocorrelation.  I determined that the "best" models did not explicitly model autocorrelation, but DID include Year and what I called the "Sample Event" (usually a single day) as random factors.

Including Sample Event as a random factor is arguable.  Perhaps samples collected on the same day are NOT more correlated than expected.  I only checked one or two models....

# Colinearity 
Yes, this is a potential problem. Most variables are partially co-linear.  Most of the quantitative environmental variables are individually only moderately collinear (correlation coefficient under 0.4).  But, several are related to seasonality (e.g. temperature) or station (salinity).  I suspect this helps create some of the complexity of model behavior and interpretation when we look at  more complex models.

When we use many (partially collinear) predictors, the problem only gets worse, as it becomes increasingly likely that some combination of variables may be acting almost as a surrogate for another variable.  I have not studied this in the context of GAMs.  I suspect that GAMs will, if anything, makes this slightly more likely to be a problem, but I don't really know.

I'm not sure exactly how to "test for collinearity".  It is possible to evaluate the "condition" of the matrix of model predictors and quantitatively assess the degree to which predictors are collinear.  I'm sure someone has turned that into a formal test.  I just have never used it.  And I don't know if it would help us with GAM fits.

# Stepwise Model Selection Again
What saves me from too much worry about collinearity is that using the "shrinkage estimators" in the GAM models, our models start out complex, but the relationships that are actually fit depend quantitatively on only a handful of predictors, as others are "shrunk" out of the model.  And since pairwise correlations are not sky high, I don't think the final models are meaningfully affected by collinearity.  Mostly.  Probably....

One of the reasons I got interested in running linear mixed effects models (rather than just the GAMs) was because I can use formal AIC-based stepwise model selection methods on the LME models.  I have not finished that analysis for all plankton species (the LME models don't work all that well....), but so far, the final models USUALLY include just a handful of significant relationships, again suggesting collinearity is not a critical problem.  Mostly.  I think....

I have not shared that notebook with you yet.  
