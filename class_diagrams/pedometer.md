![](http://www.plantuml.com/plantuml/png/bLHHRzem47xthx2YIODrfPhs5cMeNM4TqhfsGBldQt8X5edDx3CCLVtlit6QqOaq8m_8t7t-lhlBTpat19fiLJA3n2LKQ5QGuWnNe86Kvd7Cp598_xVYfP18snLofQbGUmbJGAOsGYvu3gN1lKnQWZ7yaN7tV8PqkT3AoknIbKedYSRCfgY7G_vLIToES7CO1oa99HtiPu50Xp46fL6UQevf8LriiJqCN0lSEDYDJWkH7OPWdcDA3dIbognDhdIGdU0tMAFCKBCdnjv--llm37CXXI_TC1O64LqgZVp2QvExRpN6CPyv7enuakHkOhXHUkbOmyXEakHI8n2wo0xMgsTct3MaP5fFiBz3dnfoq3VnpOD1lOQ74fCP-SiX6yPeNkGlGYmWJZ9XLa1fqGlGc8TqFruhaGL8QBpttByhepxruvqwUsfwlSLCLKYeNOyJYPe4bDS8ncgiKDAT5CHxa-kxViIRzzSqhkJ4vNlxqagEq91MsphQ9bNP4H14pqoGfbYYXj3H_QxOZo6b5TNX7nO8jNJda2aGCkVdcEUb0WenmchbUQp6EjvUmJJSZtZZtgOplq5nvFe-cuulHfFlLr4zV228w-9RY6U4gpaiKRRptev7DzVZ-NXwDfkFRwCMeX5w-tcwBrfLJZJYUxP_xFPB2szplXUuYNYzRKs9UhSepUjhxN5cgmhqrfjN61AfgNtk6NKuxfFwG6_dFdyT2D_pVmqSuawoNRCHY_Z-VjSg-1jJIzXpL8tFNwvxAUJIiDV6CHWCNzUD-P3_igW56cwSElADe8AnerPfC6Xre-TNbbQM36Ejbeq-HEzEJztluRrYNKxoRRQGXnPJ9Osjq1pDjBy0BRrqEAYRitHub8RdIXyt-85rpEqV-C3-_8L5mt7j20mdzRfd_eTrWOue0mAUkz5rZBMRyuXnTdETeyniLVu1)


# Pedometer
The Pedometer measures the number of steps taken by using data from the IMU. As it requires a window of data to operate on and not a single reading, we feed it into an effect class. This effect class can be used to do a simple detection of a step by finding the local maxima of the acceleration magnitude data. Only peaks with a minimum height above one standard deviation are treated as a step. [1] It may also be used with an AI algorithm for more accurate detection of steps or to detect push (and push type) if the user is in a wheelchair. [2] Based on the number of steps, we can do a simple calculation for the number of calories burned [3][4] as part of future work.

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


class "Pedometer:: StepStatistics" <<selector>>{
    +execute(): state
}


'=========== links
"Pedometer:: PedometerView" ..> "Pedometer:: StepStatistics" : > queries store with

"Pedometer:: InertialMeasurementUnit (IMU)" <.. "Pedometer:: ImuReading": > outputs

"Pedometer:: ImuReading" "1"*--"1" "Pedometer:: ImuData"
"Pedometer:: DetectStepFromImu" ..> "Pedometer:: ImuReading" : > consumes
"Pedometer:: DetectStepFromImu" ..> "Pedometer:: StepTaken" : > outputs

"Pedometer:: StepSummaryStatisticsReducer" ..> "Pedometer:: StepTaken" : > listens for

"Pedometer:: InertialMeasurementUnit (IMU)" .u.> "Core Architecture:: Store" : > dispatches events to
"Pedometer:: PedometerView" .u.> "Core Architecture:: Store" : > gets data from
"Pedometer:: DetectStepFromImu" .u.> "Core Architecture:: Store"
"Pedometer:: StepSummaryStatisticsReducer" .u.> "Core Architecture:: Store"
@enduml
```
