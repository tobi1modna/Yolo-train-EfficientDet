# Yolo-train-EfficientDet
The purpose of this repository is training efficientDet model with a custom dataset.
In this specific case I first want to convert a dataset from YOLO format to COCO format.

## Install

1. First create a new conda environment with the .yml file:
`conda create --file effD36.yml`

2. Activate the env:
`conda activate effD36`

3. Install with pip the following packages:
* `pip install opencv-python==3.4.2.17`
* `pip install opencv-contrib-python==3.4.2.17`
* `pip install git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI`

4. Convert your YOLO format dataset to a COCO dataset:
* `cd Yolo-train-EfficientDet/Yolo-to-COCO-format-converter`
* `python main.py -p <path/to/training_set> --imgf <image_extension> --inplace`
* `python main.py -p <path/to/validation_set> --imgf <image_extension> --inplace --output instances_val2017.json`

Specify in the `--imgf` parameter your image extension (the default for this is jpg).
The parameter `--inplace` is an option for saving your annotation json file in the `<-p>/../annotations/<--output>` folder.

## Train
* `cd Yolo-train-EfficientDet/EfficientDet`
* `python train.py --snapshot imagenet --phi 0 --gpu 0 --random-transform --compute-val-loss --freeze-backbone --batch-size 32 --steps 1000 coco /data/effD/dataset/yolo_ds_no_bg/`
* `python train.py --snapshot checkpoints/2022-04-01/coco_26_0.0371_0.3581.h5 --phi 0 --gpu 0 --random-transform --compute-val-loss --freeze-bn --batch-size 4 --steps 10000 coco /data/effD/dataset/yolo_ds_no_bg/`

## Evaluation
* `cd Yolo-train-EfficientDet/EfficientDet`
* Modify "eval/coco.py" setting the right wheights path, the phi, and the test dataset (note that the folder in which the test set is should be in the `<first_path_specified>/images/<second_path_specified>/`)
* `python eval/coco.py`

## Inference
* Again set all the right informations in `inference.py` like in Evaluation step.
* `python inference.py`