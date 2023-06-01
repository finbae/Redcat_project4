# Redcat_project4
### 스켈레톤(뼈대) 추출하기
    
    import cv2
    import mediapipe as mp

    # MediaPipe Pose 초기화
    mp_drawing = mp.solutions.drawing_utils
    mp_pose = mp.solutions.pose

    def extract_skeleton():
      cap = cv2.VideoCapture(0)
    # MediaPipe Pose 인스턴스 생성    
      with mp_pose.Pose(min_detection_confidence=0.5, min_tracking_confidence=0.5) as pose:
        while cap.isOpened():
          ret, frame = cap.read()
          if not ret:
            print("비디오 읽기 실패")
            break
      
          # BGR 이미지를 RGB 이미지로 변환            
          image = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
          image.flags.writeable = False
      
          # 이미지에서 스켈레톤 뼈대 추출            
          results = pose.process(image)
      
          # RGB 이미지를 다시 BGR 이미지로 변환            
          image.flags.writeable = True
          image = cv2.cvtColor(image, cv2.COLOR_RGB2BGR)
      
          # 스켈레톤 뼈대 그리기            
          mp_drawing.draw_landmarks(
            image, results.pose_landmarks, mp_pose.POSE_CONNECTIONS,
            mp_drawing.DrawingSpec(color=(0, 255, 0), thickness=2, circle_radius=2),
            mp_drawing.DrawingSpec(color=(0, 0, 255), thickness=2))
      
          # 화면에 출력           
          cv2.imshow('Skeleton', image)
      
          # 'q' 키를 누르면 종료            
          if cv2.waitKey(1) & 0xFF == ord('q'):
            break
      
          # 자원 해제    
          cap.release()
          cv2.destroyAllWindows()
      
    # 스켈레톤 추출 시작
    extract_skeleton()
