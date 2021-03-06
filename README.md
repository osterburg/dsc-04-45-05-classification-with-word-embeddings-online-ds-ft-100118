
# Classification with Word Embeddings

## Introduction

In this lesson, we'll discuss the practical aspects of how we can use Word Embeddings and Word2Vec models for the task of text classification!


## Objectives

You will be able to:

* Understand and explain the concept of a mean word embedding, and how this can be used to vectorize text at the sentence, paragraph, or document level
* Import and use pretrained word embeddings from popular pretrained models such as GloVe
* Effectively incorporate embedding layers into neural networks using Keras


## Getting Started

In this lesson, we'll focus mainly on the practical aspects of how we can use Word2Vec and Word Embeddings to improve our Text Classification models. We'll start by learning about how we can make use of **_Transfer Learning_** by loading pre-trained word vectors into our Word2Vec model. Then, we'll learn about how we can get the word vectors we need and combine them into **_Mean Word Vectors_**, and how we can streamline this process by writing our own vectorizer class that is compatible with scikit-learn pipelines. Finally, we'll end this lesson by examining how we can train Deep Neural Networks that include their own Word Embedding Layers, and how we can Keras to preprocess our text data to make everything run smoothly!


## Using Pretrained Word Vectors With GloVe

Perhaps the single best way to improve performance for text classification is to make use of weights from a Word2Vec model that has been trained for a very long time on a massive amount of text data. With Deep Learning, more data is almost always the single best thing that can improve model performance, and the embedded word vectors created by a Word2Vec model are no exception. For this reason, it's almost always a good idea to load one of the top-tier, industry-standard models that been open sourced for this exact purpose. The most common model to use for this is the **_GloVe_** (short for **_Global Vectors for Word Representation_**) model by the Stanford NLP Group. This model is trained on massive datasets, such as the entirety of wikipedia, for a very long time on server clusters with multiple GPUs. It would be absolutely impossible for us to train a model of similar quality on our own machines--however, because they've open-sourced the model weights, we don't actually need to! Instead, all we need to do is download the weights and go from there. 

For text classification purposes, loading the weights precludes the need for us to instantiate or train a Word2Vec model entirely--instead, we just need to:

* Get the total vocabulary in our dataset
* Download and unzip the GloVe file needed from the Stanford NLP Group's website
* Read the GloVe file, and save only the vectors that correspond to the words that appear in the vocabulary of our dataset.

This can be a fairly involved process, so the code for this is provided for you in the next lab. We strongly encourage you to take some time and examine this code until you have a general idea of what it's doing!


## Mean Word Embeddings

Loading a pretrained model like GloVe may provide us with the most accurate word vectors we could possibly hope, but each vector is still just for a single word. We can't use them for classification as is at this stage, because it's highly likely that any Text Classification we'll be performing will be focused on arbitrarily-sized blobs of text, such as sentences or paragraphs. So, how do we get these sentences and paragraphs into a format that our models can use for classification, while making use of the word vectors we've gotten from GloVe?

The answer is we need to compute a **_Mean Word Embedding_**. The idea behind this is simple. To get the vector representation for any arbitrarily-sized block of text, all we need to do is get the vector for every individual word that appears in that block of text, and average them together! The benefit of this is that no matter how big or small that block of text is, the Mean Word Embedding of that sentence will be the same size as all of the others, because the vectors we're averaging together all have the exact same dimensionality! This makes it a simple matter to get a block of text into a format that we can use with traditional Supervised Learning models such as Support Vector Machines or Gradient Boosted Trees. 


### Working With scikit-learn pipelines

As you'll see in the next lab, it's worth the extra bit of work to build a class that works with the requirements of a scikit-learn `Pipeline` object, so that we can we can pass the data straight in and generate the mean word embeddings on the fly. This way, we don't need to write the same set of code twice to generate mean word embeddings for both the training and testing set, for instance. This is also important if our dataset is too large to fit into memory, so that we can partially train our models (when applicable) and load in different chunks of the dataset. By building a Vectorizer class that handles creating the Mean Word Embeddings for us rather than just writing the code procedurally, we save ourselves a lot of work in the long run!

The code for the Mean Embedding Vectorizer class is also provided for you in the next lab. As you'll see, the class requires both a `fit()` and a `transform()` method to be compliant with scikit-learn `Pipeline` objects. We recommend you take some time to also study this code until you understand what it's doing--it isn't complex, and understanding how to do this yourself will pay dividends in the long run--after all, writing clean, reusable code always does!


## Deep Learning & Embedding Layers

One problem you may have noticed with the Mean Word Embedding strategy is that by combining all the words, we lose some information that is contained in the sequence of the words. In natural language, the position and phrasing of words in a sentence can often contain information that we pick up on. This is a downside to this approach, and one of the reasons why **_Sequence Models_** tend to outperform all of the 'shallow' algorithms (note: this term just refers to any Machine Learning algorithms that do not fall under the umbrella of Deep Learning--it doesn't make any judgments about whether they are better or worse, as that is almost always dependent on the situation!). In the next lesson, we'll learn all about sequence models such as **_Recurrent Neural Networks_** and other variants such as **_Long Short Term Memory Cells_**. However, in the next lab, we'll see an example of these, just so that we can see a live example of how we can make use of **_Embedding Layers_** directly in our Neural Networks!

An **_Embedding Layer_** is just a layer that learns the word embeddings for our dataset on the fly, right there inside the Neural Network. Essentially, its a way to make use of all the benefits of Word2Vec, without worrying about finding a way to include a separately trained Word2Vec model's output into our Neural Networks (which are probably already complicated enough!). You'll see an example of an **_Embedding Layer_** in the next lab. You should make note of a couple caveats that come with using embedding layers in your Neural Network--namely:

* The Embedding Layer must always be the first layer of the network, meaning that it should immediately follow the `Input()` layer
* All words in the text should be integer-encoded, with each unique word encoded as it's own unique integer. 
* The size of the Embedding Layer must always be greater than the total vocabulary size of the dataset! The first parameter denotes the vocabulary size, while the second denotes the size of the actual word vectors
* The size of the sequences passed in as data must be set when creating the layer (all data will be converted to padded sequences of the same size during the preprocessing step). 

In the next lab, we'll make use of Keras's text preprocessing tools to convert the data from text to a tokenized format. Then, we'll convert the tokenized sentences to sequences. Finally, we'll pad the sequences, so that they're all the same length. During this step, we'll exclusively make use of the preprocessing tools provided by Keras. Don't worry if this all seems a bit complex right now--as you'll soon see, this is actually the most straightforward part of the next lab!

For a full rundown of how to use Embedding Layers in Keras, see the [Keras Documentation for Embedding Layers](https://keras.io/layers/embeddings/).



## Summary

In this lesson, we focused on the practical and pragmatic elements of using Word2Vec and Word Embeddings for Text Classification. We learned about how to load professional-quality pretrained word vectors with the Stanford NLP Group's open source GloVe data, as well as how to generate mean word embeddings that work with scikit-learn pipelines, and how to add embedding layers into our Neural Networks with Keras!
