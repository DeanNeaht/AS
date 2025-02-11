![Screenshot 2025-02-10 at 23.53.00.png](Screenshot%202025-02-10%20at%2023.53.00.png)

Для домашнего задания взял предложенную систему.

План работы:
Распилить сервис объединяющий 3 связанных контекста.
Разобраться с энтити сервисом воркеров

### Разделение "Монолита"

#### Контекст Планирования Визитов

В связи с резким увеличением количества заказов и появлением новых требований к масштабируемости (scalability) и гибкости (agility), необходимо вынести контекст планирования визитов в отдельный сервис. Это повзволит масштабировать сервис отдельно от контекстов, к которым такие требования не предъявляются.

#### Контекст Матчинга

Контекст матчинга также следует вынести из монолита. Коты датасаентисты столкнулись с неудобствами при работе с реляционной СУБД в текущем сервисе. Для решения этой проблемы необходимо:

- **Перейти на графовую СУБД**.
- **Написать новый сервис используя архитектуру pipeline**.

### Внесение Энтити-Сервиса Воркеров в Сервис Скоринга

#### Проблема

В текущей схеме реализации энтити сервиса из-за синхронного взаимодействия с остальными сервисами при его поломке мы ломаем 4 связанных с ним сервиса. 

#### Решение

Так как мутирует данные в сервисе воркеров только сервис скоринга работников, то предлагаю объединить сервисы воркеров и сервис скоринга.
Это позволит устранить лишние неявные связи.



### Расчет Instability для Каждого Сервиса

Instability = Ce / (Ce + Ca)

##### 1. Сервис Планирования Визитов

Instability = 2 / (2 + 3) = 2 / 5 = 0.4

##### 2. Интеллектуальный матчинг сотрудников и клиентов

Instability = 0 / (0 + 1) = 0 / 1 = 0


##### 3. Контроль качества работы сотрудников

Instability = 2 / (2 + 2) = 2 / 4 = 0.5


##### 4. Скоринг работников, воркеры

Instability = 0 / (0 + 5) = 0 / 5 = 0.0

##### 5. Сборка необходимых расходников

Instability = 2 / (2 + 1) = 2 / 3 = 0.66

##### 6. Система ставок менеджеров

Instability = 2 / (2 + 0) = 2 / 2 = 1

##### 7. Расчет и списание средств у клиентов и сотрудников

Instability = 2 / (2 + 0) = 2 / 2 = 1
