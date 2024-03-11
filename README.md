# My_Video_Recorder

> ### OpenCV 라이브러리를 이용한 웹캠을 녹화하는 Video Recorder.

&nbsp;
&nbsp;


프로그램 기능 설명
-------------

사진의 용량 때문에 로딩이 길어질 수 있습니다.

#### 1. 프로그램 실행 화면

![실행 화면](https://github.com/SJ-1011/My_Video_Recorder/assets/109647265/a5b01f93-d053-43e3-8091-ebf7322bd69d)

실행 시 웹캠의 화면을 볼 수 있다.

#### 2. 프로그램 녹화 시 화면

![image](https://github.com/SJ-1011/My_Video_Recorder/assets/109647265/d845e2cb-399d-4dd4-a270-58c60b63b92d)


녹화 시 화면 위쪽에 빨간색 원 모양을 확인할 수 있다.

#### 3. 기본 녹화 영상

![video2](https://github.com/SJ-1011/My_Video_Recorder/assets/109647265/2ecc1b9e-4231-48a8-9f1d-45496dec2c24)

space 키를 통하여 녹화를 시작하고 종료할 수 있다.

모든 프레임이 다 담긴 것을 확인할 수 있다.

#### 4. 좌우 반전 녹화 영상

![video1](https://github.com/SJ-1011/My_Video_Recorder/assets/109647265/1cd988e8-9303-4939-a726-53505e1dc5a8)

F키를 누르면 좌우 반전이 되는 것을 확인할 수 있다.

#### 5. 기타

![image](https://github.com/SJ-1011/My_Video_Recorder/assets/109647265/c0113920-cbad-4ae2-9446-a6e63f3d4107)

녹화 영상을 구분하기 쉽게 순서대로 숫자를 매겨 저장하도록 만들었다.

> 참고 코드
>
>                output = cv2.VideoWriter(f'video{i}.avi', fourcc, 20.0, (640, 480))
>                i = i + 1


&nbsp;
&nbsp;


코드 설명
-------------

#### 1. 초기 세팅

        # 카메라 영상 가져오기
        cap = cv2.VideoCapture(0)
        
        # 동영상 저장을 위한 설정
        fourcc = cv2.VideoWriter_fourcc(*'XVID')
        output = None
        i = 1
        
        # 초기 모드 설정
        preview_mode = True
        recording_mode = False

        # flip 모드 확인
        flipping = 0

#### 2. 프레임 처리

        while True:
                # 프레임 읽기
                ret, frame = cap.read()

                # flip 모드 확인 / flip 모드면 좌우 반전
                if flipping:
                    frame = cv2.flip(frame, 1)
                
                if ret:
                    # 레코딩 모드인 경우 프레임 저장
                    if recording_mode:
                        if output is None:
                            # 비디오 녹화를 위한 VideoWriter 생성 / 차례대로 비디오 생성
                            output = cv2.VideoWriter(f'video{i}.avi', fourcc, 20.0, (640, 480))
                            i = i + 1
                        output.write(frame)

                        # 빨간색 원 표시
                        cv2.circle(frame, (30, 30), 20, (0, 0, 255), -1)
                    
                    # 화면에 영상 표시
                    cv2.imshow('frame', frame)
                        
                    
                    # 키 입력 대기
                    key = cv2.waitKey(1)
                    
                    # 'Space' 키를 누르면 모드 변환
                    if key == ord(' '):
                        if preview_mode:
                            recording_mode = True
                            preview_mode = False
                            print("Recording...")
                        else:
                            recording_mode = False
                            preview_mode = True
                            print("Preview mode")
                            if output is not None:
                                output.release()
                                output = None
                    
                    # 'ESC' 키를 누르면 프로그램 종료
                    if key == 27:
                        break

                    # 'F' 키를 누르면 좌우 반전
                    if key == ord('f'):
                        print("Flip mode")
                        flipping = (flipping + 1) % 2
                        
                else:
                    break
                    
            # 종료 시 해제
            cap.release()
            if output is not None:
                output.release()
            cv2.destroyAllWindows()
