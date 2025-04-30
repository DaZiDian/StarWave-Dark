

<!-- StarWave README.md -->
<div align="center">
  <img src="https://github.com/user-attachments/assets/dc78401c-e578-489c-ab31-a34727524f11" width="200" alt="StarWave Logo">
  <h1>StarWave AI歌声合成工作站</h1>
  
  [![Windows Build](https://img.shields.io/badge/Windows-Supported-success?logo=windows)](https://github.com/yourname/StarWave-Dark/actions)
  [![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
  [![Qt Version](https://img.shields.io/badge/Qt-6.6.0-blue?logo=qt)](https://www.qt.io/)
  [![CUDA](https://img.shields.io/badge/CUDA-11.8-76B900?logo=nvidia)](https://developer.nvidia.com/cuda-toolkit)

</div>

## 📌 项目概述
基于Qt6/C++开发的AI歌声合成工作站，整合RVC/SVC声学模型，提供专业级参数控制与实时音频处理能力。支持音色克隆、智能MIDI解析、多维度参数调整等功能，目标成为开源领域的VOCALOID替代方案。

---

## 🎛️ 功能矩阵
| 模块          | 功能                      | 实现状态 |
|---------------|---------------------------|----------|
| 核心引擎      | RVC/SVC模型推理           | ✅        |
|               | 实时参数插值              | ✅        |
| 音频处理      | PortAudio流式处理         | ✅        |
|               | 音高/歌词自动识别         | 🚧        |
| 界面系统      | 轨道视图                  | ✅        |
|               | 参数自动化曲线            | 🚧        |
| AI训练        | 音色模型训练 (3分钟音频)  | ✅        |

---

## 🛠️ 开发环境配置

### 必需组件
| 组件                  | 版本要求   | 下载链接                                                                 |
|-----------------------|------------|--------------------------------------------------------------------------|
| Visual Studio         | 2022       | [官网下载](https://visualstudio.microsoft.com/)                          |
| Qt                   | 6.6.0      | [Qt下载页](https://www.qt.io/download)                                   |
| CUDA Toolkit          | 11.8       | [NVIDIA CUDA](https://developer.nvidia.com/cuda-11-8-0-download-archive)|
| vcpkg                | 最新版     | [GitHub仓库](https://github.com/microsoft/vcpkg)                        |

### 一键安装脚本
```powershell
# 以管理员身份运行PowerShell
iex (iwr -useb https://raw.githubusercontent.com/yourname/StarWave-Dark/main/scripts/install_deps.ps1)
```
## 🚀 快速构建指南
### 步骤1：克隆仓库
```powershell
git clone --recursive https://github.com/yourname/StarWave-Dark
cd StarWave-Dark
```
### 步骤2：安装依赖
```powershell
# 安装C++依赖
vcpkg install --triplet=x64-windows `
    portaudio `
    onnxruntime-gpu[directml,cuda] `
    libsndfile

# 安装Python依赖
python -m pip install -r requirements.txt
```
### 步骤3：CMake配置
```powershell
mkdir build
cd build

cmake -G "Visual Studio 17 2022" -A x64 ^
    -DCMAKE_TOOLCHAIN_FILE="C:/vcpkg/scripts/buildsystems/vcpkg.cmake" ^
    -DCMAKE_PREFIX_PATH="C:/Qt/6.6.0/msvc2022_64/lib/cmake" ^
    -DUSE_CUDA=ON ^
    ..
```
### 步骤4：编译项目
```powershell
cmake --build . --config Release --target ALL_BUILD -j 8
```
## 🎹 使用教程
### 基本工作流
![workflow](https://github.com/user-attachments/assets/599032b9-c1b1-457a-9597-0c851e430b7d)

### 快捷键参考
| 操作 | 快捷键 | 说明 |
| ---- | ---- | ---- |
| 新建工程	| Ctrl + N |  |
| 导入音频 |	Ctrl + Shift + I | 支持WAV/FLAC格式 |
| 启动训练	| Ctrl + T | 需要NVIDIA GPU |
| 参数重置	| Ctrl + R | 恢复默认参数 |

## 🧩 插件开发
### 接口示例
```cpp
// 效果器插件接口
class IEffectPlugin {
public:
    virtual ~IEffectPlugin() = default;
    virtual void process(float* buffer, int frames) = 0;
    virtual QWidget* createEditor() = 0;
};
```
### 插件存放位置
```bash
StarWave/
└── plugins/
    ├── VST/
    └── LV2/  # 支持Linux
```

## 🧪 测试方案
### 单元测试
```powershell
ctest -C Release -VV
```
### 性能测试指标
| 测试项	| 目标值 |
| ---- | ---- |
| 音频延迟	| <20ms (ASIO) |
| 推理速度	| 实时x1.5 (RTX 3060) |
| 内存占用	| <500MB |

## 🤝 贡献指南
1. Fork仓库并创建分支
2. 遵循代码规范：
```cpp
// Qt对象命名规则
QPushButton *btnSettings = new QPushButton(this);  // 正确
QPushButton *settings_btn = new QPushButton(this); // 错误
```
3. 提交Pull Request前运行：
```
```powershell
clang-format -i --style=file src/*.cpp include/*.h
```
### ⚠️ 已知问题
- MIDI时钟同步存在±5ms抖动
- 中文歌词识别准确率约85%
- CUDA 12.x兼容性问题

## 📜 开源协议
### 本项目采用MIT许可证，但需注意：
- 商业使用需遵守RVC许可条款
- 训练数据集需确保版权合法性

> 提示：完整文档请访问 [![StarWave Wiki](https://starwave-wiki.dsmcc.cn)]
