@startuml
title UML-диаграмма последовательности "Сетевая доступная игра Монополия"

actor Игрок
actor "Скринридер" as SR
participant "Клиентское приложение" as UI
participant "Сервер" as Server
participant "UserService"
participant "GameService"
database "База данных" as DB

== Регистрация ==
Игрок -> UI : Открывает форму регистрации
UI -> Server : POST /register (login, password)
Server -> UserService : validateAndRegister()
UserService -> DB : SELECT * FROM users WHERE login = ?
DB --> UserService : [есть/нет]
alt Логин уже занят
    Server --> UI : Ошибка
    UI -> SR : speak("Логин уже используется")
else Новый пользователь
    UserService -> UserService : hashPassword()
    UserService -> DB : INSERT INTO users(login, password)
    DB --> UserService : OK
    Server --> UI : Успешная регистрация
    UI -> SR : speak("Регистрация прошла успешно")
end

== Вход ==
Игрок -> UI : Вводит логин и пароль
UI -> Server : POST /login
Server -> UserService : validateUser()
UserService -> DB : SELECT * FROM users WHERE login = ?
DB --> UserService : данные
alt Неверные данные
    Server --> UI : 401 Unauthorized
    UI -> SR : speak("Ошибка входа")
else Успешный вход
    Server --> UI : OK
    UI -> SR : speak("Успешный вход")
end

== Восстановление пароля ==
Игрок -> UI : Открывает форму восстановления
UI -> Server : POST /restore-password (login)
Server -> UserService : checkLogin()
UserService -> DB : SELECT * FROM users WHERE login = ?
DB --> UserService : [есть/нет]
alt Логин существует
    Server --> UI : "Инструкции отправлены"
    UI -> SR : speak("Инструкции отправлены")
else Не найден
    Server --> UI : "Логин не найден"
    UI -> SR : speak("Пользователь не найден")
end

== Создание и подключение к комнате ==
Игрок -> UI : Создать/присоединиться к комнате
UI -> Server : POST /room/create or /room/join
Server -> GameService : createOrJoinRoom()
GameService -> DB : INSERT/UPDATE room
DB --> GameService : OK
Server --> UI : Подтверждение
UI -> SR : speak("Комната создана или подключение выполнено")

== Игровой ход ==
Игрок -> UI : Нажимает "Бросить кубики"
UI -> Server : POST /roll
Server -> GameService : processMove()
GameService -> DB : SELECT player_state
GameService -> GameService : расчёт перемещения, поля
alt Поле свободно
    GameService -> GameService : Предложить купить
else Поле занято
    GameService -> GameService : Снять аренду
    GameService -> DB : UPDATE player_balance
end
alt Попал на Шанс/Казну
    GameService -> GameService : Применить карточку
end
alt Прошел Старт
    GameService -> DB : +200 к балансу
end
Server --> UI : Обновление хода
UI -> SR : speak("Вы попали на ...")

== Банкротство и победа ==
GameService -> GameService : checkBankruptcy()
alt Игрок банкрот
    GameService -> DB : Обновить статус
    UI -> SR : speak("Вы банкрот. Игра окончена для вас")
end
GameService -> GameService : checkVictory()
alt Победитель найден
    Server --> UI : Игра завершена
    UI -> SR : speak("Победа игрока ...")
end

== Выход из комнаты ==
Игрок -> UI : Нажимает "Выход"
UI -> Server : POST /leave-room
Server -> GameService : сохранить состояние
GameService -> DB : UPDATE session
DB --> GameService : OK
Server --> UI : Подтверждение
UI -> SR : speak("Вы покинули комнату. Игра приостановлена")

@enduml
