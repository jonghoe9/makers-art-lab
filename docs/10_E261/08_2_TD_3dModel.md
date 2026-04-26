# 08-2. TD에서 3d 모델과 연동

## 3. TD에서 3d 모델 나타내기
![TD에서 3d 모델 움직이기](../img/ship_01.png){width="600px"}

### 3.1 3d 모델을 TD에 넣기
![TD에 3d 모델 넣기](../img/ship_02.png){width="600px"}

- SOP_Filein 사용
- OBJ, FBX 형식의 3d 모델 파일을 사용한다.
- SOP_Transform : 3d 모델링 툴에 따라 XY 바닥이거나 XZ 바닥인 경우가 있어서 수정하기 위해 사용
- SOP_Null : 수정된 3d 모델의 최종 출력

![3d 모델 좌표 변환](../img/ship_03.png){width="300px"}

- 예시에서 OBJ 파일이 XY 바닥 기준으로 작성된 것이라 XZ 바닥 기준으로 변경함 (X축 -90도 회전)

### 3.2 3d 모델을 화면에 출력하기
![3d 모델 화면 출력](../img/ship_04.png){width="600px"}

- SOP_Null 출력에 새로운 SOP_Transform 연결
- 새로운 SOP_Transform은 3d 모델을 TD 화면에서 변화하기 위함
- SOP_Transform 출력을 Drag 해서 뽑아 COMP_Geometry 연결
- COMP_Light, COMP_Camera 추가
- TOP_Render 추가하면 출력 이미지 생성된다
- TOP_Out 까지 연결하면 출력 완성
- TOP_Out 오퍼레이터의 사각형 아래 파란색 점을 눌러 바탕화면에 출력한다.
- 카메라와 조명 위치 조절하여 화면에 잘 보이도록 표시한다.

### 3.3 슬라이드로 활동 범위 확인하기
![슬라이드와 버튼](../img/ship_050.png){width="600px"}

- Roll, Pitch, Yaw 방향이 맞게 설정되었는지, 범위는 어느 정도로 할 것인지 확인하기 위한 설정
- 슬라이드 데이터를 초기화 하기 위한 설정
    - COMP_Slider 2개 추가
    - CHOP_Rename 각각 연결
    - CHOP_Math 각각 연결
    - 슬라이더1 타입 : Slider UV
    - 슬라이더1 레이아웃 : Width, Height를 모두 100으로 설정
    - 슬라이더1의 Rename 에서 Roll, Pitch 로 설정
    - 슬라이더2의 Rename 에서 Yaw 로 설정
    - Math의 Range 출력을 -40에서 40 으로 설정
    - Math 오퍼레이터의 + 버튼을 눌러 활성화 하고
    - SOP_Transform2 의 Rotate 와 연결

    ![슬라이드와 버튼](../img/ship_060.png){width="300px"}

- 슬라이드를 움직여 Roll, Pitch, Yaw 방향에 맞게 움직이는지 확인하고 수정하기

### 3.4 슬라이드 리셋 스위치

![슬리이드 버튼](../img/ship_051.png){width="300px"}

![슬리이드 버튼](../img/ship_052.png){width="500px"}

## 4. 시리얼 데이터를 TD에서 구현하기

![시리얼포트 열기](../img/ship_07.png){width="300px"}

- DAT_Serial : 시리얼 포트 지정, 포트 속도 설정

### 4.1 버튼으로 시리얼 On/Off 하기

![시리얼포트 버튼](../img/ship_080.png){width="600px"}

- TOP_Button : 버튼 기능, Toggle Down
- CHOP_Panel : Select 에서 State 선택, 버튼 상태를 표시한다.
    - 이 상태 값을 Serial Active On/Off 에 사용한다.

    ![시리얼포트 열기](../img/ship_081.png){width="300px"}
    ![시리얼포트 열기](../img/ship_082.png){width="300px"}
    ![시리얼포트 열기](../img/ship_083.png){width="300px"}

### 4.2 시리얼 데이터 추출하기

![데이터 추출](../img/ship_090.png){width="600px"}

- DAT_Select : 입력된 데이터 중 index 1번, 1개 데이터 읽기 설정
- DAT_Convert : 시리얼 데이터를 쉼표(,)로 구분하고 테이블 배열에 넣기
- CHOP_Datto : 데이터를 chop 으로 변환

    ![데이터추출](../img/ship_091.png){width="300px"}
    ![데이터추출](../img/ship_092.png){width="300px"}
    ![데이터추출](../img/ship_093.png){width="300px"}

### 4.3 데이터 변환하기

![데이터 변환](../img/ship_100.png){width="600px"}

- CHOP_Select : CHOP_Datto 에서 추출된 데이터를 선택, 이름 바꾸기
- CHOP_Filter : 센서의 노이즈 제거
- CHOP_Math : 센서 데이터를 3d 모델의 변화값에 맞게 변환
- CHOP_Limit : 데이터가 지정된 범위를 넘지 않게 제한
- CHOP_Trail : 변환된 값을 그래프로 표시

![데이터 변환](../img/ship_101.png){width="300px"}
![데이터 변환](../img/ship_102.png){width="300px"}
![데이터 변환](../img/ship_103.png){width="300px"}
![데이터 변환](../img/ship_104.png){width="300px"}

- SOP_Transform의 Rotate 참조데이터로 사용

![데이터 변환](../img/ship_105.png){width="300px"}

- 이제 슬라이드는 동작하지 않고, 시리얼 포트로 들어오는 값이 3d 모델에 적용된다.

![TD 완성 모습](../img/ship_110.png){width="600px"}