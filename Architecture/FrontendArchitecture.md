# Архитектура фронтенд приложений на платформе NextJS

*Next.js* — это платформа *React* для создания полнофункциональных веб-приложений. Вы используете компоненты *React* для создания пользовательских интерфейсов и *Next.js* для дополнительных функций и оптимизации.

## Инструментарий

В качестве менеджера зависимостей используется стандартная реализация 
[npm](https://www.npmjs.com).

Основной инструментарий для разработки фронтенд сервисов можно разделить
на два условных раздела: для непосредственной работы приложени, а также
утилитарный набор инструментов для разработки.

Прежде, чем приступить к разработке необходимо ознакомиться с основным
инструментарием, который представлен ниже:

1. Основной инструментарий для реализации сервисов:
    - [React 19+](https://react.dev), [React Compiler](https://react.dev/learn/react-compiler)
    - [NextJs 15+](https://nextjs.org)
    - [MUI 6+](https://mui.com)
    - [axios](https://axios-http.com/docs/intro) (deprecated), use `fetch`
    - [zustand](https://zustand-demo.pmnd.rs)
    - [react-query](https://tanstack.com/query/v5/docs/framework/react/overview)
    - [openapi-typescript-codegen](https://github.com/ferdikoomen/openapi-typescript-codegen)
    - [sass](https://sass-lang.com)
2. Вспомогательные инструменты разработки
    - [turborepo](https://turbo.build/repo/docs)
    - [turbopack](https://turbo.build/pack/docs)
    - [storybook](https://storybook.js.org)
    - [webpack](https://webpack.js.org)
    - [eslint 9](https://eslint.org)
    - [prettier](https://prettier.io)

## Типовая структура сервиса

Ниже представлена типовая структура сервиса, а также описание дирректорий и файлов.
Выход за пределы структуры позволяется только с разрегения техлида или лидера компетенций
соответствующего направления, а также лидера разработки.

```
📁 {{ project name }}           // Название проекта
| 📁 .vscode                    // Настройки IDE
| 📁 apps                       // Список приложений
| | 📁 portal                   // Проект портала
| | | 📁 public                 // Общедоступные файлы
| | | 📁 src                   // Исходных код приложения
| | | | 📁 app                   // Дирректория app routing nextjs
| | | | | 📁 [lng]                   // Локализация
| | | | | | 📁 (main)                   // Группа основных роутов с авторизацией
| | | | | | 📁 auth                   // Авторизация приложений
| | | | | | layout.tsx                   // layout 
| | | | | | page.tsx                   // Страница
| | | | | | loading.tsx                   // Скелетон загрузки
| | | | | 📁 health                   // Роут для helthcheck
| | | | | | router.ts                 // Хендлер загрузки
| | | | | fonts.tsx                   // Настройки шрифтов
| | | | | favicon.ico                   // Иконка проекта
| | | | | globals.scss                   // Настройка глобальных css
| | | | | theme.tsx                   // Настройки темы  MUI
| | | | 📁 assets/fonts                   // Настройка 
| | | | 📁 providers                   // Проект
| | | | | index.ts                   // Проект
| | | | | query.ts                   // Проект
| | | | | store.ts                   // Проект
| | | | | theme.ts                   // Проект
| | | | 📁 modules                   // Проект
| | | | | 📁 common                   // Проект
| | | | | | 📁 widgets                   // Проект
| | | | | | | 📁 comopnents                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | AppToolBar.tsx                   // Проект
| | | | | | | | AppSideBar.tsx                   // Проект
| | | | | | | 📁 hooks                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | 📁 queries                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | notification.ts                   // Проект
| | | | | | | 📁 mutations                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | read-notifications.ts                   // Проект
| | | | | | | | delete-notifications.ts                   // Проект
| | | | | | | index.ts                   // Проект
| | | | | | | types.ts                   // Проект
| | | | | | 📁 features                   // Проект
| | | | | | | 📁 comopnents                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | AppToolBar.tsx                   // Проект
| | | | | | | | AppSideBar.tsx                   // Проект
| | | | | | | 📁 queries                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | notification.ts                   // Проект
| | | | | | | 📁 mutations                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | read-notifications.ts                   // Проект
| | | | | | | | delete-notifications.ts                   // Проект
| | | | | | | index.ts                   // Проект
| | | | | | | types.ts                   // Проект
| | | | | 📁 users                   // Проект
| | | | | | | 📁 comopnents                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | AppToolBar.tsx                   // Проект
| | | | | | | | AppSideBar.tsx                   // Проект
| | | | | | | 📁 queries                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | notification.ts                   // Проект
| | | | | | | 📁 mutations                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | read-notifications.ts                   // Проект
| | | | | | | | delete-notifications.ts                   // Проект
| | | | | | | index.ts                   // Проект
| | | | | | | types.ts                   // Проект
| | | | | | | utils.ts                   // Проект
| | | | | | 📁 features                   // Проект
| | | | | | | 📁 comopnents                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | ChangeAvatar.tsx                   // Проект
| | | | | | | | DeleteUserDialog.tsx                   // Проект
| | | | | | | 📁 queries                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | me.ts                   // Проект
| | | | | | | 📁 mutations                   // Проект
| | | | | | | | index.ts                   // Проект
| | | | | | | | change-avatar.ts                   // Проект
| | | | | | | index.ts                   // Проект
| | | | | | | types.ts                   // Проект
| | | | | | | utils.ts                   // Проект
| | | | | 📁 companies                   // Проект
| | | | middleware.ts
| | | .env.example                   // Проект
| | | .eslintrc.js                   // Проект
| | | nuxt-env.d.ts                   // Проект
| | | package.json                   // Проект
| | | postcss.config.js                   // Проект
| | | readme.md                   // Проект
| | | tsconfig.json                   // Проект
| | 📁 portal-e2e               // Проект e2e тестов
| 📁 packages
| | 📁 ui
| | | 📁 .storybook
| | | 📁 public
| | | 📁 src
| | | | 📁 assets
| | | | 📁 comopnents
| | | | 📁 mui
| | | | 📁 stories
| | | | 📁 types
| | | | index.ts
| | | | App.css
| | | | App.tsx
| | | | App.css
| | | package.json
| | | postcss.config.js
| | | readme.md
| | | tsconfig.json
| | | vite.config.ts
| | 📁 eslint-config
| | 📁 i18n
| | 📁 prettier-config
| | 📁 queries
| | | 📁 src
| | | | 📁 generated
| | | | exceptions.ts
| | | | index.ts
| | | | request-client.ts
| | | | request.hbs
| | | .eslintignore
| | | .eslintrc.cjs
| | | package.json
| | | tsconfig.json
| | 📁 typescript-config
| .dockerignore
| .eslintrc.js
| .npmrc
| .nvmrc
| .prettierrc.json
| Dockerfile
| Makefile
| package-lock.json
| package.json
| readme.md
| tsconfig.json
| turbo.json
```

В связи с представленной структурой проекта можно вывести правила названия файлов, а также содержащих их классов и других сущностей.

1. Название файлов

Название компонентов `tsx` назвается в нотации *CamelCase*:
```
AppToolBar.tsx      // ✅
app-tool-bar.tsx    // ❌
```

Название компонентов `ts` пишется в нотации *kebab-case*:
```
query-provider.ts   // ✅
QueryProvider.ts    // ❌
```

2. Постфикс

В папках, которые однозначно определяют сущность, например, `queries`, `providers` 
и т.д.


## Запуск приложения в режиме разработки

```bash
git clone ...
cp apps/portal/.env.example apps/portal/.env
npm install
npm run dev
```


## Принципы Solid

- **S** — принцип единственной ответственности (*Single Responsibility Principle*).
Каждый класс или модуль должен иметь одну и только одну причину для изменения, то есть отвечать за выполнение лишь одной определённой функциональности.
- **O** — принцип открытости/закрытости (*Open/Closed Principle*).
Программные сущности должны быть открыты для расширения, но закрыты для изменения.
- **L** — Принцип подстановки Лисков (*Liskov Substitution Principle*).
Объекты должны быть заменяемы экземплярами их подтипов без нарушения корректности работы программы.
- **I** — Принцип разделения интерфейса (*Interface Segregation Principle*).
Не следует заставлять клиентов зависеть от интерфейсов, которые они не используют.
- **D** — Принцип инверсии зависимостей (*Dependency Inversion Principle*).
Модули верхнего уровня не должны зависеть от модулей нижнего уровня; оба должны зависеть от абстракций.

Применение принципов `SOLID` способствует созданию устойчивой, масштабируемой и легко поддерживаемой программной системы, что особенно важно в условиях динамично меняющихся требований и технологий.


## Feature Oriented подход

Текущий подход можно назвать `feature oriented`, однако в кажой `feature` реализован
подход `fsd`.

В папке проекта, например, `{{ project name }}/apps/portal`создается дирректория 
`modules`, которая представляет из себя набор модулей, слабо связанных, которые должны быть независимы. Типовая схема модуля выглядит следующим образом:
```
📁 modules
| 📁 common
| | 📁 widgets
| | | 📁 comopnents
| | | | index.ts
| | | | AppToolBar.tsx
| | | | AppSideBar.tsx
| | | 📁 hooks
| | | | index.ts
| | | 📁 queries
| | | | index.ts
| | | | notification.ts
| | | 📁 mutations
| | | | index.ts
| | | | read-notifications.ts
| | | | delete-notifications.ts
| | | index.ts
| | | types.ts
| | 📁 features
| | | 📁 comopnents
| | | | index.ts
| | | | Notification.tsx
| | | 📁 queries
| | | | index.ts
| | | | notification.ts
| | | 📁 mutations
| | | | index.ts
| | | | read-notifications.ts
| | | | delete-notifications.ts
| | | index.ts
| | | types.ts
| | | utils.ts
```

В примере представлен пример модуля `common`, модуль, который предподалагает,
что его модули могут использоваться на всем сайте. Основные сущности модулей
следующие:
- widgets - объединения функционала, связанного с конкретной областью приложения, и обеспечивают его отображение;
- features - это минимальная, независимая единица функционала для совершения действия;
- entities - это минимальная единица, которая изолирует бизнес-логику и данные от внешнего интерфейса.

Правило использования иерархии остается также, как и в *fsd*:
- widgets;
- features;
- entities.

Аналогичному `shared` слою методологии *fsd* служит раздел `packages` проекта, основанного на
монорепе [turborepo](https://turbo.build/repo/docs).

Каждый из сервисов может содержать следующие компоненты:
- components - компоненты модуля, каждый из которых содержит один компонент;
- queries - список *react-query* запросов;
- mutations - список *reqct-query* мутаций;
- hooks - реализация кастомных хуков;
- utils - описание утилит предметной области, тех, в которых нет нужды в других модулях.

### Правила импорта

#### Импорты packages

Дирректория `packages` представляет собой shared слой всего приложения.
Из этого следует, что:
- внутри пакета импорт должен быть относительный,
- вну пакета импорт должен быть абсолютный и имплементировать только 
доступные кспортные сущности.

#### Импорты modules

Внутри каждого модуля лежат доменные области, а внутри доменных областей лежат сущности,
аналогичные сущностям методологии *fsd*.
Из чего следует правило:
- импорт доменных областей должен быть относительный;
- импорт из вне текущих доменных областей должен быть абсолютный.

Импорт между модулями **не запрещен**, однако, должен быть абсолютный и подчиняется 
правилам импорта как и внутри модуля, например:
- в модуле users сущность фичи users/features может экспортировать модуль common/features и common/entities, однако не может импортировать common/widgets;
- в модуле users сущность entities можнт экспортировать только сущность common/entities.


## Стили

Основной набор стилей наследуется путем указания темы из библиотеки MUI 6+](https://mui.com).

### Нативное преопределение стилей

### CSS Modules


Никаких других способов преопределения стилей, включая `styled-components` непридусмотрено.


## Dependency Inversion

## Гайд создания компонентов