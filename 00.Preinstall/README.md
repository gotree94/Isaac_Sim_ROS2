# [강습회] 사전설치 확인 요령

# Isaac Sim 사전설치 동작 확인 요령

## 1. Isaac Sim 실행 w/ ROS2 Humble

```bash
# Run Isaac Sim w/ Enabling ROS2 Bridge
export ROS_DOMAIN_ID=0
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp
source /opt/ros/humble/setup.bash
~/isaacsim/isaac-sim.sh --enable isaacsim.ros2.bridge
```

![image.png](attachment:35c57adb-530e-4cae-a62d-3179995b35a0:image.png)

- **(필요시) ROS2 Humble 설치**
    
    ```bash
    wget https://raw.githubusercontent.com/knowledge-intelligence/KIMe-Tutorials/master/install_ros2_humble.sh && chmod 755 ./install_ros2_humble.sh && ./install_ros2_humble.sh
    ```
    

## 2. Moveit 환경 로드

- **Isaac Sim 환경 로드**:
    - **`Window > Examples > Robotics Examples`** 이동.
        
        ![image.png](attachment:80b4b85f-aa14-4c69-91be-98434edb8e87:image.png)
        
    - **`Robotics Examples`** 탭 클릭 후 왼쪽 섹션 확장.
        
        ![image.png](attachment:73457234-c61b-40b3-a4a6-3ec9aa14afa1:image.png)
        
    - 예제 **`ROS2 > MoveIt > Franka MoveIt`** 열기.
        
        ![image.png](attachment:8adb6df5-8871-4036-a8bf-63006bb4046b:image.png)
        

## **3. Nova Carter Warehouse 환경 로드**

- **Isaac Sim 환경 로드**:
    - **Window > Examples > Robotics Examples** 이동.
    - **Robotics Examples** 탭 클릭 후 왼쪽 섹션 확장.
    - **ROS2 > Navigation > Carter** 예제 열어 [Nova Carter 로봇](https://docs.isaacsim.omniverse.nvidia.com/4.5.0/assets/nova_carter_landing_page.html#isaac-nova-carter)과 창고 시나리오 로드.
        
        ![image.png](attachment:e3731804-b40a-40de-bc0b-60d9345f4845:image.png)
        

## **4. iw.hub Warehouse 환경 로드**

- **Isaac Sim 환경 로드**:
    - **Window > Examples > Robotics Examples**로 이동.
    - **Robotics Examples** 탭 클릭 후 왼쪽 섹션 확장.
    - **ROS2 > Navigation > iw_hub** 예제 열어 iw.hub 로봇과 창고 시나리오 로드.
        
        ![image.png](attachment:1a214f9b-ec15-4cef-90f3-25a400dbd8fb:image.png)
        

## **5. Multiple Robot 환경 로드**

- **시나리오 불러오기:**
    - **병원 환경**: `Window > Examples > Robotics Examples` 이동 후 `Robotics Examples` 탭 클릭, 왼쪽 섹션 확장해서 `ROS2 > Navigation > Multiple Robots > Hospital Scene` 예제 열면 됨.
        
        ![image.png](attachment:bf991895-982e-4e67-8ac2-b8aa90c8b16b:image.png)
        
    - **사무실 환경**: `Window > Examples > Robotics Examples` 이동 후 `Robotics Examples` 탭 클릭, 왼쪽 섹션 확장해서 `ROS2 > Navigation > Multiple Robots > Office Scene` 예제 열면 됨.
        
        ![image.png](attachment:f82bfaa0-26a9-4416-86a3-f763158b5f3f:image.png)
