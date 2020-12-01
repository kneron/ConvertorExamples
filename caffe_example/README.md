# How to convert Caffe models

1. Prepare yourself two files, the model structure file with extention `prototxt` and the weight file with extension
`caffemodel`. And please make sure the model can be loaded in intel-caffe 1.0. For this example, we have
`mobilenetv2.prototxt` and `mobilenetv2.caffemodel`.
2. Run `caffe-onnx` in the docker. With the example given and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/caffe-onnx/generate_onnx.py -n mobilenetv2.prototxt -w mobilenetv2.caffemodel -o /data1/mobilenetv2.onnx
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py /data1/mobilenetv2.onnx -o /data1/mobilenetv2.opt.onnx --add-bn -t
```