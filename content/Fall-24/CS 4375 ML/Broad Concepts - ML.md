# Types of Learning
## Supervised Learning
- Training a model by showing it examples of something. When the output is wrong, tweak the parameters of the machine.
- Need to hand engineer the feature extraction mechanism
- We have many knobs / neurons and the model will give some binary output based on the input. With supervised learning we are able to directly and manually adjust these knobs to guide the model in the direction we want.
- On a high level, SL is just function optimization
### Gradients
- Stochastic Gradient Descent
	- We calculate a scalar value that shows the distance between the output and the desired output, we can use this to calculate a gradient showing in which directions will bring us closer to the desired value.
	- SGD is unique in that it only calculates values using a smaller subset of the input data, could be a single data value or just a handful.
	- Because we are only sampling a small number of data inputs at a time, we can get an extra noise in our path towards the valley in a GD, but it does tend to reach this valley faster than a generalized GD which is computing the gradient on the entire data set
	- This slight loss of accuracy can sometimes be a good thing because it avoids over tunning problems
#### Back-Propagation
- A practical application of the chain rule from calculus 1 to find the slope gradient
- essentially find the derivative of some function, then multiply the derivative to the layer above, repeating the derivation pattern
## Deep Learning
- Needs multiple non-linear layers so that they cannot be collapsed into a single matrix function
- weights of each input of each layer can change with each iteration as the model is learning. 
- Because Deep learning relies on the multiplication of vectors, we need to ensure that the vectors have distinct values, and that the matrices that we are multiplying them with are of the correct size.
	- Without any planning we can end up with matrices and vectors that contain millions or billions of entries which is not practical.
## CNN ( Convolutional Neural Networks )
- Simple cells detect local features, complex cells pool the outputs of simple cells together
- Because ConvNets are simply recognizing patterns in small areas, they can be used to detect what we would consider two objects at once. For example multiple digits.
- Can swipe a window across the image to limit the search space for each run of the function
	- does have the downside that a face or object could be larger than the window size chosen, so you must try multiple times, where you scale the image to hopefully recognize all instances regardless of object size
- AlexNet had a huge breakthrough bringing down error rate to 16% in 2012 by training the model for weeks and adding more intermediary convolution layers
### <span style="color:#7dafff">CNNs are special because they give us a method of learning to represent the world by exploiting its inherent hierarchical compositional nature.</span>
## Why does it work so well ?
#### Questions to answer
- Why do we need layers?
- Why do they work so well on natural signals?
- why doesn't SGD get trapped in local minima?
- Why do they not overfit?
### Ideas for generic feature extraction
- Basic Principle
	- Expanding the dimension of the representation so that things are more likely to become linearly separable
		- Space tiling
		- random projections
		- kernel machines
- Hierarchical levels of representation with each layer being another layer of abstraction to approximate some value.
#### Do we really need deep architectures
- We can approximate any function as close as we want with shallow architecture, so why go deep?
	- Most functions can be represented in 2-5 layers.
- Deep learning architecture are more efficient for representing certain classes of functions, particularly those involved in visual recognition
- Why are deep architectures more efficient
	- A deep architecture trades space for time
		- more layers (sequential computer)
		- less hardware
	- 