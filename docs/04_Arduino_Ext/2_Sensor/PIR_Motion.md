# PIR-Motion Sensor

인체감지 모션센서(적외선 변화 감지) 사용하기

- 동작전압 : 5~20V
- 출력전압 : 3.2V (HIGH), 0V (LOW)
- 센서각도 : 100도 이하
- 감지범위 : 7m 이하
- 동작온도 : -20 ~ 70도
- 볼륨왼쪽 : 시간조절 (0옴 : 2.5초 ~ 1메가옴 : 248초, 중간 120초 정도)
- 시간조정 : 왼쪽 끝 3초, 중간 4분40초~50초, 1/4 지점 2분30초, 최대 10분 정도
- 볼륨 오른쪽 : 감도조절 (시계방향 : 민감도 높음)
- 센서에 사람이 인식되면 HIGH, 평소에는 LOW 출력
- Arduino에서 `digitalRead()`로 읽는다. (Active High)


### Jumpper Setting
- Jumpper L : Non-Retriggering : 변화가 감지되면 일정시간 HIGH 유지
- Jumpper H : Retriggering : 변화가 감지되는 동안 HIGH 출력