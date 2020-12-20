![](http://www.plantuml.com/plantuml/png/hPHHRzem4CVV_IaiD6bKtQZKnoYhoYY2IbrR0MeV9pSvY8MnaNs1eOhlFZjcvRe4XAhc2TtzkF_bdxkxTYWibWl564fKm8SW51QyLq0sv_S6vLHc0gNHZBcvr4jXnOAxGkHcBVMCJuLoG39AQi3D4hYnM1YIo9HmZhyoxf-V8flFh2brtZFAs3Ira9SPsAihFZGQDZUmRkP45Zhns7C10fiP1zgPK6e2MQ5zRBPf1bSIrXvxi5aXysO4fbF8q4C3e_93SieZbU2TM87EmR8jOrz_x1-UmrHg6LftREaBY1dmLizOuDqWZhv0QQ4LtTavrHgZ2R4qdNhxO6tit4j48ZpdQTgp811yj4BREnDTmiU0wMXdwTdRFxuxzFYNvE75WLs95mNf64E1FH7zTU-ORepjgVDSkgN0h6X7FE4HEKjMHkPxB3hJ_cTGQ2l-EakgJYfowytQUt9m-88tn9p7mg1PAi1YunPcPiMos73owObDHjzgYMtQN0pVW4DhDZIpVQTHAWTo0MW_lNG_-61D9cmkG_UBpnRmFbeS2OGHY3oiALaTKZyiQj8QzhkZoUzHTzA_6FMxDxVtWnRvrW95ycfZcOHFbH6uhGkEb-Ro5AtnuyFbnrNwsUmKaNvly76DM-TAY7FuBAm-mRoxoV3YkJkwRv1pQ7s1kiWleoKQ-oHHA1UMK3gKcUFauhoUk9Mrsh7tQeNe1Zr9MFEteqVUmrOtw-0ezKhh2OnLGwkbu-HWqqensAwME-61XM_y_niHutMT0I3fV2T4poiHWZ-GbGY-PqukIUtiAgddZh76Oz7fNDMsXD6Lzm2TQbRVMK-GYMJiq4Vjb0rQHsaYV1ma-iUv-bGprlHbEmVOuztw33JQJFBKN1gk-KgJZJtI_B5Xz-_IE5N7_qIANuFEouNw2m00)

# Health Check Notification

Much of the health check data can be shared from the SpO2 section.

# PlantUML

```plantuml
@startuml
title Health Check Notification

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

package "Health Check Notification" {

    class Photoplethysmography <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class Pulseimetry <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class Electrocardiogram <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class HeartRateReading <<action>> {
        +type:"HEART_RATE/READING"
        +data:{reading: float}
    }

    class Spo2Reading <<action>> {
        +type:"SPO2/READING"
        +data:{reading: float}
    }

    class EcgReading <<action>> {
        +type:"ECG/READING"
        +data:{reading: float}
    }

    class IssueHealthWarning <<action>> {
        +type:"HEALTH/WARNING"
        +data:string
    }

    class MonitorVitalsStatistics <<effect>> {
        -actions$:Observable<Actions>
    }

    class HealthCheckNotificationView <<view>> {
        +render():void
    }

    class CurrentHealthStatus <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetHealthCheckStatus <<selector>>{
        +execute(): state
    }
}

'=========== links

Photoplethysmography ..> HeartRateReading
Pulseimetry ..> Spo2Reading
Electrocardiogram ..> EcgReading

MonitorVitalsStatistics .u.> HeartRateReading
MonitorVitalsStatistics .u.> Spo2Reading
MonitorVitalsStatistics .u.> EcgReading
MonitorVitalsStatistics .u.> IssueHealthWarning

CurrentHealthStatus ..> IssueHealthWarning

MonitorVitalsStatistics .u.> Store
Photoplethysmography .u.> Store
Pulseimetry .u.> Store
Electrocardiogram .u.> Store

CurrentHealthStatus .u.> Store

HealthCheckNotificationView ..> GetHealthCheckStatus
HealthCheckNotificationView .u.> Store
@enduml
```
