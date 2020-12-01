# How to convert TFLite models

1. Prepare yourself the model file which has the extension `tflite`. In this example, we have `model_unquant.tflite`.
2. Run `tflite-onnx` in the docker. With the example given and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/tflite-onnx/onnx_tflite/tflite2onnx.py -tflite model_unquant.tflite -save_path model_unquant.tflite.onnx -release_mode True
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py model_unquant.tflite.onnx -o /data1/target.onnx --add-bn -t
```