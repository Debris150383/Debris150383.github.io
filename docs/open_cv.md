# opencv

## 基本

### 表示方法

1. HSV（Hue，Saturation，Value）是另一种色彩空间，其中的颜色组成元素更接近人类对颜色的感知：

   - Hue（色调）：这是我们通常意义上的颜色名称，如红色、蓝色等。
   - Saturation（饱和度）：这是颜色的鲜艳程度，颜色从最鲜艳到最灰暗。
   - Value（亮度）：颜色从最亮（白色）到最暗（黑色）
   - 不同的库的H，S，V的表示方式和范围不同

   | 颜色 | H 范围        | S 范围 | V 范围  |
   | ---- | ------------- | ------ | ------- |
   | 红色 | 0-10, 160-180 | 50-255 | 50-255  |
   | 橙色 | 11-25         | 50-255 | 50-255  |
   | 黄色 | 26-35         | 50-255 | 50-255  |
   | 绿色 | 36-85         | 50-255 | 50-255  |
   | 青色 | 86-99         | 50-255 | 50-255  |
   | 蓝色 | 100-130       | 50-255 | 50-255  |
   | 紫色 | 131-159       | 50-255 | 50-255  |
   | 白色 | 不适用        | 0-40   | 200-255 |
   | 黑色 | 不适用        | 0-255  | 0-50    |

   ==上图是opencv的各个颜色hsv范围，为GPT4给出。准确的范围未考证。==

2. 几种表示可以相互转换;  在cv2中利用`cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)`之类的方法即可； 从BGR到灰度或者HSV的转换是通道加权转换，到了灰度由于只有一个值，从灰度转换为BGR会把这个值复制成3份给三个通道。

      ```python
      rgb_color = np.uint8([[[r, g, b]]])  
      hsv_color = cv2.cvtColor(rgb_color, cv2.COLOR_RGB2HSV)
      ```

### ROI

示例

```python
img[0:100, :, :]  
```

注意：对于坐标而言，x正向向右，y正向向下；与行列不对应

## 图像绘制

### cv2.circle

该函数可以在给定图像上绘制一个圆形，并指定圆心、半径、颜色和线条粗细等参数。

语法如下

```python
cv2.circle(image, center, radius, color, thickness)
```

参数解释：

- `image`: 要在其上绘制圆的图像。
- `center`: 圆心的坐标 `(x, y)`，其中 `(x, y)` 是整数值。
- `radius`: 圆的半径，为正整数值。
- `color`: 圆的颜色，可以是单个整数值、元组 `(B, G, R)` 或者 `(B, G, R, A)`（对于带有 alpha 通道的图像）。
- `thickness`: 线条的粗细，默认为1，**负数值表示填充整个圆**

### cv2.rectangle

OpenCV 库中用于在图像上绘制矩形的函数，它可以在给定图像上绘制一个矩形，并指定矩形的位置、颜色和线条粗细等参数。

语法：

```python
cv2.rectangle(image, start_point, end_point, color, thickness)

```

参数解释：

- `start_point`: 矩形的左上角点坐标 `(x, y)`，其中 `(x, y)` 是整数值。
- `end_point`: 矩形的右下角点坐标 `(x, y)`，其中 `(x, y)` 是整数值。



### 鼠标作为画笔

```python
import numpy as np
import cv2 as cv
drawing = False # 如果按下鼠标，则为真
mode = True 
ix,iy = -1,-1
# 鼠标回调函数
def draw_circle(event,x,y,flags,param):
    global ix,iy,drawing,mode
    if event == cv.EVENT_LBUTTONDOWN: # 按住鼠标左
        drawing = True
        ix,iy = x,y
    elif event == cv.EVENT_MOUSEMOVE: 
        if drawing == True:
            if mode == True:
                cv.rectangle(img,(ix,iy),(x,y),(0,255,0),0)
            else:
                cv.circle(img,(x,y),10,(0,0,255),-1)
    elif event == cv.EVENT_LBUTTONUP: #松开
        drawing = False
        if mode == True:
            cv.rectangle(img,(ix,iy),(x,y),(0,255,0),0)
        else:
            cv.circle(img,(x,y),5,(0,0,255),-1)

img = np.zeros((512,512,3), np.uint8)

cv.namedWindow('image')
cv.setMouseCallback('image',draw_circle)
while(1):
    cv.imshow('image',img)
    if cv.waitKey(20) & 0xFF == 27:
        break
cv.destroyAllWindows()
```



## threshold

### cv2.threshold

### cv2.adaptiveThreshold

## 腐蚀与膨胀

### cv2.dilate

### cv2.erode

## 模糊处理

### cv2.GaussianBlur

### cv2.medianBlur

## 边缘检测

### cv2.canny

### cv2.filter2D

## 轮廓检测

### cv2.findContours

### cv2.HoughCircles

### 颜色过滤

### 视频

##  其他

### cv2.convertScaleAbs

### cv2.bitwise

### cv2.inrange
