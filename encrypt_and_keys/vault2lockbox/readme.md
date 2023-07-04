Скрипт предназначен для миграции секретов из HashiCorp Vault в сервис Yandex Cloud Lockbox.

### Текущие ограничения.
 - Секреты должны находиться в KV Version 2 Secrets Engine.

_Для работы скрипта нужны конфигурационные параметры, которые могут быть переданы скрипту либо через переменные среды, либо через файл с именем .env, размещенный в том же каталоге, что и скрипт._

### Необходимые конфигурационные параметры.

| Параметр         | Значение по умолчанию | Описание                                                         | Пример значения                        |
|------------------|:----------------------|:-----------------------------------------------------------------|----------------------------------------|
| VAULT_TOKEN      |                       | Токен с правами доступа к значением секретов в Vault             | "00000000-0000-0000-0000-000000000000" |
| VAULT_URL        |                       | Адрес сервера Vault                                              | "https://localhost:8201"               |
| VAULT_ROOT_PATH  |                       | Корневой путь в хранилище секретов                               | "secret"                               |
| VAULT_KV_VERSION | 2                     | Версия KV хранилища                                              | 2                                      |
| VAULT_VERIFY_SSL | False                 | Отключить проверку сертификата при запросе API Vault             | False                                  |
| YC_TOKEN         |                       | Токен Yandex Cloud с правами создания секретов в сервисе Lockbox | "t1.9euxxx"                            |
| YANDEX_FOLDER_ID |                       | Имя папки в Yandex Cloud, где будут создаваться секреты          | "f9sdf9e"                              |
| OUT_FILE         | "secrets.json"        | Имя файла для выгрузки секретов из Vault                         | "secrets.json"                          |
| INPUT_FILE       | "secrets.json"        | Имя файла для загрузки секретов в Lockbox                        | "secrets.json"                                     |

### Параметры коммандной строки.

| Параметр                       | Описание                                                                                                                      |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| -h или --help                  | Вызов справки                                                                                                                 |
| -l или --list                  | Вывод секретов Vault на экран                                                                                                 |
| -o или --outFile [filename]    | Вывод секретов Vault в файл (если не указывать имя файла, то его имя будет загружено из переменной среды OUT_FILE)            |
| -m или --migrate               | Перенос всех секретов из Vault в Lockbox                                                                                      |
| -c или --createFrom [filename] | Создание секретов в Lockbox из файла (если не указывать имя файла, то его имя будет загружено из переменной среды INPUT_FILE) |
| -d или --deleteAll             | Удаление всех секретов в Lockbox                                                                                              |

### Сценарии.
 - Просмотр текущих секретов в консоли (для проверки успешности подключения к Vault).
 - Выгрузка секретов в файл и их анализ с возможностью редактирования списка секретов.
 - Загрузка секретов из файла в Lockbox и просмотр получившегося результата. Можно отредактировать этот файл для точной настройки того, что будет импортировано в Lockbox.
 - Удаление всех секретов из Lockbox в случае неудовлетворительного результата импорта из файла или работы миграции

### Пререквизиты.
- Установить [Python](https://www.python.org/downloads/)
- Выполнить загрузку необходимых модулей `pip install -r requirements.txt`
- Создать и заполнить файл __.env__
- Запустить скрипт в консоли `python vault_to_lockbox_migrator.py`