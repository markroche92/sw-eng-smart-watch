![](http://www.plantuml.com/plantuml/png/VLFHZjem47ptLrYeKeEkfUzHYeu2eYTLHO9J--opDuc5iP7jGD4f_zvrp1K7qlCBQ3oxivwTF3hFh6_sYX0llGAwLC2zDPnP8KrbsPvco_bgJ8ZRINrW0N0r4-OaTKLBfXmaDqfgyES3K6DzRP8BhfXpz9LGFDyOtrNMD5hCZJAsA2o8XeETJkbte-6yWDCmZt4lZKRQxngO7-OuqCw4Li_0QurOTHuc7YMSaBQsl9PYc09bYLuWQMMK-9zSiAiJ_C6Ee0LOqXBoyUlrK06br3ACxWWX1sp1Ag2ZkR50Pq7UOul6mkZYJ_HgjFKcO4LHee5mCdQ78e4HpqDHp2qm3uXsrEnYeylf5donkPWw_bVnsNcaVyZNBmxiaRqeABO1cLuPqM7NuymYTcKz2EaEpFCwY_ISHieuFnefhhJeJVPcKHWh_XldtIGTioLjuWachU_2VHgJ5ITc6Fs45cUt8nYz08Vrv_Ic9Qa0hpR0H0Xp4h2aSuXp7WRxibdE5ay_LwF42EPP_cfZVKvBPPXl1pL-OTfe35s_lUqoy-vuSP4q_KwI1X7CEaVHul3pYTuk9kBtxLVWKM5z1Fiizq3JF2SIy0Tuum67f3UBwmTTIRrpXBmzTpAPtWWGShzFP2LB8Cc3-pU3jSta4lwqBaNZA2b47j7nPg_-0W00)

# Electrocardiogram (ECG)

The Electrocardiogram class encapsulates the functionality for the sensor. Once it has collected the readings it dispatches an EcgReading event to the Store. These readings then pass through the EcgReducer to build the state. The EcgView queries the Store for the State data and renders the data each time the store updates.



# PlantUML

```plantuml
@startuml
title Electrocardiogram (ECG)

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

package "ECG" {
    class Electrocardiogram <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class EcgReading <<action>> {
        +type:"ECG/READING"
        +data:{reading: float}
    }

    class EcgView <<view>> {
        +render():void
    }

    class EcgReducer <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetEcgOverTime <<selector>>{
        +execute(): state
    }
}


'=========== links

EcgView ..> GetEcgOverTime

Electrocardiogram ..> EcgReading
EcgReducer ..> EcgReading

Electrocardiogram .u.> Store
EcgReducer .u.> Store
EcgView .u.> Store
@enduml
```
