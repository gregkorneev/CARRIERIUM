# Город профессий

<p align="center">
  <img src="assets/branding/careerium-logo.png" alt="Логотип CARRIERIUM" width="200">
</p>

Многопользовательская браузерная 3D-игра о профессиях. Игрок исследует центральную площадь, заходит в профессиональные станции, выполняет практические задания и разговаривает с наставниками. Проект рассчитан на один сервер в локальной сети или игру на том же компьютере.

**Состояние:** работающий демонстрационный прототип. В него входят центральный город и пять станций: пожарная часть, пекарня, цветочная мастерская, радио и музыкальная школа. Текущая архитектура, карта Wiki и границы реализации описаны в [GitHub Wiki](https://github.com/gregkorneev/kidburg/wiki).

## Скриншоты

<table>
  <tbody>
    <tr>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-overview-se.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-overview-se.png" alt="Центральная площадь" width="100%"></a><br><sub>Центральная площадь</sub></td>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-b-fire.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-b-fire.png" alt="Пожарная часть" width="100%"></a><br><sub>Пожарная часть</sub></td>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-b-bakery.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-b-bakery.png" alt="Пекарня" width="100%"></a><br><sub>Пекарня</sub></td>
    </tr>
    <tr>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-b-flower.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-b-flower.png" alt="Цветочная мастерская" width="100%"></a><br><sub>Цветочная мастерская</sub></td>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-b-radio.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-b-radio.png" alt="Радио" width="100%"></a><br><sub>Радио</sub></td>
      <td align="center" width="33%"><a href="docs/graphics-overhaul/plaza-screenshots/after-b-music.png"><img src="docs/graphics-overhaul/plaza-screenshots/after-b-music.png" alt="Музыкальная школа" width="100%"></a><br><sub>Музыкальная школа</sub></td>
    </tr>
  </tbody>
</table>

## Возможности

- 3D-клиент на Three.js и Preact, сервер Node.js на Fastify.
- Вход игроков, одноразовый WebSocket ticket, перемещение и синхронизация в локальной сети.
- Загрузка города чанками с ограничением кэша клиента.
- Пять профессиональных заданий с серверной проверкой и сохранением прогресса в SQLite.
- Анимированные механизмы, интерактивные объекты, NPC и общий внешний вид персонажа между локациями.
- Локальные AI-диалоги наставников и сценарии радио через Ollama. AI необязателен: без модели игра запускается, но ответы наставников и генерация не работают.
- Control Center для администрирования подключений, мира и настроек агентов.
- Локальные ресурсы клиента: во время игры не используются CDN, внешние шрифты, аналитика или облачные AI-сервисы.

## Требования

- Node.js 22 или новее.
- npm.
- Современный браузер с поддержкой WebGL.
- Для многопользовательской игры — локальная сеть между клиентами и сервером.

## Установка и запуск

Для первичной установки из GitHub:

```bash
git clone https://github.com/gregkorneev/kidburg.git
cd kidburg
npm ci
npm run setup
npm run world:build
npm run build
npm start
```

Сервер напечатает адрес для этого компьютера и адрес локальной сети, а также код комнаты. На серверном компьютере откройте `http://localhost:3000`; остальные игроки открывают адрес сервера из локальной сети и вводят код комнаты. Для первого запуска нужны установленные npm-пакеты и сгенерированные файлы мира.

### Обновить и запустить серверную копию

```bash
cd /private/tmp/kidburg-main-0282b62

git fetch origin main

git switch --detach origin/main

npm ci

npm run build

ADMIN_PASSWORD=admin1234 OLLAMA_MODEL=Qwen3:latest npm start
```

Игра не обращается к внешним сайтам во время работы. Для автономной демонстрации заранее подготовьте npm-зависимости, файлы мира и, если нужен AI, Ollama с установленной моделью.

### Локальные AI-ответы

Установите Ollama на серверный компьютер, загрузите модель и укажите её имя:

```bash
ollama pull ИМЯ_МОДЕЛИ
OLLAMA_MODEL=ИМЯ_МОДЕЛИ npm start
```

Замените `ИМЯ_МОДЕЛИ` на имя из `ollama list`. Игровой сервер принимает только локальный адрес Ollama; запросы не отправляются в облако.

### Control Center

Чтобы открыть `/admin/`, задайте пароль администратора не короче восьми символов и запустите сервер:

```bash
ADMIN_PASSWORD='замените-на-свой-пароль' npm start
```

Удалённый доступ к панели по локальной сети выключен по умолчанию. Параметры доступа, аудит действий и редактор мира описаны в [документации Control Center](docs/admin-control-center/README.md). Для ручных проверок используйте отдельную временную базу через `DB_PATH`, а не рабочий файл `data/city.db`.

## Проверки и команды

| Команда | Для чего нужна |
| --- | --- |
| `npm run verify` | Запустить тесты, поднять сервер на случайном порту и проверить `/health` |
| `npm test` | Запустить тесты Node.js |
| `npm run build` | Собрать клиентские страницы |
| `npm run check:offline` | Проверить отсутствие внешних адресов в клиенте |
| `npm run world:build` | Создать данные центрального города |
| `npm run world:fixtures` | Создать фикстуры мира для разработки |
| `npm run dev` | Запустить сервер и Vite для разработки |
| `npm run bots` | Подключить клиентских ботов для локальных сценариев |
| `npm run agent-chat` | Открыть консольный интерфейс AI-наставника |

Если в shell задан `ADMIN_PASSWORD`, запустите тестовую проверку без этой переменной: `env -u ADMIN_PASSWORD npm run verify`. В окружении без неё достаточно `npm run verify`. Перед коммитом проекта запускайте проверку.

## Структура

| Путь | Содержание |
| --- | --- |
| `client/` | браузерный 3D-клиент, игровой UI, станции и Control Center |
| `server/` | HTTP/WebSocket, авторизация, задания, AI, мир и SQLite |
| `shared/` | общие контракты и форматы для клиента и сервера |
| `content/` | материалы и игровые данные станций |
| `assets/` | локальные ресурсы |
| `tools/` | генератор города и инструменты разработки |
| `tests/` | автоматические тесты Node.js |
| `docs/` | инструкции, проектный контекст и описание подсистем |
| `../kidburg.wiki/` | Клон GitHub Wiki после `npm run wiki:pull`; его можно открыть как Obsidian vault |

Начать чтение Wiki можно с [Home](https://github.com/gregkorneev/kidburg/wiki). В ней есть [карта проекта](https://github.com/gregkorneev/kidburg/wiki/Project-Map) и [инструкция для Obsidian](https://github.com/gregkorneev/kidburg/wiki/Obsidian-Guide).

## Границы текущего прототипа

Игра предназначена для одного сервера в локальной сети. В этой версии нет общего текстового чата игроков, анкеты интересов, рекомендаций профессий, портфолио достижений и WebXR. Для продолжения разработки прочитайте [`AGENTS.md`](AGENTS.md) и [указатель документации](docs/README.md).
