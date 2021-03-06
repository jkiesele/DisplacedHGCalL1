
LLP CNN based HGCal trigger
================

Requirements:
  * DeepJetCore 3 and dependencies
  
From lxplus (CERN) run
``/eos/home-j/jkiesele/singularity/run_deepjetcore3.sh`` 
to enter interactive container environment.
```
cd <your cloned directory>
source env.sh
```

For quantized training, in default_training_cnn2.py set flag Quantized=True. You should have a pretrained unquantized model in .h5 form (default path is "full_model/model.h5"), used to initialise the model weights to speed up the training.  Select appropriate quantization map from qdictionaries.py. Make sure the layer names in the pre-loaded model nmatch those in the dictionary.
For pruning, in default_training_cnn2.py set flag Prune=True. You should have a pretrained unpruned model in .h5 form (default path is "full_model/model.h5"). Pass this to setWeights() to initilase the layer weights from the full model. Which layer to prune can be set in model_pruned(). To train:
```
python3 default_training_cnn2.py samples/ml_traindata_flatangle/dataCollection.djcdc TrainOutput --valdata samples/ml_valdata_flat/dataCollection.djcdc
```

When training the full model, the model floating point operations per layer is estimated. This can be used as an estimate of which layer has the highest impact on resources/latency and, therefore, also which layer is most sensible to compress.

predict.py command:
```
predict.py KERAS_check_model_last.h5 trainsamples.djcdc /eos/user/j/jalimena/DisplacedCalo/ML/ml_testdata_flatangle_histat/allfiles.txt PredictOutput
```
