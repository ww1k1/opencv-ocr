# 📄 文档扫描与OCR识别系统（Document Scanner + OCR）

## 📌 项目简介

本项目基于 OpenCV 与 Tesseract OCR 实现了一个**文档扫描与文字识别系统**。

系统可以从一张普通拍摄的图片中：

1. 自动检测文档区域
2. 进行透视变换（矫正角度）
3. 生成类似扫描件的清晰图像
4. 提取图像中的文本内容

实现从“拍照文档”到“可读文本”的完整流程。

---

## 🚀 项目效果

### 📷 输入

* 手机拍摄的文档图片

### 📄 输出

* 扫描后的标准文档图像（scan.jpg）：
  * OCR识别文本结果

```text id="demo1"
This is a sample text extracted from the document：
<img width="999" height="795" alt="image" src="https://github.com/user-attachments/assets/7502f54a-133d-48a6-b28e-4377932d984e" />
<img width="1434" height="1026" alt="image" src="https://github.com/user-attachments/assets/7bc23a6c-14bd-45fd-a35c-bc90f6b48065" />
<img width="1437" height="777" alt="image" src="https://github.com/user-attachments/assets/90a3c71d-4502-43ee-abe9-fbfdadbb612f" />

```
---

## 🛠 技术栈

* Python
* OpenCV
* NumPy
* Tesseract OCR
* pytesseract

---

## 📂 项目结构

```text id="proj2"
project/
│── scan.py        # 文档扫描（透视变换）
│── test.py        # OCR文字识别
│── images/        # 输入图片
│── scan.jpg       # 扫描结果
│── README.md
```

---

## ⚙️ 环境配置

### 1️⃣ 安装依赖

```bash id="dep2"
pip install opencv-python numpy pytesseract pillow
```

### 2️⃣ 安装 Tesseract

* 下载地址：https://digi.bib.uni-mannheim.de/tesseract/
* 安装后配置环境变量

或在代码中指定路径：

```python id="path"
pytesseract.pytesseract.tesseract_cmd = r'C:\unwindows\tesseract\Tesseract-OCR'
```
---

## ⚙️ 运行方法(debug软件为eclipse)

### 1️⃣ 文档扫描

```bash id="run1"
python scan.py -i images/doc.pngse
```

👉 输出：`scan.jpg`

---

### 2️⃣ OCR识别

```bash id="run2"
python test.py
```

👉 输出：终端打印识别文本

---

## 🎯 核心功能

* 文档边缘检测
* 轮廓筛选（自动找到文档区域）
* 透视变换（矫正图像）
* 二值化增强
* OCR文本识别

---

## 🔍 算法流程

1. 图像缩放与预处理
2. 灰度化 + 高斯滤波
3. Canny边缘检测
4. 轮廓检测并筛选最大四边形
5. 透视变换（Perspective Transform）
6. 二值化生成扫描效果
7. OCR文字识别

---

## 💻 核心代码展示

### 1️⃣ 文档边缘检测

```python id="code21"
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
gray = cv2.GaussianBlur(gray, (5, 5), 0)
edged = cv2.Canny(gray, 75, 200)
```

---

### 2️⃣ 轮廓筛选（寻找文档区域）

```python id="code22"
cnts = cv2.findContours(edged.copy(), cv2.RETR_LIST, cv2.CHAIN_APPROX_SIMPLE)[1]
cnts = sorted(cnts, key=cv2.contourArea, reverse=True)[:5]

for c in cnts:
    peri = cv2.arcLength(c, True)
    approx = cv2.approxPolyDP(c, 0.02 * peri, True)

    if len(approx) == 4:
        screenCnt = approx
        break
```

---

### 3️⃣ 透视变换（核心🔥）

```python id="code23"
M = cv2.getPerspectiveTransform(rect, dst)
warped = cv2.warpPerspective(image, M, (maxWidth, maxHeight))
```

---

### 4️⃣ OCR识别

```python id="code24"
text = pytesseract.image_to_string(Image.open(filename))
print(text)
```

---

## 🌟 项目亮点

* 完整实现“文档扫描 + OCR识别”流程
* 使用透视变换实现扫描效果（核心难点）
* 自动检测文档边界，无需手动标注
* 结合传统CV与OCR技术
* 实用性强（接近真实扫描App）

---

## ⚠️ 不足与改进

### 当前不足

* 对复杂背景或低对比度图像效果一般
* OCR识别准确率依赖图像质量
* 未支持多语言优化

### 改进方向

* 使用深度学习模型（如 EAST）检测文本区域
* 引入图像增强算法提升清晰度
* 使用更先进OCR模型（如 PaddleOCR）
* 增加自动裁剪与阴影去除

---

## 📬 联系方式

wechat：WangS040122
