![](http://www.plantuml.com/plantuml/png/hLN9Rjiy4BxpAGZX1twxQHmKlGbg42zEQa29KpiBUgIbiKMO8WsIic4KVVUEbaOEIXCn4Xq4QfRlcsM6kZAMQLlaWXYmLB8Sp8ObC8SDqymgJSEGc3MNvMTE1PTWznkWIjjC7IXCnbArut95bqmOED0aWXb3Vn6Apu0bwvLMXKo7IYWTHHhI8W5zUKc_Amdx4UpSTYonN4aqUyw0MRUD0MbK2VK0IIPHjjgx3RSSTcWsrKd6KxS9B9UGM3IwKI8zHYVGf2ACsHPa2fhy9kJ_R-r3KrXooSlG3I5r8OAXqa3x9RT5_q936DAvHM50esY95OETqcj4nPATHT5G0xE0dvLDfocOGH-mqQmfNvVGSsFHxhzmkZ2WjsmX89hRqXDLTVNCAsM_bg7wBENOSfjadTekfBMk6suLJr5VPzhvcp1IDwTkM96MQJV93PYreFUOKdCAw45ZoeoKd615EY_WIex0MAtsZSW9Df6WBMVY5fWfDEGWxIEYqCxazh7xCHJDgQfPKC58WDJCiLuyWP0kb1B09CgJZEaLIfP2COlVt0nB2Sv-Q-LXc05BqJQW1rDTqfLh5GQ3_iF3UFRpOZRkZoPtDm4gKcPPU1YUkqHvCKB3EdIM9210ixgTJrMplg9SAzl8VXGC7IMUKPpL8RKfMSWtfNkXeH4Uu_HdW-tv9vCcq18GP94Vujsh7UYs0zUFSVoPq7ZwZE_fzRKhXjedXYs88p7SJv_7iuju_3IENs9eYNoTcfXOxQRwJjsgRJLp0Mrl1cJmBzRnQ4Tg9zyFDT1ybLa9nbJPcmz050LOfMmsWdBctm_un1Vr39uCzVeQ5Lok3I579Y_u4dmwFyUtQnt8ss-HNk_oo72JDys6nzy1u0__SDrTz0sR6_NqYDqO-BWNhOtPlRsdqJi8Y7jbF0xUnV3u-SRVu-OUSey38TuVVw_d-dUUukCVxfFP7Gqx1QJjsWbUZjOHSWKoBNBn1m00)

# Power Levels and Battery
Any device that has a low power state listens for the PowerLevel action and adjusts their behaviour accordingly.



# PlantUML
```plantuml
@startuml
set namespaceSeparator ::
skinparam linetype ortho
skinparam shadowing false
skinparam class {
    BackgroundColor<<reducer>> HoneyDew
    BackgroundColor<<action>> Wheat
    BackgroundColor<<sensor>> Technology
    BackgroundColor<<view>> Orchid
    BackgroundColor<<effect>> Gold
    BackgroundColor<<selector>> Lavender
}

'=========== definitions

class "Core Architecture:: Store" <<framework>> {
 +<<Create>> Store(reducers: Set<Reducer>)
 -state$:Observable<State>
 -actions$:Observable<Actions>
 +dispatch(action: Action):void
 +select(selector: Selector):state
}

class "Battery:: Battery" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Battery:: InertialMeasurementUnit (IMU)" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Battery:: BatteryState" {
 +needsService: boolean
 +charge: float
 +isCharging: boolean
}

class "Battery:: BatteryReading" <<action>> {
 +type:"BATTERY/READING"
 +data:BatteryState
}

class "Battery:: ImuData" {
 +acceleration: Vector3
 +rotation: Quaternion
 +heading: float
 +temperature: float
}

class "Battery:: ImuReading" <<action>> {
 +type:"IMU/READING"
 +data:ImuData
}

enum "Battery:: PowerState" {
 FULL,
 LOW,
 OFF
}

class "Battery:: PowerLevel" <<action>> {
 +type:"POWER/LEVEL"
 +data:PowerState
}

class "Battery:: BatteryIsLowOrNoMovement" <<effect>> {
 -actions$:Observable<Actions>
}

class "Battery:: PowerDown" <<effect>> {
 -actions$:Observable<Actions>
}

class "Battery:: BrightnessLevels" <<effect>> {
 -actions$:Observable<Actions>
}

class "Battery:: BluetoothDevice" <<effect>> {
 -actions$:Observable<Actions>
}

class "Battery:: Vibration" <<effect>> {
 -actions$:Observable<Actions>
}


'=========== links

"Battery:: ImuReading" "1"*--"1" "Battery:: ImuData"
"Battery:: InertialMeasurementUnit (IMU)" ..> "Battery:: ImuReading"
"Battery:: BatteryIsLowOrNoMovement" ..> "Battery:: ImuReading"

"Battery:: BatteryReading" "1"*--"1" "Battery:: BatteryState"
"Battery:: Battery" ..> "Battery:: BatteryReading"
"Battery:: BatteryIsLowOrNoMovement" ..> "Battery:: BatteryReading"



"Battery:: PowerLevel" "1"*--"1" "Battery:: PowerState"
"Battery:: BatteryIsLowOrNoMovement" ..> "Battery:: PowerLevel"
"Battery:: PowerDown" ..> "Battery:: PowerLevel"
"Battery:: BrightnessLevels" ..> "Battery:: PowerLevel"
"Battery:: BluetoothDevice" ..> "Battery:: PowerLevel"
"Battery:: Vibration" ..> "Battery:: PowerLevel"


"Core Architecture:: Store" <.. "Battery:: Battery"
"Core Architecture:: Store" <.. "Battery:: InertialMeasurementUnit (IMU)"
"Core Architecture:: Store" <.. "Battery:: BatteryIsLowOrNoMovement"
"Core Architecture:: Store" <.. "Battery:: PowerDown"
"Core Architecture:: Store" <.. "Battery:: BrightnessLevels"

@enduml
```
