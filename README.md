# Use Variational Auto-Encoder to generate captions

## Overview
 Tensorflow Implementation of [Diverse and Accurate Image Description Using a Variational Auto-Encoder with an Additive Gaussian Encoding Space, (Nips)](https://papers.nips.cc/paper/7158-diverse-and-accurate-image-description-using-a-variational-auto-encoder-with-an-additive-gaussian-encoding-space.pdf)

## Usage

Training:
Just launch the training script:
```shell=
python main.py --gpu 'your gpu'
```
### Parameters
Parameters can be set directly in in utils/parameters.py file.
(or specify through command line parameters).
For example, if you want to train AG-CVAE model, which use cluster vectors as input to encoder and decoder, you can call:
```shell=
python main.py --gpu 0 --embed_dim 256 --dec_hid 512 --epochs 50 --temperature 0.6 --gen_name ag --dec_drop 0.7 --dec_lstm_drop 0.7 --lr 0.001 --checkpoint ag_cv_test1 --coco_dir "/home/username/mscoco/coco/" --optimizer Adam --sample_gen greedy --c_v --prior AG
```

### Generation
For list of required parameters:
```shell=
python gen_caption.py -h
```
For example:
```
python -i gen_caption.py --img_path ./images/COCO_val2014_000000233527.jpg --checkpoint ./checkpoints/gaussian_nocv.ckpt --params_path ./pickles/params_Normal_False_gaussian_nocv_False
```
Where:
- --params_path: saved Parameters class, can be saved by calling main.py --save_params
- --checkpoint: saved checkpoint
- --img_path: path for image
- -i: for launching python in interactive mode so captions can be generated by calling generator.generate_caption(img_path). This can be also used in ipython notebook

Trained CVAE without cluster vectors checkpoint + parameters file can be downloaded at:
https://yadi.sk/d/TCyXUmKk3SPVtc

### Implementation progress
- LSTM baseline (implemented)
- CVAE baseline (implemented)
- cluster vectors (impemented, vectors for test set generated using
  tensorflow object detection API and faster-RCNN)
- beam search (implemented)
- AG-CVAE (partially implemented)
- GMM-CVAE (implemented)
- Caption generation for new photos (partially implemented, will need to automate cluster vectors generation process)
- fine_tune for better result (in progress)

### Specific requirements
- zhusuan - probabilistic framework https://github.com/thu-ml/zhusuan/
- tensorflow >= 1.4.1

### Other files
- prepare_cluster_vectors_train_val.ipynb - takes MSCOCO dataset json files and generates cluster vectors
- prepare_test_vectors.ipynb - gets test set cluster vector file, prepared using tf.models API and generates cluster vector
- gen_caption_example.ipynb - generate caption for some photo (without cluster vectors inputs)
