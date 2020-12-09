![](http://www.plantuml.com/plantuml/png/hLPVJ_is57ttfx2gIIlZHz5Uempz-YU5Qb7JfIayJYQv9LPJczbEkmhntNTDkb1Gt129F8GallUSUuzz3_sf3LEct1P4Ww62RK4_inIMyCmKCr9HpoDwmuLztTA22p27Pw1IcLoUBEYSPNBFnHDTiqB3oKfQCAtf2w5uZLYwULAo5DbO5bBvleAiJ47TtD0xAU0mWNrp74iDbmB37d9WfZb6Wz3IGYMGvWA_FHsQ0tSSzXWMgJJdMNC8hDUG6WowbKNceYimv8qmP3iG6IZoIiYlVxnVD8Cr5zoMhWcfXEYDfG8wjDm6yqi5daUNY0CzwljhL0psKcqG5IMxzFsn0cO0NsqCz8-2QSm1uyT7-Iu8lT86utxneaSDQiSU2_2NncRYKgMU_h0uhBxXycN6SSjDclUhE8zMQnVUJbfzBgjE-_ytRAchfmlFaW9vhPiR5IKOAKsEJOrAOwGOshRghR4zIJ53JVerl1GJq4R9m_7JMRZHzyAD2vvk8BEWLVD7K7l6lTvebIJH_7eSpiP_1fEUbOePvY4w7dIB2wBSVeHTo3qeAppqBD1q5OO_20sZ1xn7qsbpCLLM23ie79KieeSWlWw3-o2iotYdSg1EC2g6bCFEtM6u2f8eIkwkus0yG_ZtBfCpg4iG6KhG2heCvfFP_BOLSy6ubJI6VqhSkozKknZEOWJ-Emx-MWNBf75pt3p3qkHISTs1QBXAxg9ujZnXUfIo02OSLFNp17Oy1KjKcyfBoqn-hNfcWwU5t7y3TkKkSxQ5Q5qLRFVYZUx8YYIrMrTILMzzQm5W0V3F3_hPGkYv0teBfgAiEKvjrNB0lv2M1le8TCvgZWTd0MhBjKRYUmueIsNrTRdMecjJ-EJK1HSRz6ZdLFP-x_rsTOLtvxYJyzi_o0Othg7lddiwsfspJqUuGx9ZLZiWEAUGaER_70ERwdRjfgnHUnOXNnohGTdEtnt7BM9tABUQdKPxqAc-xf3kEjk7685QN8EGip-g1eFppjmjkxNrJdYEhZjYE8vBHvXwxBfdsR4ZFq5avRRu3m00)

# Bluetooth

The PairingAuthorisationFlow effect handles the state machine for pairing. It listens to events in sequence (PairingRequested and ButtonClicked) and returns a PairingAuthorised action when it is finished (note the boolean data field, a request may be rejected).

# PlantUML
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
"Bluetooth:: BluetoothDevice" .d.> "Bluetooth:: PowerLevel"
"Bluetooth:: BluetoothDevice" .d.> "Bluetooth:: DataRecieved"
"Bluetooth:: BluetoothDevice" .d.> "Bluetooth:: DataSending"
"Bluetooth:: BluetoothDevice" .d.> "Bluetooth:: PairingRequested"
"Bluetooth:: BluetoothDevice" .d.> "Bluetooth:: PairingAuthorised"

"Bluetooth:: ButtonA" ..> "Bluetooth:: ButtonClicked"
"Bluetooth:: ButtonB" ..> "Bluetooth:: ButtonClicked"


"Bluetooth:: PairingAuthorisationFlow" .u.> "Bluetooth:: ButtonClicked"
"Bluetooth:: PairingAuthorisationFlow" .u.> "Bluetooth:: PairingRequested"
"Bluetooth:: PairingAuthorisationFlow" .u.> "Bluetooth:: PairingAuthorised"

"Bluetooth:: StoreNameOfDeviceRequiringPairing" ..> "Bluetooth:: PairingRequested"
"Bluetooth:: StoreNameOfDeviceRequiringPairing" ..> "Bluetooth:: PairingAuthorised"

"Bluetooth:: RequestPermissionView" ..> "Bluetooth:: GetNameOfPairing"

"Core Architecture:: Store" <.. "Bluetooth:: BluetoothDevice"
"Core Architecture:: Store" <.. "Bluetooth:: StoreNameOfDeviceRequiringPairing"
"Core Architecture:: Store" <.. "Bluetooth:: RequestPermissionView"
"Core Architecture:: Store" <.. "Bluetooth:: PairingAuthorisationFlow"
"Core Architecture:: Store" <.. "Bluetooth:: ButtonA"
"Core Architecture:: Store" <.. "Bluetooth:: ButtonB"
@enduml
@enduml
```
