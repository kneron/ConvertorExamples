# How to convert Pytorch model

## Use the script with pth file (not recommended)

This solution is not recommended since Pytorch model saving machanism is quite tricky. It actually do not support saving
the model to a flattened structure. For example, if you models are defined using blocks, which contains the actual
operators. Then, the saved pth file may not contains those actual operators but those blocks. And the convertor will
crash without the definition of the block.

1. Prepare yourself the model file which has the extension `pth`. Please make sure your model file has all
model structure information inside. And this converter only support Pytorch < 1.6.0. Pytorch 1.6.0 is not supported yet.
In this example, we have `resnet34.pth`.
2. Run `pytorch2onnx` with the input size. With the example given and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/pytorch2onnx.py resnet34.pth /data1/temp.onnx --input-size 3 224 224
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py /data1/temp.onnx -o /data1/target.onnx --add-bn -t
```

## Use the script with pytorch exported onnx

1. Prepare yourself the model and export it to onnx using `torch.onnx`. Let me take the `resnet34.pth` as an example:
```python
# Prepare the model. Here we load the model directly from file.
from torch.autograd import Variable
import torch
import torch.onnx
# Standard ImageNet input - 3 channels, 224x224.
# Values don't matter as we care about network structure.
# But they can also be real inputs.
dummy_input = Variable(torch.randn(1, 3, 224, 224))
# Obtain the model, it can be also constructed in your script explicitly.
model = torch.load("resnet34.pth", map_location='cpu')

# Export the model.
if torch.__version__ < '1.3.0':
    torch.onnx.export(model, dummy_input, 'exported.onnx')
else:
    torch.onnx.export(model, dummy_input, 'exported.onnx', keep_initializers_as_inputs=True)
```
2. Run `pytorch2onnx` with the exported onnx and with the toolchain docker, the command should be:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/pytorch2onnx.py exported.onnx /data1/temp.onnx
```
3. Remember to feed the result to `onnx2onnx` to further optimize the model:
```bash
python /workspace/libs/ONNX_Convertor/optimizer_scripts/onnx2onnx.py /data1/temp.onnx -o /data1/target.onnx --add-bn -t
```
