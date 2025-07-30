# Mac应用构建说明

## 🍎 使用uv构建Mac应用程序

### 📋 前提条件

1. **macOS系统**: macOS 10.12 或更高版本
2. **Python环境**: Python 3.8+ 
3. **uv包管理器**: 已安装并配置
4. **Xcode命令行工具**: 用于编译原生组件

### 🚀 快速构建步骤

#### 1. 准备环境
```bash
# 确保在项目根目录
cd /path/to/Officetools

# 激活虚拟环境
source .venv/bin/activate

# 验证uv可用
uv --version
```

#### 2. 安装构建依赖
```bash
# 使用uv安装py2app（推荐）
uv pip install py2app

# 或者让脚本自动安装
python office2pdf/setup_app.py
```

#### 3. 构建应用
```bash
# 确保在项目根目录
cd /path/to/Officetools

# 开始构建Mac应用
python office2pdf/setup_app.py py2app

# 构建完成后清理（可选）
python office2pdf/setup_app.py clean
```

#### 4. 测试应用
```bash
# 启动构建的应用
open dist/PDF转换工具.app

# 或者在Finder中双击应用图标
```

### 📊 构建过程说明

#### 自动依赖检查
脚本会自动：
- ✅ 检测uv是否可用
- ✅ 安装py2app（如果未安装）
- ✅ 验证setuptools可用性
- ✅ 显示详细的构建进度

#### 构建输出
```
🔍 检查打包依赖...
✅ 找到uv: uv 0.1.x
✅ py2app已安装: 0.28.x
✅ setuptools已安装: 68.x.x

📱 开始构建Mac应用: PDF转换工具 v2.0.0
📄 主脚本: run_gui.py

[构建过程...]

🎉 Mac应用构建完成！
==================================================
📁 应用位置: dist/PDF转换工具.app
💡 使用提示:
  - 双击.app文件启动应用
  - 可以拖拽到Applications文件夹
  - 首次运行可能需要在系统偏好设置中允许
```

### 🔧 故障排除

#### 常见问题

**1. uv未找到**
```bash
# 安装uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# 或使用Homebrew
brew install uv
```

**2. py2app安装失败**
```bash
# 手动安装
uv pip install py2app

# 如果仍然失败，尝试pip
pip install py2app
```

**3. 构建失败**
```bash
# 清理之前的构建
python office2pdf/setup_app.py clean
rm -rf build/ dist/

# 重新构建
python office2pdf/setup_app.py py2app
```

**4. 应用无法启动**
- 检查macOS安全设置
- 在"系统偏好设置 > 安全性与隐私"中允许应用
- 确保LibreOffice已安装

#### 依赖问题
如果遇到依赖问题：
```bash
# 重新安装项目依赖
uv pip install -e .

# 验证安装
python office2pdf/verify_installation.py

# 测试GUI
python office2pdf/run_gui.py
```

### 📁 构建文件说明

#### 构建后的目录结构
```
Officetools/
├── build/                    # 构建临时文件
├── dist/                     # 最终应用
│   └── PDF转换工具.app        # Mac应用程序
├── setup_app.py              # 构建脚本
└── [其他项目文件]
```

#### 应用包内容
```
PDF转换工具.app/
├── Contents/
│   ├── Info.plist           # 应用信息
│   ├── MacOS/               # 可执行文件
│   ├── Resources/           # 资源文件
│   └── Frameworks/          # Python运行时
```

### 🎯 优化建议

#### 1. 减小应用大小
```bash
# 在setup_app.py中添加排除项
'excludes': [
    'test', 'tests', 'pytest',
    'numpy.tests', 'pandas.tests',
    'matplotlib', 'scipy'  # 如果不需要
]
```

#### 2. 添加应用图标
```bash
# 创建.icns图标文件
# 在setup_app.py中设置
'iconfile': 'icon.icns'
```

#### 3. 性能优化
- 使用`--optimize`标志
- 排除不必要的包
- 压缩资源文件

### 📋 分发清单

构建完成后，您将获得：
- ✅ `PDF转换工具.app` - 可分发的Mac应用
- ✅ 支持拖拽安装到Applications
- ✅ 包含所有必要的Python依赖
- ✅ 支持文件关联（Office、文本、Markdown文件）

### 💡 使用uv的优势

1. **速度更快**: uv比pip快10-100倍
2. **更可靠**: 更好的依赖解析
3. **兼容性**: 完全兼容pip工作流
4. **现代化**: 使用Rust编写，更稳定

### 🔗 相关链接

- [uv官方文档](https://github.com/astral-sh/uv)
- [py2app文档](https://py2app.readthedocs.io/)
- [macOS应用分发指南](https://developer.apple.com/documentation/xcode/distributing-your-app-for-beta-testing-and-releases)

---

*最后更新: 2025-07-30*
