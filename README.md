Algorithm used here is based on the paper Li et al., “A Convolutional Neural Network Cascade for Face Detection, ” 2015 CVPR

1. face_preprocess_10kUS contains scripts to preprocess face data from datasets such as AFLW, Caltech, 10kUS, feret,e.t.c.
  Preprocessing crops face regions to be positive data, and negative data is generated from images without human faces.
  face_24c and face_48c data is generated to be hard negatives (regions that pass threshold of face_12c) <br><br>
  write_train_val shuffles training and validation data, and writes text files as required by the caffe library.<br><br>

2. face_detection contains scripts to use the CNN Cascade on single image face detection or FDDB dataset benchmarking.

3. face_calibration generates calibration training and validation data from cropped face positions on original images,
  including shuffling training and validation data.
  
4. face_net_surgery contains scripts to perform quantizing on weights and biases of the CNN Cascade, 
  including techniques such as hard quantizing, stochastic rounding, and soft quantizing.
  
Trained models are stored at the repository CNN_face_detection_models
