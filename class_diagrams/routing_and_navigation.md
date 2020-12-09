![](http://www.plantuml.com/plantuml/png/jLLTRzem57tthx3GjD5rfDfhbA52MZ4q1XNGxc6Q9ZU-40lNHhO3Grt_-vp4RHmIghuiBqI-9-TSd_l1fJP4cUo18wGtJ6o98WzOfuJAFHDhl29SWnVXJ80vR05BPLBf1H9EjCQF2DjdI9BDMibCq5XogQ980SqIKFq-_Ye57AvWtuuZYM5IMDYF58XfnsWGMkPKIqXIOS_MXtRWZi7UmcOgIHbjXy1g1Ocne97az9GSjv12S49s82Wez8IGOOO3dilCv2uHGV6KxDYQvFaZzE5BzM0AAoPO7j1eQ-d96d0dbWhm84_DMFfCGQVqplcuC7ayYbRMMjXBjR7oDkwUyoYA5H03zhH0Tft1EiGBCD7SsNtsyaLF6mj_5yxkDQWTkUSGBKnEK25AxtKDCoZFAjGvPNfBJ99sItY8IyXPk9ECLh3IjUwpUJOjztOM5faKo2VatJYMmYZ9EQZ6jJqi4mRKYYJmGeZz4laUmHz8CWDTNx71SiTWZrshrBvMHQMxthsURDch-ZrN_hrNzte9XvanKWomrx4UlnHZmOpFxuwlG1ib3tNb1ktm_z36d2KReDYRH8yyd_kmCxnTBcVJYtWoZhzTNtMyrY26X5R5sdAAVoB5-aQ1rczHcCocezyty-l5ugqYUL50LGLvLMK-krrUpo-cWxlnQB0SpwR7AezvLuIuBjPGAyReEx63B01x6-NnpPFLpkjc67lBqxz3UTWjMZl4HHAVyD5CklEcJoCmSQOK25DerKQeTOAmFqAMgkNgGT1l-dyAME-sLr5Upo24t52raRZ1Q8OGEYRg_KmPfI1-PRrswA1TlodX9rUlMviV27bRhlEvWmDbKVR5xVlZyCTUp_u6WUWKY-euR2LS-6KReXntCeZGSUM3BFZRzm3kVoS8MhhR1NDoMILMkxNt_REHZQCXHaMMLHLupzl-w184pHxuFm00)

# Routing & Navigation

Routing and navigation are how the user moves through the application. The primary modes of movement are through the use of buttons (named A and B). Two different presses can be used, a standard click and release, and a long press. These clicks are interpreted by the RouterStateMachine effect and converted into RouterNavigation messages where appropriate. Not all clicks will result in navigation. In this case, we chose to model it as an effect, but it would also be possible to use a reducer. An effect gives us the ability to call async actions during the navigation phase, giving us more flexibility in the final solution.

The Controller listens to the Store for changes. It then uses the GetCurrentRoute selector to query the Store for the piece of the state it cares about. If the value has changed, then it loads the appropriate View class and calls render. If not, it does nothing.

This pattern follows similar mechanics used in web development in the NGRX libraries modified for the current context. It ignores many of the lifecycle hooks that are needed to handle permissions and auth in web applications.  [1]

# Bibliography
[1]NGRX Team, ‘NgRx - @ngrx/router-store’, NGRX Documentation: @ngrx/router-store. https://ngrx.io/guide/router-store (accessed Dec. 07, 2020).

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

title Routing and Navigation

'=========== definitions
package "Core Architecture" {
    class Store <<framework>> {
        +<<Create>> Store(reducers: Set<Reducer>)
        -state$:Observable<State>
        -actions$:Observable<Actions>
        +dispatch(action: Action):void
        +select(selector: Selector):state
    }

    class Controller <<framework>> {
    }

    interface Selector <<selector>> {
        +execute():state
    }

    interface View <<view>> {
        +render():void
    }
}

package "Routing & Navigation" {
    class ButtonA <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class ButtonB <<sensor>> {
        +onInit():void
        +onDestroy():void
    }

    class ButtonClicked <<action>> {
        +type:"BUTTON/CLICKED"
        +data:string
    }

    class ButtonLongPress <<action>> {
        +type:"BUTTON/LONG_PRESS"
        +data:string
    }

    class RouterNavigation <<action>> {
        +type:"ROUTER/NAVIGATION"
        +data:{view: string
    }


    class RouterStateMachine <<effect>> {
        -actions$:Observable<Actions>
    }


    class RouterReducer <<reducer>> {
        +reduce(state: State, action: Action): State
    }

    class GetCurrentRoute <<selector>>{
        +execute(): state
    }
}

RouterStateMachine ..> RouterNavigation 
RouterStateMachine ..> ButtonClicked 
RouterStateMachine ..> ButtonLongPress 


ButtonA ..> ButtonClicked 
ButtonB ..> ButtonClicked

ButtonLongPress -[hidden]u- ButtonClicked

ButtonA ..> ButtonLongPress 
ButtonB ..> ButtonLongPress 

RouterReducer ..> RouterNavigation

Controller "1" .r.> "1" Store 
Controller "1" *-- "1..n" View
Controller "1" ..> "1..n" Selector
View "1" ..> "1..n" Selector


GetCurrentRoute .u.|> Selector

Store <.. RouterStateMachine
Store "1" o-- "1..n" RouterReducer
Store <.. ButtonA 
Store <.. ButtonB 

View -[hidden]l- Controller
@enduml
```
