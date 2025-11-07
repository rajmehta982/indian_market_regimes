# indian_market_regimes
The purpose of this repository is to explore the use of probabilistic
graphical models like Bayesian Networks and machine learning models like LSTM and Random
Forests to model and forecast bull, bear, and stagnant market regimes for the Indian market.

I also talked briefly about the market regime modelling framework I was building and how I would use it to drive asset allocation. I talked about 4 regimes that I think are both intuitive and effective to take asset allocation decisions:

Bull Run

Bear Run

Reversals (from bull to bear or vice versa)

Keeping all the complex math for later, the purpose of modelling is simple:

To develop a standardized representation of the outcomes of a process

To make predictions

For example, a gaussian distribution is a ‘standardized’ representation of outcomes - like the height of people measured several times by picking random people. Or, the list of daily returns of a stock. You can make a simple prediction like ‘there is a 50% chance that the next person you pick is 178 cm tall’ because the mean of your normal distribution is 178 cm’. Note, that is an approximation, because most processes don’t exactly follow a simple gaussian distribution.

Now, using the knowledge and coming back to the problem at hand. We want to know two things about market regimes:

Which regime are we in currently?

Which regime will we be in in the next month?

Here, the regime is the ‘outcome’ and there is a process that drives the outcome. What that process is - is open to interpretation and can be described differently by different researchers. This is the way I describe the process that would drive a regime (bull, bear, or reversal):

“Bull runs are started by a fundamental change in macro-economic conditions or companies’ performance. Macro-economic events and companies’ performance are not independent, and in fact, can have a reflexive relationship. After a bull run starts, it is sustained by momentum and investor sentiment until the momentum can no longer be sustained, or there is significant change in macro-economic conditions or companies’ performance that can start to make investors skeptical and less confident. This leads to a less stable, reversal-like regime. From the reversal-like regime, there can either be an actual reversal or the bull run can be re-started if things improve eventually. Therefore, the ‘reversal’ state can last for a few months, or much longer. A similar explanation can be made for bear runs and reversals associated with them.”

From this description, I have introduced multiple variables that we can use to model regimes and formalize our idea:

a. States (Regimes)

Regimes themselves (or states): Bull runs should show higher returns and less volatility

Reversals: Returns close to 0 and higher volatility

Bear Runs”: Negative Returns and mid to high volatility

b. Observations

Momentum

Investor Sentiment

Macro-economic variables

Company Performance

Now our problem is simplified. We want to model how the the observations, at each step in the time-series, produce the states. We have already established that there is some causal relationship between the observations and the states. We want to ‘standardize’ this causal relationship.

Primarily, there are two kinds of models that are generally used for this problem and they depend on whether we know the the labels for the states in our historical data. So we want to answer the question - do we know historically when we were in a bull, bear, or reversal state? And the answer (at least in my mind) is, YES.

The other approach is that we let the model come up with the states - we don’t tell it exactly what the three states are. But we do have some ‘prior’ knowledge of how the observations can lead to each of those states. That is, we can say that if the momentum is high, and sentiment is positive, there is 70% chance that we will remain in a bull run state. And, we have similar prior probabilities for transitioning from each of the states to the others. We also have prior information of the probability of being in each of the states (independent of observations), say 50% in bull, 25% in bear, and 25% in reversal. And finally, we have the probability of the observations remaining the same, or changing to a different level (say - interest rates remaining the same or changing). Then, we use real data and fit this ‘structure’ to it, so that the probabilities are then modified to best represent the data. This idea is called maximum likelihood estimate. There are different algorithms to do this. There is a tonne of literature and projects that you can find if you just google ‘Hidden Markov Models for Regime Modelling”, so there is not much value for me to dive deeper into this. And more importantly, we are not going to use it.

Now, coming back to the labelling - I will go through the tedious task of manually labelling the historical data into 3 regimes. This will make life simpler for us. Then we are left with choosing the appropriate model for our supervised learning task (since we have labels). We choose 3 different models that are most suitable for such a task, and evaluate the performance of each of them:

Random Forest Classifier

Bayesian Network

LSTM Classifier

Let’s start with with Random Forest Classifier
