# Cat-vs-Dog-optimizer-experiment

## Introduction
In this experiment I looked at 7 commonly used optimization algorithms for convolutional neural nets to test how much better the Adam and/or Nadam optimizer does compared to the others.

Adam is the latest state of the art of first order optimization method that’s widely used in the real world. It’s a modification of RMSprop. Loosely speaking, Adam is RMSprop with momentum. So, Adam tries to combine the best of both world of momentum and adaptive learning rate.


## Model
Base model: ImageNet -> ResNet50 -> 2048 output features
Data: dogs-vs-cats images from fast.ai containing 23000 training data and 2000 validation data.
Method: Train (finetune) top layers for 30 epochs with ReduceLROnPlateau() callback. Evaluate performance on validation accuracy and loss (binary cross-entropy loss.)


## Results
I was surprised that the Adam optimizer actually did worse than an "outdated" optimizer like SGD.

- SGD+Nestrov and SGD+Nesterov+momentum=0.9 have the best performance with a validation accuracy of 0.972 followed by SGD.
- Nadam did worse with a result of 0.966 validation accuracy while Adam came in with 0.96
- the validation loss of Adam and Nadam are close with 0.163 and 0.167 respectively
- SGD+Nestrov has the lowest validation loss with 0.110 followed by SGD


## Conclusion
Although all adaptive optimizers have better training performance, it does not imply higher accuracy in validation data.
Using large value for the learning rate, the adaptive learning rate methods are the winner here.
However, the opposite happened when we’re using small learning rate value e.g. 1e-5. It’s small enough for vanilla SGD and momentum based methods to perform well and better.

Interestingly, many recent papers use vanilla SGD without momentum and a simple learning rate annealing schedule. As has been shown, SGD usually achieves to find a minimum, but it might take significantly longer than with some of the optimizers. It is much more reliant on a robust initialization and annealing schedule, and may get stuck in saddle points rather than local minima. Consequently, if you care about fast convergence and train a deep or complex neural network, you should choose one of the adaptive learning rate methods.
Adam makes it easy to converge quickly and on the first try. SGD optimizer take more work to tune, but tends to reach better optima. Adam is more robust to learning rate changes so when you change hyperparameters, you can use Adam and get reasonably good results. However, a well tuned SGD can do better. SGD requires a well tuned learning rate, but can outperform Adam and other adaptive optimizers if done so.
