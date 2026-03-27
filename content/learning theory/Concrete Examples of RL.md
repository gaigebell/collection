
[Instrumental learning of social affiliation through outcome and intention.](https://psycnet.apa.org/doiLanding?doi=10.1037%2Fxge0001190)

Experiment Procedure

> Learning Phase
> 
> Each trial:
> 
> 1. Choose a partner (1 out of 2)
> 2. Get feedback, which reveals how the partner ranked participants (intention) and whether they matched (outcome)
> 3. If matched, play trust game with partner. Decide to return half or keep all.
> 
> Test Phase
> choose  partner without feedback

Partner setting:

| Partner                                     | A   | B   | C   | D   |
| ------------------------------------------- | --- | --- | --- | --- |
| Avg. Rank of Participants (Intention)       | 3   | 3   | 7   | 7   |
| Avg. # of matched partners                  | 2   | 4   | 6   | 8   |
| Prob of matching with participant (Outcome) | .37 | .85 | .37 | .88 |


Model-free reinforcement learning in learning phase

To model the cognitive process of estimating expected rankings and expected outcomes of choosing a partner.

First, we consider how do participants learn one particular partner. To estimate the rankings, ${\hat I}_t$ denotes the estimate ranking (intention) of this partner at trial $t$.

The core of model-free RL is learning from prediction error. Thus, consider the ranking received $I$, the prediction error ${\delta_I}_t=I-{\hat I_{t-1}}$

Participant owns a learning rate $\alpha_I$, then the update equation is

$$
{\hat I}_t = {\hat I}_{t-1} + \alpha_I {\delta_I}_t
$$

Similarly, learning the outcomes, which is whether participant is matched or not (represented by binary variable $R$), can be modelled as

$$
{\hat Q}_t = {\hat Q}_{t-1} + \alpha_R{\delta_R}_t
$$

the prediction error ${\delta_R}_t=R-{\hat Q}_{t-1}$

The next step is to integrate the variables above and form the evaluation of this partner.

Before doing so, we need to clarify the meaning behind these numbers. The bigger ${\hat I}_t$ is, the less intention the partner has to play with participant. The bigger the ${\hat Q}_t$ is, the better outcome the participant has.

Thinking about the relationship between the value of the partner and estimated intention and outcome. They probably related in a way like $V = Q - I$, or a way like $V = Q + \dfrac{1}{I}$ etc. It doesn't matter in what form do you write them down, but it's crucial to connect your variables in the right way.

There's one more step before you integrate your variables. It's worthy to check their quantity. We notice that $Q\in [0,1]$ , but $I\in[1, 8]$ which means when we try to put them together directly, $I$ may implicitly overweigh $Q$ and lead to wrong interpretation on your result.

To unify their quantities, we can transform $I$

$$
IV = 1 - \dfrac{I-1}{8}
$$

For one thing, $IV$ implies property of $-I$. For another thing, $IV$ share the same range with $Q$ .

And now we can integrate them using a simple method, weighted sum. Let $w\in[0,1]$ denote the preference weight of ,say intention. The value of this partner is

$$
EV=w(IV) + (1-w)Q
$$

Then you can do the same thing to other partners and you'll get all the $EV$s.

With $EV$s, we are able to model the decision of participants now. A common way to model the decision choice with values is using the soft-max function. It maps your $EV$s to a probability distribution which can represent your likelihood of choose these partners.

$$
P(i,t) = \dfrac{\exp(\beta\times EV_{i,t})}{\sum_j\exp(\beta\times EV_{j,t})}
$$
