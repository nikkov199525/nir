@startuml
title UML-диаграмма классов: Сетевая доступная игра "Монополия"

' == Классы, связанные с пользователем ==
class User {
  - id: int
  - login: str
  - hashedPassword: str
  - registrationDate: datetime
  + authenticate(login: str, password: str): bool
  + recoverPassword(): void
}

' == Игровые комнаты и сессии ==
class GameRoom {
  - id: int
  - roomCode: str
  - status: str
  - owner: User
  - players: List<Player>
  + addPlayer(player: Player): void
  + startGame(): void
}

class Player {
  - id: int
  - user: User
  - room: GameRoom
  - money: int
  - position: int
  - isBankrupt: bool
  + makeMove(): void
  + payRent(amount: int): void
  + receiveIncome(amount: int): void
}

' == Элементы игрового поля ==
class BoardTile {
  - id: int
  - index: int
  - type: str
  - name: str
  + applyEffect(player: Player): void
}

class Property {
  - id: int
  - name: str
  - price: int
  - rent: int
  - owner: Player
  - tile: BoardTile
  + purchase(player: Player): void
  + chargeRent(visitor: Player): void
}

class Card {
  - id: int
  - type: str
  - text: str
  + apply(player: Player): void
}

' == Логика и доступ к данным ==
class GameManager {
  + startSession(room: GameRoom): void
  + processMove(player: Player): void
  + checkVictory(): Player
}

class Repository {
  + saveGame(room: GameRoom): void
  + loadUser(login: str): User
  + saveUser(user: User): void
}

' == Связи между сущностями ==
User "1" -- "0..*" Player
Player "1" --> "1" GameRoom
GameRoom "1" --> "0..*" Player
Player "0..*" --> "0..*" Property
Property --> BoardTile
Player --> GameManager
GameRoom --> GameManager
GameManager --> Repository
GameManager --> BoardTile
BoardTile --> Card

@enduml
