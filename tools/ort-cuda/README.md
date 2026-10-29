# Experimental ORT CUDA OCR configuration

`WITH_ORT_CUDA` defaults to `OFF`. This option selects the ONNX Runtime CUDA EP for OCR while retaining CPU image preprocessing and postprocessing. It does not enable FastDeploy's native CUDA preprocessing or custom CUDA kernels.

Use a matching CUDA-enabled ONNX Runtime, CUDA/cuDNN DLLs, and the existing ORT-backed FastDeploy build. Keep the dependency versions and DLL import names compatible. Place experiment dependencies in a separate application directory; do not replace system or another MAA installation's DLLs.

Only the pinned standard-ONNX OCR models were validated. Models requiring FastDeploy custom CUDA operators need a separately validated GPU build. Inspect model operators before enabling this experiment for another resource pack.

Configure with `-DWITH_ORT_CUDA=ON` and build `Debug`. The default build and CPU selection remain unchanged. GPU placement must be verified with a profiler; successful configuration alone is not proof of GPU execution.

## 简体中文

此实验选项默认关闭，只配置 ORT CUDA 推理，图像前后处理仍在 CPU。它不代表启用了 FastDeploy 自己的 CUDA 前处理或自定义 CUDA 算子。

请隔离部署版本与导入名称匹配的 ORT GPU、CUDA/cuDNN 及 FastDeploy 运行库。不要覆盖系统或其他 MAA 安装中的 DLL。当前验证仅覆盖固定版本的标准 ONNX OCR 模型；其他资源包须先检查算子与结果一致性。配置成功不等于 GPU 运算已生效，需用 profiler 核实。

## Offline smoke test

Use a numeric CUDA device ordinal. Adapter LUID selectors are rejected in this build because a DXGI index is not a CUDA ordinal. The current WPF LUID selection path is not integrated by this experimental option.

```powershell
python tools/maa_core_eval.py --mode ocr --gpu-device 0 --dll-dir <isolated-cuda-directory> --resource <repository-root> <screenshot.png>
```

This uses the existing Debug image evaluator without connecting to a device or executing game actions. `--gpu-device` configures inference before loading resources. With the default build, the same argument retains the existing GPU provider selection.

CUDA 实验构建只接受数字设备序号，不把 DXGI/LUID 当作 CUDA 序号。当前 WPF 的 LUID 选择路径尚未接入此实验选项。上面的离线测试不连接设备、不执行游戏操作。
