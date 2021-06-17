---
description: >-
  Prototype Machine Learning model to which it will be adapted to the social
  network, so that product recommendations are objective, enhancing the
  segmentation and personalization of ads.
---

# Woonkly feed algorithm - objective segmentation

Data recommendation system to users through Tensor Flow for the Woonkly social network  
  
Real-world recommendation systems are often made up of two stages:

1. The recovery stage is responsible for selecting an initial pool of hundreds of candidates from all possible candidates. The main objective of this model is to efficiently eliminate all candidates that the user is not interested in. Because the recovery model can be dealing with millions of candidates, it has to be computationally efficient.
2. The classification stage takes the results of the recovery model and adjusts them to select the best possible handful of recommendations. Your task is to reduce the set of elements that may interest the user to a short list of possible candidates.

### **Imports**[**¶**](https://render.githubusercontent.com/view/ipynb?color_mode=dark&commit=6234c88f0660ab7c4605a394529208368e6bafa3&enc_url=68747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74656e736f72666c6f772f7265636f6d6d656e646572732f363233346338386630363630616237633436303561333934353239323038333638653662616661332f646f63732f6578616d706c65732f62617369635f72616e6b696e672e6970796e62&nwo=tensorflow%2Frecommenders&path=docs%2Fexamples%2Fbasic_ranking.ipynb&repository_id=275252389&repository_type=Repository#Imports)

Let's first get our imports out of the way.



```python
!pip install -q tensorflow-recommenders
!pip install -q --upgrade tensorflow-datasets
```

In \[ \]:

```python
import os
import pprint
import tempfile

from typing import Dict, Text

import numpy as np
import tensorflow as tf
import tensorflow_datasets as tfds
```

In \[ \]:

```python
import tensorflow_recommenders as tfrs
```

### 

**Preparing the dataset**

We're going to use the same data

This time, we're also going to keep the ratings: these are the objectives we are trying to predict

```python
ratings = tfds.load("movielens/100k-ratings", split="train")

ratings = ratings.map(lambda x: {
    "movie_title": x["movie_title"],
    "user_id": x["user_id"],
    "user_rating": x["user_rating"]
})
```

As before, we'll split the data by putting 80% of the ratings in the train set, and 20% in the test set. 

```python
tf.random.set_seed(42)
shuffled = ratings.shuffle(100_000, seed=42, reshuffle_each_iteration=False)

train = shuffled.take(80_000)
test = shuffled.skip(80_000).take(20_000)
```

Let's also figure out unique user ids and movie titles present in the data.

This is important because we need to be able to map the raw values of our categorical features to embedding vectors in our models. To do that, we need a vocabulary that maps a raw feature value to an integer in a contiguous range: this allows us to look up the corresponding embeddings in our embedding.



```python
movie_titles = ratings.batch(1_000_000).map(lambda x: x["movie_title"])
user_ids = ratings.batch(1_000_000).map(lambda x: x["user_id"])

unique_movie_titles = np.unique(np.concatenate(list(movie_titles)))
unique_user_ids = np.unique(np.concatenate(list(user_ids)))
```

### **Implementing a model**

#### **Architecture**

Ranking models do not face the same efficiency constraints as retrieval models do, and so we have a little bit more freedom in our choice of architectures.  
  
****A model composed of multiple stacked dense layers is a relatively common architecture for ranking tasks. We can implement it as follows:



```python
class RankingModel(tf.keras.Model):

  def __init__(self):
    super().__init__()
    embedding_dimension = 32

    # Compute embeddings for users.
    self.user_embeddings = tf.keras.Sequential([
      tf.keras.layers.experimental.preprocessing.StringLookup(
        vocabulary=unique_user_ids, mask_token=None),
      tf.keras.layers.Embedding(len(unique_user_ids) + 1, embedding_dimension)
    ])

    # Compute embeddings for movies.
    self.movie_embeddings = tf.keras.Sequential([
      tf.keras.layers.experimental.preprocessing.StringLookup(
        vocabulary=unique_movie_titles, mask_token=None),
      tf.keras.layers.Embedding(len(unique_movie_titles) + 1, embedding_dimension)
    ])

    # Compute predictions.
    self.ratings = tf.keras.Sequential([
      # Learn multiple dense layers.
      tf.keras.layers.Dense(256, activation="relu"),
      tf.keras.layers.Dense(64, activation="relu"),
      # Make rating predictions in the final layer.
      tf.keras.layers.Dense(1)
  ])
    
  def call(self, inputs):

    user_id, movie_title = inputs

    user_embedding = self.user_embeddings(user_id)
    movie_embedding = self.movie_embeddings(movie_title)

    return self.ratings(tf.concat([user_embedding, movie_embedding], axis=1))
```

This model takes user ids and movie titles, and outputs a predicted rating:

```python
RankingModel()((["42"], ["One Flew Over the Cuckoo's Nest (1975)"]))
```

**Loss and metrics**

The next component is the loss used to train our model. TFRS has several loss layers and tasks to make this easy.

In this instance, we'll make use of the Ranking task object: a convenience wrapper that bundles together the loss function and metric computation.

We'll use it together with the MeanSquaredError Keras loss in order to predict the ratings.

```python
task = tfrs.tasks.Ranking(
  loss = tf.keras.losses.MeanSquaredError(),
  metrics=[tf.keras.metrics.RootMeanSquaredError()]
)
```

The task itself is a Keras layer that takes true and predicted as arguments, and returns the computed loss. We'll use that to implement the model's training loop.

#### **The full model**

We can now put it all together into a model. TFRS exposes a base model class \(tfrs.models.Model\) which streamlines building models: all we need to do is to set up the components in the \_\_init\_\_ method, and implement the compute\_loss method, taking in the raw features and returning a loss value.

The base model will then take care of creating the appropriate training loop to fit our model.

\*\*\*\*

```python
class MovielensModel(tfrs.models.Model):

  def __init__(self):
    super().__init__()
    self.ranking_model: tf.keras.Model = RankingModel()
    self.task: tf.keras.layers.Layer = tfrs.tasks.Ranking(
      loss = tf.keras.losses.MeanSquaredError(),
      metrics=[tf.keras.metrics.RootMeanSquaredError()]
    )

  def compute_loss(self, features: Dict[Text, tf.Tensor], training=False) -> tf.Tensor:
    rating_predictions = self.ranking_model(
        (features["user_id"], features["movie_title"]))

    # The task computes the loss and the metrics.
    return self.task(labels=features["user_rating"], predictions=rating_predictions)
```

**Fitting and evaluating**

After defining the model, we can use standard Keras fitting and evaluation routines to fit and evaluate the model.

Let's first instantiate the model.

```python
model = MovielensModel()
model.compile(optimizer=tf.keras.optimizers.Adagrad(learning_rate=0.1))
```

Then shuffle, batch, and cache the training and evaluation data.

```python
cached_train = train.shuffle(100_000).batch(8192).cache()
cached_test = test.batch(4096).cache()
```

Then train the model:

```text
model.fit(cached_train, epochs=3)
```

As the model trains, the loss is falling and the RMSE metric is improving.

Finally, we can evaluate our model on the test set:

```text
model.evaluate(cached_test, return_dict=True)
```

