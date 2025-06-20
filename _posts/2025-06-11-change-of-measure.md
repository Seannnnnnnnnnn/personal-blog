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
{% katexmm %} $$f_Z(z) = \frac{1}{\sqrt{2\pi}} e^{-\frac{z^2}{2}} \tag{2}$$ {% endkatexmm %}
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

In our example, we found the precise amount {% katexmm %} $\Lambda = \exp^{\mu z - \mu^2/2}$ {% endkatexmm %} to tilt the {% katexmm %} $\mathbb{P}$ {% endkatexmm %} distribution to become{% katexmm %} $\mathbb{Q}${% endkatexmm %}. This amount, which I denote by {% katexmm %} $\Lambda$ {% endkatexmm %}, has a special name: it's a *Radon-Nikodym* derivative, which describe how to move from one measure to another.

The natural questions to ask at this point are; 1. Can we be sure that a Radon-Nikodym derivative exists for any given starting measure {% katexmm %} $\mathbb{P}$ {% endkatexmm %} and target measure {% katexmm %} $\mathbb{Q}$ {% endkatexmm %}? 2. How can we find the analytical form of Radon-Nikodym derivative {% katexmm %} $\Lambda$ {% endkatexmm %} and 3. How can swap between measures to compute probabilities? 

Answering the first question with mathematical rigour is out of the scope of this blog post, and more belonging to a graduate class in Measure Theory. However, 

## An Application to Digital Option Pricing 

Let's depart from mathematics from a second, and turn our attention to finance. Let's consider a particular type of contract which is traded in the markets called a *Digital Call Option*. 

A *Digital Call Option* (or henceforth, a *Digital Call*) struck at price {% katexmm %} $K>0$ {% endkatexmm %}, expiring at future time {% katexmm %} $T$ {% endkatexmm %} gives the holder of the contract a payoff of $1 in the event that some underlying security (eg a stock, bond, currency) closes above the price *K* at time {% katexmm %} $T$ {% endkatexmm %}. 

It's clear a Digital Call is a bet on the bimodal outcome for some underlying asset price ends up above or below  {% katexmm %} $K$ {% endkatexmm %}. The question is, how much would one be willing to pay to make this bet?

To price such a claim, one is interested in the probability that the asset finishes above *K*.

### Footnotes

[^1]: I've oversimplified things quite a bit here. Measures are *special* in that they ascribe volumes / sizes *consistently* amongst sets, namely that the size of the empty set should be 0, {% katexmm %} $A\cup B = \emptyset \implies \mathbb{P}(A\cup B) = \mathbb{P}(A) + \mathbb{P}(B) $ {% endkatexmm %} and {% katexmm %} $A\subseteq B \implies \mathbb{P}(A)\leq\mathbb{P}(B)$ {% endkatexmm %}. As it turns out, not every possible {% katexmm %} $A\subseteq \Omega$ {% endkatexmm %} results in these properties holding, so one needs to restrict the space of considerable sets to a collection {% katexmm %} $\mathcal{F}$ {% endkatexmm %} called a *sigma algebra*.