# How to convert Keras models

1. Prepare yourself the model file which has the extension `h5` or `hdf5`. Please make sure your model file has the
model structure information inside. Sometimes, the downloaded file only contains the weight. And this converter only
support Keras <= 2.2.4. `tf.keras` and Keras 2.3 are not supported yet. In this example, we have `onet-0.417197.hdf5`.
2. Run `keras-onnx` in the docker. With the example given and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/keras-onnx/generate_onnx.py onet-0.417197.hdf5 -o /data1/target.onnx -O --duplicate-shared-weights
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py /data1/target.onnx -o /data1/target.opt.onnx --add-bn -t
```