![](http://www.plantuml.com/plantuml/png/VLFHZjem47ptLrYeKeEkfHAFKOgEWugUL9KJgThdlMH3B8mTsHkuwCI_ruxfuL1Qlu1cnxFhsScz9J3KxYLZ94WYVv1QbtpzqcrHyHoeDK12AvxapNewPipkX6h0m9xR6afz56hBAv0MeueK2gbha6j3jOuAXGHh-IlZxZn0iTiQtQfoeQKsMMQmR0iqind_gXLsIppUva7X1tAqNpK2tUPOL5PxgHzOrCfXs-usyI3mw6XhKzIYl4t1giA276cbPVclTj9H-eRVu82gHCDEZBt_V3cyn4eeuKUtZB76IS0M-MYX3VAvRqzEeZKu2lwSlHhbf3sMPPKp48_Qx5mJpmZdBiiM1e7GeJqrETjeKvuZPPkpgUEt6nyjEVgxTFriqHpWMMAManUuC8B3Ti2P1-p2kYk5RO2AEWdqb0VAE3reKLvemPlaZqL-hF1ldFQJzCmJEqMU-AWDRNXgfKMnHp8TZxOR-Q3LetCtYRi7U8cMZExYoeb5qdcZfnk4qWSv2bSaxQESZlAdzVJJvijy-VXzDOfS08BqrGI1b5TI0vrkT_dfiiP3v8ODJ9-O_qyOziZZpoHI263I6-eSzZyV-FLU0lwt_Wgf7mzaYpmESzG0Nx1e2HEdCjpQLSgbK3lBsDjp9vFPG9-nU9Mk6llFufTUruONsyavx_6T8HmcY35sxunkz_8t)

# Blood Oxygen Saturation (SpO2)
Peripheral capillary oxygen saturation or as its more commonly known SpO2 is the percentage of oxygenated haemoglobin (the protein that carries oxygen in the blood) versus the the total amount of haemoglobin in the blood. The pulse oximeter (the sensor to measure Sp02) transmits multiple light wavelenghts, some of these will be absorbed based on the users blood oxygen levels, our sensor reads these leftover light waves. A value under 90% indicates hypoxia.

There are limitations to this method. If a user is too cold their vessels may be restricted. Obesity, low blood pressure and low sampling rates of the device can also cause issues with readings.

There may be a possibility to add more derived readings. [1] The pulse oximeter can be used as a more general photoplethysmogram (PPG) device. When used as such we can read:
- heart rate and cardiac cycle.
- respiration. 
- bood pressure.
- hypo- and hypervolemia, which may be an early warning sign for congestive heart failure, kidney failure, and liver failure.

# Bibliography
[1]Liverpool Hospital ICU, ‘LiverpoolSpO2 Monitoring | Hemoglobin | Physiology’, Scribd. https://www.scribd.com/document/399193213/LiverpoolSpO2-Monitoring (accessed Dec. 11, 2020).


# PlantUML

```plantuml 
@startuml

title Blood Oxygen Saturation (SpO2)

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


package "Core Architecture" {
    class "Store" <<framework>> {
        +<<Create>> Store(reducers: Set<Reducer>)
        -state$:Observable<State>
        -actions$:Observable<Actions>
        +dispatch(action: Action):void
        +select(selector: Selector):state
    }
}

package "SpO2" {
    class Pulseimetry <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class Spo2Reading <<action>> {
        +type:"SPO2/READING"
        +data:{reading: float}
    }

    class Spo2View <<view>> {
        +render():void
    }

    class Spo2Reducer <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetSpo2Value <<selector>>{
        +execute(): state
    }
}


'=========== links

Spo2View ..> GetSpo2Value

Pulseimetry ..> Spo2Reading
Spo2Reducer ..> Spo2Reading

Pulseimetry .u.> Store
Spo2Reducer .u.> Store
Spo2View .u.> Store

@enduml
```
