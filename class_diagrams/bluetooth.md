![](http://www.plantuml.com/plantuml/png/hLPVJ_is57ttfx2gIIlZHz1UempHFobKYvekJU5nCiajjPhQp7RQLOZllclIDV1Jt129F8GallUSUuzz3xtLXYbJRWkYmL31jg1VM0OBU66A6QcexnEzuSA-RgbUirpkkNYcArPe-B2I5Knh-aeeNaEMRPwLB4K-aeLKGQ0WBpDGDpVqNWeuZ67V7CSomwN0iCSrCDCSeq5eQQ5IoDO2lpqVcWDt7FOO5gjipVFc45Yj83COT2UBt4LNOCWxOSHs879Gv8sGN_-eBvh3YWjkIzU4L4BqHb817LXkW_cb0j-d2yI17Ws25Ie6UwasY8gINGR1I04pWAys1lf7mJJcW0cIetmNX5vfWt6_-F6J1hLZJmK42sCpSQbIJtzQ75JVSFaovxYp9bltgpYVLciN_avQVIwhJll_DsofgwSBtv82UJirDonAC5AQDJOrB8sHOe1jrLjZUv9YYfhqJ_1IZ44R9G_7JsVXXjyBDofuje7SWbRD7q7DuGNytd2Pfl7iUXHDHt-6uvwLYXdc8peUT8iBejn-XftBFIWhFFGiq6GPHJy8ZU97lCUJINCnLLO4EoWSbSpZnp2vZiA7C3gLKLCvKCSObK36OUVkC5g6QHodzzT9E9eYVDrbUWPr0I972Lf15-5iF9tTjMBE6RUI9l1FYNltXMhdWsc2m7yduL_BS94sRewRPr2QjLHSTo0QBDFxE9akFZ0zILa04mwgq_COTZm39AfDvRLb9hzMFRF1aqBklm6xSfSPsqAygWgsU_5ETsIr9BLRMwcgjxwr0400--S7_Tb2wBa3UWUcelp0KTkgvO1_8IiDz17edDKS3yuSr9PhZSGF752MazLNvLgBhatXirCNN6pGevrJsVkzzzlL5TwTuqvQjjyhFTVGTybj6EtksVK8TqXsp6e71ESK4kBup-5vvroxAMlOdaN8LyVAozlvk-EuHUmEvLQpqsXxNUhh3kckits84QZ5DGWvzwFAysxEEtEdxFRME-0vk-w2uJuktM3giUkUPSUEt8B8ostn7m00)

# Bluetooth

The PairingAuthorisationFlow effect handles the state machine for pairing. It listens to events in sequence (PairingRequested and ButtonClicked) and return a PairingAuthorised action when its finished (note the boolean data field, a request may be rejected).
The 

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

class "Bluetooth:: ButtonA" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Bluetooth:: ButtonB" <<sensor>> {
 +onInit():void
 +onDestroy():void
}

class "Bluetooth:: ButtonClicked" <<action>> {
 +type:"BUTTON/CLICKED"
 +data:string
}

enum "Bluetooth:: PowerState" {
 FULL,
 LOW,
 OFF
}

class "Bluetooth:: PowerLevel" <<action>> {
 +type:"POWER/LEVEL"
 +data:PowerState
}

class "Bluetooth:: DataRecieved" <<action>> {
 +type:"BLUETOOTH/RECIEVED"
 +data:T
}

class "Bluetooth:: DataSending" <<action>> {
 +type:"BLUETOOTH/SENDING"
 +data:T
}

class "Bluetooth:: PairingRequested" <<action>> {
 +type:"BLUETOOTH/PAIRING_REQUESTED"
 +data:string
}

class "Bluetooth:: PairingAuthorised" <<action>> {
 +type:"BLUETOOTH/PAIRING_AUTHORISED"
 +data:boolean
}

class "Bluetooth:: BluetoothDevice" <<effect>> {
 -actions$:Observable<Actions>
}

class "Bluetooth:: PairingAuthorisationFlow" <<effect>> {
 -actions$:Observable<Actions>
}

class "Bluetooth:: StoreNameOfDeviceRequiringPairing" <<reducer>> {
 +reduce(state: State, action: Action): State
}

class "Bluetooth:: GetNameOfPairing" <<selector>>{
 +execute(): state
}

class "Bluetooth:: RequestPermissionView" <<view>> {
 +render():void
}

'=========== links

"Bluetooth:: PowerLevel" "1"*--"1" "Bluetooth:: PowerState"
"Bluetooth:: BluetoothDevice" <.u. "Bluetooth:: PowerLevel"
"Bluetooth:: BluetoothDevice" <.u. "Bluetooth:: DataRecieved"
"Bluetooth:: BluetoothDevice" <.u. "Bluetooth:: DataSending"
"Bluetooth:: BluetoothDevice" <.u. "Bluetooth:: PairingRequested"
"Bluetooth:: BluetoothDevice" <.u. "Bluetooth:: PairingAuthorised"

"Bluetooth:: ButtonA" <.. "Bluetooth:: ButtonClicked"
"Bluetooth:: ButtonB" <.. "Bluetooth:: ButtonClicked"


"Bluetooth:: PairingAuthorisationFlow" <.d. "Bluetooth:: ButtonClicked"
"Bluetooth:: PairingAuthorisationFlow" <.d. "Bluetooth:: PairingRequested"
"Bluetooth:: PairingAuthorisationFlow" <.d. "Bluetooth:: PairingAuthorised"

"Bluetooth:: StoreNameOfDeviceRequiringPairing" <.. "Bluetooth:: PairingRequested"
"Bluetooth:: StoreNameOfDeviceRequiringPairing" <.. "Bluetooth:: PairingAuthorised"

"Bluetooth:: RequestPermissionView" <.. "Bluetooth:: GetNameOfPairing"

"Core Architecture:: Store" ..> "Bluetooth:: BluetoothDevice"
"Core Architecture:: Store" ..> "Bluetooth:: StoreNameOfDeviceRequiringPairing"
"Core Architecture:: Store" ..> "Bluetooth:: RequestPermissionView"
"Core Architecture:: Store" ..> "Bluetooth:: PairingAuthorisationFlow"
"Core Architecture:: Store" ..> "Bluetooth:: ButtonA"
"Core Architecture:: Store" ..> "Bluetooth:: ButtonB"
@enduml
```
