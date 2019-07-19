# Kaldi-DcAE
Kaldi-DcAE is the implementation of discriminative autoencoder (DcAE) built on open source Kaldi toolkit. 
A PATCH file with the modified nnet3 library and example scripts for WSJ task is provided.

## Prerequisites
If not already done, install Kaldi 
```
git clone https://github.com/kaldi-asr/kaldi.git kaldi --origin upstream
```

## Apply Patch
At Kaldi's base directory
```
git clone https://github.com/melodyhpt/Kaldi-DcAE.git
```

Take a look at what changes are in the patch
```
git apply --stat Kaldi-DcAE/DcAE.patch
```
Test the patch before you actually apply it
```
git apply --check Kaldi-DcAE/DcAE.patch
```
If you don’t get any errors, the patch can be applied cleanly.
```
git am --signoff < Kaldi-DcAE/DcAE.patch
```

compile nnet3 library
```
cd ../src/nnet3bin
make depend -j 4
make -j 4
```

## WSJ example
1. In stage 7 of run.sh, change command to
```
local/nnet3/run_tdnn_dcae.sh
```
or 
```
local/nnet3/run_tdnn_lstm_dcae.sh
```

2. The weight for the two output layers can be changed by modifying weight and weight_ae in run_tdnn_dcae.sh and run_tdnn_lstm_dcae.sh

#### WER obtained:

|TDNN         |tgpr_dev93|tg_dev93|bd_tgpr_dev93|bd_tgpr_dev93_fg|tgpr_eval92|tg_eval92|bd_tgpr_eval92|bd_tgpr_eval92_fg|
|-------------|------------|------------|------------|------------|------------|------------|------------|------------|
|baseline     |        9.29|        8.71|        6.62|        5.91|        6.27|        5.81|        3.92|        3.31|
|DcAE-B       |    **8.84**|    **8.38**|    **6.23**|        5.85|    **6.11**|        5.81|    **3.62**|        3.24|
|DcAE-U (append)|        8.97|        8.55|        6.52|        5.77|    **6.11**|        5.79|        3.79|        3.26|
|DcAE-U (sum) |        9.02|        8.48|        6.36|    **5.68**|         6.2|    **5.72**|        3.67|    **3.07**|


|TDNN-LSTM    |tgpr_dev93|tg_dev93|bd_tgpr_dev93|bd_tgpr_dev93_fg|tgpr_eval92|tg_eval92|bd_tgpr_eval92|bd_tgpr_eval92_fg|
|-------------|------------|------------|------------|------------|------------|------------|------------|------------|
|baseline     |         8.6|        8.22|        6.29|        5.85|         6.4|        6.13|         4.0|    **3.49**|
|DcAE-B       |        8.72|        8.28|        6.45|        5.77|    **6.29**|    **5.92**|         3.9|        3.54|
|DcAE-U (append)|        8.82|        8.33|        6.56|        5.82|        6.49|        5.97|        4.09|        3.74|
|DcAE-U (sum) |    **8.53**|    **8.19**|    **6.21**|    **5.73**|        6.43|        5.95|    **3.77**|        3.72|
