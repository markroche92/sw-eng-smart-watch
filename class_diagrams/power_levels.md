![](http://www.plantuml.com/plantuml/png/hLLRQzim57xNhn1ci6HTKyRUZ5UQIze5tAPBUc6FYdqIYyXIaEI4KFRVTtnPtJBPcsdzOEIZyttVkHp95yOoRRDK40EMIfQ2sR48bh1ZcbcbgUyJi-Ko_qofILYi3bnkw9e90ozs8i6CeK-4uZDYqNQZLIRZiH9A1u66E8j0dv_Jxqh2SG87jn-BB5SItHuJODRjOq0QbLFTGPH8j6sERiSzXmEwpNMKyDZj0kirH1QThfI8c-G4kXI28Tk3Z46JNuHy_5O_D8OrbpmFtH1I5i8RAmrqc6jRn6SQV9ykBHez6WHhh1WSbDuYAvRi90Z66fW5_2nyUbN136B01ekgV7r2Jur5lm_-V6L0xzbAGB2qEHAtokgPLvl3qeRR9p77pjeewPL-FYtt-lvUyHZtoqnxVn96wMhLzmlHFEqgkH6p5lGHKwfM7gqRKsIav4nosticLt82ncfrh4nEifa4RJaJry1CfY45QU-HXVPcr_Vzzv6eLaNDl8960iHcYVNY4VXqfPG09j4U9KnlqB8MYbdyvcQSMt3sQwyMXGMm67qzMazr8MUFE_2zqV3kRhhuURQO3YUpcoilRm-pp7yPdhj4QJP1np9q5aKWGBEodGz5ixwYNIjRsNva384IrsXEof3gb2oaknoUQQYCJPhjsM1x_iga2ZGd19cbB_bkrG5qtO7B-p3yJ6WuVyJt_FBI5KE92M4FeY66s_dZT74MJX-cuNCCjL1RfsOcL8Uvlb7NQb_C72hKDyFJFmvMSxGJTP1lfnfflaci16EAxCqxC8eCh58scK0-ysydVE2hSWOxKxs-HWMNMqD8m-HvNxnFfwVuTXq7qk4M6GmQXflytsmqunq4xl2Tjv43NQcvMRj7xER0nttGt37NXwg3l5k0k8zCEw3bOBJZtEFV3dCFUJE0aBOV_s1mxlftTi6q3tTdTKV3kf3KNUk0ShIEa0kGSPQAtm00)

# Power Levels and Battery
Any device that has a low power state listens for the PowerLevel action and adjusts their behaviour accordingly.



# PlantUML
```plantuml
@startuml
set namespaceSeparator ::
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
"Battery:: InertialMeasurementUnit (IMU)" <.. "Battery:: ImuReading"
"Battery:: BatteryIsLowOrNoMovement" <.. "Battery:: ImuReading"

"Battery:: BatteryReading" "1"*--"1" "Battery:: BatteryState"
"Battery:: Battery" <.. "Battery:: BatteryReading"
"Battery:: BatteryIsLowOrNoMovement" <.. "Battery:: BatteryReading"



"Battery:: PowerLevel" "1"*--"1" "Battery:: PowerState"
"Battery:: BatteryIsLowOrNoMovement" <.. "Battery:: PowerLevel"
"Battery:: PowerDown" <.. "Battery:: PowerLevel"
"Battery:: BrightnessLevels" <.. "Battery:: PowerLevel"
"Battery:: BluetoothDevice" <.. "Battery:: PowerLevel"
"Battery:: Vibration" <.. "Battery:: PowerLevel"


"Core Architecture:: Store" ..> "Battery:: Battery"
"Core Architecture:: Store" ..> "Battery:: InertialMeasurementUnit (IMU)"
"Core Architecture:: Store" ..> "Battery:: BatteryIsLowOrNoMovement"
"Core Architecture:: Store" ..> "Battery:: PowerDown"
"Core Architecture:: Store" ..> "Battery:: BrightnessLevels"

@enduml
```
