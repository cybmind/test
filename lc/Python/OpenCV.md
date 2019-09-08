## OpenCV

打开一张图片，以灰度的方式读取出来并显示，如果按esc就退出，如果按s就保存退出，其他建就继续等待用户输入。
核心函数：
- cv2.imread()
- cv2.imshow()
- cv2.imwrite()



```
import cv2 as cv

# 打开图片
img = cv.imread("/Users/liaocheng/PycharmProjects/OpenCVDemo/images/640.jpeg", cv.IMREAD_GRAYSCALE)
# 显示图片
cv.imshow("650.jpeg", img)
while(True):
# 设置按键退出，如果设置为0就是一直等待用户按键
    k = cv.waitKey(0)
    if k == 27:
        cv.destroyAllWindows()
        break
    elif k == ord("s"):
        cv.imwrite("demo.jpeg", img)
        cv.destroyAllWindows()
        break
```