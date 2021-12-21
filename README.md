# Capybara Object Detection

This repo is a tutorial for training and evaluating the Tensorflow Object Detection API using [Capybara Dataset](https://github.com/freds0/capybara_dataset).

The following steps will be presented regarding the workflow of an object detection project:
- Google Colab running on GPU TF2
- Collecting the images to train and validate the Object Detection model.
- Labeling the dataset using a tool like LabelImg.
- Preparing a TFRecord file for ingesting in object detection API.
- Installing the Tensorflow Object Detection API.
- Creating the OD Config file.
- Running the Object detection training and eval job.
- Exporting the model.



## Tensorflow Installation

```buildoutcfg
$ apt-get update && apt-get install -y \
    protobuf-compiler \
    python3-pip \
    python3-pil \
    python3-lxml \
    python3-opencv
```

```buildoutcfg
$ pip install tf_slim
```

```buildoutcfg
$ pip install tensorflow-gpu==2.7.0
```

# Tensorflow Object Detection API Installation

```buildoutcfg
$ pip install pycocotools
```

```buildoutcfg
$ cd /
$ git clone --depth 1 https://github.com/tensorflow/models
```

```buildoutcfg
$ cd /models/research/
$ protoc object_detection/protos/*.proto --python_out=.
```

```buildoutcfg
$ cp /models/research/object_detection/packages/tf2/setup.py /models/research
$ cd /models/research/
$ python -m pip install .
```
Testing the installation:

```buildoutcfg
$ cd /content/models/research/
$ python object_detection/builders/model_builder_tf2_test.py
```

```
Ran 24 tests in 20.478s

OK (skipped=1)
```
## Create the Environment using Docker

To build our image run:

```
$ docker build -t tensorflow_object_detection ./
```

To run container from the image use command:

```
$ docker run --name tf2od -p 8888:8888 -d tensorflow_object_detection
```

## Training on Capybara Dataset

Assuming everything went well during the requirements installation, to do the training on our example dataset [Capybara Dataset](https://github.com/freds0/capybara_dataset), just run the following command:

```
$ python train.py \
    --checkpoint_path=CHECKPOINTS_DIR
    --config_file=PIPELINE_CONFIG_FILE \
    --checkpoint_every_n=CHECKPOINTS_STEPS \
    --num_train_steps=TRAIN_STEPS \
    --num_workers=NUM_WORKS
```

## Exporting your Trained Model do Frozen Graph

Exporting the model to a Frozen Graph file is the best option if you want to continue training, perform inference or use it as a subgraph. To export the trained model, run the following command:

```
$ python export.py \
    --config_file=PIPELINE_CONFIG_FILE \
    --trained_checkpoint_dir=CHECKPOINTS_DIR \
    --output_directory=EXPORTED_CHECKPOINTS_DIR
```

## Running Inference

From a trained and exported model, you can perform inference and detect Capybaras in your own images using the following command:

```
$ python inference.py \
    --model_path=EXPORTED_CHECKPOINTS_DIR \
    --image_path=SOURCE_IMAGES_DIR \
    --label_map=LABEL_MAP_FILE \
    --output_path=OUTPUT_IMAGES_DIR
```

## Auto-Annotating Images

You can use a pre-trained model, with an early version of the dataset, to produce more annotated images for model training. Of course, just these annotated images will not contribute to learning the model. This tool aims to facilitate the data annotation step, as it will produce the bounding-boxes, classifying them. Therefore, automatically annotated images must still be validated or corrected manually.

To generate annotations in xml format, run the following command:

```
$ python annotate.py \
    --model_path=EXPORTED_CHECKPOINTS_DIR \
    --image_path=SOURCE_IMAGES_DIR \
    --label_map=LABEL_MAP_FILE
```

The annotations will be generated in the directory of the source images.

## References

- [How to train your own Object Detector with TensorFlow’s Object Detector API](https://towardsdatascience.com/how-to-train-your-own-object-detector-with-tensorflows-object-detector-api-bec72ecfe1d9)
- [TensorFlow 2 — Object Detection on Custom Dataset with Object Detection API](https://medium.com/swlh/image-object-detection-tensorflow-2-object-detection-api-af7244d4c34e)

## Copyright

See [LICENSE](https://github.com/freds0/object_detection_capybara/blob/main/LICENSE) for details. Copyright (c) 2021 Fred Oliveira.
