---
layout: post
title:  "Teaching walkers on a ring to make butterfly loops"
subtitle: "or a short summary on reservoir computing with quantum walks"
date:   2025-12-15
tags : [quantum]
categories : works
---

People are working on trying to get a physical system to do machine-learning. Some folks in Brighton UK, used a bucket of water[^1] to solve the [XOR Problem](https://www.thinkstack.ai/glossary/xor-problem/). Some other folks have employed a single pendulum[^2]. The usual candidates like optical systems, magnetic systems to even  [human tissue](https://www.researchgate.net/publication/390088531_Information_processing_via_human_soft_tissue_Soft_tissue_reservoir_computing) (yeah) is being investigated for this.

*There is a rumor that in the Wachowskis' original draft for The Matrix, the Machines didn't use humans as batteries. Instead, they used the combined processing power of billions of human brains to form a massive, biological neural network .The studio reportedly felt this concept was too complex for 1999 audiences*.[^3]

Anyway, if you want your grandfather clock to predict NIFTY50, the standard way to go about it would be to do something called reservoir computing (RC). The reservoir in the name is the physical system.

## Expansion is seperation
### or how to build a RC pipeline

1. **Find** : find data. lots and lots of it. the more the better. split it into training and testing sets. find a good reservoir. not every dynamical system is a good reservoir. [here's a guide](doi.org/10.1088/2634-4386/ac7db7) on picking a good one.

2. **Encode** : inject the input data into a parameter that governs the reservoir's dynamics. evolve the reservoir for every input (after fixing a starting point). record the readout from reservoir's state after some fixed time. (you can also record at some regular intervals).

3. **Train** : run a [linear regression](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html) model on the reservoir readout for the training set.

4.  **Predict** : use the linear model to predict the output for the reservoir readout from the test set.

(Once I find a decent detailed tutorial, I'll link it here.)

**Why should this work?**

There is no exact proof for this, but there is a hand-wavy argument.
<div style="text-align: center;">
<figure>
<img src = "{{site.url}}{{'media/why_res_2d_3d.png' | relative_url}}" height = "200" alt = "plots representing points which are linearly inseperable in 2d , becoming linearly seperable in 3d">
<figcaption> <a href = "https://iopscience.iop.org/article/10.1088/2634-4386/ac7db7">Source</a> </figcaption>
</figure>
</div>

The figure above portrays the core idea behind reservoir computing; which is that **points are easily distinguishable in a higher dimension**. A non-linearly seperable dataset in 2D (b) becomes linearly seperable in 3D (c). The idea is to use a dynamical system,(the reservoir), to project the input dataset into a higher dimensional space where it is linearly sepearble.

## Injecting the Q

What happens if you use a quantum dynamical system?

First things first. Naturally you add a Q to the name and the pipeline gets officially christened as Quantum Reservoir Computing (QRC), and it gets filed under Quantum Machine Learning (QML). Congrats! you can now officialy brag on LinkedIn that you work in Quantum AI (oh my! fancy fancy).

In addition to the status gain among your peers, there is another big benefit in choosing a quantum system. **A bigger readout space**. Each additional qubit you add, doubles the Hilbert space available for evolution.

A simple enough quantum system, implementable on hardware and  that fits our profile is the [**Quantum Walk**]({{'works/2025-11-29-quantum_walk/'|relative_url}}). 

For those who didn't bother to click the link, a quantum walker is the quantum analog of a classical walker (duh). A classical walker tosses a coin at every step and moves as per the outcome of the toss. If it's heads move right, and a tails imply moving left ( or vice versa).

In the quantum version, the coin toss is like a shaker. Each position has a probability associated with it for moving left/right from that position. When you apply coin (coin is an operator here), the probabilities change to some other value (shakes). Moving after tossing the coin in the quantum version is exchanging the probabilities between adjacent nodes.(I think I do a better job of explaining it [here]({{'works/2025-11-29-quantum_walk/'|relative_url}}).) For a quantum walk on a ring, every node has two neighbors, so things get nice and symmetric for the exchange.

## Making Butterfly Loops
### or predicting the evolution of a Lorenz attractor

Lorenz attractor is a three dimensional (3 state variables) classical chaotic system. It's evolution is governed by 3 differential equations, one for each state variable. (the equations themselves are of little importance here). For certain parameters, two of the state variables of the system evolve like the wings of a butterfly, or a rotated figure 8.

<div style="text-align: center;">
<figure>
<img src = "{{site.url}}{{'media/lorenz.png' | relative_url}}" height = "300" alt = "evolution of the variables x and z for the lorenz attractor, making the famous butterfly loop.">
<figcaption> The famous butterfly loop </figcaption>
</figure>
</div>

In this picture, note that z and x are plotted together. Our mission, should you choose to accept, is to predict z(t) given x(t).

<div style="display: flex; justify-content: center; gap: 20px; align-items: start;">

  <figure style="flex: 1; text-align: center; margin: 0;">
    <img src="{{ site.url }}{{ 'media/lorenz_z.png' | relative_url }}" 
         alt="Evolution of the variable x " 
         style="width: 100%; height: auto;">
    <figcaption>Output</figcaption>
  </figure>

  <figure style="flex: 1; text-align: center; margin: 0;">
    <img src="{{ site.url }}{{ 'media/lorenz_x.png' | relative_url }}" 
         alt="Evolution of the variable z " 
         style="width: 100%; height: auto;">
    <figcaption>Input</figcaption>
  </figure>

</div>

With a quantum walk of 15 steps on a 5 node ring, you can get a very neat prediction:

<div style="text-align: center;">
<figure>
<img src = "{{site.url}}{{'media/lorenz_pred.png' | relative_url}}" height = "300" alt = "predicted value of z plotted with the true value of z">
<figcaption> The light green in the background is where the prediction is deviating from the true value. </figcaption>
</figure>
</div>

Indeed,the quantum walkers show some potential. Let's see if I could get them to start speaking English. (NLP)
<hr>

[^1]: Fernando C, Sojakka S (2003) Pattern recognition in a bucket. In: Banzhaf W,Ziegler J, Christaller T, Dittrich P, Kim J (eds) Advances in Artificial Life.ECAL 2003., Berlin, Heidelberg, pp 588–597

[^2]: S. Mandal, S. Sinha, and M. D. Shrimali, “Machine-learning potential of a single pendulum,”Physical Review E, vol. 105, no. 5, p. 54203, May 2022, doi: 10.1103/PhysRevE.105.054203.

[^3]: All hearsay. picked up from a reddit post.