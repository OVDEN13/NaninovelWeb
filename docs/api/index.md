---
sidebar: auto
---

# Справочник по API 

Справочник по API командам сценария. Используйте боковую панель для быстрой навигации между доступными командами. 

~~Зачеркивание~~ обозначает безымянный параметр, а **жирный** - обязательный параметр; остальные параметры следует считать необязательными. Обратитесь к [руководству по скриптам naninovell](/guide/naninovel-scripts.md) если вы не знаете, что это такое.

Следующие параметры поддерживаются всеми командами сценария:

<div class=config-table>

ID | Тип | Описание
--- | --- | ---
if | Строка |  Логическое [выражение сценария](/guide/script-expressions.md), контролирующее, должна ли команда выполняться.
wait | Boolean | Должен ли проигрыватель сценариев дождаться завершения асинхронной команды перед выполнением следующей. Не действует, когда команда выполняется мгновенно.

</div>

::: note
Эта ссылка API действительна для [Naninovel v1.10](https://github.com/Elringus/NaninovelWeb/releases).
:::

## animate

#### Краткое описание
Команда анимации актеров с указанными ID с помощью ключевых кадров. Ключевые кадры для параметры анимации отделяются литералами `|`.

#### Примечание
Помните, что эта команда ищет актеров с предоставленными ID по всем менеджерам актеров, и в случае, если существует несколько актеров с одинаковым ID (например, персонаж и текстовый принтер), это повлияет только на первый найденный.  <br /><br />
При параллельном запуске команд animate (значение «wait» установлено в false) состояние затронутых актеров может изменяться непредсказуемо. Это может привести к неожиданным результатам при откате или выполнении других команд, которые влияют на состояние актера. Обязательно сбрасывайте затронутые свойства анимированных актеров (положение, оттенок, внешний вид и т. Д.) После завершения команды или используйте `@animate CharacterId` (без каких-либо аргументов), чтобы преждевременно остановить анимацию.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | ID актеров для анимации.
loop | Boolean | Следует ли зацикливать анимацию;убедитесь, что для `wait` установлено значение false, когда включен цикл, в противном случае воспроизведение сценария будет повторяться бесконечно.
appearance | Строка | Установка внешности для анимированных актеров.
transition | Строка | Тип [эффекта перехода](/guide/transition-effects.md) который будет использоваться при изменении внешнего вида (по умолчанию используется кроссфейд).
visibility | Строка | Статус видимости для анимированных актеров.
posX | Строка | Значения позиции анимированного актера, по оси Х (в диапазоне от 0 до 100, в процентах от левой границы экрана).
posY | Строка | Значения позиции анимированного актера, по оси Y (в диапазоне от 0 до 100, в процентах от нижней границы экрана).
posZ | Строка | Значения позиции анимированного актера, по оси Z (в мировом пространстве); В орто-режиме, ожно использовать только для сортировки.
rotation | Строка | Значения поворота (по оси Z) для анимированных актеров.
scale | Строка | Значения шкалы (единообразные) для анимированных актеров.
tint | Строка | Оттенок цвета для анимированных актеров.  <br /><br />  Строки, начинающиеся с `#` будет проанализирован как шестнадцатеричный следующим образом:  `#RGB` (становится RRGGBB), `#RRGGBB`, `#RGBA` (становится RRGGBBAA), `#RRGGBBAA`; Если альфа не указана, по значение по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как цвета, поддерживаются:  red, cyan, blue, darkblue, lightblue, purple, yellow, lime, fuchsia, white, silver, grey, black, orange, brown, maroon, green, olive, navy, teal, aqua, magenta.
easing | Строка | Названия функций замедления, используемых для анимации. <br /><br />  Доступные варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актера.
time | Строка | Продолжительность ключа анимации в секундах. Если значение ключа отсутствует, будет использоваться значение из предыдущего ключа. Если значение не установлено, будет использовать задержка 0.35 секунд для всех ключей.

</div>

#### Пример
```
; Анимация актера "Kohaku" за три шага анимации (ключевые кадры),
; смена позиций: первый шаг займет 1, второй - 0.5 и третий - 3 секунды.
@animate Kohaku posX:50|0|85 time:1|0.5|3

; Старт цикла анимации актеров "Yuko" и "Kohaku"; обратите внимание, что вы можете пропустить
; ключевые значения, указывающие, что параметр не должен изменяться на этапе анимации.
@animate Kohaku,Yuko loop:true appearance:Surprise|Sad|Default|Angry transition:DropFade|Ripple|Pixelate posX:15|85|50 posY:0|-25|-85 scale:1|1.25|1.85 tint:#25f1f8|lightblue|#ffffff|olive easing:EaseInBounce|EaseInQuad time:3|2|1|0.5 wait:false
...
; Остановка анимации.
@animate Yuko,Kohaku loop:false

; Запуск длинной фоновой анимации для `Kohaku`.
@animate Kohaku posX:90|0|90 scale:1|2|1 time:10 wait:false
; Вы можете сделать что-нибудь еще, пока анимация запущена.
...
; Здесь мы собираемся установить конкретную позицию для персонажа,
; но анимация все еще может работать в фоновом режиме, поэтому сначала сбросьте ее.
@animate Kohaku
; Теперь можно безопасно менять свойства, которые ранее были анимированны
@char Kohaku pos:50 scale:1
```

## append

#### Краткое описание
Добавляет предоставленный текст к текстовому принтеру.

#### Примечание
Весь текст будет добавлен немедленно, без эффекта постепенного проявления и прочих эффектов.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Безымянный параметр: значение следует указывать после ID команды без указания ID параметра. Обязательный параметр: параметр должен всегда указываться">Text</span> | Строка | Текст для добавления.
printer | Строка | ID актера принтера, который будет использован. Если параметр не указан, будет использован принтер по умолчанию.
author | Строка | ID актера, который должен быть связан с добавленым текстом.

</div>

#### Пример
```
; Напечатайте первую часть предложения как обычно (постепенно проявляя сообщение)
; затем добавьте конец предложения сразу.
Lorem ipsum
@append " dolor sit amet."
```

## arrange

#### Краткое описание
Упорядочивает указанных персонажей по оси X. Если параметры не указаны, будет выполнено автоматическое выравнивание, равномерно распределяющее видимых персонажей по оси X.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">CharacterPositions</span> | List&lt;Named&lt;Decimal&gt;&gt; | Коллекция ID персонажей по оси Х на сцене (относительно левой границы экрана, в процентах) с именованными значениями.  Позиция 0 относится к левой границе, а 100 к правой границе экрана; 50 это центр.
look | Boolean | При выполнении автоматической аранжировки определяет, следует ли также заставлять персонажей смотреть в начало сцены (по умолчанию включено).
time | Decimal | Продолжительность (в секундах) анимации аранжировки. Значение по умолчанию: 0.35 секунды.

</div>

#### Пример
```
; Равномерно распределить всех видимых персонажей 
@arrange

; Переместить персонажей с идентификаторами `Jenna` в 15%,` Felix` в 50% и `Mia` в 85%
; от левой границы экрана.
@arrange Jenna.15,Felix.50,Mia.85
```

## back

#### Краткое описание
Изменяет [актер фона](/guide/backgrounds.md).

#### Примечание
Фоны обрабатываются немного иначе, нежели персонажи, чтобы лучше приспособиться к традиционному течению визуальных новелл. Большую часть времени вы, вероятно, будете иметь одного фонового актора в сцене, который будет постоянно менять внешности. Чтобы избавиться от лишнего повторения одного и того же ID актора в сценариях, можно предоставить только внешность фона и тип перехода (необязательно) в качестве безымянного параметра, что автоматически преобразует актора "MainBackground". Если это не так, ID фонового актора может быть конкретно указан с помощью параметра `id`:

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">AppearanceAndTransition</span> | Named&lt;String&gt; | Внешний вид (или [поза](/guide/backgrounds.md#poses)) для фона и типа использованного [эффекта перехода](/guide/transition-effects.md). Если переход не предусмотрен, по умолчанию будет использоваться эффект перекрестного затухания.
pos | List&lt;Decimal&gt; | Положение (относительно границ экрана, в процентах), актера. Позиция описывается следующим образом: «0.0» - левый нижний угол, «50.50» - центр, а «100.100» - верхний правый угол экрана. Используйте Z-компонент (третий параметр, например, `,,10`) для перемещения (сортировки) по глубине в режиме орто.
id | Строка | Идентификатор актера для изменения; укажите `*`, чтобы повлиять на всех видимых актеров.
appearance | Строка | Устанавливает внешний вид или позу, изменяймому актеру.
transition | Строка | Тип используемого [эффекта перехода](/guide/transition-effects.md) (по умолчанию используется кроссфейд).
params | List&lt;Decimal&gt; | Параметры эффекта перехода.
dissolve | String | Путь к текстуре [нестандартное растворение](/guide/transition-effects.md#custom-transition-effects) (путь должен указываться относительно папки `Resources`). Действует только когда переход установлен в режим «Пользовательский».
visible | Boolean | Статус видимости для измененного актера.
position | List&lt;Decimal&gt; | Положение (в мировом пространстве) для измененного актера. Используйте Z-компонент (третий параметр) для перемещения (сортировки) по глубине в режиме орто.
rotation | List&lt;Decimal&gt; | Вращение для измененного актера.
scale | List&lt;Decimal&gt; | Масштаб для измененного актера.
tint | String | Цвет оттенка для измененного актера.  <br /><br />  Строки, начинающиеся с `#`, будут анализироваться как шестнадцатеричные следующим образом:  `#RGB` (становится RRGGBB), `#RRGGBB`, `#RGBA` (становится RRGGBBAA), `#RRGGBBAA`; если альфа не указана, по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как буквенные цвета, поддерживаются следующие параметры:  красный, голубой, синий, темно-синий, светло-синий, фиолетовый, желтый, салатовый, фуксия, белый, серебристый, серый, черный, оранжевый, коричневый, темно-бордовый, зеленый, оливковый, темно-синий, бирюзовый, аква, пурпурный.
easing | String | Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актера.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Пример
```
; Установите `River` в качестве появления основного фона
@back River

; То же, что и выше, но также используется эффект перехода `RadialBlur`
@back River.RadialBlur

; Подкрасьте все видимые фоны на сцене.
@back id:* tint:#ffdc22

; С учетом SFX `ExplosionSound` и фона` ExplosionSprite` следующая последовательность сценариев будет имитировать два взрыва, появляющихся далеко и близко к камере.
@sfx ExplosionSound volume:0.1
@back id:ExplosionSprite scale:0.3 pos:55,60 time:0 isVisible:false
@back id:ExplosionSprite
@fx ShakeBackground params:,1
@hide ExplosionSprite
@sfx ExplosionSound volume:1.5
@back id:ExplosionSprite pos:65 scale:1
@fx ShakeBackground params:,3
@hide ExplosionSprite
```

## bgm

#### Краткое описание
Воспроизведение или изменение текущей воспроизводимой дорожки[BGM (background music)](guideaudio.md#background-music) с указанным именем.

#### Примечания
Музыкальные треки зациклены по умолчанию. Если название музыкальной дорожки (BgmPath) не указано, будут затронуты все воспроизводимые в данный момент дорожки.  При вызове для дорожки, которая уже воспроизводится, воспроизведение не будет затронуто (дорожка не начнет воспроизводиться с самого начала), но будут применены указанные параметры (громкость и зацикленность дорожки).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">BgmPath</span> | String | Путь к музыкальной дорожке для воспроизведения.
intro | String | Путь к вступительной музыкальной дорожке, которую нужно воспроизвести один раз перед основной дорожкой (не зависит от параметра цикла).
volume | Decimal | Объем музыкальной дорожки.
loop | Boolean | Играть ли дорожку с начала, когда она заканчивается.
fade | Decimal | Длительность постепенного увеличения громкости при запуске воспроизведения, в секундах (по умолчанию 0.0); не действует при изменении воспроизводимой дорожки.
group | String | Аудио микшер [групповой путь](httpsdocs.unity3d.comScriptReferenceAudio.AudioMixer.FindMatchingGroups) это следует использовать при воспроизведении аудио.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию 0.35 секунды.

</div>

#### Пример
```
; Начинает воспроизведение музыкальной дорожки с названием `Sanctuary` в цикле
@bgm Sanctuary

; То же, что и выше, но затухание громкости более 10 секунд и воспроизведение только один раз
@bgm Sanctuary fade:10 loop:false

; Изменяет громкость всех воспроизводимых музыкальных треков до 50% в течение 2,5 секунд и заставляет их воспроизводиться в цикле
@bgm volume:0.5 loop:true time:2.5

; Играет в BattleThemeIntro один раз, а затем сразу в BattleThemeMain.
@bgm BattleThemeMain intro:BattleThemeIntro
```

## br

#### Краткое описание
Добавляет разрыв строки в текстовый принтер.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Count</span> | Integer | Число добавляемых разрывов строк.
printer | String | ID актера принтера для использования. Если не задано, будет использовать значение по умолчанию.
author | String | ID Идентификатор актера, который должен быть связан с добавленным переводом строки.

</div>

#### Пример
```
; Второе предложение будет напечатано в новой строке
Lorem ipsum dolor sit amet.[br]Consectetur adipiscing elit.

; Второе предложение будет печататься двумя строками под первым
Lorem ipsum dolor sit amet.[br 2]Consectetur adipiscing elit.
```

## camera

#### Краткое описание
Модифицирует основную камеру, изменяя смещение, уровень масштабирования и вращение во времени.  Проверьте [это видео](https://youtu.be/zy28jaMss8w) для быстрой демонстрации командного эффекта.

#### Параметры
<div class="config-table">

ID | Тип | Описание
--- | --- | ---
offset | List&lt;Decimal&gt; | Локальное смещение положения камеры в единицах по осям X, Y, Z.
roll | Decimal | Локальное вращение камеры по оси Z в градусах (от 0.0 до 360.0 или от -180.0 до 180.0). То же, что и третий компонент параметра вращение; игнорируется, когда указано `rotation`.
rotation | List&lt;Decimal&gt; | Локальное вращение камеры по осям X, Y, Z в градусах (от 0.0 до 360.0 или от -180.0 до 180.0).
zoom | Decimal | Установите масштаб камеры (ортогональный размер или поле зрения, в зависимости от режима рендеринга) в диапазоне от 0.0 (без увеличения) до 1.0 (при полном увеличении).
ortho | Boolean | Должна ли камера отображаться в орфографическом (истинном) или перспективном (ложном) режиме.
toggle | List&lt;String&gt; | Имена компонентов для переключения (включить, если отключено, и наоборот). Компоненты должны быть прикреплены к тому же игровому объекту, что и камера. Это можно использовать для переключения [пользовательские эффекты пост-обработки](/guide/special-effects.md#camera-effects).
set | List&lt;Named&lt;Boolean&gt;&gt; | Names of the components to enable or disable. The components should be attached to the same gameobject as the camera.  This can be used to explicitly enable or disable [custom post-processing effects](/guide/special-effects.md#camera-effects).  Specified components enabled state will override effect of `toggle` parameter.
easing | String |Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации камеры.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Пример
```
; Смещение по оси X (панорамирование) камеры на -3 единицы и смещение по оси Y на 1.5 единицы
@camera offset:-3,1.5

; Установите камеру в режим перспективы, увеличьте на 50% и вернитесь на 5 единиц.
@camera ortho:false offset:,,-5 zoom:0.5

; Установите камеру в орфографический режим и поверните на 10 градусов по часовой стрелке.
@camera ortho:true roll:10

; Смещение, масштабирование и угол одновременно анимированы в течение 5 секунд
@camera offset:-3,1.5 zoom:0.5 roll:10 time:5

; Мгновенный сброс камеры в состояние по умолчанию
@camera offset:0,0 zoom:0 rotation:0,0,0 time:0

; Переключите компоненты `FancyCameraFilter` и` Bloom`, прикрепленные к камере
@camera toggle:FancyCameraFilter,Bloom

; Включите компонент `FancyCameraFilter` и отключите` Bloom`
@camera set:FancyCameraFilter.true,Bloom.false
```

## char

#### Краткое описание
Модифицирует [актер персонажа](/guide/characters.md).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">IdAndAppearance</span> | Named&lt;String&gt; | ID, который нужно изменить (укажите `*`, чтобы затронуть все видимые символы) и внешний вид(или [поза](/guide/characters.md#poses)), который нужно установить. Если внешний вид не указан, будет использоваться либо «По умолчанию» (сущетвует), либо случайный.
look | String | Смотреть направление актера; поддерживаемые значения: слева, справа, по центру.
avatar | String | Имя (путь) [текстуры аватара](/guide/characters.md#avatar-textures) для назначения персонажу. Используйте `none`, чтобы удалить (отменить) текстуру аватара у персонажа.
pos | List&lt;Decimal&gt; | Установка положения (относительно границ экрана, в процентах), для измененного актера. Положение описывается следующим образом: «0.0» - левый нижний угол, «50.50» - центр, а «100.100» - верхний правый угол экрана. Используйте Z-компонент (третий параметр, например, `,, 10`) для перемещения (сортировки) по глубине в режиме орто.
id | String | ID для изменения; укажите `*`, чтобы повлиять на всех видимых актеров.
appearance | String | Внешний вид (или поза),  измененного актера.
transition | String | Тип используемого [эффекта перехода](/guide/transition-effects.md) (по умолчанию используется перекрестное затухание).
params | List&lt;Decimal&gt; | Параметры эффекта перехода.
dissolve | String | Путь к текстуре [custom disolve](/guide/transition-effects.md#custom-transition-effects) путь должен указываться относительно папки `Resources`). Действует только когда переход установлен в режим «Пользовательский».
visible | Boolean | Статус видимости, измененного актера.
position | List&lt;Decimal&gt; | Положение (в мировом пространстве) измененного актера. Используйте Z-компонент (третий параметр) для перемещения (сортировки) по глубине в режиме орто.
rotation | List&lt;Decimal&gt; | Вращение для измененного актера.
scale | List&lt;Decimal&gt; | Масштаб, для измененного актера
tint | String | Цвет оттенка измененного актера.  <br /><br />  Строки, начинающиеся с `#`, будут анализироваться как шестнадцатеричные следующим образом:  `#RGB` (becomes RRGGBB), `#RRGGBB`, `#RGBA` (becomes RRGGBBAA), `#RRGGBBAA`; если альфа не указана, по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как буквенные цвета, поддерживаются следующие значения: красный, голубой, синий, темно-синий, светло-синий, фиолетовый, желтый, салатовый, фуксия, белый, серебристый, серый, черный, оранжевый, коричневый , бордовый, зеленый, оливковый, темно-синий, чирок, аква, пурпурный.
easing | String | Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актера.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Пример
```
; Показывает персонажа с идентификатором `Sora` с появлением по умолчанию.
@char Sora

; То же, что и выше, но устанавливает внешний вид на «Happy».
@char Sora.Happy

; То же, что и выше, но также расположение персонажа на 45% от левой границы экрана и на 10% от нижней границы; также заставляет его смотреть налево.
@char Sora.Happy look:left pos:45,10

; Заставить Сору появиться внизу в центре и перед Феликсом
@char Sora pos:50,0,-1
@char Felix pos:,,0

; Tint all visible characters on scene.
@char * tint:#ffdc22
```

## choice

#### Красткое описание
Добавляет параметр [choice](/guide/choices.md) в обработчик выбора с указанным идентификатором (или идентификатором по умолчанию).

#### Примечание
Если параметры `goto`,` gosub` и `do` не указаны, выполнение сценария будет продолжено со следующей строки сценария.

#### Parameters

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">ChoiceSummary</span> | String | Текст для отображения на выбор. Если текст содержит пробелы, заключите его в двойные кавычки (`" `). Если вы хотите включить двойные кавычки в сам текст, избегайте их.
button | String | Путь (относительно папки `Resources`) к [кнопке prefab](/guide/choices.md#choice-button), представляющей выбор. Префаб должен иметь компонент `ChoiceHandlerButton`, присоединенный к корневому объекту. Будет использоваться кнопка по умолчанию, если она не указана.
pos | List&lt;Decimal&gt; | Локальное положение кнопки выбора внутри обработчика выбора (если поддерживается реализацией обработчика).
handler | String | ID выбора, для которого нужно добавить выбор. Будет использовать обработчик по умолчанию, если он не указан.
goto | Named&lt;String&gt; | Путь, когда вариант ответа выбирается пользователем; см. команду [@goto] для формата пути.
gosub | Named&lt;String&gt; | Путь к подпрограмме, которую нужно выбрать пользователем; см. команду [@gosub] для формата пути. Когда назначено `goto`, этот параметр будет игнорироваться.
set | String | Установите выражение для выполнения, когда вариант ответа выбран пользователем; см. команду [@set] для справки по синтаксису.
do | List&lt;String&gt; | Script commands to execute when the choice is selected by user;  don't forget to escape commas inside list values to prevent them being treated as delimiters.  The commands will be invoked in order after `set`, `goto` and `gosub` are handled (if assigned).
show | Boolean | Whether to also show choice handler the choice is added for;  enabled by default.
time | Decimal | Duration (in seconds) of the fade-in (reveal) animation. Default value: 0.35 seconds.

</div>

#### Example
```
; Print the text, then immediately show choices and stop script execution.
Continue executing this script or ...?[skipInput]
@choice "Continue"
@choice "Load another script from start" goto:AnotherScript
@choice "Load another script from \"MyLabel\" label" goto:AnotherScript.MyLabel
@choice "Goto to \"MySub\" subroutine in another script" gosub:AnotherScript.MySub
@stop

; You can also set custom variables based on choices.
@choice "I'm humble, one is enough..." set:score++
@choice "Two, please." set:score=score+2
@choice "I'll take the entire stock!" set:karma--;score=999

; Play a sound effect and arrange characters when choice is picked
@choice "Arrange" goto:.Continue do:"@sfx Click, @arrange k.10\,y.55"

; Following example shows how to make an interactive map via `@choice` commands.
; For this example, we assume, that inside a `Resources/MapButtons` folder you've
; stored prefabs with `ChoiceHandlerButton` component attached to their root objects.
# Map
@back Map
@hidePrinter
@choice handler:ButtonArea button:MapButtons/Home pos:-300,-300 goto:.HomeScene
@choice handler:ButtonArea button:MapButtons/Shop pos:300,200 goto:.ShopScene
@stop

# HomeScene
@back Home
Home, sweet home!
@goto .Map

# ShopScene
@back Shop
Don't forget about cucumbers!
@goto .Map
```

## clearBacklog

#### Summary
Removes all the messages from [printer backlog](/guide/printer-backlog.md).

#### Example
```
@clearBacklog
```

## clearChoice

#### Summary
Removes all the choice options in the choice handler with the provided ID (or in default one, when ID is not specified;  or in all the existing handlers, when `*` is specified as ID) and (optionally) hides it (them).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">HandlerId</span> | String | ID of the choice handler to clear. Will use a default handler if not provided.  Specify `*` to clear all the existing handlers.
hide | Boolean | Whether to also hide the affected printers.

</div>

#### Example
```
; Add choices and remove them after a set time (in case the player didn't pick one).
# Start
You have 2 seconds to respond![skipInput]
@choice "Cats" goto:.PickedChoice
@choice "Dogs" goto:.PickedChoice
@wait 2
@clearChoice
Too late!
@goto .Start
# PickedChoice
Good!
```

## despawn

#### Summary
Destroys an object spawned with [@spawn] command.

#### Remarks
If prefab has a `UnityEngine.MonoBehaviour` component attached the root object, and the component implements  a `Naninovel.Commands.DestroySpawned.IParameterized` interface, will pass the specified `params` values before destroying the object;  if the component implements `Naninovel.Commands.DestroySpawned.IAwaitable` interface, command execution will wait for  the async completion task returned by the implementation before destroying the object.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | String | Name (path) of the prefab resource to destroy.  A [@spawn] command with the same parameter is expected to be executed before.
params | List&lt;String&gt; | Parameters to set before destoying the prefab.  Requires the prefab to have a `Naninovel.Commands.DestroySpawned.IParameterized` component attached the root object.

</div>

#### Example
```
; Given a "@spawn Rainbow" command was executed before
@despawn Rainbow
```

## else

#### Summary
Marks a branch of a conditional execution block,  which is always executed in case conditions of the opening [@if] and all the preceding [@elseif] (if any) commands are not met.  For usage examples see [conditional execution](/guide/naninovel-scripts.md#conditional-execution) guide.

## elseIf

#### Summary
Marks a branch of a conditional execution block,  which is executed in case own condition is met (expression is evaluated to be true), while conditions of the opening [@if]  and all the preceding [@elseif] (if any) commands are not met.  For usage examples see [conditional execution](/guide/naninovel-scripts.md#conditional-execution) guide.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | A [script expression](/guide/script-expressions.md), which should return a boolean value.

</div>

## endIf

#### Summary
Closes an [@if] conditional execution block.  For usage examples see [conditional execution](/guide/naninovel-scripts.md#conditional-execution) guide.

## finishTrans

#### Summary
Finishes scene transition started with [@startTrans] command;  see the start command reference for more information and usage examples.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Transition</span> | String | Type of the [transition effect](/guide/transition-effects.md) to use (crossfade is used by default).
params | List&lt;Decimal&gt; | Parameters of the transition effect.
dissolve | String | Path to the [custom dissolve](/guide/transition-effects.md#custom-transition-effects) texture (path should be relative to a `Resources` folder).  Has effect only when the transition is set to `Custom` mode.
easing | String | Name of the easing function to use for the modification.  <br /><br />  Available options: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  When not specified, will use a default easing function set in the actor's manager configuration settings.
time | Decimal | Duration (in seconds) of the transition. Default value: 0.35 seconds.

</div>

## gosub

#### Summary
Navigates naninovel script playback to the provided path and saves that path to global state;  [@return] commands use this info to redirect to command after the last invoked gosub command.  Designed to serve as a function (subroutine) in a programming language, allowing to reuse a piece of naninovel script.  Useful for invoking a repeating set of commands multiple times.

#### Remarks
It's possible to declare a gosub outside of the currently played script and use it from any other scripts;  by default state reset won't happen when you're loading another script to play a gosub or returning back to  prevent delays and loading screens. Be aware though, that all the resources referenced in gosub script will be  held until the next state reset.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | Named&lt;String&gt; | Path to navigate into in the following format: `ScriptName.LabelName`.  When label name is ommited, will play provided script from the start.  When script name is ommited, will attempt to find a label in the currently played script.
reset | List&lt;String&gt; | When specified, will reset the engine services state before loading a script (in case the path is leading to another script).  Specify `*` to reset all the services (except variable manager), or specify service names to exclude from reset.  By default, the state does not reset.

</div>

#### Example
```
; Navigate the playback to the label `VictoryScene` in the currently played script,
; executes the commands and navigates back to the command after the `gosub`.
@gosub .VictoryScene
...
@stop

# VictoryScene
@back Victory
@sfx Fireworks
@bgm Fanfares
You are victorious!
@return

; Another example with some branching inside the subroutine.
@set time=10
; Here we get one result
@gosub .Room
...
@set time=3
; And here we get another
@gosub .Room
...

# Room
@print "It's too early, I should visit this place when it's dark." if:time<21&&time>6
@print "I can sense an ominous presence here!" if:time>21&&time<6
@return
```

## goto

#### Summary
Navigates naninovel script playback to the provided path.  When the path leads to another (not the currently played) naninovel script, will also [reset state](/api/#resetstate)  before loading the target script, unless [ResetStateOnLoad](https://naninovel.com/guide/configuration.html#state) is disabled in the configuration.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | Named&lt;String&gt; | Path to navigate into in the following format: `ScriptName.LabelName`.  When label name is ommited, will play provided script from the start.  When script name is ommited, will attempt to find a label in the currently played script.
reset | List&lt;String&gt; | When specified, will reset the engine services state before loading a script (in case the path is leading to another script).  Specify `*` to reset all the services (except variable manager), or specify service names to exclude from reset.  Specify `-` to force no reset (even if it's enabled by default in the configuration).  Default value is controlled by `Reset State On Load` option in engine configuration menu.

</div>

#### Example
```
; Loads and starts playing a naninovel script with the name `Script001` from the start.
@goto Script001

; Save as above, but start playing from the label `AfterStorm`.
@goto Script001.AfterStorm

; Navigates the playback to the label `Epilogue` in the currently played script.
@goto .Epilogue

; Load Script001, but don't reset audio manager (any playing audio won't be interrupted).
; Be aware, that excluding a service form state reset will leave related resources in memory.
@goto Script001 reset:IAudioManager
```

## hide

#### Summary
Hides (makes invisible) actors (character, background, text printer, choice handler, etc) with the specified IDs.  In case mutliple actors with the same ID found (eg, a character and a printer), will affect only the first found one.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | IDs of the actors to hide.
time | Decimal | Duration (in seconds) of the fade animation. Default value: 0.35 seconds.

</div>

#### Example
```
; Given an actor with ID `SomeActor` is visible, hide (fade-out) it over 3 seconds.
@hide SomeActor time:3

; Hide `Kohaku` and `Yuko` actors.
@hide Kohaku,Yuko
```

## hideAll

#### Summary
Hides (removes) all the actors (eg characters, backgrounds, text printers, choice handlers, etc) on scene.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
time | Decimal | Duration (in seconds) of the fade animation. Default value: 0.35 seconds.

</div>

#### Example
```
@hideAll
```

## hideChars

#### Summary
Hides (removes) all the visible characters on scene.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
time | Decimal | Duration (in seconds) of the fade animation. Default value: 0.35 seconds.

</div>

#### Example
```
@hideChars
```

## hidePrinter

#### Summary
Hides a text printer.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID of the printer actor to use. Will use a default one when not provided.
time | Decimal | Duration (in seconds) of the hide animation.  Default value for each printer is set in the actor configuration.

</div>

#### Example
```
; Hide a default printer.
@hidePrinter
; Hide printer with ID `Wide`.
@hidePrinter Wide
```

## hideUI

#### Summary
Makes [UI elements](/guide/user-interface.md#ui-customization) with the specified names invisible.  When no names are specified, will stop rendering (hide) the entire UI (including all the built-in UIs).

#### Remarks
When hiding the entire UI with this command and `allowToggle` parameter is false (default), user won't be able to re-show the UI  back with hotkeys or by clicking anywhere on the screen; use [@showUI] command to make the UI ~~great~~ visible again.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">UINames</span> | List&lt;String&gt; | Name of the UI elements to hide.
allowToggle | Boolean | When hiding the entire UI, controls whether to allow the user to re-show the UI with hotkeys or by clicking anywhere on the screen (false by default).  Has no effect when hiding a particular UI.
time | Decimal | Duration (in seconds) of the hide animation.  When not specified, will use UI-specific duration.

</div>

#### Example
```
; Given a custom `Calendar` UI, the following command will hide it.
@hideUI Calendar

; Hide the entire UI, won't allow user to re-show it
@hideUI
...
; Make the UI visible again
@showUI

; Hide the entire UI, but allow the user to toggle it back
@hideUI allowToggle:true

; Simultaneously hide built-in `TipsUI` and custom `Calendar` UIs.
@hideUI TipsUI,Calendar
```

## i

#### Summary
Holds script execution until user activates a `continue` input.  Shortcut for `@wait i`.

#### Example
```
; User will have to activate a `continue` input after the first sentence
; for the printer to contiue printing out the following text.
Lorem ipsum dolor sit amet.[i] Consectetur adipiscing elit.
```

## if

#### Summary
Marks the beginning of a conditional execution block.  Should always be closed with an [@endif] command.  For usage examples see [conditional execution](/guide/naninovel-scripts.md#conditional-execution) guide.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | A [script expression](/guide/script-expressions.md), which should return a boolean value.

</div>

## input

#### Summary
Shows an input field UI where user can enter an arbitrary text.  Upon submit the entered text will be assigned to the specified custom variable.

#### Remarks
Check out this [video guide](https://youtu.be/F9meuMzvGJw) on usage example.  <br /><br />  To assign a display name for a character using this command consider [binding the name to a custom variable](/guide/characters.html#display-names).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">VariableName</span> | String | Name of a custom variable to which the entered text will be assigned.
summary | String | An optional summary text to show along with input field.  When the text contain spaces, wrap it in double quotes (`"`).  In case you wish to include the double quotes in the text itself, escape them.
value | String | A predefined value to set for the input field.
play | Boolean | Whether to automatically resume script playback when user submits the input form.

</div>

#### Example
```
; Allow user to enter an arbitrary text and assign it to `name` custom state variable
@input name summary:"Choose your name."
; Stop command is required to halt script execution until user submits the input
@stop

; You can then inject the assigned `name` variable in naninovel scripts
Archibald: Greetings, {name}!
{name}: Yo!

; ...or use it inside set and conditional expressions
@set score=score+1 if:name=="Felix"
```

## lipSync

#### Summary
Allows to force-stop the lip sync mouth animation for a character with the provided ID; when stopped, the animation  won't start again, until this command is used again to allow it.  The character should be able to receive the lip sync events (currently generic and Live2D implementations only).  See [characters guide](/guide/characters.md#lip-sync) for more information on lip sync feature.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">CharIdAndAllow</span> | Named&lt;Boolean&gt; | Character ID followed by a boolean (true or false) on whether to halt or allow the lip sync animation.

</div>

#### Example
```
; Given auto voicing is disabled and lip sync is driven by text messages,
; exclude punctuation from the mouth animation.
Kohaku: Lorem ipsum dolor sit amet[lipSync Kohaku.false]... [lipSync Kohaku.true]Consectetur adipiscing elit.
```

## loadScene

#### Summary
Loads a [Unity scene](https://docs.unity3d.com/Manual/CreatingScenes.html) with the provided name.  Don't forget to add the required scenes to the [build settings](https://docs.unity3d.com/Manual/BuildSettings.html) to make them available for loading.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">SceneName</span> | String | Name of the scene to load.
additive | Boolean | Whether to load the scene additively, or unload any currently loaded scenes before loading the new one (default).  See the [load scene documentation](https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.LoadScene.html) for more information.

</div>

#### Example
```
; Load scene "MyTestScene" in single mode
@loadScene MyTestScene
; Load scene "MyTestScene" in additive mode
@loadScene MyTestScene additive:true
```

## lock

#### Summary
Sets an [unlockable item](/guide/unlockable-items.md) with the provided ID to `locked` state.

#### Remarks
The unlocked state of the items is stored in [global scope](/guide/state-management.md#global-state).<br />  In case item with the provided ID is not registered in the global state map,  the corresponding record will automatically be added.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Id</span> | String | ID of the unlockable item. Use `*` to lock all the registered unlockable items.

</div>

#### Example
```
@lock CG/FightScene1
```

## look

#### Summary
Activates/disables camera look mode, when player can offset the main camera with input devices  (eg, by moving a mouse or using gamepad analog stick).  Check [this video](https://youtu.be/rC6C9mA7Szw) for a quick demonstration of the command.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
enable | Boolean | Whether to enable or disable the camera look mode. Default: true.
zone | List&lt;Decimal&gt; | A bound box with X,Y sizes in units from the initial camera position,  describing how far the camera can be moved. Default: 5,3.
speed | List&lt;Decimal&gt; | Camera movement speed (sensitivity) by X,Y axes. Default: 1.5,1.
gravity | Boolean | Whether to automatically move camera to the initial position when the look input is not active  (eg, mouse is not moving or analog stick is in default position). Default: false.

</div>

#### Example
```
; Activate camera look mode with default parameters
@look

; Activate camera look mode with custom parameters
@look zone:6.5,4 speed:3,2.5 gravity:true

; Disable camera look mode and reset camera offset
@look enabled:false
@camera offset:0,0
```

## movie

#### Summary
Playes a movie with the provided name (path).

#### Remarks
Will fade-out the screen before playing the movie and fade back in after the play.  Playback can be canceled by activating a `cancel` input (`Esc` key by default).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">MovieName</span> | String | Name of the movie resource to play.

</div>

#### Example
```
; Given an "Opening" video clip is added to the movie resources, plays it
@movie Opening
```

## print

#### Summary
Prints (reveals over time) specified text message using a text printer actor.

#### Remarks
This command is used under the hood when processing generic text lines, eg generic line `Kohaku: Hello World!` will be  automatically tranformed into `@print "Hello World!" author:Kohaku` when parsing the naninovel scripts.<br />  Will reset (clear) the printer before printing the new message by default; set `reset` parameter to *false* or disable `Auto Reset` in the printer actor configuration to prevent that and append the text instead.<br />  Will make the printer default and hide other printers by default; set `default` parameter to *false* or disable `Auto Default` in the printer actor configuration to prevent that.<br />  Will wait for user input before finishing the task by default; set `waitInput` parameter to *false* or disable `Auto Wait` in the printer actor configuration to return as soon as the text is fully revealed.<br />

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Text</span> | String | Text of the message to print.  When the text contain spaces, wrap it in double quotes (`"`).  In case you wish to include the double quotes in the text itself, escape them.
printer | String | ID of the printer actor to use. Will use a default one when not provided.
author | String | ID of the actor, which should be associated with the printed message.
speed | Decimal | Text reveal speed multiplier; should be positive or zero. Setting to one will yield the default speed.
reset | Boolean | Whether to reset text of the printer before executing the printing task.  Default value is controlled via `Auto Reset` property in the printer actor configuration menu.
default | Boolean | Whether to make the printer default and hide other printers before executing the printing task.  Default value is controlled via `Auto Default` property in the printer actor configuration menu.
waitInput | Boolean | Whether to wait for user input after finishing the printing task.  Default value is controlled via `Auto Wait` property in the printer actor configuration menu.
br | Integer | Number of line breaks to prepend before the printed text.  Default value is controlled via `Auto Line Break` property in the printer actor configuration menu.
fadeTime | Decimal | Controls duration (in seconds) of the printers show and hide animations associated with this command.  Default value for each printer is set in the actor configuration.

</div>

#### Example
```
; Will print the phrase with a default printer.
@print "Lorem ipsum dolor sit amet."

; To include quotes in the text itself, escape them.
@print "Saying \"Stop the car\" was a mistake."

; Reveal message with half of the normal speed and
; don't wait for user input to continue.
@print "Lorem ipsum dolor sit amet." speed:0.5 waitInput:false
```

## printer

#### Summary
Modifies a [text printer actor](/guide/text-printers.md).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">IdAndAppearance</span> | Named&lt;String&gt; | ID of the printer to modify and the appearance to set.  When ID or appearance are not provided, will use default ones.
default | Boolean | Whether to make the printer the default one.  Default printer will be subject of all the printer-related commands when `printer` parameter is not specified.
hideOther | Boolean | Whether to hide all the other printers.
pos | List&lt;Decimal&gt; | Position (relative to the screen borders, in percents) to set for the modified printer.  Position is described as follows: `0,0` is the bottom left, `50,50` is the center and `100,100` is the top right corner of the screen.
visible | Boolean | Whether to show or hide the printer.
time | Decimal | Duration (in seconds) of the modification. Default value: 0.35 seconds.

</div>

#### Example
```
; Will make `Wide` printer default and hide any other visible printers.
@printer Wide

; Will assign `Right` appearance to `Bubble` printer, make is default,
; position at the center of the screen and won't hide other printers.
@printer Bubble.Right pos:50,50 hideOther:false
```

## processInput

#### Summary
Allows halting and resuming user input processing (eg, reacting to pressing keyboard keys).  The effect of the action is persistent and saved with the game.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">InputEnabled</span> | Boolean | Whether to enable input processing.

</div>

#### Example
```
; Halt input processing
@processInput false
; Resume input processing
@processInput true
```

## purgeRollback

#### Summary
Prevents player from rolling back to the previous state snapshots.

#### Example
```
; Prevent player from rolling back to try picking another choice.

@choice "One" goto:.One
@choice "Two" goto:.Two
@stop

# One
@purgeRollback
You've picked one.

# Two
@purgeRollback
You've picked two.
```

## resetState

#### Summary
Resets state of the [engine services](https://naninovel.com/guide/engine-services.html) and unloads (disposes)  all the resources loaded by Naninovel (textures, audio, video, etc); will basically revert to an empty initial engine state.

#### Remarks
The process is asynchronous and is masked with a loading screen ([ILoadingUI](https://naninovel.com/guide/user-interface.html#ui-customization)).  <br /><br />  When [ResetStateOnLoad](https://naninovel.com/guide/configuration.html#state) is disabled in the configuration, you can use this command  to manually dispose unused resources to prevent memory leak issues.  <br /><br />  Be aware, that this command can not be undone (rewinded back).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Exclude</span> | List&lt;String&gt; | Name of the [engine services](https://naninovel.com/guide/engine-services.html) (interfaces) to exclude from reset.  When specifying the parameter, consider always adding `ICustomVariableManager` to preserve the local variables.

</div>

#### Example
```
; Reset all the services.
@resetState

; Reset all the services except variable and audio managers (current audio will continue playing).
@resetState ICustomVariableManager,IAudioManager
```

## resetText

#### Summary
Resets (clears) the contents of a text printer.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID of the printer actor to use. Will use a default one when not provided.

</div>

#### Example
```
; Clear the content of a default printer.
@resetText
; Clear the content of a printer with ID `Fullscreen`.
@resetText Fullscreen
```

## return

#### Summary
Attempts to navigate naninovel script playback to a command after the last used [@gosub].  See [@gosub] command summary for more info and usage examples.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
reset | List&lt;String&gt; | When specified, will reset the engine services state before returning to the initial script  from which the gosub was entered (in case it's not the currently played script).  Specify `*` to reset all the services (except variable manager), or specify service names to exclude from reset.  By default, the state does not reset.

</div>

## save

#### Summary
Automatically save the game to a quick save slot.

#### Example
```
@save
```

## set

#### Summary
Assigns result of a [script expression](/guide/script-expressions.md) to a [custom variable](/guide/custom-variables.md).

#### Remarks
Variable name should be alphanumeric (latin characters only) and can contain underscores, eg: `name`, `Char1Score`, `my_score`;  the names are case-insensitive, eg: `myscore` is equal to `MyScore`. If a variable with the provided name doesn't exist, it will be automatically created.  <br /><br />  It's possible to define multiple set expressions in one line by separating them with `;`. The expressions will be executed in sequence by the order of declaratation.  <br /><br />  Custom variables are stored in **local scope** by default. This means, that if you assign some variable in the course of gameplay  and player starts a new game or loads another saved game slot, where that variable wasn't assigned — the value will be lost.  If you wish to store the variable in **global scope** instead, prepend `G_` or `g_` to its name, eg: `G_FinishedMainRoute` or `g_total_score`.  <br /><br />  In case variable name starts with `T_` or `t_` it's considered a reference to a value stored in 'Script' [managed text](/guide/managed-text.md) document.  Such variables can't be assiged and mostly used for referencing localizable text values.  <br /><br />  You can get and set custom variables in C# scripts via `CustomVariableManager` [engine service](/guide/engine-services.md).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | Set expression.  <br /><br />  The expression should be in the following format: `VariableName=ExpressionBody`, where `VariableName` is the name of the custom  variable to assign and `ExpressionBody` is a [script expression](/guide/script-expressions.md), the result of which should be assigned to the variable.  <br /><br />  It's also possible to use increment and decrement unary operators, eg: `@set foo++`, `@set foo--`.

</div>

#### Example
```
; Assign `foo` variable a `bar` string value
@set foo="bar"

; Assign `foo` variable a 1 number value
@set foo=1

; Assign `foo` variable a `true` boolean value
@set foo=true

; If `foo` is a number, add 0.5 to its value
@set foo=foo+0.5

; If `angle` is a number, assign its cosine to `result` variable
@set result=Cos(angle)

; Get a random integer between -100 and 100, then raise to power of 4 and assign to `result` variable
@set "result = Pow(Random(-100, 100), 4)"

; If `foo` is a number, add 1 to its value
@set foo++

; If `foo` is a number, subtract 1 from its value
@set foo--

; Assign `foo` variable value of the `bar` variable, which is `Hello World!`.
; Notice, that `bar` variable should actually exist, otherwise `bar` plain text value will be assigned instead.
@set bar="Hello World!"
@set foo=bar

; Defining multiple set expressions in one line (the result will be the same as above)
@set bar="Hello World!";foo=bar

; It's possible to inject variables to naninovel script command parameters
@set scale=0
# EnlargeLoop
@char Misaki.Default scale:{scale}
@set scale=scale+0.1
@goto .EnlargeLoop if:scale<1

; ..and generic text lines
@set name="Dr. Stein";drink="Dr. Pepper"
{name}: My favourite drink is {drink}!

; When using double quotes inside the expression itself, don't forget to double-escape them
@set remark="Saying \\"Stop the car\\" was a mistake."
```

## sfx

#### Summary
Plays or modifies currently played [SFX (sound effect)](/guide/audio.md#sound-effects) track with the provided name.

#### Remarks
Sound effect tracks are not looped by default.  When sfx track name (SfxPath) is not specified, will affect all the currently played tracks.  When invoked for a track that is already playing, the playback won't be affected (track won't start playing from the start),  but the specified parameters (volume and whether the track is looped) will be applied.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">SfxPath</span> | String | Path to the sound effect asset to play.
volume | Decimal | Volume of the sound effect.
loop | Boolean | Whether to play the sound effect in a loop.
fade | Decimal | Duration of the volume fade-in when starting playback, in seconds (0.0 by default);  doesn't have effect when modifying a playing track.
group | String | Audio mixer [group path](https://docs.unity3d.com/ScriptReference/Audio.AudioMixer.FindMatchingGroups) that should be used when playing the audio.
time | Decimal | Duration (in seconds) of the modification. Default value: 0.35 seconds.

</div>

#### Example
```
; Plays an SFX with the name `Explosion` once
@sfx Explosion

; Plays an SFX with the name `Rain` in a loop and fades-in over 30 seconds
@sfx Rain loop:true fade:30

; Changes volume of all the played SFX tracks to 75% over 2.5 seconds and disables looping for all of them
@sfx volume:0.75 loop:false time:2.5
```

## show

#### Summary
Shows (makes visible) actors (character, background, text printer, choice handler, etc) with the specified IDs.  In case mutliple actors with the same ID found (eg, a character and a printer), will affect only the first found one.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | IDs of the actors to show.
time | Decimal | Duration (in seconds) of the fade animation. Default value: 0.35 seconds.

</div>

#### Example
```
; Given an actor with ID `SomeActor` is hidden, reveal (fade-in) it over 3 seconds.
@show SomeActor time:3

; Show `Kohaku` and `Yuko` actors.
@show Kohaku,Yuko
```

## showPrinter

#### Summary
Shows a text printer.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID of the printer actor to use. Will use a default one when not provided.
time | Decimal | Duration (in seconds) of the show animation.  Default value for each printer is set in the actor configuration.

</div>

#### Example
```
; Show a default printer.
@showPrinter
; Show printer with ID `Wide`.
@showPrinter Wide
```

## showUI

#### Summary
Makes [UI elements](/guide/user-interface.md) with the specified prefab names visible.  When no names are specified, will reveal the entire UI (in case it was hidden with [@hideUI]).

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">UINames</span> | List&lt;String&gt; | Name of the UI prefab to make visible.
time | Decimal | Duration (in seconds) of the show animation.  When not specified, will use UI-specific duration.

</div>

#### Example
```
; Given you've added a custom UI with prefab name `Calendar`,
; the following will make it visible on the scene.
@showUI Calendar

; Given you've hide the entire UI with @hideUI, show it back
@showUI

; Simultaneously reveal built-in `TipsUI` and custom `Calendar` UIs.
@showUI TipsUI,Calendar
```

## skip

#### Summary
Allows to enable or disable script player "skip" mode.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Enable</span> | Boolean | Whether to enable (default) or disable the skip mode.

</div>

#### Example
```
; Enable skip mode
@skip
; Disable skip mode
@skip false
```

## skipInput

#### Summary
Can be used in generic text lines to prevent activating `wait for input` mode when the text is printed.

#### Example
```
; Script player won't wait for `continue` input before executing the `@sfx` command.
And the rain starts.[skipInput]
@sfx Rain
```

## slide

#### Summary
Slides (moves over X-axis) actor (character, background, text printer or choice handler) with the provided ID and optionally changes actor appearance.

#### Remarks
Be aware, that this command searches for an actor with the provided ID over all the actor managers,  and in case multiple actors with the same ID exist (eg, a character and a text printer), this will affect only the first found one.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">IdAndAppearance</span> | Named&lt;String&gt; | ID of the actor to slide and (optionally) appearance to set.
from | Decimal | Position over X-axis (in 0 to 100 range, in percents from the left border of the screen) to slide the actor from.  When not provided, will use current actor position in case it's visible and a random off-screen position otherwise.
<span class="command-param-required" title="Required parameter: parameter should always be specified">to</span> | Decimal | Position over X-axis (in 0 to 100 range, in percents from the left border of the screen) to slide the actor to.
visible | Boolean | Change visibility status of the actor (show or hide).  When not set and target actor is hidden, will still automatically show it.
easing | String | Name of the easing function to use for the modifications.  <br /><br />  Available options: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  When not specified, will use a default easing function set in the actor's manager configuration settings.
time | Decimal | Duration (in seconds) of the slide animation. Default value: 0.35 seconds.

</div>

#### Example
```
; Given `Jenna` actor is not currenly visible, reveal it with a
; `Angry` appearance and slide to the center of the screen.
@slide Jenna.Angry to:50

; Given `Sheba` actor is currenly visible,
; hide and slide it out of the screen over the left border.
@slide Sheba to:-10 visible:false

; Slide `Mia` actor from left side of the screen to the right
; over 5 seconds using `EaseOutBounce` animation easing.
@slide Sheba from:15 to:85 time:5 easing:EaseOutBounce
```

## spawn

#### Summary
Instantiates a prefab or a [special effect](/guide/special-effects.md);  when performed over an already spawned object, will update the spawn parameters instead.

#### Remarks
If prefab has a `UnityEngine.MonoBehaviour` component attached the root object, and the component implements  a `Naninovel.Commands.Spawn.IParameterized` interface, will pass the specified `params` values after the spawn;  if the component implements `Naninovel.Commands.Spawn.IAwaitable` interface, command execution will wait for  the async completion task returned by the implementation.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | String | Name (path) of the prefab resource to spawn.
params | List&lt;String&gt; | Parameters to set when spawning the prefab.  Requires the prefab to have a `Naninovel.Commands.Spawn.IParameterized` component attached the root object.

</div>

#### Example
```
; Given an `Rainbow` prefab is assigned in spawn resources, instantiate it.
@spawn Rainbow
```

## startTrans

#### Summary
Begins scene transition masking the real scene content with anything that is visible at the moment (except the UI).  When the new scene is ready, finish with [@finishTrans] command.

#### Remarks
The UI will be hidden and user input blocked while the transition is in progress.  You can change that by overriding the `ISceneTransitionUI`, which handles the transition process.<br /><br />  For the list of available transition effect options see [transition effects](/guide/transition-effects.md) guide.

#### Example
```
; Transition Felix on sunny day with Jenna on rainy day
@char Felix
@back SunnyDay
@fx SunShafts
@startTrans
; The following modifications won't be visible until we finish the transition
@hideChars time:0
@char Jenna time:0
@back RainyDay time:0
@stopFx SunShafts params:0
@fx Rain params:,0
; Transition the initially captured scene to the new one with `DropFade` effect over 3 seconds
@finishTrans DropFade time:3
```

## stop

#### Summary
Stops the naninovel script execution.

#### Example
```
Show the choices and halt script execution until the player picks one.
@choice "Choice 1"
@choice "Choice 2"
@stop
We'll get here after player will make a choice.
```

## stopBgm

#### Summary
Stops playing a BGM (background music) track with the provided name.

#### Remarks
When music track name (BgmPath) is not specified, will stop all the currently played tracks.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">BgmPath</span> | String | Path to the music track to stop.
fade | Decimal | Duration of the volume fade-out before stopping playback, in seconds (0.35 by default).

</div>

#### Example
```
; Fades-out the `Promenade` music track over 10 seconds and stops the playback
@stopBgm Promenade fade:10

; Stops all the currently played music tracks
@stopBgm
```

## stopSfx

#### Summary
Stops playing an SFX (sound effect) track with the provided name.

#### Remarks
When sound effect track name (SfxPath) is not specified, will stop all the currently played tracks.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">SfxPath</span> | String | Path to the sound effect to stop.
fade | Decimal | Duration of the volume fade-out before stopping playback, in seconds (0.35 by default).

</div>

#### Example
```
; Stop playing an SFX with the name `Rain`, fading-out for 15 seconds.
@stopSfx Rain fade:15

; Stops all the currently played sound effect tracks
@stopSfx
```

## stopVoice

#### Summary
Stops playback of the currently played voice clip.

## style

#### Summary
Permamently applies [text styles](/guide/text-printers.md#text-styles) to the contents of a text printer.

#### Remarks
You can also use rich text tags inside text messages to apply the styles selectively.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">TextStyles</span> | List&lt;String&gt; | Text formatting tags to apply. Angle brackets should be ommited, eg use `b` for &lt;b&gt; and `size=100` for &lt;size=100&gt;. Use `default` keyword to reset the style.
printer | String | ID of the printer actor to use. Will use a default one when not provided.

</div>

#### Example
```
; Print first two sentences in bold red text with 45px size,
; then reset the style and print the last sentence using default style.
@style color=#ff0000,b,size=45
Lorem ipsum dolor sit amet.
Cras ut nisi eget ex viverra egestas in nec magna.
@style default
Consectetur adipiscing elit.

; Print starting part of the sentence normally, but the last one in bold.
Lorem ipsum sit amet. <b>Consectetur adipiscing elit.</b>
```

## title

#### Summary
Resets engine state and shows `ITitleUI` UI (main menu).

#### Example
```
@title
```

## unlock

#### Summary
Sets an [unlockable item](/guide/unlockable-items.md) with the provided ID to `unlocked` state.

#### Remarks
The unlocked state of the items is stored in [global scope](/guide/state-management.md#global-state).<br />  In case item with the provided ID is not registered in the global state map,  the corresponding record will automatically be added.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Id</span> | String | ID of the unlockable item. Use `*` to unlock all the registered unlockable items.

</div>

#### Example
```
@unlock CG/FightScene1
```

## voice

#### Summary
Plays a voice clip at the provided path.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">VoicePath</span> | String | Path to the voice clip to play.
volume | Decimal | Volume of the playback.
group | String | Audio mixer [group path](https://docs.unity3d.com/ScriptReference/Audio.AudioMixer.FindMatchingGroups) that should be used when playing the audio.

</div>

## wait

#### Summary
Holds script execution until the specified wait condition.

#### Parameters

<div class="config-table">

ID | Type | Description
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">WaitMode</span> | String | Wait conditions:<br />  - `i` user press continue or skip input key;<br />  - `0.0` timer (seconds);<br />  - `i0.0` timer, that is skippable by continue or skip input keys.

</div>

#### Example
```
; "ThunderSound" SFX will play 0.5 seconds after the shake background effect finishes.
@fx ShakeBackground
@wait 0.5
@sfx ThunderSound

; Print first two words, then wait for user input before printing the remaining phrase.
Lorem ipsum[wait i] dolor sit amet.
; You can also use the following shortcut (@i command) for this wait mode.
Lorem ipsum[i] dolor sit amet.

; Start an SFX, print a message and wait for a skippable 5 seconds delay, then stop the SFX.
@sfx Noise loop:true
Jeez, what a disgusting noise. Shut it down![wait i5][skipInput]
@stopSfx Noise
```