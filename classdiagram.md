@startuml
title Диаграмма классов: Сетевая доступная игра "Монополия"

class User {
  +id: int
  +login: str
  +hashed_password: str
  +registration_date: datetime
}

class GameSession {
  +id: int
  +room_code: str
  +created_by: User
  +status: str
  +start_time: datetime
  +end_time: datetime
}

class Player {
  +id: int
  +user_id: int
  +game_id: int
  +money: float
  +position: int
  +is_bankrupt: bool
}

class Property {
  +id: int
  +title: str
  +price: float
  +rent: float
  +owner_id: int
  +game_id: int
}

User "1" -- "0..*" GameSession : создаёт
User "1" -- "0..*" Player : участвует
GameSession "1" -- "2..4" Player : содержит
GameSession "1" -- "0..*" Property : включает
Property "0..1" -- "1" Player : принадлежит
@enduml
