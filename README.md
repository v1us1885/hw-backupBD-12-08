# Домашнее задание к занятию «Резервное копирование баз данных»-Филатов А. В.

**Домашнее задание выполните в Google Docs или в md-файле в вашем репозитории GitHub.** 

Для оформления вашего решения в GitHub можете воспользоваться [шаблоном](https://github.com/netology-code/sys-pattern-homework).

Название файла должно содержать номер лекции и фамилию студента. Пример названия: «12.8. Резервное копирование баз данных — Александр Александров».

Перед тем как выслать ссылку, убедитесь, что её содержимое не является приватным, то есть открыто на просмотр всем, у кого есть ссылка. Если необходимо прикрепить дополнительные ссылки, просто добавьте их в свой Google Docs.

Любые вопросы по решению задач задавайте в чате учебной группы.

---

### Задание 1. Резервное копирование

### Кейс
Финансовая компания решила увеличить надёжность работы баз данных и их резервного копирования. 

Необходимо описать, какие варианты резервного копирования подходят в случаях: 

1.1. Необходимо восстанавливать данные в полном объёме за предыдущий день.

1.2. Необходимо восстанавливать данные за час до предполагаемой поломки.

1.3.* Возможен ли кейс, когда при поломке базы происходило моментальное переключение на работающую или починенную базу данных.

*Приведите ответ в свободной форме.*

### Решение 1

1.1. Восстановление данных за предыдущий день:   
 - Оптимальный способ: Ежедневные полные бэкапы базы данных в конце каждого дня. Восстановление производится путем применения последнего полного бэкапа.

1.2. Восстановление данных за час до предполагаемой поломки:   
 - Оптимальный способ: Использование транзакционных логов (WAL) для PostgreSQL или binlog для MySQL. Регулярное создание точек сохранения (snapshots) и архивирование транзакционных логов. Восстановление производится с использованием последнего бэкапа и применением транзакционных логов.

1.3. Моментальное переключение на работающую базу или починенную:   
 - Возможные решения:    
  - Использование высокодоступных кластеров с механизмом автоматического переключения (failover) для быстрого переключения на реплику или резервную базу.
  - Использование технологий, таких как PostgreSQL Streaming Replication, MySQL Replication, где реплика может служить в качестве мгновенно доступной базы данных в случае отказа основной.

---

### Задание 2. PostgreSQL

2.1. С помощью официальной документации приведите пример команды резервирования данных и восстановления БД (pgdump/pgrestore).

2.1.* Возможно ли автоматизировать этот процесс? Если да, то как?

*Приведите ответ в свободной форме.*

### Решение 2

2.1. Пример команды резервирования и восстановления:   
- Резервирование (pg_dump):   
```
  pg_dump -U username -h localhost -d dbname -f backup.sql
```
- Восстановление (pg_restore):   
```
pg_restore -U username -h localhost -d dbname backup.sql
```
2.1. Автоматизация процесса:

Да, процесс автоматизации возможен:   
1. Создание сценариев (скриптов) для запуска pg_dump/pg_restore по расписанию.
2. Использование инструментов управления заданиями, таких как cron в Linux.
3. Реализация систем управления бэкапами, таких как Barman.
---

### Задание 3. MySQL

3.1. С помощью официальной документации приведите пример команды инкрементного резервного копирования базы данных MySQL. 

3.1.* В каких случаях использование реплики будет давать преимущество по сравнению с обычным резервным копированием?

*Приведите ответ в свободной форме.*

### Решение 3
3.1. Пример команды инкрементного резервного копирования:

- Инкрементное резервное копирование с использованием mysqldump:   
  ```
  mysqldump --single-transaction -u username -p dbname > backup.sql
  ```
Инкрементные изменения могут быть сохранены с использованием опции --single-transaction для InnoDB.
3.1. Преимущества репликации по сравнению с обычным бэкапированием:   

Преимущества репликации:   
1. Высокая доступность: Реплика предоставляет работающую копию данных, готовую к использованию в случае отказа основной базы данных.
2. Низкая задержка: Восстановление на реплике происходит быстрее, чем восстановление из бэкапа.
3. Масштабируемость чтения: Репликация позволяет распределять нагрузку чтения между основной и репликами.
---

Задания, помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.