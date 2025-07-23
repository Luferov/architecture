Python является высокоуровневым языком программирования с простой и понятной синтаксической структурой, что значительно ускоряет процесс разработки и облегчает поддержку кода. Его читаемость и лаконичность позволяют командам разработчиков быстро вводить новые функции и исправлять ошибки без потери качества. Это особенно важно при создании архитектуры сложных систем, где ясность и поддерживаемость кода имеют первостепенное значение.
## Инструментарий
В качестве пакетных менеджеров используются два основных:

1. [uv](https://docs.astral.sh/uv/) - быстрый и эффективный менеджер пакетов для Python (рекомендуемый);
2. [pip](https://pip.pypa.io/) - стандартный менеджер пакетных зависимостей, используется в случаях, когда проект является точечным, "одноразовым" и не планирется к развитию после окончания разработки.

Основной инструментарий для разработки бэкенд сервисов можно разделить на два условных раздела: для непосредственной работы приложения, а также для помощи в разработке. Прежде, чем приступать к разработке необходимо ознакомиться с основным инструментарием, который представлен ниже:

1. Основные инструменты для использования при реализации сервисов:
    - [FastAPI](https://fastapi.tiangolo.com/)
    - [Pydantic](https://docs.pydantic.dev/latest/)
    - [taskiq](https://taskiq-python.github.io)
    - [SqlAlchemy](https://www.sqlalchemy.org/)
    - [sqlmodel](https://sqlmodel.tiangolo.com)
    - [Alembic](https://alembic.sqlalchemy.org/en/latest/)
    - [Httpx](https://www.python-httpx.org/)
    - [Python-Jwt](https://pyjwt.readthedocs.io/en/stable/)
    - [Prefect](https://www.prefect.io/)
    - [FastStream](https://faststream.airt.ai/latest)
    - [uvicorn](https://www.uvicorn.org/)
    - [apscheduler](https://github.com/agronholm/apscheduler)
    - [sentry](https://docs.sentry.io/platforms/python/)
2. Вспомогательные инструменты для разработки:
    - [pytest](https://docs.pytest.org/en/stable/)
    - [pytest-coverage](https://github.com/pytest-dev/pytest-cov)
    - [ruff](https://github.com/astral-sh/ruff)
    - [mypy](https://www.mypy-lang.org/)
    - [pre-commit](https://pre-commit.com/)
    - [black](https://black.readthedocs.io/en/stable/)
    - [ipykernel](https://ipython.readthedocs.io/en/)

## Типовая структура сервисов

Ниже представлена типовая структура сервисов, а также описание дирректорий и файлов. Выход за пределы структуры позволяется только с разрешения техлида или лидера компетенций соответствующего направления, а также лидера разработки.
```
📁 {{ project name }}           // Название проекта
| 📁 .vscode                    // Конфигурации vscode
| |- tasks.json                 // Файл с командами для расширения TaskExprorer
| |- launch.json                // Файл с командами для запуска сервиса с дебаггером
| |- settings.json              // Файл конфигурации редактора vscode
| 📁 migrations                 // Файл с миграциями базы данных alembic
| | 📁 versions
| | | 2024_09_11_comment.py     // Файл с миграцией базы данных
| | __init__.py
| | env.py                      // Настройки окружения для генерации и накатывания миграций
| | script.py.mako              // Шаблон для генерации миграций
| | utils.py                    // Вспомогательные утилиты для генерации миграций
| 📁 {{ service name }}         // Название сервиса, например: portal, users, processing ...
| | 📁 apps
| | | 📁 healthcheck            // Приложение для проверки healthcheck
| | | | __init__.py             // Испорты модуля healthcheck
| | | | router.py               // Роуты приложения healthcheck
| | | | schemas.py              // DTO схемы приложения healthcheck
| | | 📁 users                  // Приложение пользователей
| | | | 📁 repositories         // Слой репозиториев приложения users
| | | | | __init__.py           // Экспорты репозитория пользователей
| | | | | users.py              // Репозиторий пользователей
| | | | | company.py            // Репозиторий компаний
| | | | 📁 services             // Сервисный слой приложения users
| | | | | __init__.py           // Экспорты сервисного слоя приложения users
| | | | | users.py              // Сервис пользователей
| | | | | company.py            // Сервис компаний
| | | | 📁 use_cases            // Слой UseCase (UserStory)
| | | | | __init__.py           // Экспорты UseCases
| | | | | user_list.py          // Получение списка пользователей
| | | | | create_user.py        // Создание пользователя
| | | | | update_user.py        // Обновление пользователя
| | | | | delete_user.py        // Удаление пользователя
| | | | 📁 schemas              // Схемы проекта
| | | | | __init__.py           // Экспорт схем приложения users
| | | | | users.py              // Схемы пользователей
| | | | | company.py            // Схемы компаний
| | | | __init__.py
| | | | container.py            // Построение IoC контейнера
| | | | enums.py                // Константы перечислений
| | | | models.py               // Модели схемы базы данных приложения users
| | | | serializers.py          // Правила и механизм сериализации объектов
| | | | router.py               // Роутер и контроллеры
| | | | exceptions.py           // Исключения, наследуемые от core/exceptions
| | | 📁 other...               // Другие приложения в сервисе
| | 📁 certs                    // Сертификаты для локальной разработки
| | | 📁 elasticsearh           // Сертификаты для elastic search
| | | | ca.pem                  // Корневой сертификт
| | | 📁 kafka                  // Серитфикаты для kafka
| | | | ca.pem                  // Корневой сертификат
| | | | cert.pem                // Сертификат пользователя
| | | | key.crt                 // Ключ
| | 📁 entrypoints              // Точки входа в приложение
| | | | __init__.py
| | | | rest.py                 // Запуска rest приложений
| | | | grpc.py                 // Запуска grpc приложений
| | | __init__.py
| | | db.py                     // Настройки подключения к базе данных
| | | depends.py                // Построение IoC контейнера
| | | enums.py                  // Список Enums
| | | exceptions.py             // Набор общих исключений
| | | loggers.py                // Настройки логгера
| | | models.py                 // Миксины моделей схем баз данных
| | | schemas.py                // Общие схемы (DTO)
| | | use_cases.py              // Общие элементы UseCases
| | 📁 tools                    // Cli утилиты
| | | 📁 core                   // Модуль общих команд
| | | | __init__.py             // Модель общих команд
| | | | cryptography.py         // Команда cryptography
| | | app.py                    // Точка входа в cli приложение
| | | utils.py                  // Утилиты cli приложения
| | __init__.py
| | bootstrap.py                // Файл сборки приложения
| | exceptions.py               // Файл с набором обработчиков исключений
| | main.py                     // Точка входа в приложение
| | middleware.py               // Добавление сервисных прослоек
| | router.py                   // root роутер приложения
| | settings.py                 // Файл с набором конфигураций приложения
| | swagger.py                  // Файл для доп настрйоки свагера
| 📁 seed                       // Пресеты данных для заполнения в СУБД
| | 001.users.json              // Заготовка пользователей
| | 002.groups.json             // Группы пользователей
| 📁 tests                      // Unit тесты сервиса
| | 📁 apps                     // Набор приложений
| | | 📁 healthcheck            // Тесты для приложения healthcheck
| | | | conftest.py             // Фикстуры, относящиеся к приложению healthcheck
| | | | test_router.py          // Тесты роута
| | | | test_repositories.py    // Тесты слоя репозиториев
| | | | test_services.py        // Тесты сервисного слоя
| | conftest.py                 // Файл, содержащий фикстуры всего проекта
| .dockerignore                 // Игнорирование при сборке докер образов
| .env.example                  // Пример файла .env
| .gitignore                    // Игнорирование git
| .isort.cfg                    // Параметры сортировки импортов
| .logging.dev.yaml             // Параметры логгирования при локальной разработке
| .logging.yaml                 // Параметры логгирования в прод окружении
| .pre-commit-config.yaml       // Конфигурация прекоммит файлов
| .python-version               // Версия python, для pyenv
| .alembic.ini                  // Конфигцрационный файл для alembic
| .docker-compose.yaml          // Файл инфраструктуры для изолированной разработки
| Dockerfile                    // Файл описания Docker контейнера
| entrypoint.sh                 // Docker entrypoint
| Makefile                      // Файл вспомогательных команд
| manage.py                     // Точка входа для cli утилит
| uv.lock                       // Устанавливаемые зависимости и их версии
| pyproject.toml                // Файл конфигурации проекта
| pytest.ini                    // Конфигурация unit-тестов
| README.md                     // Описание проекта и предметной области
```
### Файлы проекта

```
📁 .devcontainer            // Файлы для организации разработки в контейнера
📁 .vscode                  // Настройки редактора vscode
📁 migrations               // Миграции alembic для sqlalchemy
📁 seed                     // Загрузки начальных данных, словарей
📁 src | {{ service name }} // Файлы со структурой проекта
📁 tests                    // Тесты проекта
.dockerignore               // Файл для игнорирования при копировании файлов внутрь образов
.env.example                // Пример файла для
.gitignore                  // Игнорирование файлов в VCS
.pre-commit-config.yaml     // Конфигурация прекоммита
.python-version             // Указание версии python для пакетных менеджеров
alembic.ini                 // Конфигурация для исполнения миграций
docker-compose.yaml         // Файл поднятия инфраструктуры
Dockerfile                  // Файл сборки докер образа
entrypoint.sh               // Точка входа в приложение
Makefile                    // Список вспомогательных команд
manage.py                   // Точка входа в CLI команды
pyproject.toml              // Конфигурация зависимостей и проекта
README.md                   // Описание проекта
```

## Модели (_Models_)
**Модель БД** — это структурированное описание данных, хранящихся в СУБД. Она определяет, какие таблицы существуют, какие у них поля (колонки), типы данных этих полей, связи между таблицами, ограничения (constraints), индексы и т. д. Модель может включать в себя следующие сущности:

- Таблицы - основные структуры хранения данных;
- Столбцы - поля в таблицах с различными типами данных, например, VARCHAR, INTEGER, BOOLEAN, TIMESTAMP и другие;
- Первичный ключ - уникальные идентификаторы записей, должны однозначно определять запись в таблицы и быть минимальными;
- Внешние ключи - реляционные связи между таблицами;
- Индексы - набор полей, служащий для ускорения поиска и фильтрации (замедляют вставку);
- Представления, триггеры, функции и схемы - дополнительные элементы модели БД.

**ORM (Object-Relational Mapping)** — это технология, которая позволяет программисту работать с базой данных **через обычные объекты и классы**, а не через SQL-запросы. Необходимо также помнить об объектно-реляционном разрыве:

| В ООП                        | В реляционной БД               |
| ---------------------------- | ------------------------------ |
| Классы и объекты             | Таблицы и строки               |
| Наследование классов         | Нет прямого аналога            |
| Связи через атрибуты/объекты | Связи через foreign keys       |
| Инкапсуляция                 | Данные открыты и плоские       |
| Иерархии                     | Требуют JOIN’ов и нормализации |
В качестве primary key могут использоваться следующие ключи:
- **BigInt** - AutoIncrement;
- **UUID** - только uuid версии 7.
Пример таблицы пользователей с использованием [SqlAlchemy](https://www.sqlalchemy.org) представлен ниже.

```python
# apps/users/models.py

import uuid
from datetime import datetime
import sqlalchemy as sa
from sqlalchemy import Boolean, Column, DateTime, ForeignKey, String
from sqlalchemy.orm import Mapped, mapped_column
from sqlalchemy.sql import func
from sqlalchemy_utils.types import UUIDType
from va_core.db import Base
from va_core.models import TimestampMixin


class User(Base):
    """
    Модель пользователя.
    """

    __tablename__ = 'users'

    id: Mapped[uuid.UUID] = mapped_column(UUIDType(binary=False), primary_key=True, default=uuid.uuid7, server_default=func.gen_random_uuid())
    username: Mapped[str] = mapped_column(String(length=100), unique=True, index=True) # указание Mapped[<type>] равносильно nullable=False 
    email: Mapped[str] = mapped_column(String(length=320), unique=True, index=True)
    hashed_password: Mapped[str | None] = mapped_column(String(length=1024)) # указание Mapped[<type> | None] равносильно nullable=True
    first_name: Mapped[str] = mapped_column(String(length=100))
    second_name: Mapped[str | None] = mapped_column(String(length=100))
    last_name: Mapped[str] = mapped_column(String(length=100))
    avatar: Mapped[str | None] = mapped_column(String(250))
    is_active: Mapped[bool] = mapped_column(Boolean, default=False, server_default=sa.false())
    is_deleted: Mapped[bool] = mapped_column(Boolean, default=False, server_default=sa.false())
    is_superuser: Mapped[bool] = mapped_column(Boolean, default=False, server_default=sa.false())
    login_at: Mapped[datetime | None] = mapped_column(DateTime(timezone=True))

    def __repr__(self: Self) -> str:
        return f'User(id={self.id!r}, email={self.email!r}, username={self.username!r})'
```

После описания файла с моделями в каком-то из доменов, необходимо создать миграцию БД с помощью [alembic](https://alembic.sqlalchemy.org/en/latest/).
Из файла конфигурации `alembic.ini` нас интересуют пять полей:
```ini
[alembic]
# Дирректория, в которой будут находиться миграции
script_location = migrations
# Шаблон файла для генерации новой миграции
# Например: 2024_09_11_initial.py
file_template = %%(year)d_%%(month).2d_%%(day).2d_%%(slug)s

# Добавление в sys.path дирректории
prepend_sys_path = .

# Разделитель файлового пути / или \. По умолчанию тот, который в текущей операционной системе
version_path_separator = os

# Строка подключения к СУБД, в частности, postgres
# async_fallback - попробует использовать asyncpg, если не получится - перейдет автоматически на psycopg2 (блокирующий синхронный драйвер)
# options=-csearch_path= - задание дефолтной схемы для соединения, по умолчанию public
sqlalchemy.url = %(DB_PROVIDER)s://%(DB_USER)s:%(DB_PASSWORD)s@%(DB_HOST)s:%(DB_PORT)s/%(DB_NAME)s?async_fallback=True&options=-csearch_path=%(DB_SCHEMA)s
```
Для работы с SqlAlchemy есть замечательная библиотека [SqlAlchemy-utils](https://sqlalchemy-utils.readthedocs.io/en/latest/).  Однако для корректной работы этой библиотеки, нужно помочь `alembic` генерировать типы из [SqlAlchemy-utils](https://sqlalchemy-utils.readthedocs.io/en/latest/):
```python
import sqlalchemy_utils

def render_item(type_, obj, autogen_context):
    """
    Apply custom rendering for selected items.
    """
    if type_ == 'type' and isinstance(obj, sqlalchemy_utils.types.ChoiceType):
        autogen_context.imports.add('import sqlalchemy_utils')
        autogen_context.imports.add(f'from {obj.choices.__module__} import {obj.choices.__name__}') # type: ignore
        return f'sqlalchemy_utils.types.ChoiceType({obj.choices.__name__}, impl=sa.{obj.impl.__class__.__name__}())' # type: ignore

    if type_ == 'type' and isinstance(obj, sqlalchemy_utils.types.JSONType):
        autogen_context.imports.add('import sqlalchemy_utils')
        return 'sqlalchemy_utils.types.JSONType()'

    if type_ == 'type' and isinstance(obj, sqlalchemy_utils.types.UUIDType):
        autogen_context.imports.add('import sqlalchemy_utils')
        return f'sqlalchemy_utils.types.UUIDType(binary={obj.binary})'

    # Default rendering for other objects
    return False
```
По сути, эти строчки помогают `alembic` точнее генерировать миграцию для объектов, взятых из `sqlalchemy_utils`. Однако, нужно указать эту функцию в файле `migrations/env.py`:
```python
from importlib import import_module
from logging.config import fileConfig
from typing import TYPE_CHECKING

from alembic import context
from sqlalchemy import engine_from_config, pool
from {{ project_name }}.settings import settings
from fast_clean.db import Base
# Для статического анализатора
if TYPE_CHECKING:
    from .utils import render_item
else:
    from migrations.utils import render_item

# Импорт моделей
import_module('{{ project_name }}.apps.users.models')

# Доступ к значениям из файла конфигурации alembic.ini
config = context.config
section = config.config_ini_section
# Установка параметров конфигурации, которые потом используются в строке подключения
config.set_section_option(section, 'DB_PROVIDER', settings.db.provider)
config.set_section_option(section, 'DB_USER', settings.db.user)
config.set_section_option(section, 'DB_PASSWORD', settings.db.password)
config.set_section_option(section, 'DB_HOST', settings.db.host)
config.set_section_option(section, 'DB_PORT', str(settings.db.port))
config.set_section_option(section, 'DB_NAME', settings.db.name)
config.set_section_option(section, 'DB_SCHEMA', settings.db.scheme)

# Interpret the config file for Python logging.
# This line sets up loggers basically.
if config.config_file_name is not None:
    fileConfig(config.config_file_name)

# add your model's MetaData object here
# for 'autogenerate' support
# from myapp import mymodel
# target_metadata = mymodel.Base.metadata
target_metadata = Base.metadata


def run_migrations_offline() -> None:
    """
	Run migrations in 'offline' mode.

	This configures the context with just a URL and not an Engine, though an Engine is acceptable here as well. By skipping the Engine creation we don't even need a DBAPI to be available.
	Calls to context.execute() here emit the given string to the script output.
	"""
    url = config.get_main_option('sqlalchemy.url')
    context.configure(
        url=url,
        target_metadata=target_metadata,
        literal_binds=True,
        compare_server_default=True,
        dialect_opts={'paramstyle': 'named'},
    )

    with context.begin_transaction():
        context.run_migrations()

def run_migrations_online() -> None:
    """
    Run migrations in 'online' mode.

    In this scenario we need to create an Engine and associate a connection with the context.
    """
    connectable = engine_from_config(
        config.get_section(config.config_ini_section, {}),
        prefix='sqlalchemy.',
        poolclass=pool.NullPool,
    )
    with connectable.connect() as connection:
        context.configure(
            connection=connection,
            target_metadata=target_metadata,
            render_item=render_item, # добавляем
        )
		with context.begin_transaction():
			context.run_migrations()

if context.is_offline_mode():
	run_migrations_offline()
else:
	run_migrations_online()
```

Миграция генерируется вызовом сдедующей команды:
```bash
uv run alembic revision --autogenerate -m "$(NAME)" # $(NAME) - название миграции
```
Накатывается миграция вызовом следующей команды:
```python
uv run alembic upgrade head
```
Другие команды можно посмотреть на официальном сайте `alembic`.

## Схемы (_Schemas_)
Для реализации базовых [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) операций для каждой модели необходимо написать как минимум три дополнительных [pydantic](https://docs.pydantic.dev/latest/) схемы ([DTO](https://en.wikipedia.org/wiki/Data_transfer_object)):

- **ReadSchema** - схема для чтения данных;
- **CreateSchema** - схема, которая содержит необходимые поля для создания записи в БД;
- **UpdateSchema** - схема для обновления полей в БД.

Схемы для работы с пользователем представлены ниже:
```python
# {{ project_name }}/apps/users/schemas/users.py

import uuid
from datetime import datetime
from pydantic import ConfigDict, EmailStr, Field

# Базовые схемы, содержащие общий контракт
from fast_clean.schemas import (
	CreateSchema,
	ReadSchema,
	UpdateSchema,
)


class UserCreateSchema(CreateSchema):
	"""
	Схема для создания пользователя.
	"""

	username: str
	email: EmailStr
	hashed_password: str | None
	first_name: str
	second_name: str | None
	last_name: str
	avatar: str | None
	is_active: bool
	is_superuser: bool


class UserReadSchema(ReadSchema):
	"""
	Схема для чтения пользователя.
	"""

	model_config = ConfigDict(from_attributes=True)

	username: str
	email: EmailStr
	hashed_password: str | None
	first_name: str
	second_name: str | None
	last_name: str
	avatar: str | None
	is_superuser: bool
	is_active: bool
	is_deleted: bool
	login_at: datetime | None
	created_at: datetime
	updated_at: datetime


class UserUpdateSchema(UpdateSchema):
	"""
	Схема для обновления пользователя.
	"""

	username: str | None = None
	email: EmailStr | None = None
	hashed_password: str | None = None
	first_name: str | None = None
	second_name: str | None = None
	last_name: str | None = None
	avatar: str | None = None
	is_superuser: bool | None = None
	is_active: bool | None = None
	is_deleted: bool | None = None
	login_at: datetime | None = None
```

Правилом хорошего тона будет сделать реэкспорт из `{{ project_name }}/apps/users/schemas/__init__.py`, чтобы в файле `users.py` не прописывать `__all__ = ('UserCreateSchema' , 'UserReadSchema', 'UserUpdateSchema',)`:
```python
# {{ project_name }}/apps/users/schemas/__init__.py

from .users import UserCreateSchema as UserCreateSchema
from .users import UserReadSchema as UserReadSchema
from .users import UserUpdateSchema as UserUpdateSchema
```
## Перечисления (_Enums_)
Типы перечислений записываются в соответствующих файлах `enums.py` в каждом приложении. В конце названия типа должен быть суффикс `Enum`:
```python
"""
Модуль, содержащий перечисления приложения auth.
"""
from enum import StrEnum, auto

class PasswordResetStatusEnum(StrEnum):
	"""
	Статус сброс пароля.
	"""

	ACTIVE = auto()
	APPLIED = auto()
	REJECTED = auto()
```
Использовать `ENUM` в PostgreSQL **не рекомендуется в большинстве продакшен-сценариев** по следующим причинам:

- Добавить новое значение в `ENUM` можно только **через команду `ALTER TYPE`**, это **DDL-операция**, которая:
	- блокирует доступ к типу во время выполнения;
	- не может быть выполнена внутри транзакции (`BEGIN ... COMMIT`) до PostgreSQL 12.
- Если вы используете миграции (например, Alembic), изменение `ENUM` требует **ручного вмешательства** или нестандартных паттернов.
- Трудно откатить миграцию или обновить enum-состояние без "костылей".
- Если вы экспортируете/импортируете данные или работаете с несколькими СУБД — `ENUM` не всегда переносится корректно.
- В коде (например, на Python с SQLAlchemy) `ENUM` иногда ведёт себя непредсказуемо при сериализации/десериализации, особенно если значения изменяются.
- Нельзя переименовать существующие значения без костылей.
- Нельзя удалить значение.
- Нельзя изменить порядок.
- `ENUM`- значения не поддаются "мягкому" контролю бизнес-логики (например, нельзя легко сделать "архивным").
## Репозитории (_Repositories_)
**Репозиторий** — это интерфейс, описывающий набор операций для работы с сущностями предметной области, который отделяет бизнес-логику от инфраструктурных деталей хранения данных.
#### Репозиторий СУБД
Выше описана модель `User`, которая в базу данных проецируется как таблица `users`.
В библотеке [fast_clean](https://github.com/Luferov/fast-clean) описан базовый [CRUDRepository](https://github.com/Luferov/fast-clean/blob/main/fast_clean/repositories/crud/db.py), которые содержит общие методы для всех моделей. Каждый репозиторий, который отражает таблицу в СУБД должен быть отнаследован от базового репозитория `DbCrudRepository`:
```python
class DbCrudRepository(
    DbCrudRepositoryBase[ModelType, ReadSchemaType, CreateSchemaType, UpdateSchemaType, uuid.UUID],
    Generic[ModelType, ReadSchemaType, CreateSchemaType, UpdateSchemaType],
):
    """
    Репозиторий для выполнения CRUD операций над моделями нового типа в базе данных.
    """

    __abstract__ = True
```

Класс для репозитория сервиса пользователей, с учетом описанных выше схем DTO, будет выглядеть следующим образом:
```python
class UserRepository(
	DbCrudRepository[User, UserReadSchema, UserCreateSchema, UserUpdateSchema]
):

	async def exists_with_username(self: Self, username: str) -> bool:
		"""
		Проверяем существует ли пользователь с заданным username.
		"""
		async with self.session_manager.get_session() as s:
			statement = sa.exists().where(self.model_type.username == username).select()
			return bool((await s.execute(statement)).scalar())
```

В данном случае, класс вместе со стандартными методами из класса `DbCrudRepository` содержит также метод проверки существования пользователя по `username`. Список методов из DbCrudRepository следующий:

- `async def get(self: Self, id: IdType) -> ReadSchemaBaseType` - получение записи по идентификатору;
- `async def get_or_none(self: Self, id: IdType) -> ReadSchemaBaseType | None: ` - получение записи, если запись не найдена `None`;
- `async def get_by_ids(self: Self, ids: Sequence[IdType], *, exact: bool = False) -> list[ReadSchemaBaseType]:` - получение записей по списку идентификаторов;
- `async def get_all(self: Self) -> list[ReadSchemaBaseType]:` - получение всех записей (_использовать осторожно_);
- реализация _offset_ пагинации
```python
  async def paginate(
        self: Self,
        pagination: PaginationSchema,
        user: Any,
        policies: list[str],
        *,
        search: str | None = None,
        search_by: Iterable[str] | None = None,
        sorting: Iterable[str] | None = None,
    ) -> PaginationResultSchema[ReadSchemaBaseType]:
```
- `async def create(self: Self, create_object: CreateSchemaBaseType) -> ReadSchemaBaseType:` - создание записи;
- `async def bulk_create(self: Self, create_objects: list[CreateSchemaBaseType]) -> list[ReadSchemaBaseType]:` - массовое создание записей;
- `async def update(self: Self, update_object: UpdateSchemaBaseType) -> ReadSchemaBaseType:` - обновление записи в БД;
- `async def bulk_update(self: Self, update_objects: list[UpdateSchemaBaseType]) -> None:`  - массовое обновление записей в БД под единой транзакцией;
- `async def upsert(self: Self, create_object: CreateSchemaBaseType) -> ReadSchemaBaseType:` - попытка обновить, в случае неуспеха запись создается;
- `async def delete(self: Self, ids: Sequence[IdType]) -> None:` - удаление записи;
- `def select(cls) -> sa.Select[tuple[ModelBaseType]]:` - выбор базовой модели и всех полей наследника, если такие есть (_classmethod_).
#### Остальные репозитории
Репозитории, которые отвечают за взаимодействие системы с внешним миром, могут работать в двух режимах:

1. [C персистентным соединением](https://ru.wikipedia.org/wiki/Постоянное_HTTP-соединение) - устанавливать соединение и держать его до завершения всех операций соединения.
```plantuml
@startuml
autonumber

participant Client
participant Server


Client -> Client: Открываем соединения
activate Client

Client -> Server: Запрос
activate Server
Server -> Client: Ответ
Client -> Server: Запрос
Server -> Client: Ответ
Client -> Server: Запрос
Server -> Client: Ответ
destroy Server

Client -> Client: Закрываем соединение
deactivate Client
@enduml
```
![Персистентное соединение](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKeiBSdFAyrDIYtYuYe0YcKcPnOavfKeEETafkQLe0W5fnQLWPHnGUGKTEsWggmKBioxs7HXryM6YzqNDbtOS67hXJqA5nilx0CojbpOSM7lXQr0lEDYFrTYSabcMM99AbGZab5Gqu56JGSQiFzYmuLz5nlXwc5sGoXqcu5sRhtOHC0iskbDIIqkASglXEh431w8MGI3vy9Me4de0UIGcfS2Z2O0)

2. С постоянным разрывом соединения
```plantuml
@startuml
autonumber

participant Client
participant Server


== 1 действие ==
Client -> Client: Открываем соединения
activate Client
Client -> Server: Запрос
activate Server
Server -> Client: Ответ
deactivate Server
Client -> Client: Закрываем соединение
destroy Client

== 2 действие ==
Client -> Client: Открываем соединения
activate Client
Client -> Server: Запрос
activate Server
Server -> Client: Ответ
deactivate Server
Client -> Client: Закрываем соединение
destroy Client
@enduml
```
![С постоянным разрывом соединения](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuKeiBSdFAyrDIYtYuYe0YcKcPnOavfKeEETafkQLe0W5fnQLWPHnsTeg62fSs79XwuMT5nilDbtOT677XQqAjhPS44qAkdPGxLOA5-O1vNTTRBZO3LIr0QXZZyB5nWlxW8mjG4rxBsm5uXqN-xaIaqioon9BKg4Q4IP1RGMQD1seV__5XWlxBZR2bSFS1A7GBGPQkVLY4rTAAffo37T2J2VWJf0dWMOLbnJbLqBrWOF2Q3GeGCvqICrB0IOe0000)

Очевидно, что при любой возможности необходимо использовать первый вариант. В таком случае, репозиторий должен представлять из себя контекстный менеджер, внутри которого будет обеспечиваться все взаимодействие. Пример представлен ниже:
```python
"""
Репозиторий по отправке файлов.
"""
import uuid
from dataclasses import dataclass
from contextlib import AbstractAsyncContextManager
from typing import Any, Self

import httpx

from ..schemas import (
    UserCreateSchema,
    UserResultScheme,
    UserRepositoryParams,
    UserCreateSchema,
)

@dataclass
class UserRepository(AbstractAsyncContextManager):
	"""
	Репозиторий взаимодействия с пользователями через http соединение.
	"""
	self.params = params
	self.client: httpx.AsyncClient | None = None

    async def __aenter__(self: Self) -> Self:
        if self.client is None:
            self.client = self.make_client()
        return self

    async def __aexit__(
        self, exc_type: type[BaseException] | None, exc_value: BaseException | None, traceback: Any | None
    ) -> None:
        if self.client:
            await self.client.aclose()
        self.client = None
        return None

    def make_client(self: Self, retries: int = 3, verify: bool = False) -> httpx.AsyncClient:
	    """
	    Создаем экземпляр класса соединения.
		"""
        transport = httpx.AsyncHTTPTransport(retries=retries, verify=verify)
        return httpx.AsyncClient(transport=transport)

    async def create_user(self: Self, user: UserCreateSchema) -> UserResultScheme:
        """
        Создаем user запись.
        """
        assert self.client
        response = await self.client.post(self.params.destination, json=user.model_dump(by_alias=True))
        response.raise_for_status()
        return UserResultScheme.model_validate(response.json())

    async def delete(self: Self, user_id: uuid.UUID) -> None:
        """
        Удаляем пользователя.
        """
        assert self.client
        await self.client.delete(f'{self.params.destination}/users/{user_id}')

```
При такой реализации использование будет выглядеть следующим образом:
```python

async with user_repository:
	# Внутри keep-alive соединения
	result = await user_repository.create_user(user)
	await user_repository.delete(result.id)

```
## Сервисы (_Services_)
**Сервисный слой**  — это архитектурное ядро, где живет _чистая бизнес-логика_ приложения. Сервисы инкапсулируют ключевые правила и процессы: валидацию данных, управление транзакциями, координацию между источниками информации, интеграцию с внешними системами и выполнение сложных вычислительных задач.

**Его главные достоинства — тестируемость и универсальность.** Сервисы легко проверять изолированно, а их независимость от деталей ввода/вывода (HTTP, UI) и конкретных пользовательских сценариев (_User Story_) позволяет гибко переиспользовать их в различных _UseCases_.

В качестве примера рассмотрим UserService, в котором реализована функция для создания пользователя.
```python
from dataclasses import dataclass


@dataclass
class UserService:
	user_repository: UserRepository
	password_service: PasswordService

	async def create(self: Self, create_schema: UserCreateRequestSchema) -> UserReadSchema:
        """
        Создаем пользователя.
        """
        if await self.user_repository.exists_with_username(create_schema.username):
            raise ModelAlreadyExistsError('username', 'Пользователь с таким логином уже существует')
        if await self.user_repository.exists_with_email(create_schema.email):
            raise ModelAlreadyExistsError('email', 'Пользователь с таким email уже существует')
        register_dict = create_schema.model_dump()
        password = register_dict.pop('password')
        register_dict['hashed_password'] = None
        register_dict['is_active'] = False
        if settings.allow_set_password and password is not None:
            if not await self.password_service.check_strength(create_schema.password):
                raise WeakPasswordError(self.password_service.MIN_PASSWORD_LENGTH)
            register_dict['hashed_password'] = await run_in_processpoll(self.password_service.hash, password)
            register_dict['is_active'] = True
        register_dict['avatar'] = None
        register_dict['is_superuser'] = False
        user_create_schema = UserCreateSchema.model_validate(register_dict)
        return await self.user_repository.create(user_create_schema)
```

Как видно, сервис выполняет следующие действия обеспечивая создание пользователя:

- проверку на существования пользователя с _username_;
- проверка на существование пользователя с _email_;
- проверяет адекватность пароля и хеширует его с помощью _PasswordService_;
- заполняет поля по умолчанию, затем создает пользователя и возвращает пользователя выше.
Как видно из примера, в сервисе реализована валидация, а уже само создание реализуется с помощью _UserRepository_.
### Использование внешней библиотеки
Все внешние библиотеки в проекте должны иметь единую точку входа. Это означает, что в проекте должен существовать лишь один файл, где библиотека импортируется и все использование должно идти через этот модуль или класс.
Для примера, возьмём библиотеку _python-jose_, которая имплементируют в себе две функции `encode` и `decode`:
```python
from jose import jwt
token = jwt.encode({'key': 'value'}, 'secret', algorithm='HS256')
# token == u'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJrZXkiOiJ2YWx1ZSJ9.FG-8UppwHaFp1LgRYQQeS6EDQF7_6-bMFegNucHjmWg'
jwt.decode(token, 'secret', algorithms=['HS256'])
# {u'key': u'value'}
```
Казалось бы, API библиотеки достаточно простое и почему бы сразу не использовать эти вызовы в необходимых местах. Однако, у нас возникает ситуация, при которой замена этой библиотеки на что-то другое приведет к необходимости поиска импортов по всему проекту, а также в тестах и велика вероятность что-то пропустить. Чтобы этого избежать, необходимо сделать "обертку" над этой библиотекой, поместив вызов этих функций в методы сервисного класса:
```python
from typing import Self
from jose import jwt # единственное место импорта из библиотеки jwt
from dataclasses import dataclass
from {{ project_name }} import Settings

@dataclass
class CryptoService:
	settings: Settings

	def encode(self: Self, value: dict, *, algorithm: str = 'HS256') -> str:
		return jwt.encode(value, self.settings.secret_key, algorithm=algorithm)

	def decode(self: Self, token: str, *, algorithm: str = 'HS256') -> dict:
		return jwt.decode(token, self.settings.secret_key, algorithms=[algorithm])
```
Использование во всех сервисах _CryptoService_, вместо импорта библиотеки, позволит, в случае изменения библиотеки, поменять имплементацию только в одном месте. Кроме этого, такое использование окажет положительное влияние на процесс тестирования. Такой подход называется - **инкапсуляция**.
## Варианты использования (_UseCases_)

В отличие от сервисного слоя, который группирует логику по доменным областям, **_UseCase_** — это следующий шаг декомпозиции, фокусирующийся на **одном конкретном бизнес-сценарии**, **полностью изолируя чистую бизнес-логику** от инфраструктуры. Это делает приложение ещё более гибким, тестируемым и понятным, особенно в сложных доменах. UseCase не всегда заменяет сервисный слой, но он поднимает уровень абстракции бизнес-правил на новую ступень.
Для примера, реализуем сервис регистрации пользователей:

```python
from dataclasses import dataclass

@dataclass
class UserRegisterUseCase:
	user_service: UserService
	me_service: MeService
	notify_service: NotifyService

	async def __call__(self: Self, user_schema: UserCreateRequestSchema) -> UserResponseSchema:
		"""
		Регистрация пользователя.
		"""
		current_session = await self.me_service.get_session()
		if current_session:
			raise LogoutRequiredError()
		user = await self.user.service.create(user_schema)
		await self.notify_service.notify_register(user)
		return UserResponseSchema.model_validate(user, from_attributes=True)
```
Создание пользователя (его валидация) - это реализация сервисного действия.
Отсылка уведомления - это не часть сервиса по созданию пользователя, а часть пользовательской истории регистрации.
## Роуты и контроллеры
Роутеры организуют группу связанных маршрутов. Они нужны для разделения кода по функциональности, упрощение масштабирования больших приложений, повторное использование префиксов, тегов и зависимостей. Также они определяют внешний контракт взаимодействия по API. Пример роута для FastAPI:
```python
from fastapi import APIRouter

router = APIRouter(
	prefix='/users', # Автоматический префикс для всех входящих маршрутов
	tags=['Users'], # Группировка в документации Swagger
)
```
Функция, которая используется с этим роутером является контроллером:
```python

@router.get('')
async def get_all_users() -> list[UserResponseSchema]:
	"""Получаем всех пользователей."""
	...
```
Функция `get_all_users` является контроллером в привычном понимании этого слова, в виду того, что она может быть записана и так:
```python

async def get_all_users() -> list[UserResponseSchema]:
	"""Получаем всех пользователей."""
	...

@router.get('')(get_all_users)
#           ^ роутер
#                    ^ контроллер
```
Связка роута и контроллера представляет внешний контаркт API по которому будет происходить общение с внешними системами, по сути, она должна содержать следующее:

- путь обращения;
- _UseCase_, который отражает пользовательскую _UserStory_;
- параметры запроса (Path, QueryParams, RequestSchema, Headers, etc);
- параметры ответа.

Рассмотрим пример для регистрации пользователя:
```python

from dishka.integrations.fastapi import FromDishka, inject

@router.post('')
@inject
async def register(
	register_schema: UserCreateRequestSchema, # Параметры запроса
	user_register_use_case: FromDishka[UserRegisterUseCase] # UseCase
) -> MeResponseSchema: # параметры ответа
	"""
	Регистрация пользователя.
	"""
	user = await user_register_use_case(register_schema)
	return MeResponseSchema.model_validate(user, from_attributes=True)
```

## Исключения (_Exceptions_)
Классы ошибок можно разделить на три части:

- Слой приложения - http ошибки со статус кодами:
	- 4XX - ошибки клиента;
	- 5XX - ошибки сервера;
- Слой доменной логики - ошибки бизнес логики;
- Слой внешних обращений - внешние ошибки, возникающие при обращении к внешним системам.

Наименование также является важной частью, в виду того, что для обозначений классов ошибок можно использовать суффиксы _**Exception**_ и _**Error**_:

- Exception — непредвиденные ошибки, которые возникают в результате работы программы  при неконтролируемых действиях;
- Error — ошибки бизнес-логики, пользовательские ошибки.
> PEP8 говорит:
> Because exceptions should be classes, the class naming convention applies here. However, you should use the suffix “Error” on your exception names (if the exception actually is an error)

Линтер Ruff транслирует необходимость использования вместо `class Validation(Exception)` использовать `class ValidationError(Exception)`.

В центре нашего приложения должен находиться главный и единственный класс ошибки, например:
```python
class BusinessLogicException(Exception, ABC):
    """
    Базовое исключение бизнес-логики.
    """

    @property
    def type(self: Self) -> str:
        """
        Тип ошибки.
        """
        return snakecase(type(self).__name__.replace('Error', ''))

    @property
    @abstractmethod
    def msg(self: Self) -> str:
        """
        Сообщение ошибки.
        """
        ...

    def __str__(self: Self) -> str:
        return self.msg

    def get_schema(self: Self, debug: bool) -> BusinessLogicExceptionSchema:
        """
        Получаем схему исключения.
        """
        return BusinessLogicExceptionSchema(
            type=self.type,
            msg=self.msg,
            traceback=(''.join(traceback.format_exception(type(self), self, self.__traceback__)) if debug else None),
        )
```
Его название может быть любым, как `BusinessLogicException`, так и `DomainError`.  Базовый класс должен содержать **контракт** для всех ошибок, возвращаемых по API. Ошибки второго уровня должны быть связаны с ошибками статус кодов, например:
```python

class PermissionDeniedError(BusinessLogicException):
    """
    Ошибка, возникающая при недостатке прав для выполнения действия.
    """

    @property
    def msg(self: Self) -> str:
        return 'Недостаточно прав для выполнения действия'

class ModelNotFoundError(BusinessLogicException):
    """
    Ошибка, возникающая при невозможности найти модель.
    """

    def __init__(
        self,
        model: type[ModelType] | str,
        *args: object,
        model_id: int | uuid.UUID | Iterable[int | uuid.UUID] | None = None,
        model_name: str | Iterable[str] | None = None,
        message: str | None = None,
    ) -> None:
        super().__init__(*args)
        self.model = model
        self.model_id = model_id
        self.model_name = model_name
        self.custom_message = message

    @property
    def msg(self: Self) -> str:
        if self.custom_message is not None:
            return self.custom_message
        msg = f'Не удалось найти модель {self.model if isinstance(self.model, str) else self.model.__name__}'
        if self.model_id is not None:
            if isinstance(self.model_id, Iterable):
                return f'{msg} по идентификаторам: [{", ".join(map(str, self.model_id))}]'
            return f'{msg} по идентификатору: {self.model_id}'
        if self.model_name is not None:
            if isinstance(self.model_name, Iterable):
                return f'{msg} по именам: [{", ".join(self.model_name)}]'
            return f'{msg} по имени: {self.model_name}'
        return msg


class ModelAlreadyExistsError(BusinessLogicException):
    """
    Ошибка, возникающая при попытке создать модель с существующим уникальным полем.
    """

    def __init__(self, field: str, message: str, *args: object) -> None:
        super().__init__(*args)
        self.field = field
        self.message = message

    @property
    def msg(self: Self) -> str:
        return self.message

    def get_schema(self: Self, debug: bool) -> BusinessLogicExceptionSchema:
        return ModelAlreadyExistsErrorSchema.model_validate(
            {**super().get_schema(debug).model_dump(), 'field': self.field}
        )

```

Чтобы связать эти ошибки с http статусами ошибок, необходимо использовать функционал [FastAPI exception handlers](https://fastapi.tiangolo.com/tutorial/handling-errors/):
```python
async def business_logic_exception_handler(
    settings: CoreSettingsSchema, request: Request, exception: BusinessLogicException
) -> Response:
    """
    Обработчик базового исключения бизнес-логики.
    """
    return await http_exception_handler(
        request,
        HTTPException(
            status_code=status.HTTP_400_BAD_REQUEST,
            detail=[exception.get_schema(settings.debug).model_dump()],
        ),
    )

async def permission_denied_error_handler(
    settings: CoreSettingsSchema, request: Request, error: PermissionDeniedError
) -> Response:
    """
    Обработчик ошибки, возникающей при недостатке прав для выполнения действия.
    """
    return await http_exception_handler(
        request,
        HTTPException(
            status_code=status.HTTP_403_FORBIDDEN,
            detail=[error.get_schema(settings.debug).model_dump()],
        ),
    )


async def model_not_found_error_handler(
    settings: CoreSettingsSchema, request: Request, error: ModelNotFoundError
) -> Response:
    """
    Обработчик ошибки, возникающей при невозможности найти модель.
    """
    return await http_exception_handler(
        request,
        HTTPException(
            status_code=status.HTTP_404_NOT_FOUND,
            detail=[error.get_schema(settings.debug).model_dump()],
        ),
    )


async def model_already_exists_error_handler(
    settings: CoreSettingsSchema, request: Request, error: ModelAlreadyExistsError
) -> Response:
    """
    Обработчик ошибки, возникающей при попытке создать модель с существующим уникальным
    полем.
    """
    return await http_exception_handler(
        request,
        HTTPException(
            status_code=status.HTTP_409_CONFLICT,
            detail=[error.get_schema(settings.debug).model_dump()],
        ),
    )


def use_exceptions_handlers(app: FastAPI, settings: CoreSettingsSchema) -> None:
    """
    Регистрируем глобальные обработчики исключений.
    """
    app.exception_handler(BusinessLogicException)(partial(business_logic_exception_handler, settings))
    app.exception_handler(PermissionDeniedError)(partial(permission_denied_error_handler, settings))
    app.exception_handler(ModelNotFoundError)(partial(model_not_found_error_handler, settings))
    app.exception_handler(ModelAlreadyExistsError)(partial(model_already_exists_error_handler, settings))
```
Бизнес ошибки должны наследоваться от ошибок, которые связаны с HTTP кодами, чтобы при бросании ошибок пользовательских они автоматически трансформировались в HTTP ошибки и отдавались клиенту.
Дерево ошибок выглядит как показано ниже:
```plantuml
@startmindmap
* BusinessLogicException \n(все ошибки должны наследоваться отсюда)
** PermissionDeniedError \n(ошибка привязанная к 403 статусу)
** ModelNotFoundError \n(ошибка привязання к 404 статусу)
*** UserNotFoundError \n(бизнес ошибка)
*** CompanyNotFoundError \n(бизнес ошибка)
@endmindmap
```
![Дерево ошибок](https://www.plantuml.com/plantuml/png/bT6nIiDG50RWlKznHiqas0SeQfrKNDnSGdEH2-RSS6yAkfbq4HJxAcamjbHIN-4_R-JngYXqqUty2V_t3tSKojINkUKiJuj1GWVJODc4SEAkx6HyCp55QHtJ9U-WbGeTOIFtM661DLQ4PspmYXVqya3eqKYbiOkVMqsrF4ebyuzIhSUJ_cXs1qb2vyRdDWIbZmnRauszTpwk_F0DuKtkDBGonn8D-hYWsfg6U_kaO1qtPdhD8dlgCdDzvifZD-K_cz_YS5jKyY8Oloqkb5XgkPEAVlEVdKEN5odV_gSsCfnzlS8x)

Кроме этого, если вы однозначно знаете какие ошибки могут возникнуть при обработке данных, рекомендуется писать их явно для генерации документации в _Swagger_ на основе _OpenAPI_ 3.0.
```python

@router.post(
	'',
	responses={
		403: {'description': 'Доступ запрещен'},
		404: {'description': 'Не найдено'},
		418: {'description': '# I\'m a teapot'}
	}
)
```
## Dependency Injection

Dependency Injection (DI, **внедрение зависимостей**) — это паттерн проектирования, который делает код более гибким, тестируемым и поддерживаемым. Вот ключевые причины его использования:

1. Уменьшение связности (Coupling);
2. Упрощение тестирования;
3. Гибкость и расширяемость;
4. Читаемость и прозрачность;
5. Соблюдение правил [SOLID](https://ru.wikipedia.org/wiki/SOLID_(программирование)).

Хотелось бы DI & IoC-контейнер, который:

- Представляет собой реестр (контейнер) объектов, которыми он управляет
- Позволяет декларативно конфигурировать объекты и их свойства
- Код классов не должен зависеть от IoC-фреймворка
- Берёт на себя:
    - Управление жизненным циклом объектов: создание, удаление
    - Управление зависимостями между объектами.

В качестве DI фреймворка выбран [Dishka](https://dishka.readthedocs.io/en/stable/). Эта библиотека предоставляет контейнер IoC, который действительно полезен. Если вы устали от бесконечной передачи объектов только для того, чтобы создать другие объекты, только для того, чтобы эти объекты создавали еще больше - вы не одиноки, и у нас есть решение. Не для каждого проекта требуется контейнер IoC, но посмотрите, что мы предлагаем.
**IoC-контейнер** - это специальный объект (или фреймворк, предоставляющий такой объект), который предоставляет необходимые объекты в соответствии с правилами внедрения зависимостей и управляет их сроком службы. **DI-framework** - это другое название для таких фреймворков.

Прежде, чем читать дальше, крайне рекомендуется прочитать документацию DI фреймворка  [Dishka](https://dishka.readthedocs.io/en/stable/).

Давайте опишем провайдер для регистрации пользователя:
```python
from dishka import Provider as DishkaProvider
from dishka import Scope, provide


class Provider(DishkaProvider):
    scope = Scope.REQUEST

	user_repository = provide(UserRepository)
	user_service = provide(UserService)

	# use a function
	@provide
	@staticmethod
	def get_me_service(user_repository: UserRepository) -> MeService:
		return MeService(user_repository)

	user_register_use_case = provide(UserRegisterUseCase)

provider = Provider() # Объявляем провайдер
```
После этого создаем IoC контейнер и запускаем FastAPI приложение:
```python
import os
from collections.abc import AsyncGenerator
from contextlib import asynccontextmanager
from importlib import import_module
from pathlib import Path
import tochka_jsonrpcapi as jsonrpc
from fastapi import FastAPI
from fast_clean.container import ContainerManager
from fast_clean.contrib.healthcheck.router import router as healthcheck_router

@asynccontextmanager
async def lifespan(app: FastAPI) -> AsyncGenerator[None]:
    yield
    await app.state.dishka_container.close()

app = FastAPI(lifespan=lifespan)
ContainerManager.init_for_fastapi(app)
app.router.route_class = DishkaRoute

app.middleware('http')(context_middleware)
app.include_router(healthcheck_router)
```
## Слои приложения

Если рассматривать слоистость приложения, то его можно представить следующим образом:
```plantuml
@startuml
component Application {
  component Router {
   component Controller {
     component UseCase {
       component "Service 1" {
         component "Repository 1"
         component "Service 3" {
           component "Repository 3"
         }
         component "Service 2" {
           component "Repository 2"
         }
       }
       component "Service 2" {
         component "Repository 2"
       }
       component "Service 4" {
        component "Repository 4"
       }
     }
   }
  }
}
@enduml
```
![Слоистость приложения](https://www.plantuml.com/plantuml/png/ZSpH2SCW4CRnTwUGRD0a0wHaWvGE8FOU1FL4pq8fxjuIa5n092yAz__uBObKfEoiqEW2Ul0axo5OenKPzF8hf3pAXfaWxaTsNT5JH6jhuUsPO5K9Qk1fU41y6mtoDXpvDDWWO3A4yVFVr4MJc2hHHIQ6bAutNlB6bbV4LRQFzgMPIsrevb3zzgU88XRmh-pi3m00)

Кроме этого, возможность зон обращений можно представить следующей диаграммой:
```plantuml
@startmindmap
* Application
** Router
*** Controller
**** UseCase
***** Service 1
****** Service 2
******* Repository 3
****** Repository 1
****** Repository 2
***** Serivce 2
****** Repository 3
@endmindmap
```
![Зоны обращений](https://www.plantuml.com/plantuml/png/ROz12i9034NtSugiiskzGAKtK3p0q6OHc4v24WlUtc871jJTVs_nu4yUsMBZkcvP4y9LjV2IWwKcHBZ9CyYEXJ1B3PDIJaHuECtPwGC8TxAT5uBXv2x69euMgJY7s0ikwTSDVzpOwxpts5Thehgs1sy0)

## Sync и Async функции

В современных приложениях на Python, особенно при использовании асинхронного программирования с asyncio, часто возникает необходимость выполнять синхронные (блокирующие) функции в асинхронной среде. Чтобы избежать блокировки основного асинхронного потока выполнения, синхронные функции можно запускать в отдельном пуле потоков (_thread pool_) или процессов (_process pool_).

_ThreadPoolExecutor_ подходит для выполнения блокирующих I/O операций, то есть когда функция ожидает ввода-вывода, например:

- Сетевые запросы к _API_ без асинхронных библиотек.
- Чтение и запись файлов на диск.
- Обращение к базам данных без поддержки асинхронности.

Причины использовать _ThreadPoolExecutor_ для _I/O-bound_ задач:

- _GIL (Global Interpreter Lock):_ В Python интерпретатор CPython имеет GIL, который не позволяет нескольким потокам выполнять байт-код одновременно. Однако при I/O операциях потоки освобождают GIL, позволяя другим потокам продолжить выполнение.
- _Низкие накладные расходы:_ Создание и переключение между потоками дешевле, чем между процессами.
- _Простота обмена данными:_ Потоки разделяют общую память, что упрощает передачу данных между ними.

В связи с этим синхронные функции не оборачиваем в async. Оборачивание синхронных операций в `async` добавляет некоторые накладные расходы на формирование `Future`, которая по сути ничего не ожидает.

```python
# ✅ correct
def get_todo_repositories() -> ResultSchema:
    return do_smt_sync()

# ❌ incorrect
async def get_todo_repositories() -> ResultSchema:
    return do_smt_sync()
```

Синхронные функции в асинхронной среде запускаются в соответствующих функциях для запуска `run_in_processpool` и`run_in_threadpool`:
```python
# run_in_processpool.py

"""
Модуль запуска тяжелых операций в ProcessPoolExecutor.
"""

import asyncio
import multiprocessing as mp
from concurrent.futures import ProcessPoolExecutor
from functools import partial
from typing import Callable, TypeVar

from typing_extensions import ParamSpec

P = ParamSpec('P')
R = TypeVar('R')


process_pool: ProcessPoolExecutor | None = None


async def run_in_processpool(fn: Callable[P, R], *args: P.args, **kwargs: P.kwargs) -> R:
    """
    Запуск функции в отдельном процессе.

    Используем fork в связи с https://github.com/python/cpython/issues/94765.
    """
    global process_pool
    if process_pool is None:
        process_pool = ProcessPoolExecutor(mp_context=mp.get_context('fork'))
    kwargs_fn = partial(fn, *args, **kwargs)
    loop = asyncio.get_running_loop()
    return await loop.run_in_executor(process_pool, kwargs_fn)
```

```python
# run_in_threadpool.py

"""
Модель запуска тяжелых операций в ThreadPoolExecutor.
"""

import asyncio
from functools import partial
from typing import Callable, TypeVar

from typing_extensions import ParamSpec

R = TypeVar('R')
P = ParamSpec('P')


async def run_in_threadpool(fn: Callable[P, R], *args: P.args, **kwargs: P.kwargs) -> R:
    """
    Запуск функции в отдельном потоке.
    """
    kwargs_fn = partial(fn, *args, **kwargs)
    loop = asyncio.get_running_loop()
    return await loop.run_in_executor(None, kwargs_fn)
```

## FastClean Core

В основном, все приложения одинаковые и различаются лишь бизнес фукнционалом. Системный функционал и тулинг выносится в библиотеку, которая обычно называется core. В нашем представлении, ядерная библиотека представляется набором системных утилит и приложений, и представлена в репозитории [FastClean](https://github.com/Luferov/fast-clean).
## API Request и Response схемы

Все API схемы для описания входящих запросов должны быть описаны с помощью `Pydantic` и должны быть отнаследованы от `RequestSchema`, которая показана ниже. Однако, исходящие запросы должны приводиться к нотации `camelCase`, описываться с помощью `Pydantic` моделей и наследоваться от `ResponseSchema`.
```python

from __future__ import annotations

import uuid
from typing import Generic, Self, TypeVar

from pydantic import AliasGenerator, BaseModel, ConfigDict, field_validator
from pydantic.alias_generators import to_camel

class RequestSchema(BaseModel):
    """
    Схема запроса.
    """
    # Настройка добавления алиасов и валидирования из camelCase -> snake_case
    model_config = ConfigDict(
        alias_generator=AliasGenerator(
            validation_alias=to_camel,
        )
    )

class ResponseSchema(BaseModel):
    """
    Схема ответа.
    """

    # Настройка генерирования в camelCase из snake_case -> camelCase
    model_config = ConfigDict(
        alias_generator=AliasGenerator(
            serialization_alias=to_camel,
        )
    )

RemoteResponseSchema = RequestSchema
RemoteRequestSchema = ResponseSchema

```

Межсервисное взаимодействие должно обеспечиваться с помощью этих же трансформаций, в следствие чего должны использоваться классы `RemoteResponseSchema` и `RemoteRequestSchema`.
## Транзакции
**Транзакция** — это единица работы с базой данных, состоящая из одной или нескольких операций _SQL_, которые выполняются как одно целое. Транзакции обладают свойствами, известными как _ACID_:

- **Atomicity (Атомарность):** Все операции внутри транзакции выполняются полностью или не выполняются вовсе. Если происходит ошибка, все изменения откатываются.
- **Consistency (Согласованность):** Транзакции переводят базу данных из одного согласованного состояния в другое, соблюдая все ограничения целостности.
- **Isolation (Изоляция):** Одновременные транзакции не влияют друг на друга, обеспечивая изолированность выполнения.
- **Durability (Долговечность):** После фиксации транзакции ее результаты сохраняются даже в случае сбоев системы.

Типичный код репозитория к базе данных на любой запрос открывает транзакцию. В случае, если транзакция явно не открывается - она все равно открывается внутренностями _SqlAlchemy_.

```python
import uuid
import sqlalchemy as sa
from typing import Protocol, Self

from fast_clean.depends import SessionManagerProtocol

from ..models import check_list_mark_template


class TodoRepository:
    """
    Реализация репозитория для работы со списком выбранного todo листа.
    """

    def __init__(self, session_manager: SessionManagerProtocol) -> None:
        self.session_manager = session_manager

    async def set_active(self: Self, todo_id: uuid.UUID, status: bool) -> None:
        """
        Устанавливаем шаблоны оценки чек-листа.
        """
        async with self.session_manager.get_session() as s:
            statement = (
                sa.update(self.model_type)
                .where(self.model_type.id == pk)
                .values({'status': status})
                .returning(self.model_type)
            )
            await s.execute(statement)
	        # Мы все еще находимся в транзакции и она открыта.
```
В виду того, что мы находимся в транзакции в момент, когда сессия в _SqlAlchemy_ завершается, поэтому над стандартной сессией пишется обертка, которая, в случае, если мы находимся внутри транзакции, создает вложенную транзакцию (или возвращает ее же), которая открывается на основе текущей транзакции, т.е. по сути вызывается `SAVEPOINT` внутри транзакции. Код сервиса представлен ниже:

```python
from contextlib import asynccontextmanager
from typing import AsyncContextManager, AsyncIterator, Protocol, Self

import sqlalchemy as sa
from sqlalchemy.ext.asyncio import AsyncSession


class TransactionService:
    """
    Реализация сервиса транзакций.
    """

    def __init__(self, session: AsyncSession) -> None:
        self.session = session

    @asynccontextmanager
    async def begin(self: Self, immediate: bool = True) -> AsyncIterator[None]:
        """
        Начинаем транзакцию.
        """
        async with self.session.begin():
            if immediate:
                await self.session.execute(sa.text('SET CONSTRAINTS ALL IMMEDIATE'))
            yield
```
Если несколько репозиториев нужно объединить в одну транзакцию, то стартовать транзакцию необходимо в сервисе транзакций:
```python


class TodoUseCase(UseCase[ResponseModel]):

    def __init__(
        self,
        transaction_service: TransactionService,
        service1: Service1,
        service2: Service2
    ) -> None:
        self.transaction_service = transaction_service
        self.service1 = Service1Protocol
        self.service2 = Service2Protocol

    def __call__(self) -> ResponseModel:
        async with self.transaction_service.begin():
            # Выполнения запросов service1 и service2 происходит внутри одной транзакции
            await service1.do_smt()
            await service2.do_smt()
```
## Запуск приложения в режиме разработки

```bash
git clone {{ repository }}
cp .env.example .env

# Запускаем приложение
uv run uvicorn {{ service name }}.entrypoints.main:app
```

## Литература

- [Лучшие практики FastAPI](https://github.com/zhanymkanov/fastapi-best-practices)
- [Чистая архитектура](https://www.litres.ru/book/robert-s-martin/chistaya-arhitektura-iskusstvo-razrabotki-programmnogo-obe-39113892/)
