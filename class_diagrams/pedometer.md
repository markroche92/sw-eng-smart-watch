![](http://www.plantuml.com/plantuml/png/jPLVRzem4C3VyociQD9WNQcclKKPQWkiGrhN3cZtx2ON4f7Op3x3KBNllZDskwp2K1wMXpRSdN_ttpdNo1MQfeuY3SW4Rq0lUGPJM7B5KIgM995ULCB-R5XT2S3D4fXKMCgMGfSyb-jAp5d1QmqjJLPphTbZnEYvvDbYhgGH-K3MKgMfWjnaeFfzzaKAs0nXlTkEPrX9GMO_Ik2usqQ3qDAYPf2LWcJppMx3LGLhChjHMLdbkqsWA213ChgITHvoLvF9rk5NlWAHWuf-Hz7RJtyVbaDHYSg6hgF85I8UI0Nimlf6EcyK90cR4WTYbgO5LGpMKYs8wafsagO31Ho195kpZg-PfcE0wSHNiBkrFjL8fc-IcmSDQiKVQaYdQ0yxhIkZ_aT_uMJEuYIlgFsObHrdcZ2dxYOhwMjruXBlFELFOVYtRhBrRglWSxs5N3Q0e2Z7iG255Q-lWMlAkW61TrGRrXbVttLZrkgVJroACUaxRTTI346ZaXil3Rbgp90ZZvz8FCkW1iLTHlVRO3ywbPBenTyDHr22tfsc19xJF2Uig2L79qDebfP3yNjvE8A9En-prlJwUEm69J7bVJOPNGp7twvYNtmAEl711y1JXEMCBq24kRUZuStrQ3QQd4rdeziu01g27J-B-wnaGqujy7di7m_FIu3x_7vFMsQPVjjyY6gxAFlRPuEQcgRXQcE7jz9OPThFkINw1MX3hQ2pdJvxdFwzPoy7sCa3BWUybge2VMcKWFo_-dBfFUSLi_PLuft0ByWCGeT8-jLHlvBqueggAx7GqRuMzdhz_GaahCz-6h2Ha1DxlQqhB0ypmwa4cT5HLqgl5znAovS6bmPr50LND_uGlpizfR-xxvVeq4RPD8Dm6q8cXJODwED9hOiWdCk-JJhChCaUX6Q5LD7URJcUTrmZUuR8uG-depzzkK0pM150CvHxPl2Lr3aGoDxBhA1M76ZJOUOHxJcE5UZ9OSWvYDmqzHy0)


# Pedometer
The Pedometer measures the number of steps taken by using data from the IMU. As it requires a window of data to operate on and not a single reading, we feed it into an effect class. This effect class can be used to do a simple detection of a step by finding the local maxima of the acceleration magnitude data. Only peaks with a minimum height above one standard deviation are treated as a step. [1] It may also be used with an AI algorithm for more accurate detection of steps or to detect push (and push type) if the user is in a wheelchair. [2] Based on the number of steps, we can do a simple calculation for the number of calories burned (CaloriesBurnedReducer)[3][4].

# Bibliography

- [1]‘Counting Steps by Capturing Acceleration Data from Your Mobile Device - MATLAB & Simulink’. https://www.mathworks.com/help/matlabmobile_android/ug/counting-steps-by-capturing-acceleration-data.html (accessed Dec. 04, 2020).
- [2]‘The inside story of how Apple cracked its wheelchair mode in watchOS 3’, The Indian Express, Jun. 04, 2020. https://indianexpress.com/article/technology/gadgets/apple-watchos-3-wheelchair-mode-features/ (accessed Dec. 05, 2020).
- [3]B. E. Ainsworth et al., ‘Compendium of Physical Activities: an update of activity codes and MET intensities’:, Medicine & Science in Sports & Exercise, vol. 32, no. Supplement, pp. S498–S516, Sep. 2000, doi: 10.1097/00005768-200009001-00009.
- [4]‘Walking Calorie and Distance Calculator by Pedometer Steps’. http://knightsofknee.com/calculators/walking-calories-pedometer-steps.html (accessed Dec. 05, 2020).



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

class "Pedometer:: InertialMeasurementUnit (IMU)" <<sensor>> {
    +onInit():void
    +onDestroy():void
}

class "Pedometer:: ImuData" {
    +acceleration: Vector3
    +rotation: Quaternion
    +heading: float
    +temperature: float
}

class "Pedometer:: ImuReading" <<action>> {
    +type:"IMU/READING"
    +data:ImuData
}

class "Pedometer:: StepTaken" <<action>> {
    +type:"PEDOMETER/STEP"
}

class "Pedometer:: DetectStepFromImu" <<effect>> {
    -actions$:Observable<Actions>
}

class "Pedometer:: PedometerView" <<view>> {
    +render():void
}

class "Pedometer:: StepSummaryStatisticsReducer" <<reducer>> {
    +reduce(state: State, action: Action): State
}

class "Pedometer:: CaloriesBurnedReducer" <<reducer>> {
    +reduce(state: State, action: Action): State
}

class "Pedometer:: StepStatistics" <<selector>>{
    +execute(): state
}

class "Pedometer:: CaloriesBurned" <<selector>>{
    +execute(): state
}

'=========== links
"Pedometer:: PedometerView" ..> "Pedometer:: StepStatistics" : > queries store with
"Pedometer:: PedometerView" ..> "Pedometer:: CaloriesBurned" : > queries store with

"Pedometer:: InertialMeasurementUnit (IMU)" <.. "Pedometer:: ImuReading": > outputs

"Pedometer:: ImuReading" "1"*--"1" "Pedometer:: ImuData"
"Pedometer:: DetectStepFromImu" ..> "Pedometer:: ImuReading" : > consumes
"Pedometer:: DetectStepFromImu" ..> "Pedometer:: StepTaken" : > outputs

"Pedometer:: StepSummaryStatisticsReducer" ..> "Pedometer:: StepTaken" : > listens for
"Pedometer:: CaloriesBurnedReducer" ..> "Pedometer:: StepTaken" : > listens for

"Pedometer:: InertialMeasurementUnit (IMU)" .u.> "Core Architecture:: Store" : > dispatches events to
"Pedometer:: PedometerView" .u.> "Core Architecture:: Store" : > gets data from
"Pedometer:: DetectStepFromImu" .u.> "Core Architecture:: Store"
"Pedometer:: StepSummaryStatisticsReducer" .u.> "Core Architecture:: Store"
"Pedometer:: CaloriesBurnedReducer" .u.> "Core Architecture:: Store"
@enduml
```
