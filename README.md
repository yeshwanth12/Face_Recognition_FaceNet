# Face Recognition using FaceNet
# Objective
Building a system for an office building where the office manager would like to offer facial recognition to allow the employees to enter the building. No ID cards needed. An authorized person can walk up to the building, and the door will unlock for them!

Face recognition problems commonly fall into two categories: 
- **Face Verification** - "is this the claimed person?". For example, at some airports, you can pass through customs by letting a system scan your passport and then verifying that you (the person carrying the passport) are the correct person. A mobile phone that unlocks using your face is also using face verification. This is a 1:1 matching problem. 
- **Face Recognition** - "who is this person?". For example, a [face recognition video](https://www.youtube.com/watch?v=wr4rx0Spihs) of Baidu employees entering the office without needing to otherwise identify themselves. This is a 1:K matching problem. 

FaceNet learns a neural network that encodes a face image into a vector of 128 numbers. By comparing two such vectors, you can then determine if two pictures are of the same person.

- Implement the triplet loss function
- Use a pretrained model to map face images into 128-dimensional encodings
- Use these encodings to perform face verification and face recognition

# Model Architecture
The network architecture follows the Inception model from [Szegedy *et al.*](https://arxiv.org/abs/1409.4842).
- This network uses 96x96 dimensional RGB images as its input. Specifically, inputs a face image (or batch of m face images) as a tensor of shape (m, n_C, n_H, n_W) = (m, 3, 96, 96)
- It outputs a matrix of shape (m, 128) that encodes each input face image into a 128-dimensional vector

By using a 128-neuron fully connected layer as its last layer, the model ensures that the output is an encoding vector of size 128. You then use the encodings to compare two face images as follows:
<img src="images/distance_kiank.png" style="width:680px;height:250px;">

So, an encoding is a good one if: 
- The encodings of two images of the same person are quite similar to each other. 
- The encodings of two images of different persons are very different.

The triplet loss function formalizes this, and tries to "push" the encodings of two images of the same person (Anchor and Positive) closer together, while "pulling" the encodings of two images of different persons (Anchor, Negative) further apart.
<img src="images/triplet_comparison.png" style="width:280px;height:150px;">

Here are some examples of distances between the encodings between three individuals:
<img src="images/distance_matrix.png" style="width:380px;height:200px;">

# Dependencies
The execution environment is specified in the requirements.txt

# Results
Younes is at the front-door and the camera takes a picture of [him]("/images/camera_0.jpg"). Let's see if your algorithm identifies Younes.
>it's younes, the distance is 0.659393


# References
- Florian Schroff, Dmitry Kalenichenko, James Philbin (2015). [FaceNet: A Unified Embedding for Face Recognition and Clustering](https://arxiv.org/pdf/1503.03832.pdf)
- Yaniv Taigman, Ming Yang, Marc'Aurelio Ranzato, Lior Wolf (2014). [DeepFace: Closing the gap to human-level performance in face verification](https://research.fb.com/wp-content/uploads/2016/11/deepface-closing-the-gap-to-human-level-performance-in-face-verification.pdf) 
- The pretrained model we use is inspired by Victor Sy Wang's implementation and was loaded using his code: https://github.com/iwantooxxoox/Keras-OpenFace.
- Our implementation also took a lot of inspiration from the official FaceNet github repository: https://github.com/davidsandberg/facenet 
- Convolutional Neural Networks, Deep learning Specialization by Deeplearning.AI : https://www.coursera.org/learn/convolutional-neural-networks
