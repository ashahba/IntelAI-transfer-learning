![Style](https://github.com/intel-innersource/frameworks.ai.transfer-learning/actions/workflows/style-test.yaml/badge.svg)
![Tests](https://github.com/intel-innersource/frameworks.ai.transfer-learning/actions/workflows/unit-test.yaml/badge.svg)
![Doc Test](https://github.com/intel-innersource/frameworks.ai.transfer-learning/actions/workflows/docs-test.yaml/badge.svg)
![Notebook Test](https://github.com/intel-innersource/frameworks.ai.transfer-learning/actions/workflows/notebook-test.yaml/badge.svg)

# Intel® Transfer Learning Tool Quick Start

## Features

| Use Case | Framework | Optimizations | Datasets |
|----------|-----------|----------|---------------|
| Image Classification | PyTorch* | <li>[Intel® Extension for PyTorch*](https://github.com/intel/intel-extension-for-pytorch) <li>Distributed training | <li> Custom datasets <li> [torchvision datasets](https://pytorch.org/vision/stable/datasets.html): CIFAR10, CIFAR100, Country211, DTD, Food101, FGVCAircraft, RenderedSST2 |
| Image Classification | TensorFlow* | <li>[Intel® Optimization for TensorFlow*](https://www.intel.com/content/www/us/en/developer/articles/guide/optimization-for-tensorflow-installation-guide.html) <li>Post-training quantization using [Intel® Neural Compressor](https://github.com/intel/neural-compressor), when using custom datasets <li>FP32 graph optimization using [Intel® Neural Compressor](https://github.com/intel/neural-compressor) <li>Auto mixed precision training on Intel® third or fourth generation Intel® Xeon® processors (requires TensorFlow 2.9.0 or later) | <li> Custom datasets <li> Image classification datasets from the [TensorFlow Dataset catalog](https://www.tensorflow.org/datasets/catalog/overview#image_classification) |
| Text Classification | TensorFlow | <li>[Intel® Optimization for TensorFlow](https://www.intel.com/content/www/us/en/developer/articles/guide/optimization-for-tensorflow-installation-guide.html) <li>Auto mixed precision training on Intel® third or fourth generation Xeon® processors (requires TensorFlow 2.9.0 or later) | <li> Custom datasets from .csv files <li> [TensorFlow Datasets](https://www.tensorflow.org/datasets/catalog/overview#image_classification): [glue/cola](https://www.tensorflow.org/datasets/catalog/glue#gluecola_default_config), [glue/sst2](https://www.tensorflow.org/datasets/catalog/glue#gluesst2), [imdb_reviews](https://www.tensorflow.org/datasets/catalog/imdb_reviews#imdb_reviewsplain_text_default_config), [ag_news_subset](https://www.tensorflow.org/datasets/catalog/ag_news_subset) |
| Text Classification | PyTorch | <li>[Intel® Extension for PyTorch](https://github.com/intel/intel-extension-for-pytorch) <li>Distributed training | <li> Custom datasets <li> Text Classification datasets from [Hugging Face Dataset catalog](https://huggingface.co/datasets) |
| Image Anomaly Detection | PyTorch | | <li> Custom datasets |

## Build and Install

**Requirements**:
1. Linux* system (or WSL2 on Windows*)
1. Python3* (3.8, 3.9, or 3.10), Pip/Conda and Virtualenv
1. git
1. `apt-get install build-essential python3-dev`
1. To run the quantization functions: `apt-get install libgl1 libglib2.0-0`

**Create and activate a Python3 virtual environment**\
Users are encouraged to use python virtual environments for consistent package management\
\
   Using `virtualenv`
   ```
   virtualenv -p python3 tlt_dev_venv
   source tlt_dev_venv/bin/activate
   ```

   Or `conda`:
   ```
   conda create --name tlt_dev_venv python=3.9
   conda activate tlt_dev_venv
   ```


## Basic Installation:
```
pip install intel-transfer-learning-tool
```

## Advanced/Developer Installation:
1. Clone this repo and navigate to the repo directory:
   ```
   git clone https://github.com/IntelAI/transfer-learning.git

   cd transfer-learning
   ```

1. Install the tool by either building and installing the wheel:
   ```
   python setup.py bdist_wheel
   pip install dist/intel_transfer_learning_tool-0.3.0-py3-none-any.whl

   ```
   Or, do an editable install:
   ```
   pip install --editable .
   ```

## Additional Feature-Specific Steps:
1. For TensorFlow text classification, this is also required:
   ```
   pip install tensorflow-text==2.10.0
   ```

1. For distributed/multinode training, [these instructions](/tlt/distributed) are also required.


## Getting Started with the CLI

Use `tlt --help` to see the list of CLI commands. More detailed information on each
command can be found using `tlt <command> --help` (like `tlt train --help`).

List the available models:
```
tlt list models --use-case image_classification
```
```
------------------------------
IMAGE CLASSIFICATION
------------------------------
alexnet (pytorch)
convnext_base (pytorch)
convnext_large (pytorch)
convnext_small (pytorch)
convnext_tiny (pytorch)
densenet121 (pytorch)
densenet161 (pytorch)
densenet169 (pytorch)
densenet201 (pytorch)
efficientnet_b0 (pytorch)
efficientnet_b0 (tensorflow)
efficientnet_b1 (pytorch)
efficientnet_b1 (tensorflow)
efficientnet_b2 (pytorch)
efficientnet_b2 (tensorflow)
efficientnet_b3 (pytorch)
efficientnet_b3 (tensorflow)
efficientnet_b4 (pytorch)
efficientnet_b4 (tensorflow)
efficientnet_b5 (pytorch)
efficientnet_b5 (tensorflow)
efficientnet_b6 (pytorch)
efficientnet_b6 (tensorflow)
efficientnet_b7 (pytorch)
efficientnet_b7 (tensorflow)
efficientnetv2-b0 (tensorflow)
efficientnetv2-b1 (tensorflow)
efficientnetv2-b2 (tensorflow)
efficientnetv2-b3 (tensorflow)
efficientnetv2-s (tensorflow)
googlenet (pytorch)
inception_v3 (tensorflow)
mnasnet0_5 (pytorch)
mnasnet1_0 (pytorch)
mobilenet_v2 (pytorch)
mobilenet_v2_100_224 (tensorflow)
mobilenet_v3_large (pytorch)
...
```

**Train a model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt train -f tensorflow --model-name resnet_v1_50 --dataset-dir /tmp/dataset/flower_photos --output-dir /tmp/output
```
```
Model name: resnet_v1_50
Framework: tensorflow
Training epochs: 1
Dataset dir: /tmp/dataset/flower_photos
Output directory: /tmp/output
Found 3670 files belonging to 5 classes.
...
Model: "sequential"
_________________________________________________________________
 Layer (type)                Output Shape              Param #
=================================================================
 keras_layer (KerasLayer)    (None, 2048)              23561152

 dense (Dense)               (None, 5)                 10245

=================================================================
Total params: 23,571,397
Trainable params: 10,245
Non-trainable params: 23,561,152
_________________________________________________________________
Checkpoint directory: /tmp/output/resnet_v1_50_checkpoints
86/86 [==============================] - 24s 248ms/step - loss: 0.4600 - acc: 0.8438
Saved model directory: /tmp/output/resnet_v1_50/1
```

**Evaluate a trained model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt eval --model-dir /tmp/output/resnet_v1_50/1 --dataset-dir /tmp/dataset/flower_photos
```
```
Model directory: /tmp/output/resnet_v1_50/1
Dataset directory: /tmp/dataset/flower_photos
Model name: resnet_v1_50
Framework: tensorflow
Loading model object for resnet_v1_50 using tensorflow
Loading saved model from: /tmp/output/resnet_v1_50/1/saved_model.pb
...
28/28 [==============================] - 8s 236ms/step - loss: 0.2528 - acc: 0.9163
```

**Benchmark the trained model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt benchmark --model-dir /tmp/output/resnet_v1_50/1 --dataset-dir /tmp/dataset/flower_photos --batch-size 512 --mode performance
```
```
Model directory: /tmp/output/resnet_v1_50/1
Dataset directory: /tmp/dataset/flower_photos
Benchmarking mode: performance
Batch size: 512
Model name: resnet_v1_50
Framework: tensorflow
...
performance mode benchmark result:
2022-06-28 10:22:10 [INFO] Batch size = 512
2022-06-28 10:22:10 [INFO] Latency: 3.031 ms
2022-06-28 10:22:10 [INFO] Throughput: 329.878 images/sec
```

**Quantize the model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt quantize --model-dir /tmp/output/resnet_v1_50/1 --dataset-dir /tmp/dataset/flower_photos --batch-size 512 \
  --accuracy-criterion 0.01 --output-dir /tmp/output
```
```
Model directory: /tmp/output/resnet_v1_50/1
Dataset directory: /tmp/dataset/flower_photos
Accuracy criterion: 0.01
Exit policy timeout: 0
Exit policy max trials: 50
Batch size: 512
Output directory: /tmp/output
...
2022-06-28 10:25:58 [INFO] |******Mixed Precision Statistics*****|
2022-06-28 10:25:58 [INFO] +-----------------+----------+--------+
2022-06-28 10:25:58 [INFO] |     Op Type     |  Total   |  INT8  |
2022-06-28 10:25:58 [INFO] +-----------------+----------+--------+
2022-06-28 10:25:58 [INFO] |      Conv2D     |    53    |   53   |
2022-06-28 10:25:58 [INFO] |      MatMul     |    1     |   1    |
2022-06-28 10:25:58 [INFO] |     MaxPool     |    4     |   4    |
2022-06-28 10:25:58 [INFO] |    QuantizeV2   |    5     |   5    |
2022-06-28 10:25:58 [INFO] |    Dequantize   |    4     |   4    |
2022-06-28 10:25:58 [INFO] +-----------------+----------+--------+
2022-06-28 10:25:58 [INFO] Pass quantize model elapsed time: 32164.27 ms
2022-06-28 10:25:58 [INFO] Start to evaluate the TensorFlow model.
2022-06-28 10:26:12 [INFO] Model inference elapsed time: 13921.64 ms
2022-06-28 10:26:12 [INFO] Tune 1 result is: [Accuracy (int8|fp32): 0.9008|0.9022, Duration (seconds) (int8|fp32): 13.9226|17.3321], Best tune result is: [Accuracy: 0.9008, Duration (seconds): 13.9226]
2022-06-28 10:26:12 [INFO] |**********************Tune Result Statistics**********************|
2022-06-28 10:26:12 [INFO] +--------------------+----------+---------------+------------------+
2022-06-28 10:26:12 [INFO] |     Info Type      | Baseline | Tune 1 result | Best tune result |
2022-06-28 10:26:12 [INFO] +--------------------+----------+---------------+------------------+
2022-06-28 10:26:12 [INFO] |      Accuracy      | 0.9022   |    0.9008     |     0.9008       |
2022-06-28 10:26:12 [INFO] | Duration (seconds) | 17.3321  |    13.9226    |     13.9226      |
2022-06-28 10:26:12 [INFO] +--------------------+----------+---------------+------------------+
2022-06-28 10:26:12 [INFO] Save tuning history to /tmp/output/nc_workspace/./history.snapshot.
2022-06-28 10:26:12 [INFO] Specified timeout or max trials is reached! Found a quantized model which meet accuracy goal. Exit.
...
INFO:tensorflow:SavedModel written to: /tmp/output/quantized/resnet_v1_50/1/saved_model.pb
2022-06-28 10:26:13 [INFO] SavedModel written to: /tmp/output/quantized/resnet_v1_50/1/saved_model.pb
2022-06-28 10:26:13 [INFO] Save quantized model to /tmp/output/quantized/resnet_v1_50/1
```

**Benchmark the quantized model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt benchmark --model-dir /tmp/output/quantized/resnet_v1_50/1 --dataset-dir /tmp/dataset/flower_photos --batch-size 512 --mode performance
```
```
Model directory: /tmp/output/quantized/resnet_v1_50/1
Dataset directory: /tmp/dataset/flower_photos
Benchmarking mode: performance
Batch size: 512
Model name: resnet_v1_50
Framework: tensorflow
...
performance mode benchmark result:
2022-06-28 10:28:33 [INFO] Batch size = 512
2022-06-28 10:28:33 [INFO] Latency: 0.946 ms
2022-06-28 10:28:33 [INFO] Throughput: 1056.940 images/sec
```

**Do graph optimization on the trained model**:
Make sure to specify your own file paths for `dataset-dir` and `output-dir`
```
tlt optimize --model-dir /tmp/output/resnet_v1_50/1 --output-dir /tmp/output
```
```
Model directory: /tmp/output/resnet_v1_50/1
Model name: resnet_v1_50
Output directory: /tmp/output
Framework: tensorflow
Starting graph optimization
...
2022-06-28 13:50:01 [INFO] Graph optimization is done.
...
2022-06-28 13:51:21 [INFO] SavedModel written to: /tmp/output/optimized/resnet_v1_50/1/saved_model.pb
```

More CLI Examples:
* [Image Classification](/examples/cli/image_classification.md)
* [Text Classification](/examples/cli/text_classification.md)

## Getting Started with the API
```python
from tlt.datasets import dataset_factory
from tlt.models import model_factory
from tlt.utils.types import FrameworkType, UseCaseType
import os

# Specify a directory for the dataset to be downloaded
dataset_dir = os.environ["DATASET_DIR"] if "DATASET_DIR" in os.environ else \
    os.path.join(os.environ["HOME"], "dataset")

# Specify a directory for output
output_dir = os.environ["OUTPUT_DIR"] if "OUTPUT_DIR" in os.environ else \
    os.path.join(os.environ["HOME"], "output")

# Get the model
model = model_factory.get_model(model_name="resnet_v1_50", framework=FrameworkType.TENSORFLOW)

# Load and preprocess a dataset
dataset = dataset_factory.load_dataset(dataset_dir = os.path.join(dataset_dir, "flower_photos"),
                                       use_case=UseCaseType.IMAGE_CLASSIFICATION, \
                                       framework=FrameworkType.TENSORFLOW)
dataset.preprocess(image_size=model.image_size, batch_size=32)
dataset.shuffle_split(train_pct=.75, val_pct=.25)

# Train the model using the dataset
model.train(dataset, output_dir=output_dir, epochs=1)

# Evaluate the trained model
metrics = model.evaluate(dataset)
for metric_name, metric_value in zip(model._model.metrics_names, metrics):
    print("{}: {}".format(metric_name, metric_value))

# Export the model
saved_model_dir = model.export(output_dir=output_dir)

# Create an Intel Neural Compressor config file
inc_config_file = os.path.join(output_dir, "inc_config.yaml")
model.write_inc_config_file(inc_config_file, dataset=dataset, batch_size=512, overwrite=True,
                            accuracy_criterion_relative=0.01, exit_policy_timeout=0,
                            exit_policy_max_trials=10, tuning_workspace=os.path.join(output_dir, "nc_workspace"))

# Quantize the trained model
quantization_output = os.path.join(output_dir, "quantized_model")
model.quantize(saved_model_dir, quantization_output, inc_config_file)

# Benchmark the trained model using the Intel Neural Compressor config file
model.benchmark(quantization_output, inc_config_file, 'performance')

# Do graph optimization on the trained model
optimization_output = os.path.join(output_dir, "optimized_model")
model.optimize_graph(saved_model_dir, optimization_output)
```

More API Examples:
* [TLT Tutorials](/notebooks#transfer-learning-tool-tutorial-notebooks)
* [TLT End-to-end Pipelines](/notebooks#transfer-learning-tool-end-to-end-pipelines)

#### DISCLAIMER: ####
These scripts are not intended for benchmarking Intel platforms. For any performance and/or benchmarking information on specific Intel platforms, visit https://www.intel.ai/blog.
 
Intel is committed to the respect of human rights and avoiding complicity in human rights abuses, a policy reflected in the Intel Global Human Rights Principles. Accordingly, by accessing the Intel material on this platform you agree that you will not use the material in a product or application that causes or contributes to a violation of an internationally recognized human right.
 
#### License: ####
Intel® Transfer Learning Tool is licensed under Apache License Version 2.0.
 
#### Datasets: ####
To the extent that any public datasets are referenced by Intel or accessed using tools or code on this site those datasets are provided by the third party indicated as the data source. Intel does not create the data, or datasets, and does not warrant their accuracy or quality. By accessing the public dataset(s) you agree to the terms associated with those datasets and that your use complies with the applicable license. [DATASETS](DATASETS.md)
 
Intel expressly disclaims the accuracy, adequacy, or completeness of any public datasets, and is not liable for any errors, omissions, or defects in the data, or for any reliance on the data.  Intel is not liable for any liability or damages relating to your use of public datasets.
