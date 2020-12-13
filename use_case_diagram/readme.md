![](http://www.plantuml.com/plantuml/png/RPBVQzim4CVVzLSSVK-ftQq_nXYbPcmFwnQQPNs-EPT8H9QCxhmkZFtlKnJ152C8I9_Bnpv_kNjWmI3JO9Moj1KG3y7ijC361Eh5UATuRzo80GTWZHl_QjmEjcYPbC9UV80rKr1gn7wFEuABrO11u0Mbr_2Pq8g-06JGwGf_5030nDGZH_c7eLTO2OtG-Sb9CjVTLKNws7s2P-B92cUhgLYnOH1uHg6PtDRwZj_QcNPzhfs-7pfD_Hw-UZ5RqwCOxx9-h_xMayFgm493qZXTgyc_cu7ogzvK_bvwDiTk47zFE6RpRSLyH14A1_X2lyXcx-RSMw89y694mvF_QIp1Kdj7sRqz2z9fTF5SaKYSDfzIP9ZdgBdhxgugFTg9n7lHaFLiDrTrFQsY8-QvtrJzSbhp9zfZmEEcmcBn8QrO0Kq9RGdN9Tmh5US4xnUBqyx7KPHmKyBjVuXq5od1wNvvcKB3Ew3VGcx3PRcfbzkYh1xYCGts7m00)

# Use Case Diagram

# PlantUML
```plantuml
@startuml

left to right direction
skinparam shadowing false

actor User

package "Smart Watch Health" as health {
    usecase "View Step Count" as UC0
    usecase "View ECG Results" as UC1
    usecase "View Heart Rate Results" as UC2
    usecase "View SpO2 Results" as UC3
    usecase "Health Check Notification" as UC9
}

package "Smart Watch System" as system {
    usecase "View Time" as UC4
    usecase "View Position on Map" as UC5
    usecase "View Battery Level" as UC6
    usecase "Pair With Mobile Phone" as UC7
}

package "Change Settings" as settings {
    usecase "Change Settings" as UC8
    usecase "Change Bluetooth Setting" as UC10
    usecase "Change Brightness Setting" as UC11
    usecase "Change Health Check Setting" as UC12
}

User --> UC0
User --> UC1
User --> UC2
User --> UC3
User --> UC9

UC4 <-- User
UC5 <-- User
UC6 <-- User
UC7 <-- User

UC8 <- User
UC8 <.. UC10 : extends
UC8 <.. UC11 : extends
UC8 <.. UC12 : extends

@enduml
```
