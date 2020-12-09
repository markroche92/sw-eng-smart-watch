![](http://www.plantuml.com/plantuml/png/hLHXRzem4FtkNt6A9YseM_SzogfIYjfAsrfHj7sSNESW5i56zWM6AlxxpeJHR8AORUKJzhrxp_Vox4jFqb4zhuH70YFdw1TIuICkf9Da7MIPy3Djmd8ElfI5NMapXOciFBOgbJP8wmM2TLJQLa5Lqdju5i2_AwbcKsThKmniPLsUEongXUxY0cwjmVKrhWxZf29j3SE-bIZfCCQZyJPGZL6LXlUcwyF0fSOLm-wTAdLn68AJ2IfYq8sjYcDo5KEsWf_a4as1Jco4UFlnvGS5JhJHuUXUYCQ8P60TGZze4_VN3hCC7fa74yZp2Jk6A-jcpCgMdUNvmA4avEKMa-uCyzo3b8zszlK4lFV4k3VP_PD7jvHF5UQF53gvrBZdVolscpqkdnNQBoIfCcrm6JIrNhQqmP-pPjBqry11kldNoxQYOhCVxXQb8nX92bCD0yfP9LsXxJHa8HqERde9jBvL6DAQEpOftUjPSusU6zUxhMFy3wKbkwYGohMV1u55kORq1V9wIaCr7Q4iEFS9jC8OA4FciuJd-Z0Qzg_lljmamL59CdjsJKi6ayfAsXmZtox2_sw9ss5_DFu-we-7_w_qLUCgWVrD229kc-283rSeoJ-W-wnDJRXBNmBjUznmXLNgANJ1DeRluCzmDViTZhyYVMIG1DhNEKZW3rHSI9d7HuvvlqGtrlCJIThFGtWpPrvqVgFpyulkeqVREquin5zVA2QCnLUSb9bJY8GuxK4vFEQUvbYo8dt_uaxDX1ql_Cdpn7cY5aTPEes9j4PI5-iMbsYAUbxz1000)

# Heart Rate and Electrocardiogram (ECG)

The Electrocardiogram/Photoplethysmography class encapsulates the functionality for the sensor. Once it has collected it is readings it dispatches an EcgReading/HeartRateReading event to the Store. These readings then pass through the BeatsPerMinuteReducer/EcgReducer to build the state. The HeartRateView queries the Store for the State data and renders the data each time the store updates.



# PlantUML

```plantuml
@startuml
set namespaceSeparator ::
skinparam shadowing false
skinparam linetype ortho
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

class "Heart Rate:: Electrocardiogram (ECG)" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Heart Rate:: Photoplethysmography (Heart Rate)" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Heart Rate:: EcgReading" <<action>> {
 +type:"ECG/READING"
 +data:{reading: float}
}

class "Heart Rate:: HeartRateReading" <<action>> {
 +type:"HEART_RATE/READING"
 +data:{reading: float}
}

class "Heart Rate:: HeartRateView" <<view>> {
 +render():void
}

class "Heart Rate:: BeatsPerMinuteReducer" <<reducer>> {
 +reduce(state: State, action: Action): State
}

class "Heart Rate:: EcgReducer" <<reducer>> {
 +reduce(state: State, action: Action): State
}

class "Heart Rate:: BeatsPerMinute" <<selector>>{
 +execute(): state
}

class "Heart Rate:: EcgOverTime" <<selector>>{
 +execute(): state
}

'=========== links
"Heart Rate:: HeartRateView" ..> "Heart Rate:: BeatsPerMinute"
"Heart Rate:: HeartRateView" ..> "Heart Rate:: EcgOverTime"

"Heart Rate:: Photoplethysmography (Heart Rate)" ..> "Heart Rate:: HeartRateReading"
"Heart Rate:: BeatsPerMinuteReducer" ..> "Heart Rate:: HeartRateReading"


"Heart Rate:: Electrocardiogram (ECG)" ..> "Heart Rate:: EcgReading"
"Heart Rate:: EcgReducer" ..> "Heart Rate:: EcgReading"

"Heart Rate:: Photoplethysmography (Heart Rate)" .u.> "Core Architecture:: Store"
"Heart Rate:: Electrocardiogram (ECG)" .u.> "Core Architecture:: Store"
"Heart Rate:: BeatsPerMinuteReducer" .u.> "Core Architecture:: Store"
"Heart Rate:: EcgReducer" .u.> "Core Architecture:: Store"
"Heart Rate:: HeartRateView" .u.> "Core Architecture:: Store"

@enduml
```
