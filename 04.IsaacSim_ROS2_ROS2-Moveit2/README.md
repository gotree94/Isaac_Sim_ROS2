# **[강습회] Isaac Sim + ROS2 Moveit2 실습**

# **Isaac Sim 메뉴를 통해 MoveIt 2 실습**

**독립 실행 스크립트 대신 Isaac Sim 예제 메뉴에서 튜토리얼 환경 직접 로드 가능.**

1. **Isaac Sim + ROS2 설정**:
    - `FASTRTPS_DEFAULT_PROFILES_FILE` 환경 변수 설정 확인.
    - fastdds.xml 파일 복사
        
        ```bash
        sudo mkdir /opt/.ros
        sudo cp ~/IsaacSim-ros_workspaces/humble_ws/fastdds.xml /opt/.ros/
        ```
        
2. **MoveIt2 Launch 복사 및 빌드**:
    - `~/IsaacSim-ros_workspaces/humble_ws/src/moveit/isaac_moveit/launch`폴더에 아래 파일 복사해 넣기 **(MoveIt2 Launch 코드)**
        
        [isaac_moveit_demo.launch.py](attachment:5c5809de-b1ab-4183-a8b6-a60ef97c02a8:isaac_moveit_demo.launch.py)
        
    - 빌드하기
        
        ```bash
        cd ~/IsaacSim-ros_workspaces/humble_ws/
        
        # 필요시
        rm -rf build install
        
        # 빌드
        colcon build --symlink-install
        ```
        
3. **Isaac Sim 실행 — `isaacsim.ros2.bridge` 확장 기능 활성화**:
    - **`Window > Extensions`** 메뉴에서 활성화 또는 아래와 같이 실행.
        
        ```bash
        # Run Isaac Sim w/ Enabling ROS2 Bridge
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        
        ~/isaacsim/isaac-sim.sh --enable isaacsim.ros2.bridge
        ```
        
4. **환경 로드**:
    - MoveIt 튜토리얼의 2단계(파이썬 스크립트 실행) 대신,
    - **`Window > Examples > Robotics Examples`** 이동.
    - **`Robotics Examples`** 탭 클릭 후 왼쪽 섹션 확장.
    - 예제 **`ROS2 > MoveIt > Franka MoveIt`** 열기.
5. **MoveIt2 Launch 실행**:
    
    ```bash
    export ROS_DOMAIN_ID=0
    export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
    export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
    source /opt/ros/humble/setup.bash
    source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
    
    ros2 launch isaac_moveit isaac_moveit_demo.launch.py
    ```
    

[Isaac Sim Menu - Moveit.mp4](attachment:7c803f76-844e-4849-883f-85a24186fb28:Isaac_Sim_Menu_-_Moveit.mp4)

# **Isaac Sim Python을 통해 MoveIt 2 실습**

**독립 실행 스크립트을 통해 Isaac Sim 실행.**

1. **Isaac Sim + ROS2 설정**:
    - `FASTRTPS_DEFAULT_PROFILES_FILE` 환경 변수 설정 확인.
    - fastdds.xml 파일 복사
        
        ```bash
        sudo mkdir /opt/.ros
        sudo cp ~/IsaacSim-ros_workspaces/humble_ws/fastdds.xml /opt/.ros/
        ```
        
    - `~/IsaacSim-ros_workspaces/humble_ws`폴더에 아래 파일 복사해 넣기 **(Isaac Sim Standalone Python 코드)**
        
        [isaac_moveit.py](attachment:9fa04659-3e00-47b9-bec6-89244e84c2f2:isaac_moveit.py)
        
2. **Isaac Sim Standalone Python 실행**:
    
    ```bash
    # Run Isaac Sim w/ Enabling ROS2 Bridge
    export ROS_DOMAIN_ID=0
    export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
    export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
    source /opt/ros/humble/setup.bash
    source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
    ~/isaacsim/python.sh ~/IsaacSim-ros_workspaces/humble_ws/isaac_moveit.py
    ```
    
3. **MoveIt2 Launch 복사 및 빌드**:
    - `~/IsaacSim-ros_workspaces/humble_ws/src/moveit/isaac_moveit/launch`폴더에 아래 파일 복사해 넣기 **(MoveIt2 Launch 코드)**
        
        [isaac_moveit_demo.launch.py](attachment:5c5809de-b1ab-4183-a8b6-a60ef97c02a8:isaac_moveit_demo.launch.py)
        
    - 빌드하기
        
        ```bash
        cd ~/IsaacSim-ros_workspaces/humble_ws/
        
        # 필요시
        rm -rf build install
        
        # If needed
        # pip install typing-extensions
        
        # 빌드
        colcon build --symlink-install
        ```
        
4. **MoveIt2 Launch 실행**:
    
    ```bash
    export ROS_DOMAIN_ID=0
    export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
    export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
    source /opt/ros/humble/setup.bash
    source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
    
    ros2 launch isaac_moveit isaac_moveit_demo.launch.py
    ```
    

[Isaac Sim Python - Moveit.mp4](attachment:74c9c274-5c48-47bb-98bf-5bb90c75150d:Isaac_Sim_Python_-_Moveit.mp4)

# 카메라 이미지 (RGB, Depth) 확인

- RGB 카메라 이미지 확인
    
    ```bash
    export ROS_DOMAIN_ID=0
    export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
    export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
    source /opt/ros/humble/setup.bash
    ros2 run rqt_image_view rqt_image_view /rgb
    ```
    
- 카메라 Depth 이미지 확인
    
    ```bash
    export ROS_DOMAIN_ID=0
    export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
    export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
    source /opt/ros/humble/setup.bash
    ros2 run rqt_image_view rqt_image_view /depth
    ```
    

# MoveIt2 RViz **조작**

1. **Rviz 실행 후 플래너 조작**:
    - `Planning Group`에서 **`hand` 선택**.
    - `Goal State`에서 **`open` 선택**.
2.  **손 동작 계획**:
    - `Commands` 아래 **`Plan` 클릭**.
    - 손의 계획된 움직임 시각화 확인.
3. **손 동작 실행**:
    - **`Execute` 클릭**.
    - 계획된 대로 손 움직임 시작.
4. **팔 동작 계획**:
    - `Planning Group`에서 **`panda_arm` 선택**.
    - 표시된 화살표, 회전 디스크로 **로봇 목표 위치 설정** 또는 `Goal State`에서 **`<random_valid>` 선택**.
5. **팔 동작 실행**:
    - `Commands` 아래 **`Plan` 클릭**.
    - **`Execute` 클릭**.
    - 팔의 계획된 움직임 시각화 및 실제 움직임 확인.

---

# 참고문헌

- [ROS2 Joint Control: Extension Python Scripting](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_manipulation.html#isaac-sim-app-tutorial-ros2-manipulation)
    - https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_manipulation.html#isaac-sim-app-tutorial-ros2-manipulation
- https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_moveit.html#running-moveit-2-tutorial-in-docker
    
    [MoveIt 2 — Isaac Sim Documentation.pdf](attachment:b138cf74-274d-4b30-b4d0-bc663c65918a:MoveIt_2__Isaac_Sim_Documentation.pdf)
    
- https://moveit.picknik.ai/main/doc/how_to_guides/isaac_panda/isaac_panda_tutorial.html
- https://github.com/moveit/moveit2_tutorials/blob/main/doc/how_to_guides/isaac_panda/isaac_panda_tutorial.rst
    
    [How To Command Simulated Isaac Robot — MoveIt Documentation_ Rolling documentation.pdf](attachment:a234a0fd-d133-418c-b8ca-ba5dd1c4e557:How_To_Command_Simulated_Isaac_Robot__MoveIt_Documentation__Rolling_documentation.pdf)
