# **[강습회] Isaac Sim + ROS2 Nav2 실습(3) - Block World Generator**

# **Setting up Environment and Robot**

## **Generate 3D World**

```bash
export ROS_DOMAIN_ID=0
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
source /opt/ros/humble/setup.bash
source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
~/isaacsim/isaac-sim.sh --enable isaacsim.ros2.bridge
```

- **Isaac Sim 내 [Block World Generator](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/digital_twin/ext_isaacsim_asset_generator_occupancy_map.html#ext-omni-isaac-block-world-tool) 사용.**
    - 상단 메뉴 바에서 `Tools > Robotics > Block World Generator` 클릭.
    - `Load Image` 버튼 눌러 `carter_navigation/maps/carter_warehouse_navigation.png`에 있는 occupancy map 이미지 열기.
    - `Visualization` 창 뜸.
    - `Generate` 버튼 눌러 스테이지에 입력 occupancy map에 해당하는 geometry 생성.
- **생성된 3D 월드에는 모든 점유 픽셀에 충돌 메시(collision mesh)가 자동 적용됨.**

## **Add Robot in Scene**

- **Isaac Sim Assets 브라우저 열기:** `Window > Browsers > Isaac Sim Assets`로 이동.
- **로봇 에셋 찾기:** 각 섹션 확장해서 `Samples > ROS2 > Robots` 폴더로 이동.
- **`Nova_Carter_ROS.usd` 에셋 드래그앤드롭:** 이전 단계에서 생성된 장면에 `Nova_Carter_ROS.usd` 에셋 드래그해서 놓으면 됨 (벽으로 둘러싸인 공간 안, 바닥 위 아무데나).

## **Running Navigation**

- **Isaac Sim에서 Play 클릭:** 시뮬레이션 시작.
- **새 터미널 열고 ROS 2 런치 파일 실행:**
    - `carter_navigation` 패키지 포함된 `<ros2_ws>` 소싱.
    - 다음 ROS 2 런치 파일 실행해서 Nav2 시작.
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch carter_navigation carter_navigation.launch.py
        ```
        
    - RViz2 열리고 점유 맵 로딩 시작할 거임. 맵 안 뜨면 이전 단계 반복.
- **로봇 위치 재설정:**
    - **2D Pose Estimate** 버튼 사용해서 로봇 위치 다시 설정.
    - 목표 설정 전에 꼭 해야 하고, 포즈 추정치가 대략 맞아야 함.
- **내비게이션 목표 설정:**
    - **Navigation2 Goal** 버튼 클릭 후 맵에서 원하는 위치 클릭 드래그.
    - Nav2가 경로 생성할 거고 로봇은 목적지로 이동 시작함.

* Block World.mp4
  https://drive.google.com/file/d/1ZzYLJg4okEbN141xTyAQUM1DZqZ-CIeD/view?usp=sharing

# 참고문헌

https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_navigation_block_world.html#isaac-sim-app-tutorial-ros2-navigation-block-world

[ROS 2 Navigation with Block World Generator — Isaac Sim Documentation.pdf](attachment:ab5d5de9-85fb-4177-a6e5-b3ea82f38d29:ROS_2_Navigation_with_Block_World_Generator__Isaac_Sim_Documentation.pdf)
