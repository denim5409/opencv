# OpenCV

## 1. OpenCV의 정의
<br>
- 오픈 소스 컴퓨터 비전 라이브러리 중 하나로 크로스플랫폼과 실시간 이미지 프로세싱에 중점을 둔
  라이브러리이다. Windows, Linux, OS X(macOS), iOS, Android 등 다양한 플랫폼을 지원한다.
<br><br>
- 본래 C 언어만 지원했지만 2.x 버전부터 스마트포인터 스타일을 활용하여 C++을 지원하기 시작했고,<br>
  3.3 버전인 현재 C++11을 공식으로 채택하고 있다. 과거 C 스타일(IplImage)의 코드는 현재 레거시로만 남아<br>
  있지만 실행해보면 여전히 잘 돌아간다. Python을 공식적으로 지원한 이래 현재는 관련 라이브러리를<br>
  검색하면 C++보다 파이썬이 먼저 나올 만큼 C++을 직접 활용하기보다 파이썬으로 랩핑하여 사용하는 추세이다. <br>
  특히 딥러닝 관련 연구가 파이썬으로 진행되면서 파이썬 라이브러리의 사용 빈도가 더욱 늘었다. <br>
  픽셀단위의 접근이 빈번하게 이루어진다면 당연히 C++을 써야겠지만, 단순한 매트릭스 연산에 머무는 경우<br>
  numpy와 cv2의 궁합을 이용하면 C++에 비해 월등히 편리하다. 버전별로 사용방법과 코딩 스타일이 달라지는<br>
  C++에 비해 라이브러리 인터페이스가 안정적인 것도 파이썬만의 장점이다.<br>
  그 밖에 C#은 다양한 랩핑 라이브러리가 있지만 OpenCVSharp이 많이 쓰인다. iOS와 Android도 지원하므로<br>
  사실상 Java와 Objective-C도 지원하는 셈이다. MATLAB 등의 프로그램들과 연계도 가능하다.
<br><br>
- 영상 관련 라이브러리로서 사실상 표준의 지위를 가지고 있다. 조금이라도 영상처리가 들어간다면 필수적으로<br>
  사용하게 되는 라이브러리. <br>
  OpenCV 이전에는 MIL 등 상업용 라이브러리를 많이 사용했으나 OpenCV 이후로는 웬만큼 특수한 상황이<br>
  아니면 OpenCV만으로도 원하는 영상 처리가 가능하다. <br>
  기능이 방대하기 때문에 OpenCV에 있는 것만 다 쓸 줄 알아도 영상처리/머신러닝의 고수 반열에 속하게 된다.<br>
  조금 써봤다는 사람은 많지만 다 써봤다는 사람은 별로 없으며, 최신 버전의 라이브러리를 바짝 따라가는<br>
  사람은 영상 전공자 중에서도 드물다.
<br><br>
- 영상처리를 대중화시킨 1등 공신이다. 영상처리 입문 equals OpenCV 입문으로 봐도 좋을 정도이다. <br>
  예전에는 눈이 휘둥그래지는 신기한 영상처리 결과물들이 대중적으로 평범해지고 시시해진 것에는 수많은<br>
  영상 관련 연구와 더불어 OpenCV의 기여를 결코 무시할 수 없다. 누구나 영상 처리에 입문하여 웬만한<br>
  결과들은 코드 몇 줄로 구현이 가능해짐과 동시에, 원리도 모르고 분석도 못하고 그저 있는 함수만 가져다<br>
  쓰는 입문자가 많이 늘었다.<br>

[나무위키참조](https://namu.wiki/w/OpenCV)
<br><br>


- 한마디로 정의하자면, 영상 관련 처리가 필요하다면 무조건 사용해야 하는 라이브러리입니다.<br>
- 속도 측면에서는 C++에서 유리하나, 요즘 각광받는 머신러닝이나 간단한 영상처리 측면에서는<br>
  Python으로도 문제없이 사용할 수 있습니다.<br>
- 본 페이지에서는 기본적으로 Python을 이용한 OpenCV 라이브러리 사용방법을 공유하고자 합니다.<br>
  C++을 이용한 예제는 아래쪽에 기술하도록 하겠습니다.
<br><br><br>

## 2. Python에서의 OpenCV 주요 기능

### (1) image 불러오기
- 가장 기본적인 image 파일을 불러와 화면에 띄우는 코드입니다.
```python
import cv2
import numpy as np

#이미지 로드
img = cv2.imread('images/2.jpg', cv2.IMREAD_COLOR) #이미지를 컬러로 읽어들임
# img = cv2.imread('images/2.jpg', cv2.IMREAD_GRAYSCALE) #이미지를 그레이스케일로 읽어들임

img = cv2.pyrDown(img) # 사이즈 절반으로 줄이기.
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY) # img2를 그레이로 변경

cv2.imshow("color",img)#별도 창에 image가 뜬다.
cv2.imshow("gray",img_gray)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과
<img src="/images/cv_img_load.jpg"><br>

### (2) image 붙이기
- image 두개를 상하,좌우로 붙이기 입니다.
```python
import cv2

img_color = cv2.imread("images/cat on laptop.jpg", cv2.IMREAD_COLOR)
img_flip_ud = cv2.flip(img_color, 0)#이미지 상하반전
img_flip_lr = cv2.flip(img_color, 1)#이미지 좌우반전

height, width, channel = img_color.shape

result1 = cv2.vconcat([img_color, img_flip_ud])
result2 = cv2.hconcat([img_color, img_flip_lr])


cv2.namedWindow('Color')
cv2.imshow('Color', img_color)
cv2.imshow('UpDown', result1)
cv2.imshow('LeftRight', result2)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_vconcat.jpg">
<img src="/images/cv_img_hconcat.jpg"><br><br>

- 이 외에도 image의 회전, 축소, 확대 등이 있습니다. 자세한 내용은 소스코드 파일을 참조해주세요.  
[(opencv.ipynb)](https://github.com/denim5409/opencv/opencv.ipynb)<br><br>

### (3) image를 화면에 바로 띄우기
- 기존에는 image가 별도의 창에 떴다면 이번에는 그래프창에 image를 바로 띄우는 코드입니다.
```python
#그래프에 이미지 추가
import matplotlib.pyplot as plt
import cv2
import numpy as np

img_color = cv2.imread("images/iu.jpg", cv2.IMREAD_COLOR)
height, width, channel = img_color.shape

img_gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)
merge = cv2.merge((img_gray,img_gray,img_gray))
img_color = cv2.hconcat([img_color, merge])
img_color = cv2.cvtColor(img_color, cv2.COLOR_BGR2RGB)

plt.imshow(img_color)
plt.axis('off')
```

실행결과<br>
<img src="/images/cv_img_in_plot.jpg"><br><br>


### (4) image의 특정 부분만 gray 처리하기
- image의 일부분만 gray로 변환하여 적용하는 코드입니다.
```python
import cv2
import numpy as np

img_color = cv2.imread("images/cat on laptop.jpg", cv2.IMREAD_COLOR)
height, width, channel = img_color.shape
img_color = cv2.pyrUp(img_color)


img_slice = img_color[100:300, 200:500]

img_gray = cv2.cvtColor(img_slice, cv2.COLOR_BGR2GRAY)
temp = cv2.merge((img_gray,img_gray,img_gray))
img_color[100:300, 200:500] = temp


cv2.imshow("Color1",img_color)
cv2.imshow("Color2",img_slice)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_gray1.jpg"><br><br>


### (5) image 이진화(binary) 적용하기
- image를 흰색과 검정색의 2가지 색상으로만 나타내는(binary) 코드입니다.  
  각종 영상검출면에서 아주 많이 사용되는 방식입니다.
```python
import cv2
import numpy as np

img_color = cv2.imread("images/iu.jpg", cv2.IMREAD_COLOR)
img_gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)
height, width, channel = img_color.shape

ret, img_binary = cv2.threshold(img_gray, 120,255,cv2.THRESH_BINARY)

cv2.imshow("Color",img_color)
cv2.imshow("Binary", img_binary)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_binary.jpg"><br><br>

- 이진화 방식이 여러가지가 있으므로 코드를 확인하고 적당한 방식을 사용하시기 바랍니다.<br>
<img src="/images/cv_img_binary2.jpg"><br><br>

### (6) image Edge inspection
- image를 선 형태로 검출하여 표현하는 방식입니다.
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt

img_color = cv2.imread("images/iu.jpg", cv2.IMREAD_COLOR)
img_color = cv2.pyrDown(img_color)

img_gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)
height, width, channel = img_color.shape

img_canny = cv2.Canny(img_gray, 100, 255)
img_sobel1 = cv2.Sobel(img_gray, cv2.CV_8U, 1, 0, 3) #가로선
# img_sobel2 = cv2.Sobel(img_gray, cv2.CV_8U, 0, 1, 3) #세로선
# img_sobel3 = cv2.Sobel(img_gray, cv2.CV_8U, 1, 1, 3) #대각선
                         #이미지,   정밀도,  X방향, Y방향, 커널크기             

img_laplacian = cv2.Laplacian(img_gray, cv2.CV_8U, ksize=5)
    
cv2.imshow("Color",img_color)
cv2.imshow("canny", img_canny)
cv2.imshow("sobel1", img_sobel1)
cv2.imshow("laplacian", img_laplacian)

cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_edge.jpg"><br><br>

### (7) image의 특정 색상만 검출하기
- image에서 원하는 색상만 검출하는 코드입니다. 
  + 색상을 HSV형태로 변환하여 원하는 색상 범위를 지정해주어야 합니다.
  + HSV 색상체계는 다음 사이트를 참조하세요. [색상코드](https://www.google.com/search?q=rgb+to+hex&oq=rgb&aqs=chrome.2.69i57j0l7.4543j0j8&sourceid=chrome&ie=UTF-8)
  + 빨간색의 경우 HSV 색범위에 좌우측 끝부분에 둘다 포함되어 있으므로, 색 영역을 합쳐줘야 합니다.

오렌지색 검출
```python
import cv2
import numpy as np
 
src = cv2.imread("images/apples.jpg", cv2.IMREAD_COLOR)
src = cv2.pyrDown(src)
hsv = cv2.cvtColor(src, cv2.COLOR_BGR2HSV)

h, s, v = cv2.split(hsv)

h = cv2.inRange(h, 8, 20)
orange = cv2.bitwise_and(hsv, hsv, mask = h)
orange = cv2.cvtColor(orange, cv2.COLOR_HSV2BGR)

cv2.imshow("orange", orange)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_color_filtering.jpg"><br><br>

빨간색 검출
```python
import cv2
import numpy as np
 
src = cv2.imread("images/apples.jpg", cv2.IMREAD_COLOR)
src = cv2.pyrDown(src)
hsv = cv2.cvtColor(src, cv2.COLOR_BGR2HSV)

h, s, v = cv2.split(hsv)

l_red = cv2.inRange(hsv, (0,100,100), (5,255,255))
u_red = cv2.inRange(hsv, (170,100,100), (180,255,255))
added_red = cv2.addWeighted(l_red, 1.0, u_red, 1.0, 0,0)

red = cv2.bitwise_and(hsv, hsv, mask = added_red)
red = cv2.cvtColor(red, cv2.COLOR_HSV2BGR)


cv2.imshow("red", red)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_color_red.jpg"><br><br>

### (8) opencv로 도형그리기
- opencv가 image만 불러오는게 아니라 도형/선도 그릴 수 있습니다. 텍스트 입력도 가능합니다.
- image와 조합도 가능합니다.  
  추후 이를 활용하여 회로도의 양품/불량판정, 도로의 차선 인식 등에 대한 설명도 예정입니다.
- 예제는 사각형 그리는 코드입니다. 나머지 도형은 전체 코드파일을 참조하세요.
[(opencv.ipynb)](https://github.com/denim5409/opencv/opencv.ipynb)

```python
#사각형
import numpy as np
import cv2 as cv

width = 640
height = 480
bpp = 3

img = np.zeros((height, width, bpp), np.uint8)

cv.rectangle(img, (50, 50),  (450, 450), (0, 0, 255), 3)
cv.rectangle(img, (150, 200), (250, 300), (0, 255, 0), -1)
cv.rectangle(img, (300, 150, 50, 100), (255, 0, 255), -1)

cv.imshow("result", img)
cv.waitKey(0);
```

실행결과<br>
<img src="/images/cv_img_rectangle.jpg"><br><br>

### (9) 각 도형들의 외곽선 그리기
- 주어진 image에서 각 도형들의 외곽선을 인식할 수 있고 그 테두리를 그릴 수 있습니다.
- 앞에서 배운 특정 색상만 추출하는 코드를 이용하면 해당 색상의 도형의 테두리만 그릴 수 있습니다.

```python
import cv2
from IPython.display import Image
import numpy as np

src_1 = cv2.imread("images/poly1.png", cv2.IMREAD_COLOR)
src_2 = cv2.imread("images/poly2.jpg", cv2.IMREAD_COLOR)

gray1 = cv2.cvtColor(src_1, cv2.COLOR_BGR2GRAY)
gray2 = cv2.cvtColor(src_2, cv2.COLOR_BGR2GRAY)

# contour는 하얀색 객체를 추출함
ret1, binary_1 = cv2.threshold(gray1, 127,255,cv2.THRESH_BINARY)
binary_1 = cv2.bitwise_not(binary_1) #하얀색 추출을 위해 반전
ret1, binary_1 = cv2.threshold(gray1, 127,255,cv2.THRESH_BINARY_INV) #또는 이 방법으로 반전 가능

ret2, binary_2 = cv2.threshold(gray2, 87,255,cv2.THRESH_BINARY) #정상적으로 추출 완료

contours1, hierarchy1 = cv2.findContours(binary_1, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_NONE)
contours2, hierarchy2 = cv2.findContours(binary_2, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_NONE)

# print(len(contours2))
# contours2[i] - 각 도형의 갯수순서
# contours2[0][0] - 첫번째[0] 도형의 첫번째 점[0]
# contours2[0][1] - 첫번째[0] 도형의 두번째 점[1]
# contours2[1][3] - 두번째[1] 도형의 네번째 점[3]

for i in range(len(contours1)):
    cv2.drawContours(src_1, [contours1[i]], 0, (0,0,255), 2)
    cv2.putText(src_1, str(i), tuple(contours1[i][0][0]), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,255,0), 1)
    cv2.imshow("src1", src_1)
    cv2.waitKey(0)


for i in range(len(contours2)):
    cv2.drawContours(src_2, [contours2[i]], 0, (0,0,255), 2)
    cv2.putText(src_2, str(i), tuple(contours2[i][0][0]), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,255,0), 1)
    cv2.imshow("src2", src_2)
    cv2.waitKey(0)
    
cv2.destroyAllWindows()    

cv2.imshow("gray#1",binary_1)
cv2.imshow("gray#2",binary_2)

cv2.imshow("src#1",src_1)
cv2.imshow("src#2",src_2)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_contours1.jpg"><br>
<img src="/images/cv_img_contours2.jpg"><br><br>


### (10) morpology 연산
- 색상을 이진화 한뒤 흰색(1)을 더 넓히거나 좁혀, 색상의 noise를 제거하는 방식입니다.
- 다양한 방식이 있으므로, 전체 소스코드 파일을 참조하세요.

```python
import cv2
import numpy as np

img_color = cv2.imread("images/morpology.jpg", cv2.IMREAD_COLOR)
img_color = cv2.pyrDown(img_color)
img_color = cv2.pyrDown(img_color)

kernel = cv2.getStructuringElement(cv2.MORPH_CROSS, (9,9))
img_dilate = cv2.dilate(img_color, kernel, anchor=(-1, -1),iterations=1)
img_erode = cv2.erode(img_color, kernel, anchor=(-1, -1),iterations=1)
out_img = np.concatenate((img_color, img_dilate, img_erode), axis=1)


cv2.imshow("out_img",out_img)


cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_morpology.jpg"><br><br>

### (11) 그림에서 원을 찾아 외곽선 그리기
- image의 도형중에 원 형태를 찾아 외곽선을 그릴 수 있습니다.

```python
# 그림에서 원 찾기
import cv2
import numpy as np

img_color = cv2.imread("images/ball.jpg", cv2.IMREAD_COLOR)
img_out = img_color.copy()
img_out1 = img_color.copy()
# img_out = cv2.pyrDown(img_out)
img_gray = cv2.cvtColor(img_color, cv2.COLOR_BGR2GRAY)
# img_gray = cv2.pyrDown(img_gray)


#이진화 하기
ret, dst = cv2.threshold(img_gray, 180, 255, cv2.THRESH_OTSU)

kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (3,3))
dst = cv2.morphologyEx(dst, cv2.MORPH_OPEN, kernel, iterations=1)

#원찾기
circles = cv2.HoughCircles(dst, cv2.HOUGH_GRADIENT, 1, 100, \
              param1=350, param2 =15, minRadius=50, maxRadius =95)


#해상도 비율, 원중심에서 다른 원 중심까지 더 안찾는 거리


for i in circles[0]:
    cv2.circle(img_out, (i[0],i[1]), i[2], (0,0,255),3)
    
cv2.imshow("THRESH_OTSU",img_out)


cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_houghcircles.jpg"><br><br>

- 앞에서 배운 코드와 같이 활용하면 기판의 접점 검출도 가능합니다.<br>
```python
#회로도 부품 검출
import cv2
from IPython.display import Image
import numpy as np

src_2 = cv2.imread("images/L90_NG.bmp", cv2.IMREAD_COLOR)
src_2 = cv2.pyrDown(src_2)
src_2 = cv2.pyrDown(src_2)

gray2 = cv2.cvtColor(src_2, cv2.COLOR_BGR2GRAY)
ret2, binary_2 = cv2.threshold(gray2, 87,255,cv2.THRESH_BINARY) #정상적으로 추출 완료
kernel = np.ones((7,7), np.uint8)
binary_2 = cv2.morphologyEx(binary_2, cv2.MORPH_CLOSE, kernel)
contours2, hierarchy2 = cv2.findContours(binary_2, cv2.RETR_CCOMP, cv2.CHAIN_APPROX_NONE)

for i in range(len(contours2)):
        cv2.drawContours(src_2, [contours2[i]], 0, (0,0,255), 2)
        print(i, ":", cv2.contourArea(contours2[i]))
        cv2.putText(src_2, str(i), tuple(contours2[i][0][0]), cv2.FONT_HERSHEY_COMPLEX, 0.8, (0,255,0), 1)
        cv2.imshow("src2", src_2)
        cv2.waitKey(0)
    
cv2.destroyAllWindows() 
```

실행결과<br>
<img src="/images/cv_img_contours3.jpg"><br><br>

### (12) Histogram equalization
- 이진화된 image의 색상범위(0 ~ 255)를 히스토그램 형태로 파악한뒤, 특정 범위에 몰려있는 부분을  
  균등하게 퍼트려 image를 좀 더 명확하게 해 주는 기법입니다.<br><br>

```python
#히스토그램 이퀄라이제이션
import cv2
import numpy as np
from matplotlib import pyplot as plt

src = cv2.imread("images/histogram.png", cv2.IMREAD_COLOR)
img_gray = cv2.cvtColor(src, cv2.COLOR_BGR2GRAY)

equ = cv2.equalizeHist(img_gray)
res = np.hstack((img_gray,equ))

plt.hist(equ.ravel(), 256, [0,256]); 
plt.show()

cv2.imshow('src',res)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

실행결과<br>
<img src="/images/cv_img_equalization.jpg"><br>
좌측의 그림보다 equalization을 적용한 image가 전반적으로 선명합니다.<br><br>

histogram 차이 비교<br>
<img src="/images/cv_img_histogram.jpg"><br><br>


- 위에서 배운 코드들을 기반으로 별도의 Repository에서 차선인식, 불량검출 예제를 보여드리도록 하겠습니다.