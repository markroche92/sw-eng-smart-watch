![](http://www.plantuml.com/plantuml/png/TP5VIyCm5CNVyoakUo-uplrv41aMSGyEo-hSyxMxDi4qAScjCiJlRbigPYE6F9n3VkHoxbbxHiTjhONGzCR05fog9CDHEIfPMIC4bcmTx3qv8Heitx4Yc1GrEeO3SYbXGwXPJW0zo472bu3kj9vAz1tyekWJ2gO6CjiQ7iXzTXM1xhC7s95lDVkHcaQe3VN3TyXq0QUnkSrJUe7DnFS_KOgJPwe7p0yo6kLPrJH-THrvpgrN_UgWkrjiHd8U8U-GcTm97kc3zCWjphaSbOE3OWbR-weqGIwjeP5TdhEPH5CH5CIiJFMRTjMdxUjBajj-xO6U1ZeDz4o8BXhS9CdGUsVceLc4PXcQrPyHQpB7jjO_)

# Use Case Diagram

# PlantUML
```plantuml
@startuml

left to right direction

actor User

package "Smart Watch Health" as health {
    usecase "View Step Count" as UC0
    usecase "View ECG Results" as UC1
    usecase "View Heart Rate Results" as UC2
    usecase "View SpO2 Results" as UC3
}

package "Smart Watch System" as system {
    usecase "View Time" as UC4
    usecase "View Position on Map" as UC5
    usecase "View Battery Level" as UC6
    usecase "Pair With Mobile Phone" as UC7
    usecase "Change Settings" as UC8
}

User--> UC0
User--> UC1
User--> UC2
User--> UC3

UC4 <-- User
UC5 <-- User
UC6 <-- User
UC7 <-- User
UC8 <-- User

@enduml
```
