# Архитектура LeafDank — проектное решение

**Статус:** дизайн; эти модули не реализованы и их имена пока не обязательны для кода.

## Проверенная база
- DankChat `develop`: Android, Kotlin, Compose; `app/src/main/kotlin/com/flxrs/dankchat/`.
- Настройки: `preferences/chat/ChatSettings.kt`, `preferences/appearance/AppearanceSettings.kt`.
- Сообщения и новые Twitch GIF: `data/twitch/message/PrivMessage.kt`, `data/twitch/message/TwitchGif.kt`, `ui/chat/messages/TwitchGifContent.kt`.
- Рендер/маппинг: `ui/chat/ChatMessageMapper.kt`; сообщения и каналы: существующие `data/repo/chat` и `ui/chat`.
- Исходный Leafyrino — C++/Qt, без прямого встраивания его UI в Android.

## Логическая схема

```text
Twitch / 7TV / FFZ / BTTV
        |
  Существующие сервисы DankChat
        |
 Единое состояние сообщений, каналов, авторизации
        |
   Render/UI contracts + настройки
        +-- Classic (поведение/внешность прежнего DankChat)
        +-- Modern (текущая Compose-версия)
        +-- Custom (настройки компонентов)
        |
 Опциональные feature adapters
        +-- badges / user card / filters
        +-- points / rewards / polls / predictions (после анализа API)
```

## Classic, Modern, Custom
Старый интерфейс берётся из проверенной истории upstream, не по памяти и не по неофициальным APK. На первом этапе ищем его компоненты и оцениваем, что лучше воспроизвести средствами Compose. Конкретные компоненты: вкладки, шапка чата, сообщения, поле ввода, эмоты, usercard. UI-переключение не размножает подключение/состояние/базу.

Настройки: `effective = channelOverride ?: profileSetting ?: defaultSetting`. Переопределения версионируются и мигрируют. Экспорт исключает секреты.

## Расширенные функции
Изученные референсы Leafyrino:
- `src/providers/moltorino/MoltorinoAuth.cpp` — отдельные авторизации и выбор токена по разрешениям.
- `src/providers/twitch/api/TwitchGql.cpp` — внутренние Twitch GraphQL-операции и работа с наградами.
- `src/providers/twitch/TwitchChannel.cpp` — баланс/события каналов.
- `src/widgets/settingspages/LeafyrinoPage.cpp` — богатые настройки бейджей, карточек и отображения.

Каждой интеграции нужны состояния: Disabled / RequiresAuth / Loading / Available / Unavailable / Error. Ошибка внешней функции не должна блокировать чат. Никаких обязательств использовать внутренний Twitch GraphQL до проверки правил и совместимости.

## Производительность
Нельзя переносить 3 ГБ потребления ПК Leafyrino на Android как прогноз. Проводим baseline тесты: обычный чат, 3 активных канала, канал с высокой частотой сообщений, анимированные 7TV/GIF, работа в фоне, повторное подключение. Замеряем PSS, cold start, frame time, battery drain, размер и рост кеша. Rust только если профилирование обоснует отдельную нативную библиотеку.

## Обновления и брендинг
Следить за `flex3r/DankChat` как upstream, не удалять его тестовые/сборочные инструменты. Новый applicationId, signing и OAuth Client ID внедряются отдельной проверяемой задачей. Сохранить лицензии, локализацию и возможность отката.
