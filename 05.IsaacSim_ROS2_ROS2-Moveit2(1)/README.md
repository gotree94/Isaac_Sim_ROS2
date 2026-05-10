# **[강습회] Isaac Sim + ROS2 Nav2 실습(1) - Single Robot**

# 튜토리얼 환경 설정

- **Nav2 프로젝트 필요:**
    - **Nav2 프로젝트 필수.** 설치는 [Nav2 설치 페이지](https://docs.nav2.org/getting_started/index.html#installation) 참고.
- **`isaacsim.ros2.bridge` 확장 기능 활성화:**
    - **Extension Manager** 창 ( **Window** > **Extensions** )에서 **확장 기능 활성화 필요.**
- **필수 ROS2 패키지 및 워크스페이스 설정:**
    - `carter_navigation`, `iw_hub_navigation`, `isaac_ros_navigation_goal` 패키지 필요. [[링크](https://github.com/isaac-sim/IsaacSim-ros_workspaces)]
    - 이 패키지들은 NVIDIA Isaac Sim 다운로드 시 제공되며, 해당 `ros2_ws` (`humble_ws`) 내에 위치.

# **Nav2 Setup**

![ROS2 Nav2 Overview Block Diagram](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros2_nav_1.png)

본 시나리오에서 Nav2로 발행되는 토픽 및 메시지 타입

| ROS2 Topic | ROS2 Message Type |
| --- | --- |
| /tf | tf2_msgs/TFMessage |
| /odom | nav_msgs/Odometry |
| /map | nav_msgs/OccupancyGrid |
| /point_cloud | sensor_msgs/PointCloud |
| /scan | sensor_msgs/LaserScan (published by an external [pointcloud_to_laserscan](https://index.ros.org/p/pointcloud_to_laserscan/) node) |

# **Occupancy Map**

Nav2 사용을 위해 창고 환경의 occupancy map 생성이 필요하며, 이는 NVIDIA Isaac Sim 내 [**Occupancy Map Generator 확장 기능**](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/digital_twin/ext_isaacsim_asset_generator_occupancy_map.html#ext-isaacsim-asset-generator-occupancy-map)을 활용함.

```bash
export ROS_DOMAIN_ID=0
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
source /opt/ros/humble/setup.bash
source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
~/isaacsim/isaac-sim.sh --enable isaacsim.ros2.bridge
```

1. **창고 시나리오 로드:**
    - **Window > Examples > Robotics Examples**로 이동.
    - **Robotics Examples** 탭 클릭 후 왼쪽 섹션 확장.
    - **ROS2 > Navigation > Carter** 예제 열어 [Nova Carter 로봇](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/assets/nova_carter_landing_page.html#isaac-nova-carter)과 창고 환경 로드.
2. **카메라 시점 변경:**
    - 뷰포트 좌측 상단 **Camera 클릭.**
    - 드롭다운 메뉴에서 **Top 선택.**
3. **Occupancy Map 확장 기능 실행:**
    - **Tools > Robotics > Occupancy Map**으로 이동.
4. **점유 맵 설정:**
    - Origin: `X: 0.0, Y: 0.0, Z: 0.0` 확인.
    - Lower bound: `Z: 0.1` 설정.
    - Upper Bound: `Z: 0.62` 설정. (Nova Carter Lidar 높이와 일치)
5. **경계 설정 및 확인:**
    - Stage에서 `warehouse_with_forklifts` prim 선택.
    - Occupancy Map 확장 기능에서 **BOUND SELECTION 클릭.**
    - occupancy map 경계가 선택된 prim에 맞춰 업데이트되었는지 확인.
    - 맵 파라미터가 이미지와 유사한지 확인.
    - 외곽선이 생성되고 Top View 이미지와 유사한지 확인.
        
        <img src="isaac_sample_ros_nav_2.png">
        
        ![Top View of Warehouse with Occupancy Map](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_nav_3.png)
        
6. **로봇** prim **제거:**
    - Stage에서 `Nova_Carter_ROS` prim 제거.
7. **맵 계산 및 시각화:**
    - 설정 완료 후 **CALCULATE 클릭**, 이어서 **VISUALIZE IMAGE 클릭.**
    - Visualization 팝업 창 나타남.
8. **이미지 재구성:**
    - Rotate Image: **180 degrees 선택.**
    - Coordinate Type: **ROS Occupancy Map Parameters File (YAML) 선택.** (ROS와 Isaac Sim 카메라 좌표계 차이 때문)
    - **RE-GENERATE IMAGE 클릭.**
9. **YAML 파라미터 복사:**
    - 아래 필드에 나타난 occupancy map YAML 파라미터 전체 텍스트 복사.
10. **YAML 파일 생성:**
    - 복사한 텍스트를 `carter_warehouse_navigation.yaml` 파일로 생성.
    - `carter_navigation` ROS2 패키지 내 `maps` 디렉토리(`carter_navigation/maps/`)에 저장.
11. **YAML 파일 내용 삽입:**
    - `carter_warehouse_navigation.yaml` 파일에 복사한 텍스트 삽입.
12. **이미지 저장:**
    - NVIDIA Isaac Sim의 Visualization 탭에서 **Save Image 클릭.**
    - 이미지 이름을 `carter_warehouse_navigation.png`로 지정.
    - 맵 파라미터 파일과 **같은 디렉토리에 저장.**
    - 최종 저장된 이미지가 예시 이미지와 같은지 확인.
        
        ![Sample Occupancy Map generated from warehouse stage](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/_images/isaac_sample_ros_nav_warehouse_map.png)
        

[Occupancy Map.mp4](attachment:e7ecb05a-c46b-440a-831e-ae2ad06b497e:Occupancy_Map.mp4)

# **Running Nav2**

## **Nav2 with Nova Carter in Small Warehouse**

1. **창고 시나리오 로드:**
    - **Window > Examples > Robotics Examples** 이동.
    - **Robotics Examples** 탭 클릭 후 왼쪽 섹션 확장.
    - **ROS2 > Navigation > Carter** 예제 열어 [Nova Carter 로봇](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/assets/nova_carter_landing_page.html#isaac-nova-carter)과 창고 시나리오 로드.
2. **시뮬레이션 시작:**
    - **Play 버튼 클릭**하여 시뮬레이션 시작.
3. **Nav2 런치 파일 실행:**
    - 새 터미널에서 다음 ROS2 런치 파일 실행
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch carter_navigation carter_navigation.launch.py
        ```
        
    - RViz2가 열리고 occupancy map 로드 시작. (맵이 나타나지 않으면 이전 단계 반복)
4. **로봇 위치 확인 및 재설정:**
    - 로봇 위치는 `carter_navigation_params.yaml` 파일에 정의되어 있으므로, **로봇이 이미 올바르게 로컬라이즈되었는지 확인.**
    - 필요시 **2D Pose Estimate** 버튼 사용하여 로봇 위치 재설정 가능.
5. **내비게이션 목표 설정 및 실행:**
    - **Navigation2 Goal 버튼 클릭.**
    - 맵에서 원하는 위치를 **클릭하고 드래그.**
    - Nav2가 궤적 생성 후 로봇이 목적지로 이동 시작.
    
    [Nav2_Nova_Warehouse.mp4](attachment:51b8fb51-40d1-47ae-8ed9-19133556609f:Nav2_Nova_Warehouse.mp4)
    
- **(Optional) Isaac ROS Nova Carter 로봇 설명 추가:**
    - 패키지 설정 및 빌드를 통해 추가 가능.
    - 자세한 내용은 [Using the Nova Carter Description Package (Optional)](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_navigation.html#isaac-sim-app-tutorial-ros2-nav-nova-carter-description) 섹션 참고.

<aside>
💡

- **Lidar 및 사람 감지:**
    - Carter 로봇은 기본 RTX Lidar 사용.
    - 장면에 사람 에셋 추가 시, Lidar로 감지되어 Nav2로 전달됨.
- **Hawk 카메라 이미지 발행 및 RViz 시각화:**
    - Hawk 카메라의 ROS2 이미지 발행 파이프라인은 성능 향상을 위해 기본 비활성화.
    - 이미지 발행 시작하려면, 로봇 prim 아래 `_hawk` 액션 그래프 열고 `_camera_render_product` 노드 활성화.
    - render product 노드 활성화 시, 다운스트림 ROS 카메라 발행 노드가 기본 활성화되어 발행 시작되는지 확인.
    - Nova Carter의 모든 센서 및 이미지는 [Sensor Data QoS](https://docs.ros.org/en/rolling/Concepts/Intermediate/About-Quality-of-Service-Settings.html#qos-profiles)로 발행됨.
    - RViz에서 이미지 시각화 시, 이미지 탭 확장 후 **Topic > Reliability Policy**로 이동하여 **Best Effort**로 정책 변경 필요.
- **개방 공간(**open space) **로컬라이징 문제:**
    - 개방 공간에서 로봇 로컬라이징 문제 발생 시, **성능 저하로 인한 알려진 문제.**
    - 로컬라이징 개선 위해 장면에 **특징점(feature)이 될 만한 객체들을 더 추가**해 볼 것.
- 아래와 같은 경고 메시지는 **무시해도 됨.**
    
    ```markdown
    [Warning] [omni.graph.core.plugin] /World/Nova_Carter_ROS/differential_drive/differential_controller_01: [/World/Nova_Carter_ROS/differential_drive] invalid dt 0.000000, cannot check for acceleration limits, skipping current step
    ```
    
</aside>

## **Nav2 with iw.hub in Warehouse.**

![image.png](attachment:22b4826b-c3f5-47dc-a31e-c07d393c9de9:image.png)

1. **창고 시나리오 로드:**
    - **Window > Examples > Robotics Examples**로 이동.
    - **Robotics Examples** 탭 클릭 후 왼쪽 섹션 확장.
    - **ROS2 > Navigation > iw_hub** 예제 열어 iw.hub 로봇과 창고 시나리오 로드.
2. **시뮬레이션 시작:**
    - **Play 버튼 클릭**하여 시뮬레이션 시작.
3. **Nav2 런치 파일 실행:**
    - 새 터미널에서 다음 ROS2 런치 파일 실행
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch iw_hub_navigation iw_hub_navigation.launch.py
        ```
        
    - RViz2가 열리고 occupancy map 로드 시작. (맵이 나타나지 않으면 이전 단계 반복)
4. **로봇 위치 확인 및 재설정:**
    - 로봇 위치는 `iw_hub_navigation_params.yaml` 파일에 정의되어 있으므로, **로봇이 이미 올바르게 로컬라이즈되었는지 확인.**
    - 필요시 **2D Pose Estimate** 버튼 사용하여 로봇 위치 재설정 가능.
5. **내비게이션 목표 설정 및 실행:**
    - **Navigation2 Goal 버튼 클릭.**
    - 맵에서 원하는 위치를 **클릭하고 드래그.**
    - Nav2가 궤적 생성 후 로봇이 목적지로 이동 시작.
    - **장면에 있지만 초기 맵에 포함되지 않은 팔레트와 같은 동적 장애물을 로봇이 회피하는지 확인.**

[Nav2_iw_hub.mp4](attachment:da12644e-10a7-4608-a2c6-189258463b69:Nav2_iw_hub.mp4)

## **Sending Goals Programmatically**

`isaac_ros_navigation_goal` ROS2 패키지는 파이썬 노드를 이용해 로봇의 목표 자세(goal pose)를 설정하는 데 사용. 랜덤 목표 생성 및 Nav2로 전송 가능하며, 사용자 정의 목표 자세도 전송 가능.

1. **런치 파일 파라미터 변경:**
    - `isaac_ros_navigation_goal/launch` 내 런치 파일의 파라미터 필요에 따라 변경.
    - 내용 수정 후 패키지 및 워크스페이스 **재빌드 및 source 필수.**
    - **파라미터 설명:**
        - `goal_generator_type`: 목표 생성기 유형. `RandomGoalGenerator`(랜덤 목표) 또는 `GoalReader`(사용자 정의 목표 순서대로) 사용.
        - `map_yaml_path`: occupancy map 파라미터 YAML 파일 경로. (예: `isaac_ros_navigation_goal/assets/carter_warehouse_navigation.yaml`). `RandomGoalGenerator` 사용 시 **필수.**
        - `iteration_count`: 목표 설정 반복 횟수.
        - `action_server_name`: 액션 서버 이름.
        - `obstacle_search_distance_in_meters`: 생성된 목표 자세가 장애물로부터 자유로운 최소 거리(미터).
        - `goal_text_file_path`: 사용자 정의 정적 목표를 포함하는 텍스트 파일 경로. 각 줄은 `pose.x pose.y orientation.x orientation.y orientation.z orientation.w` 형식. `GoalReader` 사용 시 **필수.**
        - `initial_pose`: 설정 시 `/initialpose` 토픽으로 발행되며, 이후 액션 서버로 목표 자세 전송. 형식: `[pose.x, pose.y, pose.z, orientation.x, orientation.y, orientation.z, orientation.w]`
2. **창고 시나리오 로드:**
    - **Window > Examples > Robotics Examples** 이동 후 **Robotics Examples** 탭 클릭.
    - 왼쪽 섹션 확장하여 **ROS2 > Navigation > Carter** 예제 열어 창고 시나리오 로드.
3. **시뮬레이션 시작:**
    - **Play 클릭**하여 시뮬레이션 시작.
4. **Nav2 런치 파일 실행:**
    - 새 터미널에서 다음 ROS2 런치 파일 실행
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch carter_navigation carter_navigation.launch.py
        ```
        
5. **`isaac_ros_navigation_goal` 런치 파일 실행:**
    - 다음 명령 실행하여 자동으로 목표 전송 시작
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch isaac_ros_navigation_goal isaac_ros_navigation_goal.launch.py
        ```
        
        [Send_Goals_Prog.mp4](attachment:7a95ac50-b977-4667-830a-d7f2254b0b1b:Send_Goals_Prog.mp4)
        
        ```python
        # Copyright (c) 2025, NVIDIA CORPORATION.  All rights reserved.
        #
        # NVIDIA CORPORATION and its licensors retain all intellectual property
        # and proprietary rights in and to this software, related documentation
        # and any modifications thereto.  Any use, reproduction, disclosure or
        # distribution of this software and related documentation without an express
        # license agreement from NVIDIA CORPORATION is strictly prohibited.
        
        import os
        from ament_index_python.packages import get_package_share_directory
        from launch import LaunchDescription
        from launch.substitutions import LaunchConfiguration
        from launch_ros.actions import Node
        
        def generate_launch_description():
        
            map_yaml_file = LaunchConfiguration(
                "map_yaml_path",
                default=os.path.join(
                    get_package_share_directory("isaac_ros_navigation_goal"), "assets", "carter_warehouse_navigation.yaml"
                ),
            )
        
            goal_text_file = LaunchConfiguration(
                "goal_text_file_path",
                default=os.path.join(get_package_share_directory("isaac_ros_navigation_goal"), "assets", "goals.txt"),
            )
        
            navigation_goal_node = Node(
                name="set_navigation_goal",
                package="isaac_ros_navigation_goal",
                executable="SetNavigationGoal",
                parameters=[
                    {
                        "map_yaml_path": map_yaml_file,
                        "iteration_count": 3,
                        "goal_generator_type": "RandomGoalGenerator",
                        "action_server_name": "navigate_to_pose",
                        "obstacle_search_distance_in_meters": 0.2,
                        "goal_text_file_path": goal_text_file,
                        "initial_pose": [-6.4, -1.04, 0.0, 0.0, 0.0, 0.99, 0.02],
                    }
                ],
                output="screen",
            )
        
            return LaunchDescription([navigation_goal_node])
        ```
        
        ```markdown
        1 2 0 0 1 0
        2 3 0 0 1 1
        3.4 4.5 0.5 0.5 0.5 0.5
        ```
        
- **패키지 처리 중단 조건:**
    1. 발행된 목표 수가 `iteration_count` 이상일 때.
    2. `GoalReader` 사용 시 설정 파일의 모든 목표가 발행되었을 때.
    3. 액션 서버에 의해 목표가 거부될 때.
    4. 드문 경우, 매우 밀집된 맵에서 `RandomGoalGenerator`가 유효하지 않은 자세를 과도하게 생성하여 최대 반복 횟수를 초과할 때.
- **참고:**
    - 단일 런치 프로세스로 Isaac Sim 및 Nav2 자동 실행, 목표 프로그래밍 방식 전송은 [Launch Isaac Sim with Nav2](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_launch.html#isaac-sim-app-tutorial-ros2-nav-goals-launch) 참고.
    - 다중 로봇에게 프로그래밍 방식으로 목표 전송은 [Sending Goals Programmatically for Multiple Robots](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_multi_navigation.html#isaac-sim-app-tutorial-ros2-multi-nav-goals) 참고.

## **Sending Goals Using ActionGraph**

1. **로보틱스 예제 탭 열기:**
    - **Window > Examples > Robotics Examples**로 이동.
2. **창고 시나리오 로드:**
    - **Robotics Examples > ROS2 > Navigation > Carter**에서 **Load Sample Scene** 버튼 클릭.
3. **웨이포인트 팔로워 파라미터 창 열기:**
    - **Robotics Examples > ROS2 > Navigation > Add Waypoint Follower**로 이동.
4. **웨이포인트 팔로워 파라미터 변경:**
    - 필요에 따라 파라미터 수정.
    - **Graph Path:** 스테이지 내 경로 지정.
    - **Frame ID:** 내비게이션 작업의 참조 프레임 지정.
    - **Navigation Modes:**
        - **Waypoint Mode:** 단일 웨이포인트를 내비게이션 목표로 설정. 로봇은 해당 웨이포인트로 이동.
        - **Patrolling Mode:** 다중 웨이포인트(2개~50개)를 생성하여 연속 순찰. 로봇은 미리 정의된 웨이포인트 사이를 계속 순찰.
    - **Waypoint Count:** **Patrolling** 모드에서 생성할 웨이포인트 개수.
        
        ![isim_4.5_ros_tut_gui_waypoint_follower_parameters.png](attachment:dc3a8b45-9e88-4a9d-8216-71e8e3c55c4d:isim_4.5_ros_tut_gui_waypoint_follower_parameters.png)
        
5. **웨이포인트 팔로워 액션 그래프 로드:**
    - **Load Waypoint Follower ActionGraph** 클릭.
    - 웨이포인트 생성 및 Stage 패널의 **Graph Path**에 액션 그래프 추가.
6. **시뮬레이션 시작:**
    - **Play 클릭**하여 시뮬레이션 시작.
7. **Nav2 런치 파일 실행:**
    - 새 터미널에서 다음 ROS2 런치 파일 실행
        
        ```bash
        export ROS_DOMAIN_ID=0
        export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
        export FASTRTPS_DEFAULT_PROFILES_FILE=/opt/.ros/fastdds.xml
        source /opt/ros/humble/setup.bash
        source ~/IsaacSim-ros_workspaces/humble_ws/install/local_setup.bash
        ros2 launch carter_navigation carter_navigation.launch.py
        ```
        
8. **로봇 위치 확인 및 재설정:**
    - `carter_navigation_params.yaml` 파일에 로봇 위치가 정의되어 있으므로, **로봇이 올바르게 로컬라이즈되었는지 확인.**
    - 필요시 **2D Pose Estimate** 버튼으로 로봇 위치 재설정 가능.
9. **내비게이션 모드 실행:**
    1. **웨이포인트(Waypoint) 모드:**
        1. 장면의 XY 평면에서 웨이포인트(`/World/Waypoints/waypoint_1`)를 조절하여 원하는 목표 위치 설정.
        2. Stage에서 `ROS_Nav2_Waypoint_Follower` 그래프를 열고 OnImpulseEvent 노드에서 **Send Impulse 클릭.**
        3. 로봇이 지정된 목표로 이동 시작하는지 확인.
        4. 각 목표 완료 후 새 웨이포인트 설정을 위해 이 단계들 반복.
    2. **순찰(Patrolling) 모드:**
        1. 장면의 XY 평면에서 웨이포인트(`/World/Waypoints/waypoint_n`)들을 조절하여 순찰 경로 정의.
        2. Stage에서 `ROS_Nav2_Waypoint_Follower` 그래프를 열고 OnImpulseEvent 노드에서 **Send Impulse 클릭.**
        3. 로봇이 설정된 웨이포인트를 따라 순찰 시작하는지 확인.

[Send_Goals_ActGraph.mp4](attachment:4db92cdf-83ac-4499-b5e5-9a32f7771f45:Send_Goals_ActGraph.mp4)

<aside>
💡

- 본 튜토리얼은 **AMCL 로컬라이저를 사용**하며, 액션 그래프가 이 로컬라이저를 **완전히 지원함.**
- 그래프 삭제 후 아래와 같은 **오류 메시지가 나타나도 무시**해도 됨. 이러한 로그를 방지하려면 그래프를 삭제하기 전에 "reload node" 버튼을 클릭하여 스크립트 노드를 정리할 수 있음.
    
    ```markdown
    2024-12-03 13:55:27 [4,715,030ms] [Error] [omni.graph] Error executing python callback omni.graph.scriptnode.ScriptNode.release_instance
    2024-12-03 13:55:27 [4,715,030ms] [Error] [omni.graph] Error executing python callback omni.graph.scriptnode.ScriptNode.release
    ```
    
</aside>

# 참고문헌

https://docs.isaacsim.omniverse.nvidia.com/4.5.0/ros2_tutorials/tutorial_ros2_navigation.html

[ROS2 Navigation — Isaac Sim Documentation.pdf](attachment:c8585da1-291f-4877-9de2-07f7df4cec47:ROS2_Navigation__Isaac_Sim_Documentation.pdf)
