# opencv
## 读取
```
cv2.imread(‘path’, 1)
```

## 转成灰色
```
cv2.cvtColor(img, cv2.COLOR_BRG2GRAY)
```
## 显示图像
```
cv2.imshow
```

## 找到图片中的文字
[参考](https://www.jianshu.com/p/1b9c275698c9)

### 最大稳定极值区域MSER

[参考1](https://www.pyimagesearch.com/2014/11/17/non-maximum-suppression-object-detection-python)
[参考2](https://www.pyimagesearch.com/2015/02/16/faster-non-maximum-suppression-python/)

### 凸包
convexHull

### nms
[参考](https://www.pyimagesearch.com/2015/02/16/faster-non-maximum-suppression-python/)
[参考](https://www.pyimagesearch.com/2014/11/17/non-maximum-suppression-object-detection-python)

## 自适应阈值化
```
cv2.adaptiveThreshold()
```

## 霍夫变化
```
cv2.HoughLines
```

## Canny边界搜索
```
cv2.Canny
```
