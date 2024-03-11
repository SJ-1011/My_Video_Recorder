# My_Video_Recorder

> ### OpenCV 라이브러리를 이용한 웹캠을 녹화하는 Video Recorder.

코드 설명
-------------

1. 초기 세팅

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

2. 프레임 처리

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
