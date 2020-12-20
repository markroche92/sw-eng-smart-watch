![](http://www.plantuml.com/plantuml/png/hPLVRzey5CRl_Iaiz4eltGQzZpBK4X15HKKYMQjfseKR7uY5iP7jy4TLl_kSc1NN3On9YxWWvpoSn-Td7tEZ3L6cN764z9A9DL5aXNL1gDmoiS1pmZLu6Su4cFqQi5Ica5uYvqHh_8AmVVeaNowKB0LD99SgZXNGCWVLw-4xAM0_W6spZkI6IM5bJmKGqwpH8BIiIcMG5yB65ljcuOR1rigcAYyORPR0V0wviQAHvFIK7RUIsd12DY0eAFIAa6660qwup9S8_V_r-60ASoPOrOT6QrkDB02t4ga0trOhCRPQgQ3bK1smjL8ZgrWSporBs4grj7wLmZsVupXHG0pOQ2rj7uZg2ATWujc1xzNRBphQMFb_qVHPWzgGPmvnMaLwHuM3hTzfRbsiTtIcJA-9oOksaqVOIQwYZMJqA7EOshzfLSjotwwYUYMryhKYTs3YsBsZaEwrWPMBU_liOP1YRD6sGtCf1g2DadilOysyqdrUWf7I50FOi1nckso7lUr_kPHAM3VTYxnGKZ0DJymKzTfJMQeS_aLvnoXZbeSt6nwFQXAZLZAP9lVNsJYvRtdxHWo9ChO2Yt-rlXuG0zNR2G-lZmkiqkyFIMWrMclhGSwXj-dPBXxrpZhq9z-6sNIQtLtFXibu-3WSV6Zg3nxPBiukzKY73uFnm-YiXOVfC6VOkz7ywYxOhWzyXEjv-uB3kN7nZmOZC4cf50XJxHFsRnxF0dQGbmRQjeu-MQTFZ06rdy06-DyM2bj-PB35xZ8DcnKKL31nzkDFSRVREvvXv2CCKkZePHC1Yo0PD8ZGcI6q-l1meE0TTwadyYu8r133NHvMdEqkNy15yd0WcxW5YiQbTduKZ58GFylEUGOpJyi_Qb7zZuC_TJhONyaX77UxJV2yxEbsFT7R5dinxoGXT0E2bYkEVW40)

# Clock
On startup the SynchroniseWithClockSource emits a BluetoothDataTx Action to fetch the current time from the users mobile device. This is then stored and the systems clocks tick is used for updating. The SynchroniseWithClockSource can periodically emit a sync event to help compensate for minor clock drift.

One piece of future work that would improve the time acuracy and allow for untethered operation of the clock would be to use the GPS reciever as our time source and pulse per second (PPS) source. The position data (from the `$GPGGA` sentence code) could be used to lookup timezone information, the `$GPZDA` sentence code provides the UTC date and time.


# Bibliography
[1]Glenn Baddeley, ‘GPS - NMEA sentence information’, Jul. 20, 2001. http://aprs.gids.nl/nmea/ (accessed Dec. 11, 2020).
[2]NMEA, ‘National Marine Electronics Association - NMEA’, NMEA 2000® Interface Standard. https://www.nmea.org/content/STANDARDS/NMEA_2000 (accessed Dec. 11, 2020).
[3]E. S. Raymond, ‘NMEA Revealed’, NMEA Revealed, Mar. 2019. https://gpsd.gitlab.io/gpsd/NMEA.html (accessed Dec. 11, 2020).



# PLantUML
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

title Clock

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

package Clock {
    class SystemClock <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class BluetoothDeviceRxTx <<effect>> {
        -actions$:Observable<Actions>
    }

    class SynchroniseWithClockSource <<effect>> {
        -actions$:Observable<Actions>
    }

    class ClockTick <<action>> {
        +type:"CLOCK/TICK"
        +data:Timestamp/DateTime
    }

    class ClockSynchronise <<action>> {
        +type:"CLOCK/SYNC"
        +data:GpsData
    }

    class BluetoothDataRx<G> <<action>> {
        +type:"BLUETOOTH/RECIEVED"
        +data:T
    }

    class BluetoothDataTx<T> <<action>> {
        +type:"BLUETOOTH/SENDING"
        +data:T
    }

    class ClockReducer <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetCurrentTime <<selector>>{
        +execute(): state
    }

    class GetBatteryLevel <<selector>>{
        +execute(): state
    }


    class ClockView <<view>> {
        +render():void
    }

}

SystemClock ..> ClockTick
ClockReducer ..> ClockTick

ClockView ..> GetCurrentTime
ClockView ..> GetBatteryLevel

SynchroniseWithClockSource ..> BluetoothDataTx
BluetoothDataTx .d.> BluetoothDeviceRxTx
BluetoothDeviceRxTx .l.> BluetoothDataRx
SynchroniseWithClockSource ..> BluetoothDataRx
SynchroniseWithClockSource ..> ClockSynchronise
ClockReducer ..> ClockSynchronise

BluetoothDeviceRxTx -[hidden]u- BluetoothDataTx
BluetoothDataRx -[hidden]l- BluetoothDataTx

Store *-- ClockReducer
Store <.. BluetoothDeviceRxTx
Store <.. SynchroniseWithClockSource
Store <.. ClockView
Store <.. SystemClock

@enduml
```
