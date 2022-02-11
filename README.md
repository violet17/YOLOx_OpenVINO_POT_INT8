# YOLOx_OpenVINO_POT_INT8
Optimize YOLOx to INT8 precision with OpenVINO Post-training Optimization Tool

validated on OpenVINO 2021.4.752

- Set up Environment and Convert to ONNX Model
```
git clone https://github.com/Megvii-BaseDetection/YOLOX
cd YOLOX
pip install -v -e .  # or  python setup.py develop

wget https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_s.pth
python tools/export_onnx.py --output-name yolox_s.onnx -n yolox-s -c yolox_s.pth --no-onnxsim
```

- Convert to IR Files with OpenVINO Model Optimizer
```
source /opt/intel/openvino_2021/bin/setupvars.sh
python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model yolox_s.onnx
```

- Revise POT YOLOx API
Add __init__.py and yolo.py to  /opt/intel/openvino_2021.4.752/deployment_tools/open_model_zoo/tools/accuracy_checker/accuracy_checker/adapters/

- Set up POT with Accuracy Checker
```
cd /opt/intel/openvino/deployment_tools/model_optimizer/install_prerequisites
sh sudo ./install_prerequisites.sh
cd /opt/intel/openvino/deployment_tools/open_model_zoo/tools/accuracy_checker
python setup.py install
cd /opt/intel/openvino/deployment_tools/tools/post_training_optimization_toolkit
python setup.py install
```

- Revise Accuracy Checker Configuration File yolox.yml

- Revise POT Configuration File yoloxs_AAQ.json

- Run POT
```
pot -c yoloxs_AAQ.json
```


