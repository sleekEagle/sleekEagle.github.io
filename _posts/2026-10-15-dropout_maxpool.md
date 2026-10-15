---
layout: post
title: Dropout right before max pooling
date: 2026-10-15
description: Why dropout followed by max pooling makes training and evaluation disagree, a fix from Wu & Gu (2015), and two toy experiments that test both.
tags: [machine-learning, deep-learning]
categories: [articles]
toc:
  sidebar: left
---

This report looks at what happens when a network applies dropout and then immediately max-pools the result. In short: the network behaves differently in training and in evaluation, and evaluation results can get much worse. The report explains why, describes a fix from the literature (Wu & Gu, 2015), and tests both the problem and the fix on two toy models.

## 1. Summary

- **Dropout before max pooling makes training and evaluation disagree.** In training, the pool output is on average **higher** than the plain max that the pool gives in evaluation. The usual dropout scaling (×1/(1−q)) does not correct this. For equal values in a 2×2 window and q = 0.2, the training output is 1.248× the evaluation output.
- **The reason:** the scaling assumes a dropped unit becomes 0. After a max pool, a dropped unit is replaced by the next largest survivor instead. So dropping costs less than the scaling assumes, and the scaling overshoots.
- **The fix (Wu & Gu, 2015): weighted pooling at evaluation time.** Sort each window and output a weighted sum of its values instead of the max. The weights are the chances that each value wins the max in training. This gives exactly the average training output. Training is not changed, nothing is learned, and an already trained model can use it.
- **Toy results (q = 0.3):** with plain max at evaluation, test error is about 30× (CNN) to 110× (dense) higher than without dropout, and predictions are about 30% off in scale. Weighted pooling, on the same trained weights, brings the error back to the no-dropout level in the CNN and close to it in the dense model.
- **Practical advice:** the simplest option is to not put dropout right before a max pool. If you do, use weighted pooling at evaluation.

## 2. Background: why dropout needs a scaling fix

Dropout with drop probability q sets each unit to 0 with probability q, independently, during training. At evaluation it is turned off.

Take one unit with value 6 and q = 0.2:

- **Training:** kept 80% of the time (6), dropped 20% of the time (0). On average the next layer sees 0.8 × 6 = **4.8**.
- **Evaluation:** dropout is off, so the next layer always sees **6**.

The next layer was trained on inputs that average 4.8, and now gets 6. There are two standard ways to fix this:

| Version | Training | Evaluation |
|---|---|---|
| Original dropout (Hinton et al., 2012) | kept values unchanged | outputs × (1 − q) |
| Inverted dropout (PyTorch's `nn.Dropout`) | kept values × 1/(1 − q) | unchanged |

Both make **the average training value of each unit equal its evaluation value**. With inverted dropout, a kept 6 becomes 7.5, and 0.8 × 7.5 = 6.

### When this per-unit fix is enough

A conv or linear layer computes a weighted sum of its inputs. The average of a sum is always the sum of the averages. So if each input is right on average, the layer's output is right on average too.

A max pool is not a sum. The average of a max is **not** the max of the averages, so the per-unit fix does not carry over.

Example with two units, 6 and 5, q = 0.2, inverted dropout:

| Outcome | Probability | Unit A | Unit B | Sum A + B | Max of A, B |
|---|---|---|---|---|---|
| both kept | 0.64 | 7.5 | 6.25 | 13.75 | 7.5 |
| only A kept | 0.16 | 7.5 | 0 | 7.5 | 7.5 |
| only B kept | 0.16 | 0 | 6.25 | 6.25 | 6.25 |
| both dropped | 0.04 | 0 | 0 | 0 | 0 |
| **training average** | | **6** | **5** | **11** | **7.0** |
| **evaluation value** | | 6 | 5 | 11 | 6 |

- Each unit on its own: matches.
- Sum: 11 in both. Matches.
- Max: 7.0 in training, 6 in evaluation. **Does not match.**

## 3. The problem in detail

### 3.1 What comes out of the pool in training

Dropout runs as usual, then the max pool takes the largest value that survived. Take a 2×2 window [1, 6, 5, 3] with q = 0.2 (keep probability p = 0.8). With inverted dropout, every kept value is also multiplied by 1.25.

| What happened | Pool output | Probability | Output × probability |
|---|---|---|---|
| 6 kept (others don't matter) | 6 × 1.25 = 7.5 | 0.8 | 6.0 |
| 6 dropped, 5 kept | 5 × 1.25 = 6.25 | 0.2 × 0.8 = 0.16 | 1.0 |
| 6, 5 dropped, 3 kept | 3 × 1.25 = 3.75 | 0.2² × 0.8 = 0.032 | 0.12 |
| 6, 5, 3 dropped, 1 kept | 1 × 1.25 = 1.25 | 0.2³ × 0.8 = 0.0064 | 0.008 |
| all dropped | 0 | 0.2⁴ = 0.0016 | 0 |
| **training average** | | | **7.128** |

The probability column only depends on how many values in the window are **larger**. The largest value wins whenever it is kept, so its chance is just p. Each larger value that must be dropped multiplies the chance by q.

The last column adds up to the average pool output in training. If you ran this window through dropout and the pool 10,000 times, about 8,000 outputs would be 7.5, about 1,600 would be 6.25, and so on. The mean of all 10,000 outputs is 7.128.

### 3.2 The mismatch

- **Training:** the next layer gets **7.128** on average from this window.
- **Evaluation:** dropout is off and the pool outputs the plain max, **6**.

The ×1.25 boost was meant to make up for dropped values becoming 0. But when the 6 is dropped, the output does not become 0. It falls back to 5, which is still boosted. So the boost makes up for a loss that mostly does not happen.

The gap is largest when the values in a window are about equal, which is common in smooth regions of a feature map. For four equal values a and q = 0.2, the training average is (1 + 0.2 + 0.04 + 0.008) · a = **1.248 · a**, while evaluation gives a.

The gap grows with q, and it repeats at every dropout + pool stage in the network. How much it hurts depends on the task:

- **Classification** is fairly tolerant. A ReLU network scales roughly linearly, so scaling all activations up or down tends to scale all logits together, and the predicted class barely changes.
- **Regression of absolute values** is not tolerant. A scale error in the features becomes an error in the answer. Whether predictions go up or down depends on the signs of the later weights.

## 4. The fix: Wu & Gu (2015)

Haibing Wu and Xiaodong Gu, *Towards Dropout Training for Convolutional Neural Networks*, Neural Networks 71 (2015). arXiv: [1512.00242](https://arxiv.org/abs/1512.00242).

### 4.1 Main points of the paper

1. **Max-pooling dropout is random selection.** The paper does not change dropout. It shows that, seen from the pool output, dropout followed by max pooling is the same as picking one value from each window at random. For sorted values a₁ ≤ … ≤ aₙ, value aᵢ is picked with probability pᵢ = p · q^(n−i), and 0 with probability qⁿ. The largest value is most likely, but weaker values sometimes win.
2. **This is a useful regularizer.** The network cannot depend on one strong activation per window. The paper places it between max pooling (always the top value) and average pooling (all values count equally). It also compares it with stochastic pooling (Zeiler & Fergus, 2013), which picks the pool output at random too: max-pooling dropout with typical keep probabilities often beat stochastic pooling by a large margin.
3. **Evaluation should use probabilistic weighted pooling.** The paper compares three rules for evaluation:
   - plain max pooling;
   - scaled max pooling, p × max (the usual dropout correction applied to the pool output);
   - **probabilistic weighted pooling**: Σᵢ pᵢ · aᵢ, the average training output.
4. **Results on MNIST (their Fig. 4):** weighted pooling has the lowest test error at every keep probability tested (0.2 to 0.8). With heavy dropout the other two rules do badly: at p = 0.2, about 0.95% (plain max) and 1.6% (scaled max) versus 0.55% (weighted). As p approaches 1 the three rules converge, and at p = 1 they are identical.
5. **Dropout placement:** dropout on the input of conv layers helped, but often less than dropout before max pooling or before fully connected layers. The paper's explanation is that conv layers share weights, have few parameters and overfit less.

The paper uses original (non-inverted) dropout and ReLU activations. All experiments are classification on MNIST, CIFAR-10 and CIFAR-100, without data augmentation.

### 4.2 Why the paper fixes evaluation instead of moving dropout

Moving dropout after the pool gives what the paper calls convolutional dropout, which it found less effective. The authors wanted the regularization of max-pooling dropout, so they kept it and fixed the evaluation side instead.

This was 2015, when batch normalization had just appeared and dropout was one of the main regularizers. Today CNNs are usually regularized with batch normalization, data augmentation and weight decay, and dropout inside conv blocks is less common.

### 4.3 Weighted pooling with inverted dropout

With inverted dropout (PyTorch), each winner is also multiplied by 1/p, so the weights become pᵢ / p = q^(n−i). For the window [1, 6, 5, 3] and q = 0.2:

| Sorted value | Chance it wins | × 1/p | Weight |
|---|---|---|---|
| 6 | 0.8 | × 1.25 | 1 |
| 5 | 0.16 | × 1.25 | 0.2 |
| 3 | 0.032 | × 1.25 | 0.04 |
| 1 | 0.0064 | × 1.25 | 0.008 |

6 · 1 + 5 · 0.2 + 3 · 0.04 + 1 · 0.008 = **7.128**, the training average from section 3.1.

| | Dropout | Pool |
|---|---|---|
| Training | on | normal max pool |
| Evaluation | off | weighted pool |

Weighted pooling has no learnable parameters. The weights depend only on q. An existing model trained with dropout before max pooling can use it without retraining.

### 4.4 Negative activations

The paper assumes ReLU, so values are never negative. With activations that can be negative (for example LeakyReLU), a dropped unit (0) beats every negative value. A negative value can then only win if **no** unit in the window was dropped (probability pⁿ), and only when the whole window is negative. My implementation handles this:

- non-negative values: weights q^(n−i), as above;
- all-negative window: the max × p^(n−1);
- negative values in a mixed window: weight 0.

### 4.5 Check of the implementation

To check the formula, dropout followed by a 2×2 max pool was run 200,000 times on random inputs, with all-positive, mixed and all-negative windows. The average output was compared with the evaluation output:

| q | Weighted pooling error | Plain max error |
|---|---|---|
| 0.2 | 0.004 | 0.42 |
| 0.5 | 0.008 | 1.20 |

The weighted pooling errors are at the level of simulation noise.

### 4.6 A limit

Weighted pooling is exact for one dropout + pool stage. Later layers still get noisy inputs in training and clean inputs in evaluation, and with non-linear layers in between this is not handled exactly. The same is true for ordinary dropout. The paper's results, and the toys below, show the approximation works well.

## 5. Toy experiments

Two small regression models, each trained in three variants:

- **no dropout**: plain max pooling;
- **dropout → max pool**: evaluated three ways (plain max, weighted pooling, and the average of 200 forward passes with dropout on, called "train-mode average"). The last one is the value the network was trained to give, so it shows the best that weighted pooling can do;
- **max pool → dropout**: the usual order, as a reference.

**Setup:**

- dropout q = 0.3; Adam, learning rate 1e-3 with cosine decay, 4,000 steps; MSE loss; 3 seeds per variant;
- each training batch is newly generated data, so the models cannot overfit. Dropout then has nothing to fix, and any loss from it comes from the train/eval mismatch.

Metrics: test MSE (mean ± std over seeds) and **scale** = mean prediction / mean target (1.0 = no bias).

### 5.1 Dense toy

**Input:** 8 numbers, each from N(0, 1).

**Target:** y = 2 + sin(x·w₀) + 0.5·(x·w₁)² + 0.3·cos(3·x·w₂), with fixed random vectors w₀, w₁, w₂ (entries N(0, 1)/√8, drawn once). The +2 keeps y positive (mean about 2.5), so the scale is meaningful.

**Model:** Linear(8→256) → ReLU → [pool over groups of 4 units] → Linear(64→128) → ReLU → [pool over groups of 2] → Linear(64→1). "Max pooling" here takes the max over groups of hidden units.

**Data:** 256 new samples per step; test set of 20,000 fixed samples.

| Model | Evaluation pooling | Test MSE | Scale |
|---|---|---|---|
| no dropout | plain max | 0.0081 ± 0.0018 | 1.001 |
| dropout → max pool | plain max | **0.920 ± 0.065** | **0.696** |
| dropout → max pool | weighted | 0.0260 ± 0.0094 | 0.978 |
| dropout → max pool | train-mode average | 0.0238 ± 0.0101 | 0.997 |
| max pool → dropout | plain max | 0.0943 ± 0.0085 | 0.927 |

### 5.2 CNN toy: estimating blur

**Input:** a 32×32 grayscale image, made in four steps:

1. pick a blur σ uniformly between 0.5 and 3.0 px;
2. make a 44×44 white-noise image (pixels uniform in [0, 1]);
3. blur it with a 13×13 Gaussian kernel of that σ, keeping only positions where the kernel fits fully (32×32 output, no edge effects);
4. add Gaussian noise with std 0.01.

**Target:** σ, one number. Small σ gives sharp, grainy texture, and large σ gives smooth blobs.

**Model:** two blocks of [conv 3×3 → ReLU → conv 3×3 → ReLU → pool 2×2] (16 and 32 channels), a third conv block without pooling (32 channels), global average pooling, Linear(32→1). Dropout is element-wise (`nn.Dropout`).

**Data:** 128 new images per step; test set of 4,000 fixed images.

| Model | Evaluation pooling | Test MSE | Scale |
|---|---|---|---|
| no dropout | plain max | 0.0112 ± 0.0012 | 0.999 |
| dropout → max pool | plain max | **0.341 ± 0.273** | **0.718** |
| dropout → max pool | weighted | 0.0116 ± 0.0013 | 0.996 |
| dropout → max pool | train-mode average | 0.0115 ± 0.0014 | 0.999 |
| max pool → dropout | plain max | 0.0207 ± 0.0077 | 0.955 |

Two simplifications of the CNN toy:

- White noise has texture everywhere, so every pixel shows the blur. Real images have flat regions where blur cannot be seen, so this task is easier than real blur estimation.
- A 13×13 kernel reaches ±6 px, which is only 2σ at σ = 3. The largest blurs are slightly less blurred than their label. This is the same for all variants, so the comparison is fair.

## 6. Conclusions from the toys

1. **Dropout right before max pooling, evaluated with plain max, is by far the worst variant.** Test error is about 110× (dense) and 30× (CNN) higher than without dropout. Predictions are about 30% off in scale. In the CNN, the RMSE is about 0.58 px on blurs of 0.5–3 px, versus 0.11 px without dropout. The large spread over seeds (± 0.27) shows that how badly a model is hit is also unpredictable.
2. **The network itself is fine. Only evaluation is wrong.** The same trained weights, averaged over dropout masks (train-mode average), are about as accurate as the no-dropout model in the CNN.
3. **Weighted pooling recovers almost all of it** with no retraining. Its error matches the train-mode average (0.0116 vs 0.0115 in the CNN, 0.026 vs 0.024 in the dense model). So the formula computes the value the network was trained to give, in one pass instead of 200.
4. **In these toys the predictions go down (scale about 0.7), but the direction is not fixed.** The pooled features are always larger in training than with plain max at evaluation. Whether that makes the final output larger or smaller depends on the signs of the later weights, so in other networks predictions can just as well come out too high.
5. **Dropout after the pool is a small cost here, not a fix.** It is somewhat worse than no dropout, because ReLU layers sit between dropout and the output and the ×1/(1−q) scaling is only exact for sums. Its error is still 10–16× smaller than dropout before the pool with plain max.
6. **What the toys do not show:** whether dropout before max pooling helps against overfitting. The data is unlimited, so there is no overfitting to reduce. Testing that needs a small fixed training set.

### Practical advice

- The simplest option: do not put dropout directly before a max pool. If regularization is needed, place dropout after the pool, use channel dropout (`Dropout2d`), or rely on other regularizers.
- If dropout before max pooling is wanted (for example for its regularizing effect), use weighted pooling at evaluation. It is a few lines of code, and models already trained this way can use it without retraining.
- A quick check for any network with dropout: compare the loss of the same batch in train mode (averaged over a few passes) and in eval mode. A large gap points to a train/eval mismatch like this one.

## References

- H. Wu, X. Gu. *Towards Dropout Training for Convolutional Neural Networks.* Neural Networks 71, 2015. [arXiv:1512.00242](https://arxiv.org/abs/1512.00242)
- G. Hinton et al. *Improving neural networks by preventing co-adaptation of feature detectors.* arXiv:1207.0580, 2012.
- M. Zeiler, R. Fergus. *Stochastic Pooling for Regularization of Deep Convolutional Neural Networks.* ICLR 2013.
