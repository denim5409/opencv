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

### (1) Image 불러오기
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

### (2) Image 붙이기
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

실행결과
<img src="/images/cv_img_vconcat.jpg">
<img src="/images/cv_img_hconcat.jpg"><br><br>

- 이 외에도 image의 회전, 축소, 확대 등이 있습니다. 자세한 내용은 소스코드 파일을 참조해주세요.  
(opencv.ipynb)<br><br>

### (3) Image를 화면에 바로 띄우기
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

실행결과
<img src="/images/cv_img_in_plot.jpg"><br><br>

- 이 외에도 image의 회전, 축소, 확대 등이 있습니다. 자세한 내용은 소스코드 파일을 참조해주세요.  
(opencv.ipynb)<br><br>