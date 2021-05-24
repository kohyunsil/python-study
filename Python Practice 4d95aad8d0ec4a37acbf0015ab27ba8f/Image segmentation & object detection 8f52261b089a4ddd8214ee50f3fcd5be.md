# Image segmentation & object detection

그랩컷 - machine running 과 관련 (Deep running (X))

```python
import sys
import numpy as np
import cv2

# 입력 영상 불러오기
src = cv2.imread('nemo.jpg')
  
if src is None:
	print('Image load failed!')
  sys.exit()
# 사각형 지정을 통한 초기 분할
rc = cv2.selectROI(src)
mask = np.zeros(src.shape[:2], np.uint8)

# 숫자 '5' 변경하는것 가능
cv2.grabCut(src, mask, rc, None, None, 5, cv2.GC_INIT_WITH_RECT)

# 0: cv2.GC_BGD, 2: cv2.GC_PR_BGD
# np.where (조건문) for문 보다 수십배는 빠르다.
mask_fg = np.where((mask == 0) | (mask == 2), 0, 1).astype('uint8')
mask_bg = np.where((mask == 1) | (mask == 3), 0, 1).astype('uint8')
cv2.imshow('mask_fg', mask_fg*255)
cv2.imshow('mask_bg', mask_bg*255)

dst_fg = src * mask_fg[:, :, np.newaxis]
dst_bg = src * mask_bg[:, :, np.newaxis]

# 초기 분할 결과 출력 
cv2.imshow('dst_fg', dst_fg) 
cv2.imshow('dst_bg', dst_bg)

cv2.waitKey()
cv2.destroyAllWindows()
```

![Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled.png](Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled.png)

```python
import sys
import numpy as np
import cv2

# 입력 영상 불러오기
src = cv2.imread('messi5.jpg')

if src is None:
    print('Image load failed!')
    sys.exit()

# 사각형 지정을 통한 초기 분할
mask = np.zeros(src.shape[:2], np.uint8) # 마스크 
bgdModel = np.zeros((1, 65), np.float64) # 배경 모델 
fgdModel = np.zeros((1, 65), np.float64) # 전경 모델

rc = cv2.selectROI(src)

cv2.grabCut(src, mask, rc, bgdModel, fgdModel, 1, cv2.GC_INIT_WITH_RECT)

# 0: cv2.GC_BGD, 2: cv2.GC_PR_BGD
mask2 = np.where((mask == 0) | (mask == 2), 0, 1).astype('uint8')
dst = src * mask2[:, :, np.newaxis]

# 초기 분할 결과 출력
cv2.imshow('dst', dst)

# 마우스 이벤트 처리 함수 등록
def on_mouse(event, x, y, flags, param):
    if event == cv2.EVENT_LBUTTONDOWN:
        cv2.circle(dst, (x, y), 3, (255, 0, 0), -1)
        cv2.circle(mask, (x, y), 3, cv2.GC_FGD, -1)
        cv2.imshow('dst', dst)
    elif event == cv2.EVENT_MOUSEMOVE:
        if flags & cv2.EVENT_FLAG_LBUTTON:
            cv2.circle(dst, (x, y), 3, (255, 0, 0), -1)
            cv2.circle(mask, (x, y), 3, cv2.GC_FGD, -1)
            cv2.imshow('dst', dst)
        elif flags & cv2.EVENT_FLAG_SHIFTKEY:
            cv2.circle(dst, (x, y), 3, (0, 0, 255), -1)
            cv2.circle(mask, (x, y), 3, cv2.GC_BGD, -1)
            cv2.imshow('dst', dst)

cv2.setMouseCallback('dst', on_mouse)
  
while True:
    key = cv2.waitKey()
    if key == 13:  # ENTER
        # 사용자가 지정한 전경/배경 정보를 활용하여 영상 분할
        cv2.grabCut(src, mask, rc, bgdModel, fgdModel, 1, cv2.GC_INIT_WITH_MASK) 
        mask2 = np.where((mask == 2) | (mask == 0), 0, 1).astype('uint8')
        dst = src * mask2[:, :, np.newaxis]
        cv2.imshow('dst', dst)
    elif key == 27:
        break

    
cv2.destroyAllWindows()
```

![Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%201.png](Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%201.png)

- Shift  누르고 지울 영역 선택

![Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%202.png](Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%202.png)

Match shape

- 외곽선이 잇는 그림을 받으면 그 외곽선을 가지고 쉐입을 기억할 수 있다.
- 쉐입이 회전하거나 뒤집어져 있어도 기억하고 있습 ('hu(후즈모멘트)' 알고리즘)

    (but, 외곽을 따는 함수는 findContours 함수로 딴다.)

```python
import sys
import numpy as np
import cv2
# 영상 불러오기
obj = cv2.imread('spades.png', cv2.IMREAD_GRAYSCALE) 
src = cv2.imread('symbols.png', cv2.IMREAD_GRAYSCALE)

if src is None or obj is None:
    print('Image load failed!')
    sys.exit()

# 객체 영상 외곽선 검출
_, obj_bin = cv2.threshold(obj, 128, 255, cv2.THRESH_BINARY_INV)
obj_contours, _ = cv2.findContours(obj_bin, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE) 
obj_pts = obj_contours[0]

# 입력 영상 분석
_, src_bin = cv2.threshold(src, 128, 255, cv2.THRESH_BINARY_INV)
contours, _ = cv2.findContours(src_bin, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)

# 결과 영상
dst = cv2.cvtColor(src, cv2.COLOR_GRAY2BGR)

# 입력 영상의 모든 객체 영역에 대해서 
for pts in contours:
    if cv2.contourArea(pts) < 1000:
        continue
    
    rc = cv2.boundingRect(pts)
    cv2.rectangle(dst, rc, (255, 0, 0), 1)

    # 모양 비교 > 유사도 받아옴 > 유사도가 얼마 이상이면 빨간색 사각형으로 
    dist = cv2.matchShapes(obj_pts, pts, cv2.CONTOURS_MATCH_I3, 0)
    
    cv2.putText(dst, str(round(dist, 4)), (rc[0], rc[1] - 3),
                cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 0, 0), 1, cv2.LINE_AA)
      
    if dist < 0.1:
        cv2.rectangle(dst, rc, (0, 0, 255), 2)

cv2.imshow('obj', obj)
cv2.imshow('dst', dst)
cv2.waitKey(0)
```

![Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%203.png](Image%20segmentation%20&%20object%20detection%208f52261b089a4ddd8214ee50f3fcd5be/Untitled%203.png)