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
  - We next need to find the length of the longest sentence in the corpus. To do this, we'll iterate over all of the sequences and find the longest one with code like this. Once we have our longest sequence length, the next thing to do is pad all of the sequences so that they are the same length. We will pre-pad with zeros to make it easier to extract the label, you'll see that in a few moments. So now, our line will be represented by a set of padded input sequences that looks like this. Now, that we have our sequences, the next thing we need to do is turn them into x's and y's, our input values and their labels. When you think about it, now that the sentences are represented in this way, all we have to do is take all but the last character as the x and then use the last character as the y on our label. We do that like this, where for the first sequence, everything up to the four is our input and the two is our label. Similarly, here for the second sequence where the input is two words and the label is the third word, tokenized to 66. Here, the input is three words and the label is eight, which was the fourth word in the sentence. By this point, it should be clear why we did pre-padding, because it makes it much easier for us to get the label simply by grabbing the last token.
 
 
# Neural Network
  
  - This model was created using tf.keras.models.Sequential, which defines a SEQUENCE of layers in the neural network. These sequence of layers used were the following:
  - One Embedding layer:  This is the process that help us to go from just a string of numbers representing words to actually get text sentiment. This process is     called embedding, with the idea being that words and associated words are clustered as vectors in a multi-dimensional space. 
  - One GlobalAveragePooling1D layer
  - Two Dense layers: This adds a layer of neurons. Each layer of neurons has an activation function to tell them what to do. Therefore, the first Dense layer       consisted in 24 neurons with relu as an activation function. The second, have 1 neuron and sigmoid as activation function. 

- We built this model using adam optimizer and binary_crossentropy as loss function, as we're classifying to different classes.

- The number of epochs=30

- We obtained Accuracy 0.8800 for the train data and Accuracy 0.8171 for the validation data.
  
 <p align="center">
  <img src="https://github.com/lilosa88/Sarcasm-detection/blob/main/Images/Screenshot%20from%202021-05-31%2016-10-14.png" width="320" height="460">
 </p>  
