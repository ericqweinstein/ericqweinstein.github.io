---
layout: post
title: On Translation and Neural Networks
date: 2016-01-24 14:00:00
summary: Translation, machine learning, &c.
categories: machine learning translation clojure neural networks
---

I'm sort of staggered by the parallels between poetry and programming. Both are (relatively) compact modes of expression and are designed to _do_ something in the world. They concern themselves with syntax, idiom, style, form, and clarity. Both are, in my opinion, read much less frequently and widely than they should be. I believe that, fundamentally, poems _are_ programs&mdash;they're just programs meant to elicit emotional responses in humans rather than perform computations on machines.

One of the ways I grew most as a poet was by doing translations, usually from French and German (I particularly like translating [Jacques Pr&eacute;vert](https://en.wikipedia.org/wiki/Jacques_Pr%C3%A9vert) and [Rilke](https://en.wikipedia.org/wiki/Rainer_Maria_Rilke)). Translation makes us carefully and deeply examine our assumptions about language and how we view the world, and it's one of the few ways we grow both as practitioners of a new skill (the foreign language) and those we take for granted (thinking and writing clearly). I've come to view the translation of ideas as fundamental to the critical examination of those ideas, so I do my best to apply translation as a clarifying tool whenever possible.

I became very interested in machine learning and neural networks last year. I'm in the process of [reading a bunch of papers](reading/learning/2015/12/27/reading-list-2016/) and taking [a course](https://www.udacity.com/course/viewer#!/c-ud120) or [two](https://www.udacity.com/course/viewer#!/c-ud730/l-6370362152/m-6379811815) on Udacity, and while doing all this I stumbled across [this tutorial](http://iamtrask.github.io/2015/07/12/basic-python-network/") explaining simple neural networks and backpropagation. I found this post helpful and wonderfully readable, but even though I'm literate in Python, I found myself constantly looking up what the [NumPy](http://www.numpy.org/) functions were doing. I figured that since translation has served me so well in poetry, it might be a nice exercise to translate this bit of Python into something I know a little better. So! I translated Part I of the tutorial (originally in Python + NumPy) to Clojure + the [core.matrix library](https://github.com/mikera/core.matrix). It looks like this:

```clj
(ns neural.net
  {:doc "A little neural network written in Clojure.
        See: http://iamtrask.github.io/2015/07/12/basic-python-network/"
   :author "Eric Weinstein <eric.q.weinstein@gmail.com>"}
  (:require [clojure.core.matrix :as m]
            [clojure.pprint :refer [pprint]]))

(defn- sigmoid
  "Our nonlinear activation function. See:
  https://en.wikipedia.org/wiki/Sigmoid_function"
  [n]
  (/ 1 (+ 1 (Math/exp (* -1 n)))))

(defn- sigmoid-deriv
  "Derivative of the sigmoid function. See:
  http://www.ai.mit.edu/courses/6.892/lecture8-html/sld015.htm"
  [n]
  (* n (- 1 n)))

(def input
  "Input data!"
  [[0 0 1]
   [0 1 1]
   [1 0 1]
   [1 1 1]])

(def output
  "Output data!"
  [[0] [0] [1] [1]])

(defn- synapse
  "Creates a synapse with random weights. Example:

  (synapse 3 2)
  ;; => [[-0.5  -0.1]
         [-0.25 -0.2]
         [-0.1  -0.3]]"
  [row col]
  (partition col
    (mapv #(* 2 (- % 0.5))
          (take (* col row)
                (repeatedly rand)))))

;; Create a synapse (a connection between
;; our input and hidden layers, which are
;; layer-0 and layer-1 below, respectively).
(def synapse-0 (atom (synapse 3 1)))

(defn- train
  "Trains our neural network. Takes a number, n,
  which is the number of iterations to run."
  [n]
  (dotimes [i n]
    (let [layer-0 input
          ;; Feed forward through our hidden layer
          layer-1 (mapv #(mapv sigmoid %) (m/dot layer-0 @synapse-0))
          ;; How much were we off by?
          error   (m/sub output layer-1)
          ;; Multiply our error by the slope of the
          ;; sigmoid function at the values in layer-1
          delta   (m/dot error (mapv #(map sigmoid-deriv %) layer-1))]
      ;; Update our weights
      (swap! synapse-0 (partial m/add (m/dot (m/transpose layer-0) delta)))
      ;; How are we doing?
      (pprint layer-1))))

(defn -main
  [& args]
  (train 10000))
```

This is a reasonably faithful translation from the original Python, though there are a few bits and pieces that aren't perfect:

1. I'm not jazzed about the continual rebinding (`let` inside `dotimes`).
2. This version prints much more frequently than the original (every line rather than just at the end), but I wanted to watch the hidden layer change with each iteration.
3. I'm not thrilled that I'm not seeding `rand` each time, so my results aren't reproducible. You _can_ do it by redefining `clojure.core/rand`:

```clj
(def r (java.util.Random. 0))

(defn rand
  ([]  (.nextDouble r))
  ([n] (.nextInt r n)))
```

...but I'm not sure I recommend it. Redefining core functions can lead to wonky behavior, and I'm sure there's a better way to ensure reproducibility (either a more elegant Java interop or a numerical/machine learning library that abstracts this away). The point though, I think, is that I understand the code and the library calls in a way that I wouldn't have had I simply typed the program from memory or just read through the NumPy documentation.

Translation, like teaching, requires a thorough understanding&mdash;a filling-in of all the blank spots on the map&mdash;and I think this is crucial to deep learning (pun super intended) for everything from nineteenth-century German poetry to twenty-first century neural networks.
