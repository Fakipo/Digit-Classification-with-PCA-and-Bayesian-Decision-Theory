CSE 569 - Fundamentals Of Statistical Learning

Introduction

The main goal of this project is to categorize handwritten digits "5" and "6" inside a specially created subset of the MNIST dataset using statistical learning and pattern recognition techniques. With the ultimate goal of accomplishing accurate digit classification, the main objective is to investigate and apply a number of basic tasks, such as feature normalization, Principal Component Analysis (PCA), data visualization, density estimation, and Bayesian classification. We set P(5) = P(6) = 0.5, assuming identical prior probabilities for these two groups despite variations in sample sizes.

The dataset includes matching testing samples as well as 5421 training samples for digit 5 and 5918 training samples for digit 6. In order to guarantee uniformity in feature scaling over all samples, we start by standardizing the data. We then use PCA to reduce the dimensionality of the data, display the information in a two-dimensional space, and look at the distribution of classes in this smaller dimension. We also estimate two Gaussian distributions' parameters for the "5" and "6" classes. In the end, we classify using Bayesian decision theory and provide the accuracy for both training and testing datasets. This report aims to offer a comprehensive understanding of the implementation process, along with observations and results. Finally, it will provide a summary of the project's outcomes and the efficacy of the selected methodologies in attaining precise digit classification.

Method

Step 1:

First, we load the image data for digits "5" and "6" from the training and testing datasets, which are stored in separate .mat files using the SciPy library. This data is essential for subsequent analysis and classification tasks.

Step 2: 

After that, we vectorize the 28x28 photos to create 784-dimensional arrays out of them. The data is made simpler for additional analysis by this flattening procedure.

We utilize the "reshape" function to create 784-dimensional vectors from the images of the numbers "5" and "6" for the training data. To verify the transformation, the resulting data arrays' shapes are printed.

To ensure consistency in data preparation for upcoming jobs, the testing data for the digits "5" and "6" are subjected to the same vectorization method.

Step 3:

Next, we create unified datasets for training and testing by combining and merging the picture data for the numbers "5" and "6" from both the training and testing datasets. By building complete datasets that include all of the samples that are available for these two digits, this consolidation makes the analysis that follows easier. For additional statistical learning and classification tasks, the resulting arrays, "X_cumulative_train_data" and "X_cumulative_test_data," are the foundation.


Step 4:

Next, we design labels that correspond to every image, serving as indicators for which digit is a "5" or a "6." In order to do this, we first create two arrays with zeros and ones in them, where 0 stands for the number "5" and 1 for the number "6."

In order to create the training data, we concatenate two arrays of zeros and ones that represent the pictures of the digits "5" and "6" in the training set using the "np.hstack" function. The "y_label_train" array, which offers labels for each training picture, is the end product of this operation.

In a similar manner, we follow the same process for the testing data, generating the "y_label_test" array to designate the testing photos as "5" or "6." In order to train and assess classification models, supervised learning requires these label arrays.


Step 5:

Then we move on to the important task of data normalization, which gets the data ready for insightful analysis. The goal is to ensure that the standard deviation stays within the range of 0 to 1 and to ensure that the data's mean is centered around 0.

We start by determining the combined training data's mean and standard deviation, which yields the arrays "x_train_mean_array" and "x_train_standard_deviation_array." When the standard deviation is precisely zero, we introduce a small value (epsilon) to avoid any division by zero problems. The training data is then subjected to the normalization formula, yi = (xi - mi)/si, which results in "normalized_X_train."

For the testing data, the same process is used; we compute the mean and standard deviation and use the normalization formula to produce "normalized_X_test." The data is guaranteed to be on a consistent scale by these normalized datasets, which paves the way for efficient analysis and classification in the following stages. The completed normalization process is validated by the printed forms of the normalized datasets.




Step 6:

The covariance matrix for the normalized training data is then computed, which is an essential step in figuring out how various features relate to one another.

The first thing we do is count the number of features (p) and data points (n) in the dataset. The covariances between the features are then stored in an empty covariance matrix called "cov_matrix_train_X," which is initialized with zeros.

We compute the covariances between the features by iterating over them using nested loops. Cov(X) = 1/n * [(X - E[X])(X - E[X])^T] is the formula used to calculate covariance, where E[X] is the data mean. Keeping in mind that the covariance matrix is symmetric, the resulting covariances are stored in it. The "cov_matrix_train_X" printed shape attests to the computation of covariances between features being successful. The relationships within the data are revealed by this covariance matrix, which is helpful for dimensionality reduction and further analysis.


Step 7:

Then, we use Principal Component Analysis (PCA) to take the normalized training data's covariance matrix and extract the principal components from it. A dimensionality reduction method called PCA aids in locating the most important patterns in the data.

Using the NumPy function np.linalg.eig, we first calculate the eigenvalues and eigenvectors of the covariance matrix "cov_matrix_train_X." The eigenvectors in "eigen_vectors_train_X" show the directions of these components, while the eigenvalues, which are saved in "eigen_values_train_X," indicate the significance of each principal component.

We generate "eigen_pairs," a list of pairs made up of the absolute eigenvalues and their matching eigenvectors, in order to better comprehend and rank these components. We arrange these pairs according to the eigenvalue magnitudes descending order, which places the most important components at the top of the list. The foundation for dimensionality reduction and feature selection in the project's later stages is provided by this PCA analysis and sorting.

Step 8:

Next, we find the number of key principal components that account for a significant portion of the variance in the data, adhering to the project's directive to use only the top two (k = 2). In terms of feature selection and dimensionality reduction, this is a crucial step.

"explained_variance_threshold," the threshold for explained variance, is set at 0.95, which corresponds to the Gaussian distribution, in which roughly 95% of the data are contained within two standard deviations. The variable k is defined as 2 given the specific directive to keep the top two principal components.

The number of principal components required to surpass the threshold is determined by computing the cumulative variances explained by each principal component. In this case, the threshold is precisely two. This procedure helps to choose the ideal number of components for dimensionality reduction while maintaining a significant amount of data. This adherence to the project guidelines guarantees that we are concentrating on the most important elements for the analysis that follows.

Step 9:

After that, we project the data onto the top two principal components to complete the dimensionality reduction. The most important information is preserved while the dataset is made simpler through this process.

By stacking the top two eigenvectors, which represent the principal components, the code creates a "projection_matrix". The transformation that places the data in the reduced-dimensional space is specified by this matrix. To verify the projection matrix's dimensions, a print of its shape is made.

We then use matrix multiplication to project the normalized training and testing data onto the top two principal components. In order to facilitate further analysis and classification, the resulting "projected_train_data" and "projected_test_data" are now represented in a 2-D space. These projected datasets' shapes are printed to confirm the dimensionality reduction's success.

Step 10:

The distribution and separation of the training data in the 2-D space, which is represented by the top two principal components, are then visualized. It is easier to comprehend how the classes "5" and "6" are grouped thanks to this visualization.

In order to achieve this, we first build training and testing Pandas DataFrames with the reduced 2-D data and the corresponding labels. With the help of these DataFrames, "train_data_frame" and "test_data_frame," we can easily manipulate and visualize the data.


We then use Seaborn to create a scatterplot, with the first principal component (zero) on the x-axis and the second principal component (one) on the y-axis. By differentiating between the classes "5" and "6," the color hue sheds light on how these classes are distributed and separated in two dimensions. To make it clear that "5" and "6" are the represented classes in the plot, the legend has been relabeled. To obtain a visual understanding of the distribution of the data in the reduced dimension, this visualization is a necessary first step.

Step 11:

Then we follow the same steps for Test Data. To plot its scatter graph.


Step 12:

The data spread for the number "5" in the 2-D space that is represented by the top two principal components is then examined. Through this analysis, we are able to gain insights into the characteristics of this particular class by comprehending the distribution of the data points.

Based on their labels, we initialize two empty lists, "x5_0" and "x5_1," to hold the data points in the reduced dimension. The training DataFrame's rows are iterated through, and data points are classified as either class "0" (digit "5") or class "1" (digit "6"). These lists are then transformed into NumPy arrays for additional analysis.

The code then uses PC-1, the first principal component, and PC-2, the second principal component, to create histograms of the data spread for the digit "5" in the 2-D space. Plotting the histograms in various colors is done; PC-1 is represented by green, and PC-2 by red. To enhance clarity, the title and labels are adjusted, and a legend is incorporated to differentiate the two main components. The distribution and properties of the digit "5" in the reduced dimension are revealed by this visualization.

Step 13:

Then, using the top two principal components, we analyze the data distribution for the digit "6" in the 2-D space in this section of code. Based on their labels, data points for the digit "6" are sorted into "x6_0" and "x6_1" lists before being converted into NumPy arrays. The data spread for the digit "6" is represented graphically by histograms using PC-1 (orange) and PC-2 (purple). This graphic provides information about the properties and distribution of the number "6" in the smaller dimension.

Step 14:

Next, we concentrate on the number "5". Essential statistical insights are provided by the code, which computes the mean vector and covariance matrix for this class in the 2-D space.

For class "5" in the reduced dimension, the mean vectorÑwhich depicts the center of the dataÑand the covariance matrixÑwhich characterizes the spread and correlations of the dataÑare calculated. The outcomes provide important statistical data for further examination and categorization.

Step 15:

Next, we focus on the number "6." The program computes and displays the covariance matrix and mean vector for this class in two dimensions. These are essential statistical tools for comprehending the properties and distribution of data points associated with the number "6."

Calculated and presented is the mean vector, which is the center of the data distribution for the digit "6" in the reduced dimension (which includes PC1 and PC2). The code also computes the covariance matrix, which gives information about the distribution and correlation of data points in the 2-D space. For a clear representation, complex values are removed.

Step 16:

The probability density of data points classified as "5" and "6" in the reduced 2-D space is then computed by defining probability functions. The multivariate normal distribution, which is defined by the mean vector and covariance matrix for each class, is the foundation for these probability functions, which are named probability_of_train_x_five and probability_of_train_x_six.

A classification function, classify, is also defined to choose the class label for a given data point (0 for "5" and 1 for "6"). The function weights the probability density estimates for the two classes according to predetermined probabilities (0.5 for each class by default, meaning that equal probabilities are taken into account). Next, it determines whether the likelihood of belonging to the digit "5" is higher than or equal to the likelihood of 6 to determine the class label.

Step 17:

After that, we give the data points in the training and testing datasets class labels to complete the classification process. For the digits "5" and "6," we estimate equal probabilities, setting probability_five and probablity_six to 0.5 each.

Next, for every data point in the anticipated training and testing datasets, the code applies the classify classification function. This procedure uses the specified probabilities and estimated probability densities to assign a predicted class label (0 for "5" and 1 for "6").

Step 18:

Next, we assess the training and testing datasets' classification accuracy. By contrasting the true class labels ('Y') and predicted class labels ('Y_pred') in the DataFrames, the code determines the accuracy.

The classification model's performance on the training dataset is indicated by the training accuracy, which is computed and presented. In a similar manner, testing accuracy is calculated and reported, showing how well the model performs on data that hasn't been seen before. These accuracy values are crucial metrics for evaluating the classification outcomes because they offer a quantitative assessment of the model's performance in classifying data points as either digit "5" or digit "6."




Results

Task 1:

We get standard deviation and mean in this task for both training and testing data, then we get normalized data.



Task 2:

We get the covariance matrix, eigen values and corresponding eigen vectors. Also we get the key principal components.




Task 3:

We get the Scatter plot of the distribution of 5 images and 6 images for both training and testing data.





Task 4: 

We further get many graphs to observe the gaussian pattern in the training and testing data of Ô5 imagesÕ and Ô6 imagesÕ.




Task 5:


We get the result of accuracy of our prediction.




Conclusion

In Conclusion, this project used a portion of the MNIST dataset to conduct a thorough investigation of statistical learning and pattern recognition, concentrating on the handwritten digits "5" and "6." The project started with data conditioning, which included Principal Component Analysis (PCA) for feature normalization and dimensionality reduction. Subsequently, the distribution of the data in the reduced 2-D space was thoroughly analyzed, which facilitated a comprehensive comprehension of the positioning and clustering of the digits "5" and "6". In addition, the data distribution was modeled using Gaussian density estimation, which finally resulted in the application of Bayesian Decision Theory for the best classification.

The training and testing datasets showed a promising 92% classification accuracy, according to the results. This shows how well the developed classification model works in differentiating between the digits "5" and "6" according to their two-dimensional representations. The project effectively demonstrated the use of basic statistical learning techniques and offered insightful information on classification, density estimation, data preprocessing, and dimensionality reduction.

To sum up, this project provides an excellent overview of the basic ideas behind statistical learning and pattern recognition. In addition to giving practical experience with dimensionality reduction and data preprocessing, it also showed how Bayesian Decision Theory can effectively classify data. The attained accuracy rates highlight the model's resilience in differentiating between two handwritten digits, setting the stage for later, more sophisticated uses of machine learning and pattern recognition methods.



