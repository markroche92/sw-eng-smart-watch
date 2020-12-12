![](http://www.plantuml.com/plantuml/png/VPFHRzem4CRV_LTOQDAejdNlKOPA2u9Asre5j3rEr-H2B8mTsHTOLF6_puxPS6XOdj3TZ-yxVNU-imI6cfrap6w5gi70ZjiA2dqGQiDBa1QZZXGAgQsHQqELZXgv16lvA-FkkuTykp6wKSM3bjgagS6YoT5C9dof5ROpF0npa9FGoc4_AmGQPYmggxtK6lDAkTgc7GRt0WyEUp9v9OfX1CiIStBGGilYcfrqI6Vu5VQe2ZJioDZxB-UF5rWA9VpebjLE03R8Hm_Q89zwSt82ZS7HAPsGr6X5sjVIj7JnuK6RhRC8XFy-fEc3GI1qrGuTdqAq2LyXfTafqfj___XaoU7lagSNYsOFBnBJ5Nc1Cn7ojJrc6cgJitCXR0sKL-E09pmWDybUY-ACXMJ6VmFoOuLVDqatIKSUNLZdJ9O8XdW6r8KHfV5SQTArHAfQkzCR0tNLycZNKIvQFRgin_4qeJn3IqQtSUSOUtJctZj3AFnjH_SMwVlhJaRB-JHR_ygcw_ddR3wTFNvVZAA0W21vDK4cuQNKGF_n-o7mmCCzzgrCTqvN1xv78FkCvfjG3U5fuJn-JP5MA8wxv1FUBVuZlrnWgBzrMY3rpNXy-94B_iRSjTt8lB_ZYnSXXTfQra_WzdRoreYnmTLxzd9VR3YF8VIQQdDxUap3MZ7Gdprgi3jKHREJVm00)

# Heart Rate

The Photoplethysmography class encapsulates the functionality for the sensor. Once it has collected it is readings it dispatches an HeartRateReading event to the Store. These readings then pass through the BeatsPerMinuteReducer to build the state. The HeartRateView queries the Store for the State data and renders the data each time the store updates.

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

package "Heart Rate" {

    class Photoplethysmography <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class HeartRateReading <<action>> {
        +type:"HEART_RATE/READING"
        +data:{reading: float
    }

    class HeartRateView <<view>> {
        +render():void
    }

    class BeatsPerMinuteReducer <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetBeatsPerMinute <<selector>>{
        +execute(): state
    }
}


'=========== links
HeartRateView ..> GetBeatsPerMinute

Photoplethysmography ..> HeartRateReading
BeatsPerMinuteReducer ..> HeartRateReading

Photoplethysmography .u.> Store
BeatsPerMinuteReducer .u.> Store
HeartRateView .u.> Store

@enduml
```
