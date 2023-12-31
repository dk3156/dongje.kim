---
title: "Sentiment Analysis App"
mainSectionTitle: "AI"
---
<div align="center">

##  Toxic Comment Classification with 🤗 Transformers

Link to my Huggingface space:
https://huggingface.co/spaces/dk3156/sentiment_analysis_app

Link to my github repo:
https://github.com/dk3156/sentiment_analysis_app

</div>

## Description

A research initiative founded by Jigsaw and Google (both a part of Alphabet) are working on tools to help improve online conversation. One area of focus is the study of negative online behaviors, like toxic comments (i.e. comments that are rude, disrespectful or otherwise likely to make someone leave a discussion). So far they’ve built a range of publicly available models served through the Perspective API, including toxicity.

If words that are associated with swearing, insults or profanity are present in a comment, it is likely that it will be classified as toxic, regardless of the tone or the intent of the author e.g. humorous/self-deprecating. This could present some biases towards already vulnerable minority groups.

The purpose of the project is to deploy a streamlit application that utilizes a multi-headed model for predicting tweet toxicity. This will involve leveraging the Hugging Face transformer library and fine-tuning the model on an existing dataset to enhance its ability to detect harmful content in tweets more quickly and efficiently.

The github repository provides the trained models & code to predict toxic comments on a Jigsaw challenge: Toxic comment classification.
This is the part of Project Milestones for Artificial Intelligence course at NYU, Tandon School of Engineering.

The dataset used in this project can be found at: https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge 
<br>
The pretrained finetune model application can be viewed at: https://huggingface.co/dk3156/toxic_tweets_model

## WalkThrough
The huggingface app displays the first ten toxic tweets of the dataset as default. 
Users are prompted to enter a text along with language of model of their choices: 

- Finetune: The multi-headed model trained with Jigswaw's classification challenge will be used to analyze the text.
  - Upon runnning the application, users will be displayed with detailed analysis of toxicity scores of the following lables:
  
    - `toxic`
    - `severe_toxic`
    - `obscene`
    - `threat`
    - `insult`
    - `identity_hate`
   
  - The pre-trained model was fine-tuned for sequence classification using the following hyperparameters, which were selected from a validation set:

    Batch size = 16
    Learning rate = 5e-5
    Epochs = 1
    
- Binary: Pre-trained Distilbert language model will be used to analyze the text.
  - the result is displayed with the following lables:
  
    - Positive
    - Negative

   - distilbert-base-uncased-finetuned model of Huggingface transfomer is implemented.
   
- Non-Binary: Pre-trained Roberta language modell will be used to analyze the text. 
  - the result is displayed with the following lables:
  
    - anger
    - disgust
    - fear
    - joy
    - neutral
    - sadness
    
  - distilroberta-base model of Huggingface transformer is implemented. 
 Scores ranges from 0 to 1. Heavier scores will be displayed first.

## Documentation
A detailed documentation for each part of the code is provided in the folder /documentations
- app.py 
- finetune.py

## Google Site
The following link connects to the landing site for the Huggingface streamlit app. Links to both my github repo and Huggingface are provided in the site.
<br>
https://sites.google.com/nyu.edu/sentimentanalysis/home

## Demo
The following link is the tutorial video for demonstration purposes. 

{{< youtube DEza2H9qdpk>}}


## Performance Metrics

F1 and Recall scores for the first five comments

<img width="261" alt="metric5" src="https://user-images.githubusercontent.com/78196670/235417649-a6aca9c7-874d-4398-aadd-486777285207.png">

F1 score visualization in box-plot

<img width="500" src="https://user-images.githubusercontent.com/78196670/235417878-24b142e7-066d-4ea2-bf9c-77f2da81ed5a.png">
<br>
<img width="500" src="https://user-images.githubusercontent.com/78196670/235417936-db792c34-3df5-42ec-b002-64b3d05b2cb5.png"><br>
<br>
<img width="500" src="https://user-images.githubusercontent.com/78196670/235418023-213650db-11ca-4768-8a28-387d7703063f.png">
<br>
<img width="500" src="https://user-images.githubusercontent.com/78196670/235418023-213650db-11ca-4768-8a28-387d7703063f.png">
<br>
<img width="500" src="https://user-images.githubusercontent.com/78196670/235418082-1051e337-00dc-416b-b3a6-18c88922e4a7.png">

