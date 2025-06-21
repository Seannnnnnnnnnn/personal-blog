---
title: Change of Probability Measure
subtitle: A powerful technique for simulation.
layout: default
date: 2025-06-11
keywords: probability
published: true
katex: true
---
Change of probability measure is a powerful and beautiful technique, though its presentation in textbooks is often initially met with confusion. 

In most resources I've encountered, the presentation of the topic typically begins with technical results from measure theory, followed by an involved example which is often related to finance. 

Whilst this is fine, in my experience I found it difficult to develop a *first principles* understanding of where the theory comes from. In this post, I aim to go the oposite way; beginning with what is hopefully an accesible example of how one might develop the technique, before diving into a presentation of the actual theory. To close, I present a more complicated and practical applcation in regards to pricing a particular financial derivative. 

## A Simple Example

Imagine we can readily sample numbers from a {% katexmm %} $N(0,1)$ {% endkatexmm %} distribution, but we require a sample of a {% katexmm %} $N(\mu, \sigma^2)$ {% endkatexmm %}. One option is to to take some a sample {% katexmm %} $z\sim N(0, 1)$ {% endkatexmm %} and then set {% katexmm %} $x = \mu + z$ {% endkatexmm %} so that {% katexmm %} $x\sim N(\mu, 1)$ {% endkatexmm %}. In some sense, one can think of this as *shifting* the outcome of the first distribution to match a sample from our desired distrbution. 

Another approach is as follows: Suppose we can sample {% katexmm %} $z\sim N(0,1)$ {% endkatexmm %} but we wish to sample a {% katexmm %} $N(\mu,1)$ {% endkatexmm %}. We know the target distribution has density 
{% katexmm %} $$f(x|\mu) = \frac{1}{\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2}} \tag{1}$$ {% endkatexmm %}
While the distribution we can sanmple from has density
{% katexmm %} 
$$f_Z(z) = \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}} \tag{2}$${% endkatexmm %}
The densities look quite similiar. In fact, 
{% katexmm %} 
$$
\begin{aligned}
    f(x|\mu) = \frac{1}{\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2}} 
    &= \frac{1}{\sqrt{2\pi}} e^{-\frac{1}{2}(x^2-2x\mu+\mu^2)} \\
    &= f_Z(x) \cdot e^{x\mu - \frac{\mu^2}{2}}
\end{aligned}
$$
{% endkatexmm %}
So, if we sample a {% katexmm %} $z\sim N(0,1)$ {% endkatexmm %} then 
{% katexmm %} $$f_Z(z)\cdot e^{z\mu - \mu^2/2} \stackrel{d}{=} N(\mu, 1)$$ {% endkatexmm %}
as desired. 

So what happened here? Instead of *shifting* the outcome *z* by some constant, we found we could *reweight* its density by the amount {% katexmm %} $\exp(\mu z  - \mu^2/2)$ {% endkatexmm %} to match the desired probability distribution we were targeting.

## The Radon-Nikodym Derivative

If we take a step back and think about probabilities as assinging some volume, representing likelihood of some event {% katexmm %} $A$ {% endkatexmm %} where sits in a space of possible outcomes {% katexmm %} $A\subseteq \Omega$ {% endkatexmm %}[^1], then the *probability* denoted by {% katexmm %} $\mathbb{P}(A)$ {% endkatexmm %} denotes the volume, or *size* that {% katexmm %} $A$ {% endkatexmm %} occupies in {% katexmm %} $\Omega$ {% endkatexmm %}. 

Here, {% katexmm %} $\mathbb{P}$ {% endkatexmm %} is a measure function, it takes some set {% katexmm %} $A$ {% endkatexmm %} and spits out a corresponding *size*. In particular {% katexmm %} $\mathbb{P}$ {% endkatexmm %} is a *probability measure* which means it has the special property that {% katexmm %} $\mathbb{P}(\cdot)\in[0,1]$ {% endkatexmm %}.

In our example we began with a way to sample {% katexmm %} $N(0,1)$ {% endkatexmm %}, which means we have access to a measure {% katexmm %} $\mathbb{P}$ {% endkatexmm %} which assings sizes of sets in accordance to a standard Gaussian distribution. We wanted to swap however, to a different measure {% katexmm %} $\mathbb{Q}$ {% endkatexmm %}, which assinged probability accordining to a 
{% katexmm %} $N(\mu, 1)$ {% endkatexmm %} random variable.

In our example, we found the precise amount {% katexmm %} $\Lambda = \exp(\mu z - \mu^2/2)$ {% endkatexmm %} to tilt the {% katexmm %} $\mathbb{P}$ {% endkatexmm %} distribution to become{% katexmm %} $\mathbb{Q}${% endkatexmm %}. This amount, which I denote by {% katexmm %} $\Lambda$ {% endkatexmm %}, has a special name: it's a *Radon-Nikodym* derivative, which describe how to move from one measure to another.

The natural questions to ask at this point are; 1. Can we be sure that a Radon-Nikodym derivative exists for any given starting measure {% katexmm %} $\mathbb{P}$ {% endkatexmm %} and target measure {% katexmm %} $\mathbb{Q}$ {% endkatexmm %}? 2. How can we find the analytical form of Radon-Nikodym derivative {% katexmm %} $\Lambda$ {% endkatexmm %} and 3. How can swap between measures to compute probabilities? 

The [Radon-Nikodym Theorem](https://en.wikipedia.org/wiki/Radon%E2%80%93Nikodym_theorem) from Measure Theory answers the first question precisly for us; so long as for any set {% katexmm %} $A$ {% endkatexmm %} such that {% katexmm %} $\mathbb{Q}(A)=0$ {% endkatexmm %} we have {% katexmm %} $\mathbb{P}(A)=0$ {% endkatexmm %} then there is a unique function {% katexmm %} $\Lambda$ {% endkatexmm %} such that [^2]
{% katexmm %} $$\mathbb{Q}(A) = \int_A \Lambda d\mathbb{P} \tag{*}$$ {% endkatexmm %}
I won't proceed with a full proof, but I'll attempt to unpack and intuit why these conditions are necessary, and how one can see that the theorem should hold. 

First the condition of the theorem: for any set {% katexmm %} $A$ {% endkatexmm %} such that {% katexmm %} $\mathbb{Q}(A)=0$ {% endkatexmm %} we have {% katexmm %} $\mathbb{P}(A)=0$ {% endkatexmm %}. This type of relationship is special in Measure Theory and even has it's own name and notation. We say that {% katexmm %} $\mathbb{Q}$ {% endkatexmm %}  is *absolutely continuous* with respect to {% katexmm %} $\mathbb{P}$ {% endkatexmm %} which we denote by {% katexmm %} $\mathbb{Q}\ll\mathbb{P}$ {% endkatexmm %}. 

In the context of probability, the absolute continuity requirement states that the target distribution {% katexmm %} $\mathbb{Q}$ {% endkatexmm %} needs to agree with 
{% katexmm %} $\mathbb{P}$ {% endkatexmm %}  on what outcomes are impossible. If we didn't have this condition, inconsistent outcomes would arise where once impossible events would become possible when switching to a new measure.

Proving that we can always to find a {% katexmm %} $\Lambda$ {% endkatexmm %} in (*) is less straightforward, but the proof follows a constructive argument by taking a sequence of simpler functions {% katexmm %} $\{\Lambda_n\}$ {% endkatexmm %} whose integrals with respect to {% katexmm %} $\mathbb{P}$ {% endkatexmm %} match the desired behaviour. The sequence is constructed such that each successive element gives a more refined measured that captures the distribuition of {% katexmm %} $\mathbb{Q}$ {% endkatexmm %} more closely.  In the limit, we can show that equation in * holds. Moreover, since the limit of a sequence is unique, we can immiediately deduce that {% katexmm %} $\Lambda$ {% endkatexmm %} is unique as well.

## Finding Radon-Nikodym Derivatives Analytically

Once we have shown that {% katexmm %} $\mathbb{Q} \ll \mathbb{P}$ {% endkatexmm %} finding Radon-Nikodym derivatives aren't that difficult. Recall that {% katexmm %} $\mathbb{P}$ {% endkatexmm %} is a measure, so we can write the measure of any set {% katexmm %} $\mathbb{P}(A)$ {% endkatexmm %} as the integtal
{% katexmm %} $$\mathbb{P}(A) = \int_A d\mathbb{P} \tag{4}$$ {% endkatexmm %}
If you're not used to seeing an expression like (4), just think that the {% katexmm %} $d\mathbb{P}$ {% endkatexmm %} term means the change in measure {% katexmm %} $\mathbb{P}$ {% endkatexmm %}. For a probability measure, what would this change in measure be? Hopefully after pondering for a few minutes, you would agree that it's the change in the corresponding distribution function of the measure, {% katexmm %} $F(A):=\mathbb{P}(A\in dx)$ {% endkatexmm %}. So, the change in the distribution function by a small amount is a change in it's derivative according to change in it's argument by a small amount. Said more mathematically, {% katexmm %} $d\mathbb{P} = dF(x) = f(x)dx$ {% endkatexmm %} so we can also write (4) as 
{% katexmm %} $$\mathbb{P}(A) = \int_A d\mathbb{P} = \int_A d F(x)  = \int_A f(x)dx$$ {% endkatexmm %}
Going back to Radon-Nikodym derivatives, we know that 
{% katexmm %} $$\mathbb{Q}(A) = \int_A \Lambda d\mathbb{P} $$ {% endkatexmm %}
and from above, we also have 
{% katexmm %} $$\mathbb{Q}(A) = \int_A d\mathbb{Q}$$ {% endkatexmm %}
Now it should be obvious that the correct form we need is 
{% katexmm %} $$\Lambda = \frac{d\mathbb{Q}}{d\mathbb{P}} = \frac{g(x)}{f(x)}$$ {% endkatexmm %}
where {% katexmm %} $g,f$ {% endkatexmm %} are the {% katexmm %} $\mathbb{Q}, \mathbb{P}$ {% endkatexmm %} densitities respectively. 

In short, finding Radon-Nikdoym derivatives analytically is quite simple: take your target density {% katexmm %} $g$ {% endkatexmm %} and your starting density {% katexmm %} $f$ {% endkatexmm %} then look at their ratio {% katexmm %} $g/f$ {% endkatexmm %}. We can confirm this by looking at our simple example from before. 

Suppose we want to find the Radon-Nikodym derivative to move from {% katexmm %} $N(0,1)$ {% endkatexmm %} to {% katexmm %} $N(\mu,1)$ {% endkatexmm %}, then 

{% katexmm %} 
$$
\begin{aligned}
    \Lambda &= \frac{\frac{1}{\sqrt{2\pi}\exp(-(x-\mu)^2/2)}}{\frac{1}{\sqrt{2\pi}\exp(-x^2/2)}} \\
            &= \exp(x\mu - \mu^2/2)
\end{aligned}
$$
{% endkatexmm %}

## Swapping Between Measures

There's a few reasons in for why you might want to swap between measures. The first scnenario we explored is related to sampling from a target distribution. The other two common reasons are; Firstly, some random process you are interested in studying is more nicely behaved under a different measure, most commonly through becoming a martingale. The other situation is analysis or estimation becomes more tractable when working with a nice distribution. 

It's important to conceptualise that we start with our real-world / actual probability measure {% katexmm %} $\mathbb{P}$ {% endkatexmm %}, then swap to a *nicer* {% katexmm %} $\mathbb{Q}$ {% endkatexmm %} to perform some calculations. Sometimes, we can't interpret this {% katexmm %} $\mathbb{Q}$ {% endkatexmm %} measure directly though, so we need to then swap back to 
{% katexmm %} $\mathbb{P}$ {% endkatexmm %}.

Before closing out with an example, let's put this all together on how we can seemlessly swap between two measures {% katexmm %} $\mathbb{Q}, \mathbb{P}$ {% endkatexmm %} once we have their Radon-Nikodym Derivative {% katexmm %} $\Lambda = d\mathbb{Q} / d\mathbb{P}$ {% endkatexmm %}.

By Random-Nikodymn theorem, we then know that any {% katexmm %} $\mathbb{Q}(A)$ {% endkatexmm %} can be written as 
{% katexmm %} 
$$\mathbb{Q}(A) = \int_A \Lambda d\mathbb{P} = \mathbb{E_P}[\Lambda \cdot 1_{A}]$$
{% endkatexmm %}
Before closing out with an example, let's put this all together on how we can seemlessly swap between two measures {% katexmm %} $\mathbb{Q}, \mathbb{P}$ {% endkatexmm %} once we have their Radon-Nikodym Derivative {% katexmm %} $\Lambda = d\mathbb{Q} / d\mathbb{P}$ {% endkatexmm %}.

By Random-Nikodymn theorem, we then know that any {% katexmm %} $\mathbb{Q}(A)$ {% endkatexmm %} can be written as 
{% katexmm %} 
$$\mathbb{Q}(A) = \int_A \Lambda d\mathbb{P} = \mathbb{E_P}[\Lambda \cdot 1_{A}]$$
{% endkatexmm %}
where {% katexmm %} $1_A$ {% endkatexmm %} is the *indicator function*. Once we've calculated the desired probability, we can *swap back* with an inverted procedure 
{% katexmm %} 
$$\mathbb{P}(A) = \int_A \Lambda^{-1} d\mathbb{Q} = \mathbb{E_Q}[\Lambda^{-1} \cdot 1_{A}]$$
{% endkatexmm %}


## An Application to Digital Option Pricing 

Let's depart from mathematics from a second, and turn our attention to finance. We consider a particular type of contract which is traded in the markets called a *Digital Call Option*. 

A *Digital Call Option* (or henceforth, a *Digital Call*) struck at price {% katexmm %} $K>0$ {% endkatexmm %}, expiring at future time {% katexmm %} $T$ {% endkatexmm %} gives the holder of the contract a payoff of $1 in the event that some underlying security (eg a stock, bond, currency) closes above the price *K* at time {% katexmm %} $T$ {% endkatexmm %}. 

It's clear a Digital Call is a bet on the bimodal outcome for some underlying asset price ends up above or below  {% katexmm %} $K$ {% endkatexmm %}. The question is, how much would one be willing to pay to make this bet?

Without any formal mathematical finance theory, one approach we could take is to simply estimate the probability {% katexmm %} $p := \mathbb{P}(S_T > K)$ {% endkatexmm %}. We then know by the binary outcome of the contract that the expected payoff is {% katexmm %} $\$1\cdot p$ {% endkatexmm %}.

Unfortunately, the probabilites of stock price movements aren't easily observable but suppose for our purposes, the final value of the asset is uniformally distribution on the interval {% katexmm %} $S_T \sim U(0, 100)$ {% endkatexmm %}. By the above, the value of the digital option is therefore
{% katexmm %} 
$$C = \mathbb{E_P}(1_{S_T > K}) = \int_0^{100} 1_{z > K}dz$$
{% endkatexmm %} 
Finding the value of this definite integral is cumbersome, so let's attack the problem with Monte-Carlo. The strategy is to simulate   {% katexmm %} $n$ {% endkatexmm %} random variables   {% katexmm %} $U_i\sim U(0,100)$ {% endkatexmm %} and then form the estimate 
{% katexmm %}
$$\hat{C}_n = \frac{1}{n}\sum_{k=1}^{n}1_{U_k > K} \tag{5}$$
{% endkatexmm %}
By the law of large numbers, we know that that the right hand side approachs the *true* price given by the integral as we take {% katexmm %} $n\to\infty$ {% endkatexmm %}. 

Suppose we're considering an option that's struck at {% katexmm %} $K=90$ {% endkatexmm %}. Looking at (5), we see that one in every ten samples {% katexmm %} $U_k$ {% endkatexmm %} will have non-zero value. 

### Footnotes

[^1]: I've oversimplified things quite a bit here. Measures are *special* in that they ascribe volumes / sizes *consistently* amongst sets, namely that the size of the empty set should be 0, {% katexmm %} $A\cap B = \emptyset \implies \mathbb{P}(A\cup B) = \mathbb{P}(A) + \mathbb{P}(B) $ {% endkatexmm %} and {% katexmm %} $A\subseteq B \implies \mathbb{P}(A)\leq\mathbb{P}(B)$ {% endkatexmm %}. As it turns out, not every possible {% katexmm %} $A\subseteq \Omega$ {% endkatexmm %} results in these properties holding, so one needs to restrict the space of considerable sets to a collection {% katexmm %} $\mathcal{F}$ {% endkatexmm %} called a *sigma algebra*.

[^2]: Another slight ommision is a technical condition that the measures used in the Radon-Nikodym theorem must be *sigma finite*. Fortuntately, all probability measures {% katexmm %} $\mathbb{P}$ {% endkatexmm %} satisfy this condition so we can skip over this when working in the probability context.