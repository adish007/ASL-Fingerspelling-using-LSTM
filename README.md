# ASL-Fingerspelling-using-LSTM
Created a model to translate sign language using a LSTM model

Used Kaggle Dataset to create a Recurrent Neural Network (RNN) specifcally a Long-Short-Term_Memory model. 
This neural network would overcome the problem of vanishing gradient descent that would otherwise occur in RNNs with the amount of frames we need to process.

Preproccessed the data by omitting data related to face and body. Only used data from hands and extracted dominant hand which is important in fingerspelling. 
Labels were taken for data and consolidated into TFRrecord format. This facilitates the caching and fetching of data batches later on during training.
Each character is mapped to an integer value, e.g. "a" is mapped to 32. We introduced characters to mark the start of a phrase, end of a phrase, and padding at the end of the phrase 
('<', '>' , and 'P' , respectively). Our final alphabet contains sixty-two key-value pairs, which will be the amount of classes the final algorithm will classify.

Then we split Data into Training, Validation, and Test set. Each subset is decoded, converted, batched, and prefetched to optimize training performance. 
The resulting datasets are cached in memory to speed up access during training.

**THE MODEL**
The model included a LSTM model using Keras library.
Defined the variables for input/output layer. 
The number of classes in the output layer is the number of unique characters in the data set, i.e. 62 characters. Each batch of the input data has the shape ( batch_size, frame_len, num_features).
Then created a sequnetial model which is a stack of layers in Keras. The first LSTM layer is 75 units. Then another layer is used to act as a encoder layer. 
It reads thes input sequence and encodes it to a fixed length sequence. epeatVector layers were added as an adapter since the subsequent LSTM layer requires a 3D input, but the output of the previous
LSTM layer is 2D. The second LSTM layer has 50 units. This LSTM model serves as a decoder layer, i.e. decoding a fixed length vector and outputting a predicted sequence. 
The next layer is a TimeDistributed layer, which applies a fully connected (Dense) layer with a softmax activation to each time step of the sequence.
There were some issued with matching the number of frames in the predicted output and the number of frames in the target (128 vs 64). 
To solve this we decided to apply to the output of the previous layer a downsizing operation via a Lambda layer. 
This operation downsamples by halving the sequence length (::2) while keeping the same number of features.
The model is then compiled with the categorical cross-entropy loss function, the Adam optimizer, and the accuracy metric for evaluation during training.
The overall architecture is a sequence-to-sequence model with two LSTM layers, followed by a TimeDistributed dense layer for multi-class classification, and a custom downsampling step before training.

The model is trained with fit method using the training data set and validated on validation set using 20 epochs. Created a custom callback called Display Outputs
to display model outputs during training. It essentially converts the tensor numeric representation into textual phrases. 
![Step1](https://github.com/adish007/ASL-Fingerspelling-using-LSTM/assets/34896163/019f8da0-37cb-4ee0-bc85-cb745013f640)
![Step2](https://github.com/adish007/ASL-Fingerspelling-using-LSTM/assets/34896163/d95be673-a953-49ac-81e9-b516488dedf0)
![Step3](https://github.com/adish007/ASL-Fingerspelling-using-LSTM/assets/34896163/b028e57a-b444-4217-b282-a8ce6b705b33)
![Step4](https://github.com/adish007/ASL-Fingerspelling-using-LSTM/assets/34896163/38b1e530-c4f6-491d-9f33-2202bbf83143)
