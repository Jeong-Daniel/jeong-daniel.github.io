---
title: Rectification 이해하기
date:   2023-01-02 22:07:41 +0900
categories: [Computer_Vision, Study]
tags: [opencv, math, vision]
---

*노션에다가 스터디 정리용으로 올린거였는데 여기서는 Latex 형식의 수학 수식을 지원하지 않아서 다 깨져서 나오네요 나중에 시간 있으면 수정을 하도록 하겠습니다.

# Rectification을 이해해보자

<aside>
💡 **Goal & Road map**

1. Stereo Vision
2. Epipolar geometry
3. Rectification
4. Matirx
    
    4.1 Essential Matrix
    
    4.2 Fundamental Matrix
    
</aside>

---

# 1. Stereo Vision

같은 물체에 대해 서로 다른 장소에서 촬영한 여러 이미지에서 물체의 3차원 정보를 계산하는 학문 분야

- Subfield
1. **카메라 캘리브레이션이 되어 있는지 아닌지(calibrated or uncalibrated)**
2. 카메라를 하나를 사용하고 이를 움직여가며 촬영하는 방식인지
3. 아니면 여러 대의 카메라를 사용하는 것인지(isolated camera or video sequence)
4. 수 십, 수 백장을 처리하는 시스템인지

      etc…… 다양하게 파생되는 연구 분야 존재

- narrow meaning

**서로 다른 장소에서 촬영한 두 장의 이미지에서 거리 정보를 추출**해 내는 분야

+TMI : 사람과 동물이라면 사용하는 “양안시/입체시”역시 스테레오 비전이고 우리가 공간적으로 느끼는 거리감도 학습의 결과다. 예를 들어 선천적으로 시각장애를 얻은 사람이 나중에 기술적인 도움이나 이식으로 시각을 얻더라도 계단을 인지하지 못한다고 한다. (거리를 학습하지 못했기 때문에 벽에 줄그어둔 것과 차이를 못느낌)

![eyes](https://user-images.githubusercontent.com/85277660/210238462-50aa9faf-e01d-4a25-b4bd-3fbfe23bb512.png)

이쪽도 VR기기와 사용자 환경개발에  있어 중요한 연구분야

---

## 거리 정보와 Disparity의 관계

![disparty 1](https://user-images.githubusercontent.com/85277660/210238524-be61d347-12e0-4a46-8603-fe2cf639d7a1.png)

양쪽 이미지에 어떻게 대응하는가? → 얼마나 거리(Depth) 차이가 나는가?

가까이 있는 물체는 그 차이가 크고(초록색공) 멀리 있는 물체는 차이가 적음(주황색공)

![수식 2](https://user-images.githubusercontent.com/85277660/210238578-391831aa-103f-4668-bdea-2ec08e5ba442.png)

극단적으로 멀리 있는 물체의 경우 차이가 거의 없으며 왼쪽영상의 한 점에 위치에 비해 오른쪽 영상의 대응점 위치의 차이를 디스패리티(disparity/상이)라고 부릅니다.

$$
disparity = x- x' = {B*f \over z}
$$

왼쪽 영상 한 점의 위치($x$)에서 대응되는 오른쪽 영상 한 점($x'$)의 위치 차이는

1. 그 점의 3차원 공간 상의 거리 z에 반비례
2. 초점 거리 $f$와 두 영상을 촬영할 때 거리차 $B$(베이스라인)에 비례한다

![수식 유도 과정](https://user-images.githubusercontent.com/85277660/210238626-fa77d67d-931f-46cd-bcb1-ed723c193d3d.jpg)

어떤 3D점 $X(p,q,r)$를 찾기 위해서 베이스라인($B)$, 주점($f)$, $x, x'$을 알고 있다고 가정할때 $X$좌표 유도 과정 (triangulation 삼각 측량법)

$disparity$ 식에서 얻을 수 있는 것

해상도가 불만족스럽고 더 높은 정밀도가 필요하다면 카메라간의 사이(베이스라인)를 넓히거나 초점거리가 긴모델로 바꾸는 것도 하나의 방법이지만 거리 정보를 계산 할 수 있는 3차원 공간 상의 영역은 감소하게 됩니다. 그래서 베이스라인과 초점거리를 바꾸어가며 촬영에 적절한 범위나 값을 찾아야할 필요가 있습니다.

---

## 매칭(Matching) / 일치(Correspondence) 문제

그렇다면 왼쪽상에 있는 어떤 점과 오른쪽상에 있는 어떤 점이 같은 요소를 가르키는지는 어떻게 알 수 있을까?

영상의 모든 지점을 다 뒤져보는데 하나의 픽셀 단위로 찾으면 오류가 있거나 놓칠 수 있으니 블럭 단위로 찾아보고 뭔가 한다.

---

# 2. epipolar geometry

### 에피폴라 제한조건 (Epipolar constraint)

카메라가 어떻게 배치되어 있는지 알 수 있다면 매칭 문제에 대해서 답을 할 수 있고 이를 수학적으로 정리한 것이 에피폴라 제한조건이다. 결론부터 말하면 우리는 어떤 이미지 평면상에서 대응하는 포인트를 찾아야 하는 2D 문제를 어떤 직선 위의 점을 찾는 1D 문제로 바꿀 수 있다.

![epipolar line](https://user-images.githubusercontent.com/85277660/210239757-6821cafd-687e-41e6-89a5-c9725ffd3d37.png)

3차원 공간상에 $X$라는 점이 존재를 하고 렌즈 초점 $O_L, O_R$을 연결하는 레이상에 놓이게 되므로 $X_L, X_R$로 투영되게 된다. 계속해서 살펴보면

1. $X, X_1, X_2, X_3$가 있을때 projection하는 과정에서  $X_L$에만 나타나 거리 정보 소실
2.  오른쪽에 있는 화면에 나타나는 어떤 $X_R$는 $\overline{X_R, e_R}$ 위에 존재

그리고 그 선분을 구성하는 $e$는 카메라의 중심을 연결한 $\overline{O_LO_R}$ 위에 직선이 이미지를 관통하는 지점 $e_L, e_R$로 존재합니다. 이 때 파악된 $e$를 에피폴(epipole)이라고 부르고 에피폴을 지나는 후보군 선분을 에피폴라 라인(epipolar line)이라고 부릅니다. 이러한 조건들을 에피폴라 제한조건이라고 합니다.

### 그렇다면 어떻게 에피폴라를 찾는가?

일단 우리는 카메라 켈리브레이션을 해야하는데 했다고 치고 찾는데 필요한 행렬을 가지고 있다고 가정하고 수학적인 과정을 건너 뛰자, 스테레오 카메라 캘리브레이션이 완료되면 에피폴의 위치를 알 수 있고 왼쪽 카메라 한 점에 대응되는 오른쪽 카메라 영상의 에피폴라 라인을 알 수 있다.

![before 4](https://user-images.githubusercontent.com/85277660/210239797-3a75aad5-bc3f-4619-8379-b1bd308a0ba7.png)

# 3. Rectification

이제 에피폴라 라인만 뒤지면 문제는 해결할 수 있는데 이 상태에서 라인을 뒤지는 것도 일이다. 그래서 문제를 좀더 단순화 하기 위해서 에피폴라 라인을 평행하도록 이미지를 변환하는 과정을 렉티피케이션(Rectification)이라고 한다.

![after 5](https://user-images.githubusercontent.com/85277660/210239825-464b54ed-3e95-4ed0-aa42-c24d07fce866.png)

이제 우리는 한 직선상에서 포인트를 찾으면 된다.

![summary 6](https://user-images.githubusercontent.com/85277660/210239858-85215a10-aeea-4251-8ce0-0cdf12e0ce18.png)

전체 과정 다시 요약

왜곡을 보정하고 Rectify로 에피폴라인을 일직선이 되게 교정을 한다

# 4. Matrix

앞에서 건너뛴 수학적인 과정을 다시 돌아와서 우리는 특정 포인트를 담을 수 있는 직선인 에피폴 라인은 유일하게 결정할 수 있고 다른 영상에서 대응하는 라인을 계산해주는 변환 행렬 Fundamental Matrix, Essential Matrix가 필요하다.

계속 보기전에 알고가야 하는 표현식

**Normalized camera matrix & normalized image coordinates**

![Untitled 7](https://user-images.githubusercontent.com/85277660/210239888-6febd785-9422-49f0-8b86-e6f45564e00d.png)

이렇게 하는 이유는 회전행렬과 평행이동벡터를 한꺼번에 표현이 가능하다.

---

## 4.1 Essential Matrix

![Untitled 8](https://user-images.githubusercontent.com/85277660/210239898-bad027e5-ba18-4f5b-aab1-7509c265993b.png)

3D 공간상의 $X$가 왼쪽 영상에서는 $x$로 오른쪽 영상에서는 $x'$에 투영됐다고하면 두 영상 좌표 $x, x'$사이에는 다음 관계를 만족하는 행렬이 항상 존재한다는 것이 epipolar gemoetry의 핵심 요소(단 $x, x'$는 이미 normalized image 평면에서의 homogeneous좌표)

$$
x'^T Ex = 0
$$

$$

\begin{bmatrix}
u' & v' & 1 \\
\end{bmatrix}E \begin{bmatrix}
u \\ v \\ 1 \\
\end{bmatrix} = 0

$$

위 식을 epipolar constraint, 3 x 3 행렬 $E$를 Essential Matrix라고 한다.

![Untitled 9](https://user-images.githubusercontent.com/85277660/210239933-216860b9-75a6-47d7-9d85-e7dfd8e57880.png)

그렇다면 Essential Matrix가 왜 항상 존재하는가 하면 결국 각 카메라에 투영되는 좌우 두 점의 관계는 카메라의 회전과 평행이동에 의해 결정되는 요소이기 때문

두 카메라 좌표축 사이의 행렬을 $R$ 평행이동 벡터를 $t$라고 했을때 외부 공간상의 한점을 두 카메라 좌표계에서 봤을 때의 관계식은 다음과 같이 표현 가능하다.

- $x'$→$x$ 와 $x$→$x'$ 두 과정은 Essential Matrix 포함하여 반대로 진행하니 주의

$$
x' = Rx+t
$$

이 때 Essential Matrix를 다음과 같이 잡으면

$$
E= R[t]_\times
$$

위 식의 의미는 $R$ 회전행렬로 먼저 돌리고 $[t]_\times$ 벡터로 외적을 곱한다는 의미(행렬과 벡터를 먼저 외적하고 시작하는게 아님)

$$
x'^T Ex = 0
$$

이 식을 얻을 수 있는지 확인을 해보기 위해서 좌변에 $E$를 대입하면 

$$
x'^TR[t]_\times x = x^TR^T R[t]_\times x = 0
$$

 $x'^T = x^TR^T$ 표현할 수 있고 $R^TR = I$ 가 된다.  그러면 

$$
x^T[t]_\times x = 0
$$

외적의 성질에 의해서 자기 자신을 곱하면 결과는 0이 된다.

정리

정규화된 이미지 평면에서의 매칭 쌍들 사이의 기하학적 관계(회전, 평행이동)를 설명하는 행렬

## 4.2 Fundamental Matrix

임의의 두 이미지 A,B에 대해서 매칭되는 픽셀 좌표 $p, p'$사이에는 항상 다음과 같은 관계를 만족하는 행렬 $F$가 존재하고 이를 fundamental matrix라고 합니다.

$$
p'^T Fp = 0
$$

$$

\begin{bmatrix}
x' & y' & 1 \\
\end{bmatrix}E \begin{bmatrix}
x \\ y \\ 1 \\
\end{bmatrix} = 0

$$

이 때, 이미지 A에 대한 카메라 내부 파라미터 행렬을 $K$ 이미지 B에 대한 카메라 행렬을 $K'$, 이미지 A, B 사이의 Essential Matrix를 $E$라 하면 fundamenta matrix $F$는 다음과 같이 주어집니다.

$$
E = K'^TFK
$$

이를 $F$에 대해서 다시 정리 하면

$$
F=(K'^T)^{-1} EK^{-1}
$$

여기서 이미지 A, B를 동일한 카메라로 촬영을 한다면 카메라매트릭스 역시 동일 하기 때문에 $K=K'$이 됩니다.

Essential Matrix를 설명하며 처음에 이미지 픽셀 좌표계와 normalized된 좌표계 사이에는 카메라 매트릭스에 대해서 다음과 같은 관계식을 가지게 됩니다.

$$
x_{img} = Kx \\ 
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}=
\begin{bmatrix}
f_x & 0 & c_x \\
0 & f_y & c_y \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
u \\
v \\
1
\end{bmatrix}
$$

그러면 이미지 A와 B에 대해서 각각 다음과 같은 관계식을 만들 수 있습니다.

$$
x_{img}=Kx, x_{img}'=K'x'
$$

이 식을 다시 아래 식에 대입을 하면

$$
x'^T Ex = 0 \\
(K'^{-1}x_{img}')^TEK^{-1}x_{img} = 0 \\
x_{img}'^T(K'^T)^{-1}EK^{-1}x_{img} = 0 \\
F=(K'^T)^{-1}EK^{-1}
$$

$F$를 얻게 됩니다.

정리

카메라 파라미터까지 포함한 두 이미지의 실제 픽셀(pixel) 좌표 사이의 기하학적 관계를 표현하는 행렬

---

마지막으로 보통 카메라파라미터는 카메라켈리브레이션을 하는 과정에서 얻을 수 있기에 essential matrix만 구하면 됩니다. 그리고 이를 결정하기 위한 매칭점 들을 필요로 하는데 알고리즘에 따라서 n-point 알고리즘을 사용합니다. 

이때 E는 회전변환 R의 3자유도 그리고 스케일을 무시한(카메라와의 거리) 평행이동 두방향 t 2자유도 이므로 총 5쌍의 매칭점을 필요로 합니다. 다만 이보다 적은 알고리즘이 있는 경우에는 더 추가적인 constraint을 가정하기 때문에 가능합니다.

다시 정리해서 epipolar geometry에서 essential matrix 가 중요한 이유는 이로부터 두 카메라 A, B 사이의 회전, 평행이동 관계를 파악할 수 있기 때문입니다.