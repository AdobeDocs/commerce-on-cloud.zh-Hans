---
source-git-commit: 5236f2718e5091dfc2fc140e15d4ac87a073d1d0
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---
# 用于图像优化的预提交挂接

此目录包含预提交挂接，用于在将图像提交到存储库之前自动优化图像。

## 钩子做什么

- **自动检测**&#x200B;暂存的图像文件(PNG、JPG、JPEG、GIF、SVG)
- **运行`image_optim`**&#x200B;以压缩和优化图像
- **自动重新存放优化映像**
- **确保所有已提交的映像**&#x200B;都已正确优化

## 优点

- 减少了存储库大小
- 更快地加载文档的页面
- 所有参与者均具有一致的图像质量
- 无需手动优化

## 先决条件

- Ruby 3.0或更高版本
- Bundler
- Git

## 设置

### 自动设置（推荐）

```bash
.githooks/setup-hooks.sh
```

### 手动设置

```bash
git config core.hooksPath .githooks
chmod +x .githooks/*
```

### 完成项目设置

1. 克隆存储库：

   ```bash
   git clone <repository-url>
   cd commerce-admin.en
   ```

2. 启用预提交挂接：

   ```bash
   .githooks/setup-hooks.sh
   ```

3. 安装Jekyll依赖项：

   ```bash
   cd _jekyll
   bundle install
   ```

## 测试挂钩

1. 将图像文件添加到存储库
2. 暂存： `git add <image-file>`
3. 尝试提交： `git commit -m 'test'`
4. 挂接应自动优化图像

### 预期输出

```bash
Found 1 staged image(s). Running optimization...
Optimizing: path/to/your/image.png
Re-staged optimized image: path/to/your/image.png
Image optimization complete!
```

## 图像准则

- **PNG**：用于屏幕截图和UI元素（将自动优化）
- **SVG**：用于图标和简单图形（默认情况下禁用优化）
- **JPEG**：用于照片（将自动优化）
- **GIF**：用于动画（将自动优化）

预提交挂接将在提交时自动优化所有图像。

## 手动优化

对于手动图像优化：

```bash
cd _jekyll
bundle exec rake images:optimize path=../path/to/images
```

## 配置

挂接使用配置文件`_jekyll/.image_optim`自定义优化设置：

- **PNG**：使用`advpng`、`optipng`和`pngquant`
- **JPEG**：使用`jhead`、`jpegoptim`和`jpegtran`
- **GIF**：使用`gifsicle`
- **SVG**：默认情况下已禁用SVG优化（可能会破坏复杂的矢量图形和动画）

## 故障排除

### 挂接未运行

- 检查挂接配置： `git config core.hooksPath`
- 确保挂接文件可执行： `chmod +x .githooks/pre-commit`
- 验证您是否在`_jekyll`目录的正确存储库中

### 优化失败

- 验证`bundle install`是否已在`_jekyll`目录中运行
- 检查是否已安装`image_optim`和`image_optim_pack` gem
- 查看`.image_optim`配置文件

### 性能问题

- 在`_jekyll/.image_optim`中调整线程计数
- 设置`DEBUG=1`环境变量以获取详细的错误信息

## 工作原理

1. **预提交触发器**：运行`git commit`时，挂接会自动执行
2. **图像检测**：扫描暂存文件以获取图像扩展名
3. **优化**：在每个暂存映像上运行`image_optim`
4. **正在重新暂存**：自动将已优化的映像添加回暂存区
5. **提交继续进行**：如果优化成功，提交将正常继续

## 支持的图像格式

- **PNG** (`.png`) — 无损和有损压缩
- **JPEG** (`.jpg`， `.jpeg`) — 包含元数据清理的有损压缩
- **GIF** (`.gif`) — 动画和静态优化
- **SVG** (`.svg`) — 矢量优化（默认禁用）

## 最佳实践

1. **测试挂接**：尝试先提交小图像以确保其正常工作
2. **查看更改**：检查Git差异以查看优化结果
3. **监视器性能**：大型映像可能需要一段时间才能优化
4. **版本控制**：挂接存储在此`.githooks/`目录中

## 支持

对于预提交挂接的问题：

1. 检查挂接输出中的错误消息
2. 验证您的`image_optim`设置是否正常工作
3. 首先使用手动Rake任务进行测试
4. 查看挂接日志和配置
5. 检查挂接配置： `git config core.hooksPath`
