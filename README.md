# Music-Taste-as-a-Function-of-Happiness

## Music and Mental Health: A Comparative Analysis of Classification and Neural Network Techniques

by Shon Shtern, Elizabeth Kelly, Shiying Cheng, Jenna Lorio, and Nithishma Allu

## Introduction
Mental health and the status of a person’s emotional and psychological well-being can be greatly impacted by one’s music listening habits, in both positive and negative ways. In this paper, we aim to find a link between a person’s mental health status and their music listening habits to predict their favorite musical genre. In the process, we explored how these favorite genre predictions can be made using various machine-learning techniques, such as decision trees, support vector machines, and neural networks. Overall, we hypothesized that a person’s music choice will have a strong correlation with their mental and emotional health.
Background/Processing Techniques
Since the dataset is small, mean, mode, and KNN imputation were used to fill missing values instead of dropping rows. Timestamp and permissions columns were dropped as they do not contribute to prediction accuracy. BPM was also dropped since there are a lot of missing values in this column and it has a low correlation with the predicted variable. The frequency column was ordinally encoded ranging from zero to three for never to very frequently. The column that represents the music's impact on mood was also ordinally encoded with -1 for worsening mood, 0 for having no effect on mood, and 1 for improving mood. Finally, the remaining categorical variables were one hot encoded. A stratified split was used to divide the train and test sets to ensure a proportionate amount of data corresponding with each genre was in the test set. Since the data set is small with 16 classes, SMOTE based on one neighbor was used to increase the training set. This helped handle the class imbalance in the data set.
Decision Trees
One machine learning model we attempted was a decision tree. A decision tree could be a good representation of the data set since they are known to capture nonlinear relationships between features. Additionally, decision trees are easily interpretable and require minimal preprocessing. Thus, these models are easy to create, analyze, and deploy if they meet the desired results.
	
## **Random Forests**

The random forest classifier, an ensemble of decision trees, was used to predict individuals' favorite music genre from the dataset, with an accuracy of approximately 62.33%. 
Given the complexity of personal preference data, the model displayed reasonable predictive power. The accuracy demonstrates the classifier's ability to recognize patterns and correlations in the data but also indicates room for improvement. The random forest's performance in this situation is most likely influenced by the nature of the data, which includes variance and potential overlap in variables that determine musical genres, as well as uniqueness in genre preference.
         To further enhance the model's performance, hyperparameter tuning was performed using GridSearchCV. The optimal parameters were a deep tree structure with a maximum depth of 30 and a large number of estimators set to 125. Despite these optimal settings, the tuned model did not outperform the baseline accuracy, indicating that the model complexity was sufficient with the default parameters and that overfitting was unlikely. The reasons for this plateau in performance improvement could be attributed to a variety of factors, including the possibility that the most significant features were already captured by the default model or individual uniqueness in musical preferences may not be fully captured by the available features and may require more sophisticated modeling techniques to discern.
       The insights from this analysis suggest that while the random forest is a robust classifier, its effectiveness is inherently tied to the quality and nature of the data at hand. In cases like predicting personal music preferences, where subjectivity and high variability are expected, even a well-tuned model may face limitations that stem from the data itself rather than the modeling approach.

## **Support Vector Machines and Logistic Regression**

Multiple support vector machine models were built with various strategies of picking optimal hyperparameters. Several Linear SVC models were trained using both dual and primal on both scaled and unscaled data after performing grid search to find the best hyperparameters. Models were also trained using radial basis function and polynomial kernels with their corresponding optimal hyperparameters found in grid search. Two linear SVC models were also trained, setting the decision_function_shape hyperparameter to one-vs-one and one-vs-rest. Of all of the models, the linear SVC model with default parameters trained on the unscaled data performed the best. Finally, multinomial logistic regression models were trained on both the scaled and unscaled data. Overall, the best performance was from the logistic regression model trained on the unscaled data with a test score of 0.6027. 
In an effort to improve this score, the feature importances were studied for each class using two metrics. First, the least important coefficient from each model, determined by the absolute smallest coefficient, was found and the most common unimportant coefficient was found to be ‘Primary streaming service_Pandora’. Another metric was taking the mean of the absolute value of the coefficients for every class. This found the two least important variables to be 'Age' and 'Primary streaming service_Pandora’. The logistic regression model was retrained after dropping combinations of these features. However, the best performing model remained the logistic regression model trained on the entire feature set with a test score of 0.6027.

## **Convolutional Neural Networks**

Moving away from classifiers and towards neural networks, the first type of model we trained was a Convolutional Neural Network (CNN). As described in the network diagram, the model takes as input a feature embedding using all features from our dataset and outputs a probability distribution over the favorite genre target classes. The class with the highest probability then becomes the predicted label for the model. As we were dealing with a smaller dataset, we needed to consider methods to prevent overfitting as much as possible while training the model. These methods included opting for a simpler CNN architecture and utilizing both Batch Normalization and Dropout techniques to allow for the model to learn more general patterns in the data. 
The best-performing model was trained for a total of 30 epochs with a batch size of 16 using an RMSProp optimizer and categorical cross-entropy loss formula. The batch size, number of epochs, and optimizer type for the model were tuned, cross-validated, and selected via Grid Search. Since we have balanced our dataset using balancing techniques mentioned earlier in this paper, we used model accuracy as the performance metric to evaluate the model’s performance. The observed loss results and accuracy for the trained optimized model can be viewed in the plots below.
Overall, this model performed very well on the training and validation sets, achieving the highest accuracy of ~93.86% and ~89.94% respectively. However, the performance on the test set was not as high, with an accuracy of ~57.53%. This could still be attributed to some overfitting during training due to our dataset being smaller as the difference between the training and test performance is quite vast. In the confusion matrix below, we can see that the model tends to correctly predict the favorite music genres of the survey participants, but still incorrectly predicts or confuses some of the genres. For example, though the model correctly predicts the favorite genre “Metal” a majority of the time, it still tends to confuse it with “Rock”.

## **Deep Neural Networks**

Our deep neural network model is designed with a series of layers to optimize generalization and mitigate overfitting, starting with an input layer that matches the feature count of X_train2. The architecture includes a first dense layer of 128 neurons with ReLU activation to handle complex data patterns, followed by a batch normalization layer that stabilizes the learning process by normalizing activations, thus enhancing efficiency. A dropout layer with a rate of 0.2 is then applied to reduce overfitting by randomly deactivating some neurons during training, which increases the robustness of the model. Subsequently, the network incorporates another dense layer with 64 neurons and ReLU activation, complemented by additional batch normalization and dropout, deepening the network's ability to refine features. The final layer is a softmax activation layer aligned with the number of classes in y_train2, which outputs a probability distribution over the class labels, facilitating accurate classification.
Training is managed using the Adam optimizer with an initial learning rate of 0.01, adjusted through an exponential decay schedule to fine-tune the learning process over 30 epochs and a 20% validation split. This method aims to balance training speed and accuracy, achieving a peak validation accuracy of 91.61% in the 25th epoch. However, the test accuracy is only 54.1%, showing that the model is not good at processing unseen data. It suggests potential areas for further tuning to enhance model stability and effectiveness on unseen data.

## **Conclusion and Future Work**

Out of the 6 models we trained, we found that the logistic regression model performed the best on the test set based on accuracy. This is likely due to the fact our dataset was smaller and therefore the optimal logistic regression model was able to generalize better compared to the other models. While the neural network models performed similarly to the classifiers on the training and validation sets (roughly in the 90%-96% range), they both performed slightly worse on the test set when compared to the optimal model. This can also be attributed to a lack of data since neural networks perform and generalize better when there is more input data available. For future work, it would be interesting to explore how feature importance could lead to better-performing models. Though there are only 35 features in use during our experiment, it is possible that performing dimensionality reduction could improve the metrics of our models and allow for better generalization.
