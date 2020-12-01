# How to convert Tensorflow models

1. Prepare yourself the frozen graph file which has the extension `pb`. In this example, we have `mnist.pb`.
2. Run `tensorflow2onnx` in the docker. With the example given and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/tensorflow2onnx.py mnist.pb mnist.onnx
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py minist.onnx -o /data1/target.onnx --add-bn -t
```