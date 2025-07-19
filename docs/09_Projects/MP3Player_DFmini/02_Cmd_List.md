# 02. Command List

* DF Player Command List

```
---- 구현할 명령어 1 ----
dfPlayer.volume(10);                //0-30사이의 값을 인수로 입력.
dfPlayer.volumeUp();                //볼륨을 1단계씩 키울 때 사용
dfPlayer.volumeDown();              //볼륨을 1단계씩 내릴 때 사용

dfPlayer.enableDAC();               //음악파일에서 디코딩 상태로 설정 (UNMUTE)
dfPlayer.disableDAC();              //음악파일을 디코딩 하지 않음 (MUTE)

dfPlayer.pause();                   //일시 정지
dfPlayer.start();                   //다시 재생
dfPlayer.next();                    //다음 곡 재생
dfPlayer.previous();                //이전 곡 재생

dfPlayer.play(1);                   //1번째 노래 재생(SD카드에 파일이 저장된 순서대로 재생됨)
dfPlayer.loop(1);                   //첫번째 노래 반복재생
dfPlayer.enableLoop();              //현재 재생 중인 트랙을 반복 재생 모드로 지정
dfPlayer.disableLoop();             //현재 반복 재생 트랙을 반복재생 모드에서 해제

dfPlayer.randomAll();               //모든 음악파일 랜덤 재생

---- 구현할 명령어 2 ----
dfPlayer.enableLoopAll();           //전체 MP3파일 재생(SD카드에 파일이 저장된 순서대로 쭉 재생)
dfPlayer.disableLoopAll();          //전체 MP3파일 재생 중지

dfPlayer.playFolder(15, 4);         //지정한 폴더의 트랙 재(15번 폴더의 4번 트랙재생)
dfPlayer.loopFolder(5);             //지정한 폴더엥 있는 모든 음악 순회재생

//----Read imformation----
Serial.println(dfPlayer.readState()); //mp3 모듈 상태 읽어오기
Serial.println(dfPlayer.readVolume()); //현재 볼륨값 읽어오기
Serial.println(dfPlayer.readEQ()); //EQ세팅 읽어오기
Serial.println(dfPlayer.readFileCounts()); //SD카드내의 전체 음악파일 개수 얻어오기
Serial.println(dfPlayer.readCurrentFileNumber()); //현재 재생중인 폴더의 파일번호 얻어오기
Serial.println(dfPlayer.readFileCountsInFolder(3)); //특정 폴더내의 파일 개수 얻어오기

----이퀄라이즈 모드 지정시 사용 ----   
dfPlayer.EQ(DFPLAYER_EQ_NORMAL);    // 일반모드
dfPlayer.EQ(DFPLAYER_EQ_POP);       // 팝모드
dfPlayer.EQ(DFPLAYER_EQ_ROCK);      // 락모드
dfPlayer.EQ(DFPLAYER_EQ_JAZZ);      // 재즈모드
dfPlayer.EQ(DFPLAYER_EQ_CLASSIC);   // 클래식 모드
dfPlayer.EQ(DFPLAYER_EQ_BASS);      // BASS모드

---디바이스 모드 지정시 사용: 기본 SD카드 모드 지정, 사용하지 않아도 됨 ----
dfPlayer.outputDevice(DFPLAYER_DEVICE_U_DISK);   
dfPlayer.outputDevice(DFPLAYER_DEVICE_SD);       
dfPlayer.outputDevice(DFPLAYER_DEVICE_AUX);      
dfPlayer.outputDevice(DFPLAYER_DEVICE_SLEEP);
dfPlayer.outputDevice(DFPLAYER_DEVICE_FLASH);

----mp3 모듈 제어 모드----
dfPlayer.sleep();                   //슬립모드
dfPlayer.reset();                   //슬립모드로부터 복귀
dfPlayer.outputSetting(true, 15);   //이 모듈에서는 적용되지 않음
dfPlayer.setTimeOut(500);           //시리얼 통신용 타임아웃 시간 지정

----Mp3 재생----
dfPlayer.advertise(3);              // 광고모드를 실행 (/ADVERT/000x.mp3)
dfPlayer.stopAdvertise();           //광고 모드 해제

dfPlayer.playLargeFolder(2, 999);   //폴더내에 mp3파일이 여러개 여서 0001.mp3형식으로 저장했을 때 사용 
dfPlayer.playMp3Folder(4);          //mp3폴더에 저장된 트랙을 재생(SD:/MP3/0004.mp3 재생)
```