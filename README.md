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


If you're not familiar with caffe's flow yet, dennis-chen's reply [here](https://github.com/BVLC/caffe/issues/550) gives a great picture.

### In order to train CNN Cascade: 
1. You should first download all faces from the AFLW dataset, and at least 3000 images without any faces (negative images).
2. Create negative patches by running **face_preprocess_10kUS/create_negative.py** with data_base_dir modified to the folder containing the negative images.
3. Create positive patches by running **face_preprocess_10kUS/aflw.py**
4. Run **face_preprocess_10kUS/shuffle_write_positives.py** and **face_preprocess_10kUS/shuffle_write_negatives.py** to shuffle and write position and labels of images to file.
5. Run **face_preprocess_10kUS/write_train_val.py** to create train.txt, val.txt and move images to corresponding folders as caffe requires.
6. Use scripts in **CNN_face_detection_models/create_lmdb_scripts/** to create lmdb files as caffe requires.
7. Start training by using such commands in terminal. <br>
`./build/tools/caffe train --solver=models/face_12c/solver.prototxt`

24 net and 48 net can be created in a similar way, however negative images shoud be created by running **face_preprocess_10kUS/create_negative_24c.py** and **face_preprocess_10kUS/create_negative_48c.py**

Calibration nets are also trained similarly, scripts can be found in **face_calibration/**

