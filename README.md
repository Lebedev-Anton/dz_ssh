# Практика: SSH

Выполнены первые четыре кейса из [задания](https://github.com/CanUFeelMyHeart/urfy_seminars/blob/main/%D0%9F%D1%80%D0%B0%D0%BA%D1%82%D0%B8%D0%BA%D0%B0/04.practice_ssh.pdf). Для практики использован локальный Ubuntu-контейнер с адресом `127.0.0.1` и SSH-портом `2222`.

## 1. Установка и запуск SSH-сервера

Проверены пакет и состояние сервера:

```bash
dpkg-query -W openssh-server
service ssh status
```

Результат:

```text
openssh-server  1:9.6p1-3ubuntu13.20
* sshd is running
```

## 2. Подключение к серверу

```bash
ssh -p 2222 student@127.0.0.1
```

Проверка на сервере:

```text
$ whoami
student
```

Подключение выполнено успешно.

## 3. Копирование файлов с помощью SCP

Передача файла на сервер:

```bash
printf 'SSH/SCP practice file\n' > scp_source.txt
scp -P 2222 scp_source.txt student@127.0.0.1:/home/student/scp_source.txt
```

Копирование файла обратно:

```bash
scp -P 2222 student@127.0.0.1:/home/student/scp_source.txt scp_received.txt
cmp scp_source.txt scp_received.txt
```

`cmp` завершился без ошибок: исходный и полученный файлы совпадают.

## 4. Доступ по SSH-ключу

Создание ключа:

```bash
ssh-keygen -t ed25519 -C "ssh-practice"
```

Добавление публичного ключа на сервер:

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 student@127.0.0.1
```

Проверка входа без пароля:

```bash
ssh -i ~/.ssh/id_ed25519 -p 2222 -o BatchMode=yes student@127.0.0.1
```

Результат: подключение по ключу выполнено успешно без запроса пароля.

> Приватные SSH-ключи в репозиторий не добавляются.
