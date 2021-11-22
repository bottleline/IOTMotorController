# TARS (IoT 기기 제어)

## 주요기능

- 소형 모터장치를 블루투스와(단거리) MQTT(장거리) 통신을 사용하여 제어하는 어플리케이션

## 수행역할
- 단독 프로젝트
- 앱 기획 및 총 제작
- 하드웨어 설계 및 제작

## 사용한 기술
- `Swift 5`, `Xcode 12`
- `RxSwift`
- `CocoaMQTT`
- `CoreBluetooth`

## 주요 구현 장면

### 1. 블루투스 스캔(CoreBluetooth) / ioT 기기 검색 기능

![tars1-1](https://user-images.githubusercontent.com/42457589/142857905-09f62219-ce27-45f1-bd23-16f4aea46645.gif)
![tars1-2](https://user-images.githubusercontent.com/42457589/142857909-c24dd7f7-c5b5-4008-96c2-bc36c5b1677d.gif)
  
### 2. 디바이스 동작
![tars4-1](https://user-images.githubusercontent.com/42457589/142858015-625483aa-48f9-4769-b094-620b72802e23.gif)
![tars4-2](https://user-images.githubusercontent.com/42457589/142858027-02474ac1-905e-4534-9366-c1363b270c1d.gif)
  
### 3. 디바이스 이름 설정
![tars2](https://user-images.githubusercontent.com/42457589/142857940-f8f76369-46c6-4176-a7bd-ee9929f32f34.gif)
  
### 4. 디바이스 커스텀 동작 설정
![tars3](https://user-images.githubusercontent.com/42457589/142857978-4e463a15-39c0-4de6-8078-edbd56deca24.gif)
  
### 5. Wifi Hub 연결기능
![tars5-1](https://user-images.githubusercontent.com/42457589/142858107-b463c211-c97f-4d0f-a663-779841bc8280.gif)
![tars5-2](https://user-images.githubusercontent.com/42457589/142858122-3ee74cee-a324-4bfc-8caa-da52961f3f86.gif)
wifi hub를 ble 스캔하여 찾은후 등록한다.  
hub 등록이 되면 hub를 통해 iot 기기와 스마트폰간 wifi 통신이 가능해진다
  
### 6. 바로가기 동작기능 구현
![tars6-1](https://user-images.githubusercontent.com/42457589/142858211-660d8ad1-f1ef-43d8-8a5c-ae1d96c2a7d2.gif)
![tars6-2](https://user-images.githubusercontent.com/42457589/142858218-f0bb8b53-e962-4a3d-97ec-85c1ec2af73e.gif)
숏컷 아이콘을 만들어 번거롭게 손가락을 움직이는 횟수를 줄인다.


