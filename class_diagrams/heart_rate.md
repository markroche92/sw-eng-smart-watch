![](http://www.plantuml.com/plantuml/png/VPFHRzem4CRV_LTOQDA2kh7tA4EbWA3IjbO1RO-Jcro8XR4Z-mABAlxtsJ5R71hg9pZ_ydsdxyxt1fd6-Y08GOu2w1hiVveo14BCdikAQNQWfcIvEd6vemKJ1e8RmILWKm5L6aiLN6I26KDV2RNdWMNxdLQrpETAA9qa6l8w0psTqhMIq2pWDCon3BcI5ljL0iDXne0qoabj8IkbhUsQOV38uMIn9vsLF1z6e2WWGmkjbCXlsGcBj8RVs15a3fgS2VdujJiqXu9BxbetfB82R0TqD5SQwCoPenMeDOmkwVYaHXjKhfOaXOqFJahlhOKZ_BbBaha6Xc2hBHfTGZGntG0cwINIyVylFXkq-8VuwSM0Fh8N0Sa6dK17-7nDZvdvMaVTvTnK3BCoydXCFJAEZuhd7UQJYVu5vDholyPnsqbBdcrONIRTjhamWZIUIuMg4e1bOmvgfrbLDZIOTP2Baeysw-Yw6oKNO52h9hmvXnwjkVDEWULkjuDz2_JTTiUZzNAMRd-di-toIxgSBHv_h4P1G0nP_AgzJ4mBeHYUtp7yoU54_KBslNIxJpSxVW26vXdqTovhXCl4QVYS0YrVZDheOzfE_XEzdg2lltLQ0VRDQBZvWGlyWSnUHrQgF-Ih9o6ut1lIJs0ocRur8cHmzewz7XWPpcC8lQLQJowlQLWh1Fgz1nVt8FFw8Fu2)

# Heart Rate

The Photoplethysmography class encapsulates the functionality for the sensor. Once it has collected it is readings it dispatches an HeartRateReading event to the Store. These readings then pass through the BeatsPerMinuteReducer to build the state. The HeartRateView queries the Store for the State data and renders the data each time the store updates.

# PlantUML

```plantuml
@startuml

title Heart Rate

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
        +data:{reading: float}
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
