
# AI Image Detector

This is a machine learning project I built in Jupyter Notebook to learn how image classification works with convolutional neural networks.

The goal of the model is to classify an image as either **real** or **AI-generated**. I trained it using the CIFAKE dataset and used TensorFlow/Keras to build the CNN.

## Dataset

I used the CIFAKE dataset, which contains real images and AI-generated images.

The dataset is divided into:

- 50,000 real training images
- 50,000 AI-generated training images
- 10,000 real test images
- 10,000 AI-generated test images

The dataset itself is not included in this repository because of its size.

Dataset:
https://www.kaggle.com/datasets/birdy654/cifake-real-and-ai-generated-synthetic-images

## How the model works

The images are resized to 32 × 32 pixels and divided into training, validation and testing data.

Before training, I apply light data augmentation using:

- horizontal flipping
- small image translations

The CNN then uses three convolutional stages with 32, 64 and 128 filters. Max pooling is used between convolution layers to reduce the image representation.

After the convolution layers, the model uses global average pooling, dropout and a sigmoid output layer for binary classification.

The final output is a value between 0 and 1:

- closer to 0 → REAL
- closer to 1 → AI-GENERATED

## Training

I trained the model using the Adam optimizer and binary cross-entropy loss.

I also used EarlyStopping to monitor validation loss. This stops training when validation performance stops improving and restores the weights from the best epoch.

During my training, the model started showing signs of overfitting. Training accuracy continued to improve while validation performance stopped improving and became less stable.

This was an important part of the project because it showed me that good training accuracy does not automatically mean that a model will generalize well to new images.

## Results

On the CIFAKE test set, my model achieved approximately:

- Accuracy: 84.61%
- Precision for AI-generated images: 78.43%
- Recall for AI-generated images: 95.50%
- F1 score: 86.13%
- ROC-AUC: 95.09%

The model correctly detected 9,550 out of 10,000 AI-generated test images. However, it also incorrectly classified 2,627 real images as AI-generated.

## Testing with my own image

I also tested the trained model using a real photo that I took myself. The image had been sent through WhatsApp and then downloaded again before I tested it.

Even though the image was genuinely real, the model classified it as AI-generated with very high confidence. This showed an important limitation of the model.

The CNN learned patterns from the CIFAKE dataset, but those patterns do not necessarily generalize to every real-world image. Image compression, phone processing, resizing and other differences from the training dataset may affect the prediction.

It also supports what I observed during training: the model was beginning to overfit and depended heavily on the characteristics of the data it had learned from.

Because of this, this project should be treated as a learning experiment rather than a reliable AI image detector.

## What I learned

Through this project I practiced:

- loading and preparing image datasets with TensorFlow
- splitting data into training, validation and testing sets
- using data augmentation
- building a CNN from scratch
- understanding convolution and pooling layers
- using EarlyStopping
- comparing training and validation performance
- identifying overfitting
- evaluating a classifier using precision, recall, F1 score and ROC-AUC
- using confusion matrices
- testing the trained model on an external image

One of the main things I learned is that evaluation metrics alone do not tell the whole story. A model can perform well on its test dataset and still make confident mistakes on real-world images.


## How to run the project

To run this project, first clone or download the repository and open the project folder on your computer. Install the required Python libraries by running:

pip install -r requirements.txt

Next, download the CIFAKE: Real and AI-Generated Synthetic Images dataset from Kaggle:

https://www.kaggle.com/datasets/birdy654/cifake-real-and-ai-generated-synthetic-images

The dataset contains separate train and test folders, each with REAL and FAKE image classes.

After downloading it, keep the dataset as a ZIP file and rename it to:

cifake.zip

Place cifake.zip in the same folder as the Jupyter Notebook:

ai-image-detector/
├── ai_image_detector.ipynb
├── cifake.zip
├── requirements.txt
├── README.md
└── .gitignore

The notebook uses the following variables to locate and extract the dataset:

zip_path = "cifake.zip"
extract_folder = "cifake_data"

After extraction, the notebook expects the dataset folders to be located at:

TRAIN_PATH = "cifake_data/train"
TEST_PATH = "cifake_data/test"

You normally do not need to change these variables if cifake.zip is placed in the same folder as the notebook.

Then open:

ai_image_detector.ipynb

using Jupyter Notebook or JupyterLab and run the cells from top to bottom. The notebook will load and prepare the dataset, train the CNN, evaluate the model and allow custom-image testing.

To test your own image after training, place the image in the same project folder and change:

CUSTOM_IMAGE_PATH = "test_image.jpeg"

to the exact filename of your image. For example:

CUSTOM_IMAGE_PATH = "my_photo.jpg"

Then run the final prediction cell. The model will display the image together with its prediction, confidence score, probability of being AI-generated and probability of being real.
The model correctly detected 9,550 out of 10,000 AI-generated test images.

However, it also incorrectly classified 2,627 real images as AI-generated.
