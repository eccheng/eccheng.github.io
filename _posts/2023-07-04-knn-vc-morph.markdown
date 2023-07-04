---
layout: post
title:  "Morphing between voices using kNN-VC"
date:   2023-07-04
categories: ml audio vc
---

This demo resulted from my playing around with [kNN-VC][knn-vc], a new simplified voice conversion method introduced by [Baas et al. (2023)][orig-paper].

A benefit of their approach is that the self-supervised feature vectors always inhabit a shared latent space, regardless of which target speaker we are generating for. As a result, simply by interpolating the feature vectors returned from the k-nearest neighbors matching step, we can easily interpolate (or extrapolate) between the voices of different target speakers. In fact, we can even morph continuously between any number of target speakers within a single utterance. Doing this produces the results shown below.

While I'm not entirely sure how novel these capabilities are (speaker interpolation, at least, is certainly possible with prior models), I was impressed by how well this seems to work out of the box with kNN-VC. At the very least, it's a cool effect to hear one voice seamlessly blending into another!

Everything here was produced using [a very slight fork of kNN-VC][my-fork]; see the [Colab notebook][notebook] for the generation process.

#### Interpolation example 1

| Source | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/source.wav"></audio> |
| Speaker A[^A] | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/lj.wav"></audio> |
| Speaker B[^B] | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/587.wav"></audio> |
| 50% A + 50% B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/lj_587_midpoint.wav"></audio> |
| Morph A -> B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/lj_to_587.wav"></audio> |

#### Interpolation example 2

| Source | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/source.wav"></audio> |
| Speaker C[^C] | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/1334.wav"></audio> |
| Speaker B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/587.wav"></audio> |
| 50% C + 50% B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/1334_587_midpoint.wav"></audio> |
| Morph C -> B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/1334_to_587.wav"></audio> |

#### Extrapolation example

| Source | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/source.wav"></audio> |
| Speaker D[^D] | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/1235.wav"></audio> |
| Speaker B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/587.wav"></audio> |
| 150% D - 50% B | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/1235_extrap_from_587.wav"></audio> |
| 150% B - 50% D | <audio controls src="/post_assets/2023-07-04-knn-vc-morph/587_extrap_from_1235.wav"></audio> |


[^A]: speaker from [LJSpeech][ljspeech]
[^B]: speaker ID `587` from [LibriSpeech][librispeech] `train_clean_100` split
[^C]: speaker ID `1334` from [LibriSpeech][librispeech] `train_clean_100` split
[^D]: speaker ID `1235` from [LibriSpeech][librispeech] `train_clean_100` split

[knn-vc]: https://bshall.github.io/knn-vc/
[orig-paper]: https://arxiv.org/pdf/2305.18975.pdf
[my-fork]: https://github.com/eccheng/knn-vc
[notebook]: https://colab.research.google.com/github/eccheng/knn-vc/blob/master/knnvc_interpolation.ipynb
[ljspeech]: https://keithito.com/LJ-Speech-Dataset/
[librispeech]: https://www.openslr.org/12
