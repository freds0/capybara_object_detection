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
apt-get update && apt-get install -y \
    protobuf-compiler \
    python3-pip \
    python3-pil \
    python3-lxml \
    python3-opencv 
```

```buildoutcfg
pip install tf_slim
```

```buildoutcfg
pip install tensorflow-gpu==2.7.0
```

# Tensorflow Object Detection API Installation

```buildoutcfg
!pip install pycocotools
```

```buildoutcfg
cd /
git clone --depth 1 https://github.com/tensorflow/models
```

```buildoutcfg
cd /models/research/
protoc object_detection/protos/*.proto --python_out=.
```

```buildoutcfg
cp /models/research/object_detection/packages/tf2/setup.py /models/research
cd /models/research/
python -m pip install .
```
Testing the installation:

```buildoutcfg
cd /content/models/research/
python object_detection/builders/model_builder_tf2_test.py
```

```
Ran 24 tests in 20.478s

OK (skipped=1)
```
## Create the Environment using Docker

To build our image run:

```
docker build -t tensorflow_object_detection ./
```

To run container from the image use command:

```
docker run --name tf2od -p 8888:8888 -d tensorflow_object_detection
```
## References

- [How to train your own Object Detector with TensorFlow’s Object Detector API](https://towardsdatascience.com/how-to-train-your-own-object-detector-with-tensorflows-object-detector-api-bec72ecfe1d9)
- [TensorFlow 2 — Object Detection on Custom Dataset with Object Detection API](https://medium.com/swlh/image-object-detection-tensorflow-2-object-detection-api-af7244d4c34e)

## Copyright

See [LICENSE](https://github.com/freds0/object_detection_capybara/blob/main/LICENSE) for details. Copyright (c) 2021 Fred Oliveira.
