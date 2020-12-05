![](http://www.plantuml.com/plantuml/png/hLLHQzim47xNhpZCO0wTsRlnIjCqj8LjBMdO7iOOgdM9HMp9IETaeVI_Jx9IqfN4ThVcoT9zztswJtVAcILkgAbAHhe6qd2lYNG5GXhCIMh5c4K2nIkqDS_n3cjkE6a3QShiIYg_hC0MNEYDL4jOyDBYiqXUSclXWO7xdVDyjJIwKMAYIssopA1eSZIdft2b5MulS7COnzkZEDZt0ZaTnbXKLdkgEUQ5SdlBxM7WMkB6mMvCNaXn68AB1URaG9Uw5CVaIWTf1J_pDIg1XZqozlxJ_WS25r99VtJBWW_HH1k4iPScbzuOJ5EuSpGOGPOjd66uqMRbI9rZ9rasCSW9tRB5n3k_hCj1ocOxzqOC4aiEzowzkRTerlo-nEoEVAOB1VFiNy5ns7FX4o5jpIalue1B8SH6wLfBuUAXqFYfNYyTlaPfA-ggVYhk2haXc77oLKqzokYS6o7rqhT2F9rSZYBONrLRf5RNIb8SzDhr1LgNkDrj7UE_BJJfkaGgjhRo0dMnXNWFUJkbQRwS8HUkkoFOzs9BITiQqyZLzN4s7LzSVxsClAESUFfWGaeAYr9pUZn6tYxyTxV4rNGyc_-QZUVJ_rVw9d4JmMuGWeXf6xZ7XtFaP6_HV96g8TnrNmJxCGvSOHLRybdGjk47UDbSOR_JyJUbxoWaWkVJx2NmD-Ok43iUstFCcpMQkQm6ahnu7qgfLfPrtrAIT1--DxtZp7svJTBrO37s-e5CkXd_ZR1xdjYm9Xv2rFLu9o8PDYwCiO4FPRzRmscEFMYiO_0wyuQRyPAOzRNTZq8AWUgd6UJem0lUqvR9g_z9s1agqLJb7m00)

# Heart Rate and Electrocardiogram (ECG)

The Electrocardiogram/Photoplethysmography class encapsulates the functionality for the sensor. Once it has collected it's readings it dispatches a EcgReading/HeartRateReading event to the store. These readings are then passed through the BeatsPerMinuteReducer/EcgReducer to build the state. The HeartRateView queries the Store for the State data and renders the data each time the store updates.



# PlantUML

```plantuml
@startuml
top to bottom direction

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
"Heart Rate:: HeartRateView" <-- "Heart Rate:: BeatsPerMinute"
"Heart Rate:: HeartRateView" <-- "Heart Rate:: EcgOverTime"
"Heart Rate:: HeartRateView" <-u- "Core Architecture:: Store"

"Heart Rate:: Photoplethysmography (Heart Rate)" <-- "Core Architecture:: Store"
"Heart Rate:: Photoplethysmography (Heart Rate)" <-- "Heart Rate:: HeartRateReading"
"Heart Rate:: BeatsPerMinuteReducer" <-- "Heart Rate:: HeartRateReading"
"Core Architecture:: Store" <-d- "Heart Rate:: BeatsPerMinuteReducer" 


"Heart Rate:: Electrocardiogram (ECG)" <-- "Core Architecture:: Store"
"Heart Rate:: Electrocardiogram (ECG)" <-- "Heart Rate:: EcgReading"
"Heart Rate:: EcgReducer" <-- "Heart Rate:: EcgReading"
"Core Architecture:: Store" <-d- "Heart Rate:: EcgReducer"

'=========== only used for formatting ===========
"Heart Rate:: HeartRateView" <-[hidden]r- "Heart Rate:: BeatsPerMinuteReducer"
"Heart Rate:: BeatsPerMinuteReducer" <-[hidden]l- "Heart Rate:: Photoplethysmography (Heart Rate)"

@enduml
```
