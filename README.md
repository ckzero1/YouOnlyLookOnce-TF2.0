# YouOnlyLookOnce-TF2.0
Pure Tensorflow 2.0 Keras implementation for the original [YOLO](https://arxiv.org/abs/1506.02640) Paper

This repository has no dependency on [Darknet](https://pjreddie.com/darknet/yolo/)

This is the final project for [CSCI-1470 Deep Learning](https://cs.brown.edu/courses/csci1470/index.html) @ Brown University

Credits:
* [LoveHRTF](https://github.com/LoveHRTF), **20' ScM.**
* [Gengg](https://github.com/Genggg), **20' ScM.**
* [SHIXINLIU](https://github.com/SHIXINLIU), **20' ScM.**
* [ZeyuRuan](https://github.com/ZeyuRuan), **20' ScM.**


| ----------- | ----------- | ----------- |
| ----------- | ----------- | ----------- |
|![](/doc/p1.jpg)|![](/doc/p3.jpg)|![](/doc/p4.jpg)
|![](/doc/p4.jpg)|![](/doc/p8.jpg)|![](/doc/p6.jpg)
|![](/doc/p5.jpg)|![](/doc/p7.jpg)|![](/doc/p10.jpg)


## Breaf and Branches

### `dataset-pascal-voc` 
Original YOLO, which can be trained on __PASCAL-VOC 2007__ and __2012__ datasets with 20 classes, and outputs tensor with size \[None, 7, 7, 30] for 20 classes of objects. 

### `master`
Modified code for training on __COCO 2017__ dataset, and the last dense layer was increased to 8192 to compensate for extra object classes, and outputs a tensor of \[None, 7, 7, 90] for 80 classes of object.

## Tensorboard
Tensorboard was enabled in this project. Log files were kept in `~\YouOnlyLookOnce-TF2.0\src\logs`
To start the local tensorboard server, use the following command in `~\YouOnlyLookOnce-TF2.0\src`:

`tensorboard --logdir logs/gradient_tape`


## File Structure

### Code
```
~\YouOnlyLookOnce-TF2.0

  --src-|
        |-logs-|
        |      \-gradient_tape\ ........................... Log files generated by tensorboard logger
        |-config.py ....................................... Training configrations
        |-main.py ......................................... Main function
        |-model.py ........................................ Model class contains model, optimizer, and loss functions
        |-dataset.py ...................................... Dataset class for generating batches of data
        |-train.py ........................................ Train loop
        |-test.py ......................................... Test loop
        |-video_application.py ............................ Real-time detection using webcam
        |-image-application.py ............................ Perform detection on single image
        \-visualize.py-| .................................. Bounding box drawing and convert detection results to image
                       |-generate_prediction()
                       |-visualization()
                       |-decoder()
                       \-nms()
        
  --data-|
         |-voc_label.py ................................... Generate label files for PASCAL-VOC 2007 and 2012
         |-train.txt ...................................... PASCAL-VOC 2007 + 2012 training image absolut path list
         |-test.txt ....................................... PASCAL-VOC 2007 + 2012 testing image absolut path list
         |-train_coco.txt ................................. COCO 2017 training image absolut path list
         |-test_coco.txt .................................. COCO 2017 testing image absolut path list
         |-VOCdevkit (Using PASCAL VOC)-| ................. PASCAL-VOC 2007 + 2012 dataset
         |                              |-VOC2007/
         |                              \-VOC2012/
         \-coco (Using COCO 2017)-| ....................... COCO 2017 dataset
                                  |-annotations/
                                  \-images-|
                                           |-train2017/
                                           \-val2017/
                     
  --tool-|
         |-preprocess_coco.py ............................. Function to generate COCO 2017 train/test absolut path list from dataset
         |-coco_format.py ................................. COCO 2017 data preprocess class
         \-preprocess_pascal_voc.py ....................... Function to generate PASCAL-VOC train/test absolut path list from datasets
```

### Model Checkpoints and Temporary Files
```
  --checkpoints-| ......................................... Model checkpoint files
                |-checkpoint
                |-ckpt-xx.index
                |-ckpt-xx.data-00000-of-00002
                \-ckpt-xx.data-00001-of-00002
  
  --evaluation-/ .......................................... Evaluation results

  --tmp-| ................................................. Generated image files
        |-default_folder/
        |-single_images/
        \-epoch_x/
  
  --doc-\
```

## Trained Model

Trained models were provided in Google Drive. To use the model trained on PASCAL-VOC dataset with 20 classes of objects, you will need to switch to branch `dataset-pascal-voc`. Otherwise use `master` branch for using the model trained on COCO 2017 with 80 classes of objects.

Two trained models were provided below for both PASCAL-VOC 2007 + 2012 and COCO. Model for PASCAL-VOC was trained for around an entire day on one single __AMD RADEON VII__ GPU, and model for COCO 2017 was trained on one signle __NVIDIA RTX 2080Ti__ GPU.

* Model on PASCAL-VOC 2012 and 2014 for 100 epochs: [here](https://drive.google.com/drive/folders/1JxkL6rAFiH_MAmWPon3pc1w_PJXzEN9f?usp=sharing)
* Model on COCO 2017 for 400 epochs: [TBD]()



## 1. Data Gather and Pre-process ()

### 1.1. Using COCO 2017

__This Section Requires Using Branch `master`__

`setup_coco.sh` was provided for automation. It proves a one line solution for downloading all required datasets and generates trainable files. Following command is for utilizing this file, the process may take over an hour:

`bash setup_coco.sh`

Manual setup is also available by using the following instructions.

* Follow the instructions [here](https://github.com/LoveHRTF/YouOnlyLookOnce-TF2.0/tree/master/tool#coco-2017)

### 1.2. Using PASCAL VOC 2012 and 2007

__This Section Requires Using Branch `dataset-pascal-voc`__

`setup_pascal_voc.sh` was provided for automation. It proves a one line solution for downloading all required datasets and generates trainable files. Following command is for utilizing this file:

`bash setup_pascal_voc.sh`

Manual setup is also available by using the following instructions.

### 1.2.1. Data Download and Extraction
Under dir ~/YouOnlyLookOnce-TF2.0/src/ 

Follow the instructions [here](https://github.com/LoveHRTF/YouOnlyLookOnce-TF2.0/blob/master/data/README.md)
This step will download and extract the PASCAL VOC 2012 and 2007 datasets

### 1.2.2. Re-scale and Coordinate Transformation
Under dir ~/YouOnlyLookOnce-TF2.0/src/ 

Follow the instructions [here](https://github.com/LoveHRTF/YouOnlyLookOnce-TF2.0/blob/master/tool/README.md).
This step will generate path for training data `train.txt` and `test.txt`



## 2. Train
Under dir ~/YouOnlyLookOnce-TF2.0/src/ 

Test outputs will be generated every time when model was auto saved (per 5 epochs for `dataset-pascal-voc` and everytime for `master`), and located in ` /YouOnlyLookOnce-TF2.0/tmp/epoch_x`

### 2.1. Train a new model
To train from scratch, model checkpoints will be stored in /YouOnlyLookOnce-TF2.0/checkpoints/:
`python main.py` 

### 2.2. Restore from a checkpoint
To resume training from the latest saved checkpoint:
`python main.py --restore` 



## 3. Test
Under dir ~/YouOnlyLookOnce-TF2.0/src/ 

To test the latest checkpoint on test set:
`python main.py --mode=test`

Note that dropout will be eliminated in test and visualization mode by using this command.



## 4. Evaluation

To evaluate the model, we have to test the model first and get the test results logged. After testing, there should be a directory ~/YouOnlyLookOnce-TF2.0/evaluation, under which we have the test result of each class. Then run the command to evaluate: \
`python main.py --mode=eval`



## 5. Visualization

### 5.1. Real-time Video Detection
We have developed a simple script to visualize the detection, a webcam is required. \
To perform realtime detection: \

* Place trained model under `~/YouOnlyLookOnce-TF2.0/checkpoints`
* Connect a webcam and ensure the driver was installed
* Run `python video_application.py` under dir `~/YouOnlyLookOnce-TF2.0/src/`
* The realtime video will be shown on screen

### 5.2. Image Detection

To perform detection on a list of image, we have provided a simple script to read the candidate image, perform detection, and sotre the file in local drive. \

To perform the detection: \

* Place trained model under `~/YouOnlyLookOnce-TF2.0/checkpoints`
* Use `image_application.py` under dir `~/YouOnlyLookOnce-TF2.0/src/`
* The generated image file will be stored in `~/YouOnlyLookOnce-TF2.0/tmp/single_images`

Following is the sample command for utilizing this script: \

`python image_application.py --path=/home/foo/Documents/YouOnlyLookOnce-TF2.0/test_image.jpg`







