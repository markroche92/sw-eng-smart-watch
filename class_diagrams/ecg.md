![](http://www.plantuml.com/plantuml/png/VLFHZjem47ptLrYeKZcrfUzHYeu2eYTLHO9J--opDuc5iP7jGD69V-ywXh9mQVq26i_kh6SdZo4A7zkjOInijDq9BxOyD4AvWxOrhuG9G6wCjX2FE-3EnyQH2sb42FoLSJnVXTpKthLMJPrnlYmzg5Q27u_vDsVXE8D3Fq_8g9r5sgy6HEpd1B31fLRF81kBM7ti9-ur792szBBHgfy2LGKo8cdXZFgNd45A9_XTxC4gyEp4sFilryCLLDhgD7fWZEsmXQY13wRE0vya-OWjMW-3ip_Pgy4wkeILPOK6mi7v3OfaHZeVodBgGKH0jACEppQ6WgyXbgkpgGz_Apw5YFHtnV8bWD-B5mFbEY7ZAoCx76uuauoDhyfAXvs8iXbcUi4pvQ7OEwskjEpDy69H6Yl_UoYwIJhcYPs89_FfujQ5UQhmJWgljAjJaiYAYHdEFg75Gpf2XcSGiFv8RqwC2iXw1KAbC9E0aSufpaKQxFDgFfaz_LWCY14YYkBLv_g2LyQ9s2ln4yF6BvcZtNqNcVzEb_V8oMT26sHms1cA3gUVZ_n-Bnb_stu14HMMU_3FUWkSnfb8m6-GRGGSaj_jxJRdHjjDOEpotD5eV2V0sDjr8eliW947tztqrRQZS_Pf7KNpA1HYZ-XukpL_0000)

# Electrocardiogram (ECG)

The Electrocardiogram class encapsulates the functionality for the sensor. Once it has collected the readings it dispatches an EcgReading event to the Store. These readings then pass through the EcgReducer to build the state. The EcgView queries the Store for the State data and renders the data each time the store updates.



# PlantUML

```plantuml
@startuml

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
        +data:{reading: float
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
