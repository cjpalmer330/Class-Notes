# What is back propagation

When the model - regardless of the number of layers or inputs - creates an output, it does so by some amount of matrix multiplication with different weighted sums. After the model has produced some output, either manually or automatically the model is informed about how close its output estimate is from the true value it was supposed to find.
Back Propagation is the process of taking this incorrect value, and moving backwards through the model to change the weights of the non-linear layers as the hopefully adjust and correct the model to create a more accurate estimation in the next iteration.

# Parameterized Model

- some function that has a vector of parameters or inputs and a deterministic output
  - Ex: Linear Regression or Nearest neighbor
- Ideally you calculate a single node, then run it through some form of gradient descent and adjust
  - However this is incredibly inefficient as we have thousands of cores and threads, so we test in batches
    - These batches should be roughly 2-3x the number of variables / parameters you have in the model

## Traditional Neural Net

- Stacked linear and non linear functional blocks
  - weighted sums, matrix-vector product
  - Each component of the input vector is multiplied by each element in the first layers nodes. This means a 6 unit vector must be multiplied by a 3x6 matrix of weights to get the weighted sums of the first linear layer
- The combination of the Weighted sum nodes, and their output is considered one layer. The input is also sometimes considered its own layer
- These non-linear layers in a sense "split up" the weighted sums across multiple matrix match equations. This allows for a new point to insert changes.
  - Non-linear layers are what make Deep learning need multiple layers, because without them - meaning each layer only outputs its value to a single node - then having more than one layer is pointless and only **_really_** consists of effectively a single layers of matrix math
- Discrete Convolution
  - Convolution Definition
    - y = jΣ (wj xi - j)
  - In Practice
    - y = jΣ wj xi + j

## CNN continued

- Multiple Character Recognition
  - Every layer is a convolution, and as such we can separate each character apart to its own convolution layer set
  - Then putting these Single Character Recognizers together we can hopefully decode entire words
- Humans naturally pick up on continuation very well. CNNs work because they pick up on this continuation
- CNNs work when the input is a multidimensional array
  - Where there is a continuation of data, with strong local correlations. (edges, small patterns, notable features)
    - because CNN relies on a sliding window, it can only detect features that fit within its window. So smaller features are more likely to be detected within a given window
  - Features that can appear anywhere in the image, and are resistant to translations and distortions.
    - ( i.e. a rotated letter can still be read, but a smeared painting becomes unrecognizable)
- running an image through multiple layers of scaling can help create more accurate models. This is because at the higher resolution, (larger image), the model can pick up on details and smaller hints such as faces, but when scaled down to fit wholly within the window, the model can recognize larger features like mountains and buildings.
