https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3


---

# **1) Implement perceptron neural network (gate operations, classification etc) and compute the accuracy.**

### **AIM**

To implement a Perceptron Neural Network for binary classification and compute its accuracy.

### **Description**

The Perceptron is a supervised learning algorithm used for binary classification problems. It is a single-layer neural network that computes a weighted sum of input features and applies a step activation function to produce the final output. The model updates its weights iteratively based on classification error.

In this program, the Iris dataset is converted into a binary classification problem. The dataset is split into training and testing sets, standardized for better convergence, and trained using the perceptron algorithm. The accuracy score is calculated to evaluate how well the model classifies unseen data.

### **Output**

Accuracy ≈ 95% – 100%
(Accuracy may vary slightly depending on random train-test split)

---

# **2) Implement various activation functions step, sign, linear, sigmoid, tanh and ReLU using any basic neural network.**

### **AIM**

To implement and visualize different activation functions used in neural networks.

### **Description**

Activation functions introduce non-linearity into neural networks, enabling them to learn complex relationships between inputs and outputs. Without activation functions, neural networks behave like linear models.

In this implementation, a linear combination of inputs is computed and passed through various activation functions such as Step, Sign, Linear, Sigmoid, Tanh, and ReLU. The Diabetes dataset is used to generate input data. Graphs are plotted to visually understand how each activation function transforms input values.

### **Output**

Sigmoid Sample Output: Values between 0 and 1
Graphs plotted for:

* Step Function
* Sigmoid Function
* Tanh Function
* ReLU Function

---

# **3) Implement multi-layer feed forward neural network. Justify the number of neurons in the hidden layer, number of hidden layers to have maximum accuracy.**

### **AIM**

To implement a Multi-Layer Feed Forward Neural Network and determine the optimal hidden layer configuration for maximum accuracy.

### **Description**

A Multi-Layer Perceptron (MLP) consists of an input layer, one or more hidden layers, and an output layer. Each layer contains neurons that apply weighted sums and activation functions to learn complex patterns from data.

In this program, different hidden layer configurations are tested to compare performance. The California Housing dataset is used for regression. The network performance is evaluated using the R² score. By comparing multiple architectures, the model with the highest R² score is selected as the best configuration. This demonstrates how increasing layers or neurons can improve learning up to an optimal point.

### **Output**

R² Scores printed for different architectures
Best Architecture Example: (100, 50)
Highest R² Score ≈ 0.78 – 0.82

---

# **4) Implement binary class classification using a suitable neural network.**

### **AIM**

To implement binary classification using a neural network and compute its accuracy.

### **Description**

Binary classification involves predicting one of two possible classes. A neural network with one hidden layer using ReLU activation and an output layer using Sigmoid activation is implemented. The Sigmoid function outputs probabilities between 0 and 1.

The Breast Cancer Wisconsin dataset is used to classify tumors as benign or malignant. The model is trained using binary crossentropy loss and optimized using Adam optimizer. Accuracy and confusion matrix are used to evaluate model performance and classification effectiveness.

### **Output**

Accuracy ≈ 95% – 99%
Confusion Matrix displayed showing correct and incorrect predictions.

---

# **5) Implement multi class classification using a suitable neural network.**

### **AIM**

To implement multi-class classification using a neural network and compute its accuracy.

### **Description**

Multi-class classification predicts one of more than two classes. A neural network with a hidden layer and an output layer using Softmax activation is implemented. Softmax converts output values into probability distributions across multiple classes.

The Iris dataset is used with three output classes. The model is trained using sparse categorical crossentropy loss. After training, predictions are compared with actual labels, and accuracy is computed. The confusion matrix helps analyze performance across all classes.

### **Output**

Accuracy ≈ 95% – 100%
Confusion Matrix showing classification results for all three classes.

---

# **6) Implement backpropagation neural network, trace the error and compute the accuracy.**

### **AIM**

To implement a Backpropagation Neural Network, trace the training error, and compute the accuracy.

### **Description**

Backpropagation is a supervised learning algorithm used to train multi-layer neural networks by minimizing the error between predicted and actual outputs. It calculates gradients of the loss function and updates weights using optimization techniques such as Adam.

In this program, the Iris dataset is used for classification. The network is trained for multiple epochs, and the loss value is recorded at each epoch. A graph of loss versus epochs is plotted to trace how the error decreases over time. Finally, accuracy is calculated to measure model performance.

### **Output**

Accuracy ≈ 95% – 100%
Loss vs Epoch graph showing gradual decrease in training error.

---




https://github.com/randomknowme/cn1

https://github.com/randomknowme/cn2

https://github.com/randomknowme/cn3
