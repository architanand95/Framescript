# Framescript: Narrative synthesis from image sequences

Due to computational configuration limitations, the code for this project had to be run in two separate parts: image captioning and story generation. Below is an overview of each part:

## Image Captioning

### Dataset
For this study, the dataset used was the Flickr8k dataset consisting of images and their respective captioning. It involved preprocessing the dataset to obtain image features and tokenize captions.

### Architecture of the Image Captioning Model
#### EncoderCNN
Image features were extracted using the EncoderCNN model which is Inception v3 based transfer learning with some modifications. The last fully connected layer in Inception v3 was substituted with a linear layer that produces image embeddings. Training began by tuning only the fully connected layer.

#### DecoderRNN
To generate captions, we employed a recurrent neural network (RNN) model called DecoderRNN with LSTM cells. An embedding layer that converts tokenized captions to embeddings is followed by LSTM and a linear layer for predicting. A dropout layer at a rate of 0.5 was added to avoid overfitting.

#### Encoder-Decoder Architecture
For end-to-end training, two models: Encoder CNN and Decoder RNN were combined into an Encoder-Decoder. An Adam optimizer was used during training with learning rate `3e-4` along with a Cross-Entropy loss function.

### Vocabulary and Data Processing
A vocabulary class was created in order to build a vocabulary based on the captions while adhering to a frequency threshold at the same time. The Flickr8k dataset class was used to load images and their respective captions to apply transformations. During the training process, data was collated using a custom collate function for handling variable-length sequences.

### Training Procedure
The model was trained for 60 epochs for a batch size of 64. Using mini-batches from the data loader, the training loop optimized the model parameters. Torch was set to benchmark mode to speed up the training process.

### Image Captioning and Evaluation
Test photos from the Flickr8k dataset were captioned using the trained model. The quality of the generated captions was evaluated using evaluation criteria like the BLEU score and visual examination.

## Story Generation

### Story Generation
In addition, the MPT-7B-Instruct model was used to generate stories. This model excels at processing brief instructions and producing succinct answers. Using the MPT-7B-Instruct model, the text creation pipeline was set up to produce stories in response to given prompts.

### Innovations and Model Enhancements
Several recent developments in language modeling, such as Attention with Linear Biases (ALiBi), FlashAttention, and Nvidia's FasterTransformer, are incorporated into the MPT-7B-Instruct model. To handle enormous datasets and produce succinct responses, these advances optimize transformer implementation, reduce GPU read-writes, and increase efficiency.

The same has been depicted graphically in the below diagram:

![methodology](https://github.com/architanand95/framescript/assets/106612899/8c0bacf6-251c-4358-b23f-988fe71beff3)
