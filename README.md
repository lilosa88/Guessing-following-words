# Guessing-following-words
# Objective

- This project was carried out as a part of a specialization called [DeepLearning.AI TensorFlow Developer Specialization](https://www.coursera.org/account/accomplishments/specialization/certificate/L6R6AFWVXHZT) which is given by DeepLearning.AI. This specialization is conformed by 4 courses: 
1. Introduction to TensorFlow for Artificial Intelligence, Machine Learning, and Deep Learning 
2. Convolutional Neural Networks in TensorFlow 
3. Natural Language Processing in TensorFlow 
4. Sequences, Time Series and Prediction

  Specifically this project is part of the third course in this specialization. 
  
- We taken a traditional Irish song and the purpouse is to create a model that allow us to guees the following word in a sentence.


# Code and Resources Used

- **Phyton Version:** 3.0
- **Packages:** pandas, numpy, sklearn, re, nltk, json, keras, tensorflow

# Data description

- We put the entire Irish sond into a single string with the simbol \n denoting line breaks. 

# Preprocessing

- By calling the split function on \n, I create a Python list of sentences(corpus) from the data and I convert all to lowercase.
- Using the tokenizer, specifically the method fit_on_texts I create the dictionary of words. 
- By taking the corpus(text) we turn it into training data. We do this by:
  - For each line in the corpus, we'll generate a token list using the tokenizers, specifically the texts_to_sequences method. 
  - We iterate over this list of tokens and create a number of n_grams_sequences, namely the first two words in the sentence, then the first three words and so       on. The result is somehting like this: for the first line in the song, the following input sequences will be generated.

 <p align="center">
  <img src="https://github.com/lilosa88/Guessing-following-words/blob/main/Images/Captura%20de%20Pantalla%202021-05-31%20a%20la(s)%2018.40.21.png" width="320" height="200">
 </p>  

  - The same process will happen for each line. Therefore the input sequences are simply the sentences being broken down into phrases, the first two words, the       first three words, etc. 
  - We found the length of the longest sentence in the corpus. To do this, we iterate over all of the sequences and find the longest one.
  -  Once we have our longest sequence length, the next thing to do is pad all of the sequences so that they are the same length. So now we have somehting like:

 <p align="center">
  <img src="https://github.com/lilosa88/Guessing-following-words/blob/main/Images/Captura%20de%20Pantalla%202021-05-31%20a%20la(s)%2018.52.02.png" width="320" height="200">
 </p> 
 
  - We turn tthis into x's and y's, i.e. the input values and their labels. When you think about it, now that the sentences are represented in this way, all we       have to do is take all but the last character as the x and then use the last character as the y on our label. 
  - I did one-hot encode into the labels as this really is a classification problem. So to one-hot encode, I can use the keras utility to convert a list to a         categorical. I simply give it the list of labels and the number of classes which is my number of words, and it will create a one-hot encoding of the labels. 
    So for example, I will obtain something like: if we consider this list of tokens as a sentence, then the x is the list up to the last value, and the label is     the last value which in this case is 70. The y is a one-hot encoded array whether length is the size of the corpus of words and the value that is set to one    is the one at the index of the label which in this case is the 70th element. 
    
  <p align="center">
  <img src="https://github.com/lilosa88/Guessing-following-words/blob/main/Images/Captura%20de%20Pantalla%202021-05-31%20a%20la(s)%2018.59.31.png" width="320" height="200">
 </p> 
 
 
# Neural Network

  - This model was created using tf.keras.models.Sequential, which defines a SEQUENCE of layers in the neural network. These sequence of layers used were the following:
  - One Embedding layer:  This is the process that help us to go from just a string of numbers representing words to actually get text sentiment. This process is     called embedding, with the idea being that words and associated words are clustered as vectors in a multi-dimensional space. We'll want it to handle all of       our words, so we set that in the first parameter. The second parameter is the number of dimensions to use to plot the vector for a word. I'm going to keep it     at 64 for now. Finally, the size of the input dimensions will be fed in, and this is the length of the longest sequence minus 1. We subtract one because we       cropped off the last word of each sequence to get the label, so our sequences will be one less than the maximum sequence length.
  - One Bidirectional LSTM layer: I use 20 units.
  - One Dense layers: This adds a layer of neurons. Each layer of neurons has an activation function to tell them what to do. Therefore, the Dense layer              consisted in 263 neurons which correspond to the total number of words, which is the same size that we used for the one-hot encoding. Thus this layer will have one neuron, per word and that neuron should light up when we predict a given word. Softmax is the choseen activation function.

- We built this model using adam optimizer and categorical_crossentropy as loss function, as we're classifying to different classes.

- The number of epochs=500

- We obtained Accuracy 0.9536 for the train data.
  
 <p align="center">
  <img src="https://github.com/lilosa88/Guessing-following-words/blob/main/Images/Screenshot%20from%202021-05-31%2019-05-51.png" width="320" height="260">
 </p>  
