# LeafDank: исходники для изучения

## Состав

- Основной Android-клиент: исходные файлы `app/` из форка [DankChat](https://github.com/flex3r/DankChat), ветка `develop`.
- Дополнительный референс: `references/leafyrino/` — Git-субмодуль [Leafyrino](https://github.com/Leafyzito/leafyrino), ветка `leafyrino`, **закреплённый коммит `de8cd64fe2f011448c70ac2b46cd88193bc47089`** (1 ноября 2026).
- Leafyrino — настольное приложение на C++/Qt. Субмодуль не включён в Gradle и не предназначен для компиляции внутри APK.
- Лицензия Leafyrino: MIT (см. `references/leafyrino/LICENSE`). При переносе отдельных фрагментов необходимо сохранять уведомления и проверять сторонние лицензии.

## Подготовка локального клона

Новый клон без загрузки вложенных C++ библиотек:

```bash
git clone https://github.com/Akseleron/LeafDank.git
cd LeafDank
git submodule update --init references/leafyrino
```

Если репозиторий уже склонирован:

```bash
git submodule update --init references/leafyrino
```

Leafyrino сам использует многочисленные вложенные Git-субмодули. Для **чтения исходников они не нужны**. Только если возникнет доказанная потребность собрать Leafyrino на ПК, после проверки дискового пространства и зависимостей:

```bash
git submodule update --init --recursive references/leafyrino
```

Внимание: режим `--recursive` дополнительно скачает чужие библиотеки. Сборка LeafDank не должна зависеть от этой команды.

## Источники ключевых функций Leafyrino

| Функция | Исходный файл относительно `references/leafyrino/` |
| --- | --- |
| Расширенная авторизация / аккаунты | `src/providers/moltorino/MoltorinoAuth.cpp` и `src/widgets/dialogs/MoltorinoAuthPage.cpp` |
| Бейджи и настройки карточек | `src/widgets/settingspages/LeafyrinoPage.cpp` и `src/providers/*/*Badges.cpp` |
| Баллы канала и события | `src/providers/twitch/TwitchChannel.cpp` |
| Награды и GraphQL-операции | `src/providers/twitch/api/TwitchGql.cpp` |
| Меню баллов | `src/widgets/dialogs/ChannelPointsDialog.cpp` |
| Опросы и прогнозы | `src/controllers/commands/builtin/twitch/Poll.cpp` и `Prediction.cpp` |
| Набор расширенных настроек | `src/widgets/settingspages/MoltorinoPage.cpp` и `GeneralPage.cpp` |

**Важно:** внутренний GraphQL Twitch и чужие OAuth Client ID из исходников не являются публичным контрактом. Legacy Login с исполнением браузерного скрипта не переносить. Исследование вариантов API и безопасность авторизации обязательны до реализации.

## Правила работы с референсом

1. Leafyrino хранится как **неизменяемый исходный референс**. Не вносить правки в файлы субмодуля.
2. Перед использованием алгоритма фиксировать путь и SHA исходного файла, проверить лицензию и осмысленно адаптировать его под Kotlin/Compose.
3. Не добавлять CMake/Qt, C++-библиотеки и вспомогательные платформенные файлы в Android-сборку.
4. Обновлять закреплённую ревизию только отдельным изменением после сравнения upstream, проверки рисков и разрешения владельца.
5. При отсутствии `references/leafyrino/src/` в среде Codex сначала инициализировать субмодуль командой выше, **не заявлять, что исходники отсутствуют**.

## Обновление исходного референса

```bash
git -C references/leafyrino fetch origin leafyrino
git -C references/leafyrino log --oneline HEAD..origin/leafyrino
```

После просмотра изменений можно отдельно согласовать перевод на новый commit. Никогда не делать `git submodule update --remote` автоматически.

## Влияние на Android

`references/leafyrino/` не является исходным кодом `app`, не входит в APK и не должен обрабатываться Kotlin-линтерами или Gradle. CI может проверять Android без инициализации субмодуля. Для задач по переносу функций Codex инициализирует только референс.
