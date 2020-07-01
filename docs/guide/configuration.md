# Конфигурация
Конфигурация движка хранится в нескольких скриптовых ассетах, расположенных в папке `Assets/NaninovelData/Resources/Naninovel/Configuration`. Они автоматически генерируются при первом открытии соответствующих меню конфигурации в редакторе Unity.

Используйте `Naninovel -> Configuration` или `Edit -> Project Settings -> Naninovel` для доступа к меню конфигурации.

Обратите внимание, что все меню конфигурации поддерживают [функцию пресетов Unity](https://docs.unity3d.com/Manual/Presets). Это может помочь в создании нескольких пресетов конфигурации при подготовке сборок для различных целевых платформ (например, мобильных, standalone?, консольных и т.д.).

[!55f5c74bfc16e1af2455034647525df3]

Можно изменять объекты конфигурации во время выполнения, добавлять новые пользовательские конфигурации и изменить способ доступа к объектам во время выполнения (например, считывать конфигурацию из JSON файлы, хранящихся на удаленном устройстве?); см. руководство по [пользовательским настройкам](/guide/custom-configuration.md) для получения дополнительной информации.

::: note
Данная информация о конфигурации? актуальна для [Naninovel v1. 10](https://github.com/Elringus/NaninovelWeb/releases).
:::

## Аудио

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Audio Loader | Audio- (Addressable, Project) | Отвечает за конфигурацию загрузчика ресурсов, используемого при работе с аудиоресурсами (BGM и SFX).
Voice Loader | Voice- (Addressable, Project) |  Отвечает за конфигурацию загрузчика ресурсов, используемого при работе с аудиоресурсами голосовой озвучки.
Default Master Volume | 1 | Уровень звукового мастер-канала при первом запуске игры.
Default Bgm Volume | 1 | Уровень канала BGM при первом запуске игры.
Default Sfx Volume | 1 | Уровень канала SFX при первом запуске игры.
Default Voice Volume | 1 | Уровень канала голосовой озвучки при первом запуске игры.
Enable Auto Voicing | False | При включении параметра каждая команда `PrintText` будет пытаться проиграть голосовой клип из `VoiceResourcesPrefix/ScriptName/LineIndex.ActionIndex`.
Voice Overlap Policy | Prevent Overlap | Диктует принципы обработки одновременно воспроизводящихся голосов:<br> • Allow Overlap – одновременно звучащие голоса будут воспроизводиться без ограничений.<br> • Prevent Overlap — предотвращает одновременное воспроизведение путем остановки воспроизводимого в данный момент голосового клипа перед воспроизведением нового.<br> • Prevent Character Overlap – предотвращает одновременное воспроизведение голосовых клипов для персонажа; голоса разных персонажей (автоматическое озвучивание) и любое количество команд [@voice] могут быть воспроизведены одновременно?.
Custom Audio Mixer | Null | Аудиомикшер для контроля аудиогрупп. По умолчанию использует стандартный, если никакого иного не предоставлено.
Master Volume Handle Name | Master Volume | Имя обработчика микшера (предоставленный параметр) для управления уровнем мастер-канала.
Bgm Group Path | Master/BGM | Путь к группе микшера для управления уровнем BGM канала. *(в англ. тексте опечатка – master volume?)*
Bgm Volume Handle Name | BGM Volume | Имя обработчика микшера (предоставленный параметр) для управления уровнем BGM канала.
Sfx Group Path | Master/SFX | Путь к группе микшера для управления уровнем SFX канала. *(в англ. тексте опечатка – background music?)*
Sfx Volume Handle Name | SFX Volume | Имя обработчика микшера (предоставленный параметр) для управления уровнем SFX канала.
Voice Group Path | Master/Voice | Путь к группе микшера для управления уровнем канала голосовой озвучки. *(в англ. тексте опечатка – sound effects?)*
Voice Volume Handle Name | Voice Volume | Имя обработчика микшера (предоставленный параметр) для управления уровнем канала озвучки.

</div>

## Фоны

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Default Metadata | Object Ref | Метаданные, используемые по умолчанию при создании фоновых акторов, и пользовательские метаданные для созданного актора с неуказанным ID?.
Metadata | Object Ref | Метаданные, используемые при создании фоновых акторов с конкретным ID.
Scene Origin | (0.5, 0.0) | Исходная точка, используемая для отсчета при позиционировании актеров в сцене.
Z Offset | 100 | Начальное смещение по оси Z (глубина) акторов относительно камеры, установленное при создании акторов.
Z Step | 0.1 | Расстояние по оси Z, установленное между акторами при их создании; используется для предотвращения проблем наложения.
Default Easing | Linear | Функция смягчения, используемая по умолчанию для всех анимаций модификации актора (изменение внешности, положения, оттенка и пр.). *(в англ. тексте опечатка – Eeasing?)*
Auto Show On Modify | True | Следует ли автоматически выводить (показывать) актора при выполнении команд модификации.

</div>

## Камера

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Reference Resolution | (1920, 1080) | Эталонное разрешение используется для оценки правильных размеров рендеринга, чтобы спрайтовые ассеты (например, фоны и персонажи) были правильно расположены на сцене. Как правило, должно быть установлено в значение, равное разрешению фоновых текстур, которые вы используете в игре.
Auto Correct Ortho Size | True | Следует ли автоматически корректировать ортогональный размер кадра на основе текущего соотношения сторон дисплея, чтобы обеспечить правильное расположение фонов и персонажей?.
Default Ortho Size | 5.35 | Ортогональный размер, устанавливаемый по умолчанию при отключенной автоматической коррекции.
Initial Position | (0.0, 0.0, -10.0) | Исходное положение камеры в пространстве.
Orthographic | True | Должна ли камера по умолчанию рендерить в ортогональном (включен) или перспективном (отключен) режиме. Не имеет эффекта, когда назначается пользовательский префаб камеры.
Custom Camera Prefab | Null | Префаб с компонентом камеры, используемой для рендеринга. Будет использоваться стандартный, если иного не указано. Если вы хотите настроить некоторые свойства камеры (цвет фона, FOV, HDR и т.д.) или добавить сценарии постобработки, создайте префаб с необходимой настройкой камеры и назначьте данный префаб этому полю.
Use UI Camera | True | Следует ли визуализировать пользовательский интерфейс в отдельной камере. Это позволит использовать раздельные конфигурации для основной камеры и камеры UI и предотвратить влияние эффектов постобработки (изображения) на пользовательский интерфейс за счет незначительных ресурсных расходов на рендеринг.
Custom UI Camera Prefab | Null | Префаб с компонентом камеры, используемой для рендеринга пользовательского интерфейса. Будет использоваться стандартный, если иного не указано. Не имеет никакого эффекта, если параметр `UseUICamera` отключен. *(в англ. тексте опечатка – пропущена точка в конце?)*
Default Easing | Linear | Функция смягчения, используемая по умолчанию для всех модификаций камеры (изменение масштаба, положение, поворот и пр.).
Thumbnail Resolution | (240, 140) | Разрешение, в котором будут захвачены миниатюры для превью в слотах сохранения.
Hide UI In Thumbnails | False | Игнорировать ли слой интерфейса при захвате миниатюр.

</div>

## Персонажи

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Auto Arrange On Add | True | Следует ли равномерно распределять персонажей по оси X при добавлении нового персонажа без заданной позиции.
Default Metadata | Object Ref | Метаданные, используемые по умолчанию при создании акторов персонажей; пользовательские метаданные для созданного актора без указанного ID.
Metadata | Object Ref | Метаданные, используемые при создании акторов персонажей с определенными ID.
Avatar Loader | Character Avatars- (Addressable, Project) | Конфигурация загрузчика ресурсов, используемого для ресурсов текстур аватаров персонажей.
Scene Origin | (0.5, 0.0) | Исходная точка отсчета при позиционировании акторов в сцене.
Z Offset | 50 | Начальное смещение по оси Z (глубина) акторов относительно камеры, установленное при создании акторов.
Z Step | 0.1 | Расстояние по оси Z, установленное между акторами при их создании; используется для предотвращения проблем наложения.
Default Easing | Smooth Step | Функция смягчения, используемая по умолчанию для всех анимаций модификации актора (изменение внешности, положения, оттенка и пр.). *(в англ. тексте опечатка – Eeasing?)*
Auto Show On Modify | True | Следует ли автоматически выводить (показывать) актора при выполнении команд модификации.

</div>

## Обработчики вариантов выбора

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Default Handler Id | Button List | ID обработчика выбора, используемое по умолчанию.
Default Metadata | Object Ref | Метаданные, используемые по умолчанию при создании акторов обработчиков; пользовательские метаданные для созданного актора без указанного ID.
Metadata | Object Ref | Метаданные, используемые при создании акторов обработчиков с определенными ID.
Default Easing | Linear | Функция смягчения, используемая по умолчанию для всех анимаций модификации актора (изменение внешности, положения, оттенка и пр.). *(в англ. тексте опечатка – Eeasing?)*
Auto Show On Modify | True | Следует ли автоматически выводить (показывать) актора при выполнении команд модификации.

</div>

## Пользовательские переменные

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Predefined Variables | Object Ref | Список переменных для инициализации по умолчанию. Глобальные переменные (имена которых начинаются с `G_` или `g_`) инициализируются при первом запуске приложения, а другие – при каждом сбросе состояния?.

</div>

## Движок

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Generated Data Path | Naninovel Data | Относительный (к каталогу данных приложения) путь для хранения автоматически сгенерированных ассетов.
Override Objects Layer | False | Следует ли назначать определенный слой всем объектам движка. Камера движка будет использовать слой для обрезной маски. Используйте это, чтобы изолировать объекты Naninovel от визуализации другими камерами.
Objects Layer | 0 | Если включен параметр `Override Objects Layer`, указанный слой будет присвоен всем объектам движка.
Async Exception Log Type | Error | Тип лога для исключений, связанных с UniTask.
Initialize On Application Load | True | Следует ли автоматически инициализировать движок при запуске приложения.
Show Initialization UI | True | Показывать ли UI загрузки во время инициализации движка.
Custom Initialization UI | Null | UI для отображения во время инициализации движка (если он включен). Будет использован стандартный, если иного не указано.
Show Title UI | True | Следует ли автоматически отображать UI титульного экрана (главное меню) после инициализации движка. Вы можете изменить пользовательский интерфейс титульного экрана с помощью функции настройки пользовательского интерфейса (см. онлайн-руководство для получения дополнительной информации).
Enable Development Console | True | Нужно ли включать консоль разработки.
Toggle Console Key | Back Quote | Клавиша, используемая для доступа к консоли разработки. Вы также можете включать консоль с помощью нескольких (3 или более) касаний при использовании сенсорных экранов.

</div>

## Ввод

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Spawn Event System | True | Следует ли создавать систему событий при инициализации.
Custom Event System | Null | Префаб с компонентом `EventSystem`, который будет создаваться для обработки входящих данных. Будет создан стандартный, если иного не указано.
Spawn Input Module | True | Следует ли создавать модуль ввода при инициализации.
Custom Input Module | Null | Префаб с компонентом `InputModule`, который будет создаваться для ввода данных. Будет создан стандартный, если иного не указано.
Touch Frequency Limit | 0.1 | Ограничивает частоту регистрируемых вводных команд, в секундах.
Process Legacy Bindings | True | Следует ли обрабатывать устаревшие вводные привязки. Отключите его, если вы используете новую систему ввода Unity и не хотите, чтобы устаревшие привязки работали в дополнение к действиям ввода?.
Bindings | Object Ref | Привязки для обработки вводных данных.

</div>

## Локализация

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Loader | Localization- (Addressable, Project) | Конфигурация загрузчика ресурсов, используемого для ресурсов локализации.
Source Locale | En | Локаль исходных ресурсов проекта (языка, на котором были созданы ресурсы проекта).
Default Locale | Null | Локаль, используемая по умолчанию при первом запуске игры. Будет использована `Source Locale`, если иная не указана.

</div>

## Управляемый текст?

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Loader | Text- (Addressable, Project) | Конфигурация загрузчика ресурсов, используемого для управляемых текстовых документов.

</div>

## Видеоролики

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Loader | Movies- (Addressable, Project) | Конфигурация загрузчика ресурсов, используемого для ресурсов видеороликов.
Skip On Input | True | Следует ли пропускать воспроизведение ролика при нажатии пользователем клавиши команды `cancel`. 
Skip Frames | True | Нужно ли пропускать кадры, чтобы догнать текущее время??. Whether to skip frames to catch up with current time.
Fade Duration | 1 | Время в секундах для плавного появления/затухания перед началом/окончанием воспроизведения ролика.
Custom Fade Texture | Null | Текстура для появления/затухания. Будет использована простая черная, если иная не задана.
Play Intro Movie | False | Следует ли автоматически воспроизводить интро-ролик после инициализации движка и перед отображением главного меню.
Intro Movie Name | Null | Путь к ресурсу интро-ролика.

</div>

## Провайдер ресурсов

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Resource Policy | Static | Диктует политику загрузки и выгрузки ресурсов во время выполнения скрипта:<br> • Статическая — все ресурсы, необходимые для выполнения сценария, предварительно загружаются при запуске воспроизведения (маскируются загрузочным экраном) и выгружаются только после завершения воспроизведения сценария. Эта политика используется по умолчанию и рекомендуется для большинства случаев.<br> • Динамическая — во время выполнения сценария предварительно загружаются только ресурсы, необходимые для выполнения следующих команд количеством, указанным в `Dynamic Policy Steps`, а все неиспользуемые ресурсы немедленно выгружаются. Используйте этот режим при работе с платформами со строгими ограничениями памяти и невозможностью правильно организовать сценарии Naninovel. При загрузке ресурсов в фоновом режиме во время игры могут наблюдаться сбои.
Dynamic Policy Steps | 25 | определяtb количество команд скрипта для предварительной загрузки, когда включена динамическая политика ресурсов.
Optimize Loading Priority | True | Понижает приоритет потока фоновой загрузки Unity, чтобы предотвратить сбои при загрузке ресурсов во время воспроизведения.
Log Resource Loading | False | Следует ли выводить операции загрузки ресурсов на экране загрузки.
Enable Build Processing | True | Следует ли регистрировать пользовательский обработчик воспроизведения для обработки ресурсов, назначенных в качестве ресурсов Naninovel.<br><br> Предупреждение: чтобы эта настройка вступила в силу, необходимо перезапустить редактор Unity.
Use Addressables | True | При установке адресируемой системы ассетов включение этого свойства оптимизирует этап обработки ассетов, уменьшая время загрузки?.
Auto Build Bundles | True | Следует ли автоматически создавать адресируемые пакеты ассетов при создании плеера. Не имеет эффекта, когда `Use Addressables` отключено.
Allow Addressable In Editor | False | Следует ли использовать адресируемый провайдер в редакторе. Включите его, если вы вручную назначаете ресурсы с помощью адресации?, а не назначаете их менеджерам ресурсов Naninovel. Имейте в виду, что включение этого может вызвать проблемы, когда ресурсы как назначаются в диспетчере ресурсов, так и регистрируются с помощью адресации, после чего переименовываются или дублируются. *(в англ. тексте опечатка – could cuase issues?)*
Extra Labels | Null | Адресируемый провайдер будет работать только с ассетами, которые имеют назначенные метки в дополнение к метке `Naninovel`. Может использоваться для фильтрации ассетов, используемых движком, на основе пользовательских критериев (например, HD и SD текстуры). 
Local Root Path | %DATA%/Resources | Корневой путь, используемый для локального провайдера ресурсов. Это может быть абсолютный путь к папке, в которой находятся ресурсы, или относительный путь с одним из доступных источников:<br> • %DATA% — папка игровых данных на целевом устройстве (UnityEngine.Application.dataPath).<br> • %PDATA% – каталог постоянных данных на целевом устройстве (UnityEngine.Application.persistentDataPath).<br> • %STREAM% - папка `StreamingAssets` (UnityEngine.Application.streamingAssetsPath).<br> • %SPECIAL{F}% - специальная папка ОС (где F – значение из System.Environment.SpecialFolder).
Project Root Path | Naninovel | Путь относительно папок `Resources`, в которых находятся специфические для Naninovel ассеты.
Google Drive Root Path | Resources | Корневой путь, используемый для провайдера ресурсов Google Drive.
Google Drive Request Limit | 2 | Максимум допустимых одновременных запросов при обращении к API Google Drive.
Google Drive Caching Policy | Smart | Политика кэша, используемая при загрузке ресурсов. `Smart` попытается использовать Changes API, чтобы проверить наличие изменений на диске. `PurgeAllOnInit` повторно загрузит все ресурсы при инициализации провайдера.

</div>

## Плеер сценариев

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Skip Time Scale | 10 | Шкала ускорения времени в режиме пропуска (быстрой перемотки вперед).
Min Auto Play Delay | 3 | Минимум секунд ожидания перед выполнением следующей команды в режиме авточтения.
Show Debug On Init | False | Нужно ли показывать окно отладки плеера при инициализации движка.

</div>

## Scripts

<div class="config-table">

Свойство | Значение по умолчанию | Описание
--- | --- | ---
Loader | Scripts- (Addressable, Project) | Конфигурация загрузчика ресурсов, используемого для ресурсов сценариев Naninovel.
Initialization Script | Null | Имя сценария, воспроизводимого сразу после инициализации движка.
Title Script | Null | Имя сценария, воспроизводимого при отображении титульного экрана. Может использоваться для настройки сцены титульного экрана (фон, музыка и т.д.). *(в англ. тексте опечатка – backgound?)*
Start Game Script | Null | Имя сценария, воспроизводимого при запуске новой игры. Будет использован первый доступный, если иного не указано.
Auto Add Scripts | True | Следует ли автоматически добавлять созданные сценарии Naninovel к ресурсам.
Hot Reload Scripts | True | Следует ли повторно загружать измененные (как с помощью визуального, так и внешнего редакторов) сценарии и применять изменения во время воспроизведения без перезапуска.
Count Total Commands | False | Нужно ли вычислять количество команд, существующих во всех доступных сценариях Naninovel при инициализации сервиса. Если вы не используете свойство `TotalCommandsCount` диспетчера сценариев и функцию `CalculateProgress` в выражениях сценариев Naninovel, отключите данное свойство, чтобы сократить время инициализации движка.
Enable Visual Editor | True | Whether to show visual script editor when a script is selected.
Hide Unused Parameters | True | Whether to hide un-assigned parameters of the command lines when the line is not hovered or focused.
Insert Line Key | Space | Hot key used to show `Insert Line` window when the visual editor is in focus. Set to `None` to disable.
Insert Line Modifier | Control | Modifier for the `Insert Line Key`. Set to `None` to disable.
Save Script Key | S | Hot key used to save (serialize) the edited script when the visual editor is in focus. Set to `None` to disable.
Save Script Modifier | Control | Modifier for the `Save Script Key`. Set to `None` to disable.
Editor Page Length | 1000 | How many script lines should be rendered per visual editor page.
Editor Custom Style Sheet | Null | Allows modifying default style of the visual editor.
Graph Orientation | Horizontal | Whether to build the graph vertically or horizontally.
Graph Auto Align Padding | (10.0, 0.0) | Padding to add for each node when performing auto align.
Graph Custom Style Sheet | Null | Allows modifying default style of the script graph.
Enable Community Modding | False | Whether to allow adding external naninovel scripts to the build.
External Loader | Scripts- (Local) | Configuration of the resource loader used with external naninovel script resources.
Enable Navigator | True | Whether to initializte script navigator to browse available naninovel scripts.
Show Navigator On Init | False | Whether to show naninovel script navigator when script manager is initialized.
Navigator Sort Order | 900 | UI sort order of the script navigator.

</div>

## Spawn

<div class="config-table">

Property | Default Value | Description
--- | --- | ---
Loader | Spawn- (Addressable, Project) | Configuration of the resource loader used with spawn resources.

</div>

## State

<div class="config-table">

Property | Default Value | Description
--- | --- | ---
Save Folder Name | Saves | The folder will be created in the game data folder.
Default Settings Slot Id | Settings | The name of the settings save file.
Default Global Slot Id | Global Save | The name of the global save file.
Save Slot Mask | Game Save{0:000} | Mask used to name save slots.
Quick Save Slot Mask | Game Quick Save{0:000} | Mask used to name quick save slots.
Save Slot Limit | 99 | Maximum number of save slots.
Quick Save Slot Limit | 18 | Maximum number of quick save slots.
Binary Save Files | True | Whether to compress and store the saves as binary files (.nson) instead of text files (.json). This will significantly reduce the files size and make them harder to edit (to prevent cheating), but will consume more memory and CPU time when saving and loading.
Load Start Delay | 0.3 | Seconds to wait before starting load operations; used to allow pre-load animations to complete before any load-related stutters could happen.
Reset On Goto | True | Whether to reset state of the engine services and unload (dispose) resources when loading another script via [@goto] command. It's recommended to leave this enabled to prevent memory leak issues. If you choose to disable this option, you can still reset the state and dispose resources manually at any time using [@resetState] command.
Enable State Rollback | True | Whether to enable state rollback feature allowing player to rewind the script backwards.
State Rollback Steps | 1024 | The number of state snapshots to keep at runtime; determines how far back the rollback (rewind) can be performed. Increasing this value will consume more memory.
Saved Rollback Steps | 128 | The number of state snapshots to serialize (save) under the save game slots; determines how far back the rollback can be performed after loading a saved game. Increasing this value will enlarge save game files.
Game State Handler | Naninovel.IO Game State Slot Manager, Elringus.Naninovel.Runtime, Version=0.0.0.0, Culture=neutral, Public Key Token=null | Implementation responsible for de-/serializing local (session-specific) game state; see `State Management` guide on how to add custom serialization handlers.
Global State Handler | Naninovel.IO Global State Slot Manager, Elringus.Naninovel.Runtime, Version=0.0.0.0, Culture=neutral, Public Key Token=null | Implementation responsible for de-/serializing global game state; see `State Management` guide on how to add custom serialization handlers.
Settings State Handler | Naninovel.IO Settings Slot Manager, Elringus.Naninovel.Runtime, Version=0.0.0.0, Culture=neutral, Public Key Token=null | Implementation responsible for de-/serializing game settings; see `State Management` guide on how to add custom serialization handlers.

</div>

## Text Printers

<div class="config-table">

Property | Default Value | Description
--- | --- | ---
Default Printer Id | Dialogue | ID of the text printer to use by default.
Max Reveal Delay | 0.06 | Delay limit (in seconds) when revealing (printing) the text messages. Specific reveal speed is set via `message speed` in the game settings; this value defines the available range (higher the value, lower the reveal speed).
Max Auto Wait Delay | 0.02 | Delay limit (in seconds) per each printed character while waiting to continue in auto play mode. Specific delay is set via `auto delay` in the game settings; this value defines the available range.
Scale Auto Wait | True | Whether to scale the wait time in auto play mode by the reveal speed set in the print commands.
Default Metadata | Object Ref | Metadata to use by default when creating text printer actors and custom metadata for the created actor ID doesn't exist.
Metadata | Object Ref | Metadata to use when creating text printer actors with specific IDs.
Scene Origin | (0.5, 0.0) | Origin point used for reference when positioning actors on scene.
Z Offset | 100 | Initial Z-axis offset (depth) from actors to the camera to set when the actors are created.
Z Step | 0.1 | Distance by Z-axis to set between the actors when they are created; used to prevent z-fighting issues.
Default Easing | Linear | Eeasing function to use by default for all the actor modification animations (changing appearance, position, tint, etc).
Auto Show On Modify | False | Whether to automatically reveal (show) an actor when executing modification commands.

</div>

## UI

<div class="config-table">

Property | Default Value | Description
--- | --- | ---
Loader | UI- (Addressable, Project) | Configuration of the resource loader used with UI resources.
Objects Layer | 5 | The layer to assign for the UI elements instatiated by the engine. Used to cull the UI when using `toogle UI` feature.
Render Mode | Screen Space Camera | The canvas render mode to apply for all the managed UI elements.
Sorting Offset | 1 | The sorting offset to apply for all the managed UI elements.

</div>

## Unlockables

<div class="config-table">

Property | Default Value | Description
--- | --- | ---
Loader | Unlockables- (Addressable, Project) | Configuration of the resource loader used with unlockable resources.

</div>

