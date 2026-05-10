# **[강습회] Isaac Sim + ROS2 Nav2 실습(2) - Multiple Robot**

# **Occupancy Map**

## Occupancy Map 생성: [Occupancy Map Generator extension](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/digital_twin/ext_isaacsim_asset_generator_occupancy_map.html#ext-isaacsim-asset-generator-occupancy-map)

```bash
export ROS_DOMAIN_ID=0
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
source /opt/ros/humble/setup.bash
source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
~/isaacsim/isaac-sim.sh --enable isaacsim.ros2.bridge
```

- **Hospital Environment 세팅**
    1. **병원 에셋 불러오기:**
        - Isaac Sim Assets 브라우저 (`Window > Browsers > Isaac Sim Assets`)로 이동.
        - `Environments > Hospital` 경로 확장.
        - `hospital.usd` 에셋 검색 후 장면에 드래그앤드롭.
        - Transform 속성에서 모든 `Translate` 구성 요소 0으로 설정하여 원점에 배치 확인.
    2. **상단 뷰 설정:**
        - 뷰포트 좌측 상단 `Perspective` 클릭 후 드롭다운 메뉴에서 `Top` 선택.
        - `/Hospital` prim 선택 후 `F` 눌러 선택 영역으로 확대.
        - 필요에 따라 카메라 뷰 조정.
    3. **Occupancy Map 확장 기능 실행:**
        - `Tools > Robotics > Occupancy Map`으로 이동.
    4. **Occupancy Map 설정:**
        - `Origin`은 `X: 0.0, Y: 0.0, Z: 0.0`으로 설정 확인.
        - `Lower Bound`는 `Z: 0.1`로 설정.
        - `Upper Bound`는 `Z: 0.62`로 설정. (Carter 로봇 Lidar의 지면 대비 수직 거리 0.62m에 맞춤.)
    5. **Hospital prim 바운드 선택:**
        - 스테이지에서 Hospital prim 선택.
        - Occupancy Map 확장 기능에서 `BOUND SELECTION` 클릭.
        - Occupancy Map 바운드가 선택된 Hospital prim을 포함하도록 업데이트됨.
    
    **참고:** 맵 파라미터는 제공된 이미지와 유사하게 보이며, 생성된 외곽선도 이미지와 비슷하게 나타날 것임.
    
    ![Occupancy Map Properties UI Window for the Hospital Environment](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_multiple_robot_nav_1.png)
    
    Map parameters
    
    ![Top View of Hospital with Occupancy Map](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_multiple_robot_nav_2.png)
    
    Top View
    
- **Office Environment 세팅**
    1. **사무실 에셋 불러오기:**
        - Isaac Sim Assets 브라우저 (`Window > Browsers > Isaac Sim Assets`)로 이동.
        - `Environments > Office` 경로 확장.
        - `office.usd` 에셋 검색 후 장면에 드래그앤드롭.
        - Transform 속성에서 모든 `Translate` 구성 요소 0으로 설정하여 **원점**에 배치 확인.
    2. **상단 뷰 설정:**
        - 뷰포트 좌측 상단 `Perspective` 클릭 후 드롭다운 메뉴에서 **Top** 선택.
        - `/Office` prim 선택 후 **F** 눌러 선택 영역으로 확대.
        - 필요에 따라 카메라 뷰 조정.
    3. **Occupancy Map 설정:**
        - `Tools > Robotics > Occupancy Map`으로 이동.
        - Occupancy Map 확장 기능에서 **맵 파라미터**를 제공된 이미지처럼 설정.
        - `Upper Bound Z` 거리는 Nova Carter Lidar의 지면 대비 수직 거리인 **0.62미터**에 맞춤.
    
    **참고:** 외곽선이 생성되면 제공된 이미지(상단 뷰)와 유사한지 확인해야 함.
    
    ![Occupancy Map Properties UI Window for Office Environment](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_multiple_robot_nav_3.png)
    
    Map Parameters
    
    ![Top View of Office with Occupancy Map](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_multiple_robot_nav_4.jpg)
    
    Top View
    

---

## ROS2 Navigation Map

1. **환경 설정 완료 후:**
    - "CALCULATE" 클릭 후 "VISUALIZE IMAGE" 클릭.
    - 시각화 팝업창 나타남.
2.  **이미지 및 파라미터 설정:**
    - "Rotate Image" 180도 선택.
    - "Coordinate Type"은 "ROS Occupancy Map Parameters File (YAML)" 선택.
    - "RE-GENERATE IMAGE" 클릭.
    - YAML 형식 Occupancy Map 파라미터가 아래 필드에 나타남.
    - 이미지 이름은 선호하는 대로 변경.
    - 전체 텍스트 복사.
3. **YAML 파일 생성 및 저장 (병원 환경 예시):**
    - `carter_hospital_navigation.yaml` 이름으로 Occupancy Map 파라미터 YAML 파일 생성.
    - `carter_navigation/maps/` 디렉토리에 저장.
4. **복사한 텍스트 삽입:**
    - `carter_hospital_navigation.yaml` 파일에 이전에 복사한 텍스트 붙여넣음.
    - 💡참고: 사무실 환경의 경우 파일명만 `carter_office_navigation.yaml`로 다르게 생성
5. **이미지 저장:**
    - NVIDIA Isaac Sim 시각화 탭에서 "Save Image" 클릭.
    - 맵 파라미터와 동일한 이미지 이름으로 설정.
    - 맵 파라미터 파일과 동일한 디렉토리에 저장.
    
    ![Sample Occupancy Map generated from hospital stage](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_nav_hospital_map.png)
    
    병원 환경의 최종 저장 이미지
    
    ![Sample Occupancy Map generated from office stage](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_nav_office_map.png)
    
    사무실 환경의 최종 저장 이미지
    

# **Multiple Robot ROS2 Navigation Setup**

1. **병원 씬 열기:**
    - `Window > Examples > Robotics Examples`로 이동.
    - `Robotics Examples` 탭 클릭 후 왼쪽 섹션 확장.
    - `ROS2 > Navigation > Multiple Robots > Hospital Scene` 예제 열기.
    - (ROS2 내비게이션 설정 상세 내용은 [ROS2 Navigation Sample](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_navigation.html#isaac-sim-app-tutorial-ros2-navigation) 참고.)
2. **다중 로봇 운영 위한 네임스페이스 활용:**
    - 동일 환경 내 다중 로봇 운영 위해 네임스페이스 사용됨.
    - 이는 ROS2 패키지들의 `rostopic` 및 `rosnode` 이름을 수정하여 동일 ROS2 노드의 다중 인스턴스 동시 실행 가능케 함.
3. **네임스페이스 설정:**
    - `Nova_Carter_ROS_X` 아래 각 액션 그래프 내 `node_namespace` OmniGraph 노드가 해당 로봇 이름으로 설정됨.
    - `carter_navigation` ROS2 패키지 내 `multiple_robot_carter_navigation_hospital.launch.py` 및 `multiple_robot_carter_navigation_office.launch.py` 런치 파일들도 동일한 로봇 네임스페이스로 구성됨.

# **Running Multiple Robot ROS2 Navigation**

1. **시나리오 불러오기:**
    - **병원 환경**: `Window > Examples > Robotics Examples` 이동 후 `Robotics Examples` 탭 클릭, 왼쪽 섹션 확장해서 `ROS2 > Navigation > Multiple Robots > Hospital Scene` 예제 열면 됨.
    - **사무실 환경**: `Window > Examples > Robotics Examples` 이동 후 `Robotics Examples` 탭 클릭, 왼쪽 섹션 확장해서 `ROS2 > Navigation > Multiple Robots > Office Scene` 예제 열면 됨.
2. **시뮬레이션 시작:**
    - **Play** 버튼 클릭해서 시뮬레이션 시작.
3. **ROS2 런치 파일 실행:**
    - 새 터미널 열고 원하는 환경에 맞춰 특정 ROS2 런치 파일 실행해서 다중 로봇 내비게이션 시작.
        - **병원 환경:**
            
            ```bash
            export ROS_DOMAIN_ID=0
            export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
            export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
            source /opt/ros/humble/setup.bash
            source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
            
            ros2 launch carter_navigation multiple_robot_carter_navigation_hospital.launch.py
            ```
            
        - **사무실 환경:**
            
            ```bash
            export ROS_DOMAIN_ID=0
            export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
            export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
            source /opt/ros/humble/setup.bash
            source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
            
            ros2 launch carter_navigation multiple_robot_carter_navigation_office.launch.py
            ```
            
    - RViz2 창 세 개 뜰 거고, 시작하는 데 시간 좀 걸릴 수 있음.
4. **로봇 네임스페이스 확인:**
    - 각 RViz2 창에서 **Displays** 패널에 있는 **Map** 클릭해서 **Topic** 이름 확인하고, 해당 RViz2 창에 맞는 로봇 네임스페이스 잘 봐두면 됨.
5. **로봇 초기 위치 확인:**
    - 각 로봇 위치는 `carter_navigation/params/hospital/` 또는 `carter_navigation/params/office/`에 있는 파라미터 파일에 정의돼 있어서, 로봇들 이미 제대로 자기 위치 잡혀있을 거임.
6. **`/carter1` 로봇 목표 설정:**
    - `/carter1` 네임스페이스 RViz2 창에서 **2D Nav Goal** 버튼 클릭 후, 맵에서 원하는 지점 클릭 드래그하면 됨.
    - ROS2 내비게이션 스택이 경로 생성할 거고, `/carter1` 로봇이 목표 지점으로 이동 시작함!
7. **다른 로봇들 목표 설정:**
    - `/carter2`랑 `/carter3` 로봇도 위 단계랑 똑같이 반복하면 됨.

[Multi Robot in Hospital](attachment:166725ff-a893-49d3-ac4f-5069fc40a52f:MultiRobot_Hospital.mp4)

Multi Robot in Hospital

[Multi Robot in Office](attachment:753d353b-e123-4d1e-a6f4-2aec547dbab7:MultiRobot_Office.mp4)

Multi Robot in Office

<aside>
💡

- **ROS2 이미지 발행 파이프라인 기본 비활성화 상태임 (성능 향상 목적).**
- **이미지 발행 시작하려면:**
    - 각 `Nova_Carter_ROS` prim 아래의 `_hawk` 액션 그래프 열기.
    - `_camera_render_product` 노드 활성화.
    - render product 노드 하위의 ROS 카메라 발행 노드는 기본 활성화되어 있어 렌더 제품 노드 활성화 시에만 발행 시작됨.
- **Nova Carter의 모든 센서 및 이미지 데이터는 [`Sensor Data QoS`](https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-profiles)로 발행 중임.**
- **RViz에서 이미지 시각화 원하면:**
    - 이미지 탭 확장.
    - `Topic > Reliability Policy`로 이동.
    - 정책을 `Best Effort`로 변경.
</aside>

# **Troubleshooting**

- **높은 CPU 사용량 유의.**
- **로봇 충돌 또는 위치 인식 문제 발생 시 원인:**
    - Nav2 스택이 센서 데이터와 제대로 동기화 안 돼서 컨트롤러 명령 누락될 가능성 높음.
- **Nav2 성능 개선 방안:**
    1. **Publish Full Scan 활성화 시도:**
        - 각 `Nova_Carter_ROS_X` 로봇 아래 `ros_lidars` 액션 그래프에 있는 `publish_front_3d_lidar_scan` OmniGraph 노드 통해 `Publish Full Scan` 체크박스 활성화.
    2. **그래도 문제 발생 시, 터미널에서 Isaac Sim 실행:**
        - 다음 명령어로 Isaac Sim 실행 시도
            
            ```python
            ./isaac-sim.fabric.sh --reset-user
            ```
            
            <aside>
            ⚠️
            
            - 위 명령어는 실험적인 거임.
            - Isaac Sim의 모든 기능이 지원되진 않을 수 있음.
            - 하지만 전반적인 성능은 더 좋을 수도 있음.
            </aside>
            

# 참고문헌

https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_multi_navigation.html#isaac-sim-app-tutorial-ros2-multi-navigation

[Multiple Robot ROS2 Navigation — Isaac Sim Documentation.pdf](attachment:092fadae-bc0b-4ecc-941c-588fa5b971a5:Multiple_Robot_ROS2_Navigation__Isaac_Sim_Documentation.pdf)
