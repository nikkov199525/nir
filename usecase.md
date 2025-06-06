@startuml
title Диаграмма вариантов использования: Сетевая доступная игра "Монополия"

actor "Пользователь" as User
actor "Скринридер" as SR

rectangle "Приложение \"Монополия\"" {

    User -- (Запуск приложения)
    (Запуск приложения) --> (Главное меню)
    SR -- (Главное меню) : озвучивает

    (Главное меню) --> (Регистрация)
    SR -- (Регистрация) : озвучивает
    (Регистрация) --> (Ввод логина и пароля)
    (Регистрация) --> (Подтверждение регистрации)

    (Главное меню) --> (Авторизация)
    SR -- (Авторизация) : озвучивает
    (Авторизация) --> (Ввод логина и пароля)
    (Авторизация) --> (Подтверждение входа)

    (Главное меню) --> (Восстановление пароля)
    SR -- (Восстановление пароля) : озвучивает
    (Восстановление пароля) --> (Ввод почты)
    (Восстановление пароля) --> (Сообщение об отправке)

    (Главное меню) --> (Создание игровой комнаты)
    SR -- (Создание игровой комнаты) : озвучивает
    (Создание игровой комнаты) --> (Указание числа игроков)
    (Создание игровой комнаты) --> (Генерация кода комнаты)

    (Главное меню) --> (Присоединение к комнате)
    SR -- (Присоединение к комнате) : озвучивает
    (Присоединение к комнате) --> (Ввод кода комнаты)
    (Присоединение к комнате) --> (Подключение)

    (Главное меню) --> (Выход из приложения)

    User --> (Игровой процесс)
    SR -- (Игровой процесс) : озвучивает

    (Игровой процесс) --> (Бросить кубики)
    (Игровой процесс) --> (Перемещение по полю)
    (Игровой процесс) --> (Покупка собственности)
    (Игровой процесс) --> (Уплата аренды)
    (Игровой процесс) --> (Получение дохода за \"Старт\")
    (Игровой процесс) --> (Обработка \"Шанс\" и \"Казна\")
    (Игровой процесс) --> (Банкротство)
    (Игровой процесс) --> (Завершение игры)

    (Игровой процесс) --> (Управление с клавиатуры)
    (Игровой процесс) --> (Масштабирование интерфейса)
}

@enduml
