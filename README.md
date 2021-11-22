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
![blescan](https://user-images.githubusercontent.com/42457589/132497623-40bbe259-23c9-4a7c-8e85-2cae8aebfa52.gif)
  
### 2. 블루투스  / Wifi 제어 기능
![sendPacket](https://user-images.githubusercontent.com/42457589/132497622-a3af1e94-81d4-4090-8e30-e642fa94a709.gif)
  
### 3. 디바이스 이름변경 및 저장 (sqlite)
![change_dname](https://user-images.githubusercontent.com/42457589/132497612-7337b7d4-b8d2-4dc8-88d3-5b5ceb332306.gif)
  
### 4. 동작 이름 변경 및 추가  / 색상 및 아이콘 선택 / 
![change_aname](https://user-images.githubusercontent.com/42457589/132497626-9e4a1661-9c82-4d9c-9229-4aed6651b401.gif)
  
### 5. 원하는 동작 설정 기능 / 


