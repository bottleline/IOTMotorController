# TARS (진행중) (IoT 기기 제어)

## 주요기능

- 소형 모터장치를 블루투스와(단거리) MQTT(장거리) 통신을 사용하여 제어하는 어플리케이션

## 수행역할
- 단독 프로젝트
- 앱 기획 및 총 제작
- 하드웨어 설계 및 제작

## 사용한 기술
- `Swift 5`, `Xcode 12`
- `CocoaMQTT`
- `CoreBluetooth`

 ## 적용한 주요 Layout
- `TableView`

 ## 한계점, 개선점
 - 바로가기 아이콘을 생성하여 실행할 시 백그라운드에 App이 켜져있지 않으면 바로가기 동작이 되지 않음   
 - A31R118 BLE MCU 의 전력소모량이 너무큼, -> Nordic의 NRF 계열 BLE 칩으로 교체하려 했으나 대리점과의 거래성사가 이루어지지 않음


## 주요 구현 장면

### 1. 블루투스 스캔(CoreBluetooth) / ioT 기기 검색 기능

![tars1-1](https://user-images.githubusercontent.com/42457589/142857905-09f62219-ce27-45f1-bd23-16f4aea46645.gif)
![tars1-2](https://user-images.githubusercontent.com/42457589/142857909-c24dd7f7-c5b5-4008-96c2-bc36c5b1677d.gif)  

### 1
``` swift
//구현 로직 설명

 // 1. 디바이스 스캔시작 
 centralManager.scanForPeripherals(withServices: nil) 

``` 
### 2
``` swift
 // 1. 디바이스 발견시 연결 
func centralManager(_ central: CBCentralManager, didDiscover peripheral: CBPeripheral, advertisementData: [String : Any], rssi RSSI: NSNumber) {
        if peripheral.name == "iotDevice"{
            if let code = String(describing: advertisementData["kCBAdvDataManufacturerData"]){ // advertisment 패킷에 제조사 정보에 담긴 일련번호를 추출한다.
                if checkCode(code){
                  central.connect(peripheral,options:nil) // 일련번호를 확인후 연결한다
                }
            }
        }
    }

``` 
``` swift
 // 2. 서비스 연결 후 응답 패킷 요청 
 func peripheral(_ peripheral: CBPeripheral, didDiscoverCharacteristicsFor service: CBService, error: Error?) {
     guard let charactistics = service.characteristics else{return}
     if charactistic.uuid == notiUUID{
                peripheral.setNotifyValue(true, for: charactistic) // notifiying 서비스를 구독하여 iot 기기에서 변경된 값을 감시
            }else if charactistic.uuid == writeUUID{
                if !registDevice{
                    peripheral.writeValue(deviceDataList![n].messege) // 응답을 요청하는 패킷 발송
                }
            }
      }
 }

``` 
### 3
``` swift
// 3. 응답패킷 수신 
 func peripheral(_ peripheral: CBPeripheral, didUpdateValueFor characteristic: CBCharacteristic, error: Error?) {
        switch characteristic.uuid{
        
        case notiUUID:
            guard let s = characteristic.value else {return}
            guard let sData = s.data(using: .utf8) else {return}
            if let string = String(data: sData,encoding: String.Encoding.utf8){
                if string == "success"{ // 연결을 허용할 시
                  registDB(peripheral) // DB 에 peripheral 디바이스의 ADV 패킷에서 전송한 디바이스 고유번호를 저장
                }else{ // PIN 번호를 요구할 시
                  checkPin() // PIN 번호 전송 메소드 호출
                }
            }
        default:
            print("Unhandled Charactistic UUID: \(characteristic.uuid)")
        }
    }
```

Ble 스캔을 통해 디바이스를 검색하고 등록한다.
  
### 2. 디바이스 동작
![tars4-1](https://user-images.githubusercontent.com/42457589/142858015-625483aa-48f9-4769-b094-620b72802e23.gif)
![tars4-2](https://user-images.githubusercontent.com/42457589/142858027-02474ac1-905e-4534-9366-c1363b270c1d.gif)  
설정한 액션을 동작한다.
# 단거리 동작
``` swift
// 단거리 : 블루투스 동작
peripheral.writeValue(messege)
```
# 장거리 동작
``` swift
// 단거리 : 블루투스 동작
mqtt.publish(Topic, messege)
```
  
    
### 3. 디바이스 이름 설정
![tars2](https://user-images.githubusercontent.com/42457589/142857940-f8f76369-46c6-4176-a7bd-ee9929f32f34.gif)  
디바이스의 이름을 설정한다

### 4. 디바이스 커스텀 동작 설정
![tars3](https://user-images.githubusercontent.com/42457589/142857978-4e463a15-39c0-4de6-8078-edbd56deca24.gif)  
디바이스의 동작을 원하는대로 설정한다.
  
### 5. Wifi Hub 연결기능
![tars5-1](https://user-images.githubusercontent.com/42457589/142858107-b463c211-c97f-4d0f-a663-779841bc8280.gif)
![tars5-2](https://user-images.githubusercontent.com/42457589/142858122-3ee74cee-a324-4bfc-8caa-da52961f3f86.gif)  
wifi hub를 ble 스캔하여 찾은후 등록한다.  
hub 등록이 되면 hub를 통해 iot 기기와 스마트폰간 wifi 통신이 가능해진다

![image](https://user-images.githubusercontent.com/42457589/143727332-6f544800-5fd6-4252-b008-c9ff088065e8.png)

  
### 6. 바로가기 동작기능 구현
![tars6-1](https://user-images.githubusercontent.com/42457589/142858211-660d8ad1-f1ef-43d8-8a5c-ae1d96c2a7d2.gif)
![tars6-2](https://user-images.githubusercontent.com/42457589/142858218-f0bb8b53-e962-4a3d-97ec-85c1ec2af73e.gif)  
숏컷 아이콘을 만들어 번거롭게 손가락을 움직이는 횟수를 줄인다.

### 1
``` 
1. Capabilites 에 Siri 추가
```

### 2
``` swift
// 2. Scene Delegate 에 추가
//Tells the delegate to handle the specified Handoff-related activity.

func scene(_ scene: UIScene, continue userActivity: NSUserActivity) {
        if let vc = window?.rootViewController as? ViewController{
            vc.shortcut() // 숏컷으로 실행할 메소드 추가
        }
        
    }
```

### 3
``` swift
func shortcut(){
        //Key moment 저장
        let activity = NSUserActivity(activityType: "kr.kevin.ioT.Excute") // NSUserActivity 를 생성하여 실행되는 순간을 포착함
        
        activity.title = "동작실행하기" // user activity 타이틀 지정
        
        //프로퍼티를 활성화하여 객체를 사용할수 있는 작업을 구성
        activity.isEligibleForSearch = true // activity 검색 가능 여부
        activity.isEligibleForPrediction = true // activity 예측 가능 여부
        activity.isEligibleForHandoff = true // activity handoff 기능 지원 여부
        activity.isEligibleForPublicIndexing = true
        
        activity.resignCurrent()
        
        self.userActivity = activity
        self.userActivity?.becomeCurrent() // useractivity 객체를 시스템에 등록한다.

        self.excute(2, 0) // 지정한 동작을 실행
    }
```


