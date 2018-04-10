# Visual Search for AWS DeepLens

This project is a visual search engine.  After an AWS DeepLens device is shown an item, the device’s deep learning model output is used to populate a list of the top visually similar items.  Visual search is the central component of an interface where instead of asking for something by voice or text, you *show* what you are looking for.  

In brief, when shown an item, the DeepLens device generates a “feature vector”, and a Search Lambda function then compares the incoming feature vector of that item to a data store of reference item feature vectors.  The Search Lambda function returns the top visually similar reference item matches, which are then consumed by a web app via an API Lambda function fronted by API Gateway.  

![Overview](./images/diagram-large.png)

## References 

The idea of using a pretrained CNN to extract features from images is discussed in the paper by **Babenko** et al (2014), *“Neural Codes for Image Retrieval”*.  To do feature extraction from images, the CNN layer before the last fully connected layer is used.  The output of this layer can be considered as a “feature vector” or “neural code” for an image that summarizes the image’s features.  For a typical CNN for images, namely ResNet, the feature vector consists of a vector of 2,048 floating point numbers.  

Given an image of a query item and a data store of feature vectors of reference items to be compared against the query, the cosine similarity metric can be used to find the most similar items to the query – the higher the cosine, the more similar.  For normalized feature vectors, the cosine is simply the dot product of the query item feature vector and the feature vector of the item to be compared.  The cosine similarity metric was chosen as the relevant metric after a review of relevant papers, such as **Machado** (2017), *“Image visual similarity with deep learning: application to a fashion e-commerce company.”*  Machado found that cosine similarity overall produced the best results after evaluating the performance of several distance metrics, including Manhattan, Euclidean, Minkowski, correlation, and Chebyshev as well as cosine similarity.

## How to Access the Model

The model for this project was created with Apache MXNet.  In regard to the location of the model, both the .json model definition and the model parameter file are hosted on Amazon S3.  The S3 bucket location has public read access, and is located at:

```
s3://deeplens-sagemaker-brent/VisualSearch
```

## How to Deploy

In order to deploy this project, you'll need to spin up multiple pieces of infrastructure.  The following instructions cover setup of the minimum number of pieces needed to view results.







