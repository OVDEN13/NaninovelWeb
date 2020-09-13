﻿---
sidebar: auto
---

# Справочник по API 

Справочник по API командам сценария. Используйте боковую панель для быстрой навигации между доступными командами. 

~~Зачеркивание~~ обозначает безымянный параметр, а **жирный** – обязательный параметр; остальные параметры следует считать необязательными. Обратитесь к [руководству по скриптам Naninovel](/ru/guide/naninovel-scripts.md) если вы не знаете, что это такое.

Следующие параметры поддерживаются всеми командами сценария:

<div class=config-table>

ID | Тип | Описание
--- | --- | ---
if | Строка |  Логическое [выражение сценария](/ru/guide/script-expressions.md), контролирующее, должна ли команда выполняться.
wait | Boolean | Должен ли проигрыватель сценариев дождаться завершения асинхронной команды перед выполнением следующей. Не действует, когда команда выполняется мгновенно.

</div>

::: note
Эта ссылка API действительна для [Naninovel v1.10](https://github.com/Elringus/NaninovelWeb/releases).
:::

## animate

#### Краткое описание
Команда анимации акторов с указанными ID с помощью ключевых кадров. Ключевые кадры для параметры анимации отделяются литералами `|`.

#### Примечание
Помните, что эта команда ищет акторов с предоставленными ID по всем менеджерам акторов, и в случае, если существует несколько акторов с одинаковым ID (например, персонаж и текстовый принтер), это повлияет только на первый найденный.  <br /><br />
При параллельном запуске команд animate (значение «wait» установлено в false) состояние затронутых акторов может изменяться непредсказуемо. Это может привести к неожиданным результатам при откате или выполнении других команд, которые влияют на состояние актора. Обязательно сбрасывайте затронутые свойства анимированных акторов (положение, оттенок, внешний вид и т.д.) после завершения команды или используйте `@animate CharacterId` (без каких-либо аргументов), чтобы преждевременно остановить анимацию.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | ID акторов для анимации.
loop | Boolean | Следует ли зацикливать анимацию; убедитесь, что для `wait` установлено значение false, когда включен цикл, в противном случае воспроизведение сценария будет повторяться бесконечно.
appearance | Строка | Установка внешности для анимированных акторов.
transition | Строка | Тип [эффекта перехода](/ru/guide/transition-effects.md) который будет использоваться при изменении внешнего вида (по умолчанию используется кроссфейд).
visibility | Строка | Статус видимости для анимированных акторов.
posX | Строка | Значения позиции анимированного актора, по оси Х (в диапазоне от 0 до 100, в процентах от левой границы экрана).
posY | Строка | Значения позиции анимированного актора, по оси Y (в диапазоне от 0 до 100, в процентах от нижней границы экрана).
posZ | Строка | Значения позиции анимированного актора, по оси Z (в мировом пространстве); В орто-режиме, ожно использовать только для сортировки.
rotation | Строка | Значения поворота (по оси Z) для анимированных акторов.
scale | Строка | Значения шкалы (единообразные) для анимированных акторов.
tint | Строка | Оттенок цвета для анимированных акторов.  <br /><br />  Строки, начинающиеся с `#` будет проанализирован как шестнадцатеричный следующим образом:  `#RGB` (становится RRGGBB), `#RRGGBB`, `#RGBA` (становится RRGGBBAA), `#RRGGBBAA`; Если альфа не указана, по значение по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как цвета, поддерживаются:  red, cyan, blue, darkblue, lightblue, purple, yellow, lime, fuchsia, white, silver, grey, black, orange, brown, maroon, green, olive, navy, teal, aqua, magenta.
easing | Строка | Названия функций замедления, используемых для анимации. <br /><br />  Доступные варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актора.
time | Строка | Продолжительность ключа анимации в секундах. Если значение ключа отсутствует, будет использоваться значение из предыдущего ключа. Если значение не установлено, будет использовать задержка 0.35 секунд для всех ключей.

</div>

#### Примеры
```
; Анимация актора "Kohaku" за три шага анимации (ключевые кадры),
; смена позиций: первый шаг займет 1, второй - 0.5 и третий - 3 секунды.
@animate Kohaku posX:50|0|85 time:1|0.5|3

; Старт цикла анимации акторов "Yuko" и "Kohaku"; обратите внимание, что вы можете пропустить
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
printer | Строка | ID актора принтера, который будет использован. Если параметр не указан, будет использован принтер по умолчанию.
author | Строка | ID актора, который должен быть связан с добавленым текстом.

</div>

#### Примеры
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

#### Примеры
```
; Равномерно распределить всех видимых персонажей 
@arrange

; Переместить персонажей с идентификаторами `Jenna` в 15%,` Felix` в 50% и `Mia` в 85%
; от левой границы экрана.
@arrange Jenna.15,Felix.50,Mia.85
```

## back

#### Краткое описание
Изменяет [актор фона](/ru/guide/backgrounds.md).

#### Примечание
Фоны обрабатываются немного иначе, нежели персонажи, чтобы лучше приспособиться к традиционному течению визуальных новелл. Большую часть времени вы, вероятно, будете иметь одного фонового актора в сцене, который будет постоянно менять внешности. Чтобы избавиться от лишнего повторения одного и того же ID актора в сценариях, можно предоставить только внешность фона и тип перехода (необязательно) в качестве безымянного параметра, что автоматически преобразует актора "MainBackground". Если это не так, ID фонового актора может быть конкретно указан с помощью параметра `id`:

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">AppearanceAndTransition</span> | Named&lt;String&gt; | Внешний вид (или [поза](/ru/guide/backgrounds.md#poses)) для фона и типа использованного [эффекта перехода](/ru/guide/transition-effects.md). Если переход не предусмотрен, по умолчанию будет использоваться эффект перекрестного затухания.
pos | List&lt;Decimal&gt; | Положение (относительно границ экрана, в процентах) актора. Позиция описывается следующим образом: «0.0» - левый нижний угол, «50.50» - центр, а «100.100» - верхний правый угол экрана. Используйте Z-компонент (третий параметр, например, `,,10`) для перемещения (сортировки) по глубине в режиме орто.
id | Строка | Идентификатор актора для изменения; укажите `*`, чтобы повлиять на всех видимых акторов.
appearance | Строка | Устанавливает внешний вид или позу изменяемого актора.
transition | Строка | Тип используемого [эффекта перехода](/ru/guide/transition-effects.md) (по умолчанию используется кроссфейд).
params | List&lt;Decimal&gt; | Параметры эффекта перехода.
dissolve | String | Путь к текстуре [нестандартное растворение](/ru/guide/transition-effects.md#custom-transition-effects) (путь должен указываться относительно папки `Resources`). Действует только когда переход установлен в режим «Пользовательский».
visible | Boolean | Статус видимости для изменяемого актора.
position | List&lt;Decimal&gt; | Положение (в мировом пространстве) для изменяемого актора. Используйте Z-компонент (третий параметр) для перемещения (сортировки) по глубине в режиме орто.
rotation | List&lt;Decimal&gt; | Вращение для изменяемого актора.
scale | List&lt;Decimal&gt; | Масштаб для изменяемого актора.
tint | String | Цвет оттенка для изменяемого актора.  <br /><br />  Строки, начинающиеся с `#`, будут анализироваться как шестнадцатеричные следующим образом:  `#RGB` (становится RRGGBB), `#RRGGBB`, `#RGBA` (становится RRGGBBAA), `#RRGGBBAA`; если альфа не указана, по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как буквенные цвета, поддерживаются следующие параметры:  красный, голубой, синий, темно-синий, светло-синий, фиолетовый, желтый, салатовый, фуксия, белый, серебристый, серый, черный, оранжевый, коричневый, темно-бордовый, зеленый, оливковый, темно-синий, бирюзовый, аква, пурпурный.
easing | String | Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актора.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
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
Воспроизведение или изменение текущей воспроизводимой дорожки [BGM (фоновой музыки)](/ru/guide/audio.md#background-music) с указанным именем.

#### Примечания
Музыкальные треки зациклены по умолчанию. Если название музыкальной дорожки (BgmPath) не указано, будут затронуты все воспроизводимые в данный момент дорожки.  При вызове для дорожки, которая уже воспроизводится, воспроизведение не будет затронуто (дорожка не начнет воспроизводиться с самого начала), но будут применены указанные параметры (громкость и зацикленность дорожки).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">BgmPath</span> | String | Путь к музыкальной дорожке для воспроизведения.
intro | String | Путь к вступительной музыкальной дорожке, которую нужно воспроизвести один раз перед основной дорожкой (не зависит от параметра цикла).
volume | Decimal | Громкость музыкальной дорожки.
loop | Boolean | Играть ли дорожку с начала, когда она заканчивается.
fade | Decimal | Длительность постепенного увеличения громкости при запуске воспроизведения и затухания при остановке, в секундах (по умолчанию 0.0); не действует при изменении воспроизводимой дорожки.
group | String | [Группа](https://docs.unity3d.comScriptReferenceAudio.AudioMixer.FindMatchingGroups) в аудиомикшере, используемая при воспроизведении аудио.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию 0.35 секунды.

</div>

#### Примеры
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
printer | String | ID актора принтера для использования. Если не задано, будет использовать значение по умолчанию.
author | String | ID актора, который должен быть связан с добавленным переводом строки.

</div>

#### Примеры
```
; Второе предложение будет напечатано в новой строке
Lorem ipsum dolor sit amet.[br]Consectetur adipiscing elit.

; Второе предложение будет печататься двумя строками под первым
Lorem ipsum dolor sit amet.[br 2]Consectetur adipiscing elit.
```

## camera

#### Краткое описание
Модифицирует основную камеру, изменяя смещение, уровень масштабирования и вращение во времени. См. [это видео](https://youtu.be/zy28jaMss8w) для быстрой демонстрации эффекта команды.

#### Параметры
<div class="config-table">

ID | Тип | Описание
--- | --- | ---
offset | List&lt;Decimal&gt; | Локальное смещение положения камеры в единицах по осям X, Y, Z.
roll | Decimal | Локальное вращение камеры по оси Z в градусах (от 0.0 до 360.0 или от -180.0 до 180.0). То же, что и третий компонент параметра вращение; игнорируется, когда указано `rotation`.
rotation | List&lt;Decimal&gt; | Локальное вращение камеры по осям X, Y, Z в градусах (от 0.0 до 360.0 или от -180.0 до 180.0).
zoom | Decimal | Установите масштаб камеры (ортогональный размер или поле зрения, в зависимости от режима рендеринга) в диапазоне от 0.0 (без увеличения) до 1.0 (при полном увеличении).
ortho | Boolean | Должна ли камера отображаться в орфографическом (истинном) или перспективном (ложном) режиме.
toggle | List&lt;String&gt; | Имена компонентов для переключения (включить, если отключено, и наоборот). Компоненты должны быть прикреплены к тому же игровому объекту, что и камера. Это можно использовать для переключения [пользовательские эффекты пост-обработки](/ru/guide/special-effects.md#camera-effects).
set | List&lt;Named&lt;Boolean&gt;&gt; | Имена включаемых/выключаемых компонентов. Компоненты должны быть прикреплены к тому же игровому объекту, что и камера.  This can be used to explicitly enable or disable [custom post-processing effects](/ru/guide/special-effects.md#camera-effects). Указанное состояние компонентов перекроет эффект от параметра `toggle`.
easing | String |Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации камеры.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
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
Модифицирует [актор персонажа](/ru/guide/characters.md).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">IdAndAppearance</span> | Named&lt;String&gt; | ID, который нужно изменить (укажите `*`, чтобы затронуть все видимые символы) и внешний вид(или [поза](/ru/guide/characters.md#poses)), который нужно установить. Если внешний вид не указан, будет использоваться либо «По умолчанию» (сущетвует), либо случайный.
look | String | Смотреть направление актора; поддерживаемые значения: слева, справа, по центру.
avatar | String | Имя (путь) [текстуры аватара](/ru/guide/characters.md#avatar-textures) для назначения персонажу. Используйте `none`, чтобы удалить (отменить) текстуру аватара у персонажа.
pos | List&lt;Decimal&gt; | Установка положения (относительно границ экрана, в процентах), для измененного актора. Положение описывается следующим образом: «0.0» - левый нижний угол, «50.50» - центр, а «100.100» - верхний правый угол экрана. Используйте Z-компонент (третий параметр, например, `,, 10`) для перемещения (сортировки) по глубине в режиме орто.
id | String | ID для изменения; укажите `*`, чтобы повлиять на всех видимых акторов.
appearance | String | Внешний вид (или поза),  измененного актора.
transition | String | Тип используемого [эффекта перехода](/ru/guide/transition-effects.md) (по умолчанию используется перекрестное затухание).
params | List&lt;Decimal&gt; | Параметры эффекта перехода.
dissolve | String | Путь к текстуре [custom disolve](/ru/guide/transition-effects.md#custom-transition-effects) путь должен указываться относительно папки `Resources`). Действует только когда переход установлен в режим «Пользовательский».
visible | Boolean | Статус видимости, измененного актора.
position | List&lt;Decimal&gt; | Положение (в мировом пространстве) измененного актора. Используйте Z-компонент (третий параметр) для перемещения (сортировки) по глубине в режиме орто.
rotation | List&lt;Decimal&gt; | Вращение для измененного актора.
scale | List&lt;Decimal&gt; | Масштаб, для измененного актора
tint | String | Цвет оттенка измененного актора.  <br /><br />  Строки, начинающиеся с `#`, будут анализироваться как шестнадцатеричные следующим образом:  `#RGB` (becomes RRGGBB), `#RRGGBB`, `#RGBA` (becomes RRGGBBAA), `#RRGGBBAA`; если альфа не указана, по умолчанию будет FF.  <br /><br />  Строки, которые не начинаются с `#`, будут анализироваться как буквенные цвета, поддерживаются следующие значения: красный, голубой, синий, темно-синий, светло-синий, фиолетовый, желтый, салатовый, фуксия, белый, серебристый, серый, черный, оранжевый, коричневый , бордовый, зеленый, оливковый, темно-синий, чирок, аква, пурпурный.
easing | String | Имя функции замедления, используемой для модификации.  <br /><br />  Доступные Варианты: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указан, будет использоваться функция замедления по умолчанию, установленная в настройках конфигурации менеджера актора.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
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

; Тонируем всех видимых персонажей на сцене.
@char * tint:#ffdc22
```

## choice

#### Краткое описание
Добавляет параметр [choice](/ru/guide/choices.md) в обработчик выбора с указанным идентификатором (или идентификатором по умолчанию).

#### Примечание
Если параметры `goto`,` gosub` и `do` не указаны, выполнение сценария будет продолжено со следующей строки сценария.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">ChoiceSummary</span> | String | Текст для отображения на выбор. Если текст содержит пробелы, заключите его в двойные кавычки (`" `). Если вы хотите включить двойные кавычки в сам текст, избегайте их.
button | String | Путь (относительно папки `Resources`) к [кнопке prefab](/ru/guide/choices.md#choice-button), представляющей выбор. Префаб должен иметь компонент `ChoiceHandlerButton`, присоединенный к корневому объекту. Будет использоваться кнопка по умолчанию, если она не указана.
pos | List&lt;Decimal&gt; | Локальное положение кнопки выбора внутри обработчика выбора (если поддерживается реализацией обработчика).
handler | String | ID выбора, для которого нужно добавить выбор. Будет использовать обработчик по умолчанию, если он не указан.
goto | Named&lt;String&gt; | Путь, когда вариант ответа выбирается пользователем; см. команду [@goto] для формата пути.
gosub | Named&lt;String&gt; | Путь к подпрограмме, которую нужно выбрать пользователем; см. команду [@gosub] для формата пути. Когда назначено `goto`, этот параметр будет игнорироваться.
set | String | Установите выражение для выполнения, когда вариант ответа выбран пользователем; см. команду [@set] для справки по синтаксису.
do | List&lt;String&gt; | Команды скрипта для выполнения, когда вариант ответа выбран пользователем; не забывайте экранировать запятые внутри значений списка, чтобы они не рассматривались как разделители. Команды будут вызываться по порядку после обработки `set`,` goto` и `gosub` (если назначены).
show | Boolean | Показывать ли также обработчик выбора, для которого добавлен выбор; включен по умолчанию.
time | Decimal |Продолжительность (в секундах) анимации постепенного появления (раскрытия). Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Напечатайте текст, затем немедленно покажите варианты и остановите выполнение скрипта.
Продолжить выполнение этого скрипта или ...? 
@choice "Continue"
@choice "Load another script from start" goto:AnotherScript
@choice "Load another script from \"MyLabel\" label" goto:AnotherScript.MyLabel
@choice "Goto to \"MySub\" subroutine in another script" gosub:AnotherScript.MySub
@stop

; Вы также можете установить пользовательские переменные в зависимости от выбора.
@choice "I'm humble, one is enough..." set:score++
@choice "Two, please." set:score=score+2
@choice "I'll take the entire stock!" set:karma--;score=999

; Воспроизвести звуковой эффект и расставить персонажей, когда будет выбран вариант ответа
@choice "Arrange" goto:.Continue do:"@sfx Click, @arrange k.10\,y.55"

; В следующем примере показано, как создать интерактивную карту с помощью команд `@choice`.
; В этом примере мы предполагаем, что внутри папки `Resources / MapButtons` вы сохранили префабы с компонентом` ChoiceHandlerButton`, прикрепленным к их корневым объектам.
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

#### Краткое описание
Удаляет все сообщения из [принтер бэклога](/ru/guide/printer-backlog.md).

#### Примеры
```
@clearBacklog
```

## clearChoice

#### Краткое описание
Удаляет все варианты ответа в обработчике выбора с предоставленным идентификатором (или по умолчанию, если идентификатор не указан; или во всех существующих обработчиках, если в качестве идентификатора указан `*`) и (необязательно) скрывает его (их).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">HandlerId</span> | String | ID обработчика выбора очистить. Если он не указан, будет использоваться обработчик по умолчанию. Укажите `*`, чтобы очистить все существующие обработчики.
hide | Boolean | Следует ли также скрывать затронутые принтеры.

</div>

#### Example
```
; Добавьте варианты ответа и удалите их через установленное время (если игрок не выбрал один).
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

#### Краткое описание
Уничтожает объект, созданный командой [@spawn].

#### Примечание
Если в префабе есть компонент `UnityEngine.MonoBehaviour`, прикрепленный к корневому объекту, и компонент реализует интерфейс` Naninovel.Commands.DestroySpawned.IParameterized`, перед уничтожением объекта будут переданы указанные значения `params`; если компонент реализует интерфейс `Naninovel.Commands.DestroySpawned.IAwaitable`, выполнение команды будет ждать выполнения задачи асинхронного завершения, возвращаемой реализацией, прежде чем уничтожить объект.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | String | Имя (путь) префаба, который нужно уничтожить. Ожидается, что команда [@spawn] с тем же параметром будет выполнена раньше.
params | List&lt;String&gt; | Параметры, которые нужно установить перед уничтожением префаба. Требуется, чтобы к префабу был прикреплен компонент  `Naninovel.Commands.DestroySpawned.IParameterized` , связанный с корневым объектом.

</div>

#### Примеры
```
; Учитывая, что команда "@spawn Rainbow" была выполнена раньше
@despawn Rainbow
```

## else

#### Краткое описание
Отмечает ветвь блока условного выполнения, который всегда выполняется в случае, если не выполняются условия открывающей [@if] и всех предшествующих [@elseif] (если есть) команд. Примеры использования см. в руководстве [условное выполнение](/ru/guide/naninovel-scripts.md#conditional-execution).

## elseIf

#### Краткое описание
Отмечает ветвь блока условного выполнения, которая выполняется в случае выполнения собственного условия (выражение оценивается как истинное), в то время как условия открытия [@if] и всех предшествующих команд [@elseif] (если есть) не выполняются. Примеры использования см. в руководстве [условное выполнение](/ru/guide/naninovel-scripts.md#conditional-execution).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | [Выражение сценария](/ru/guide/script-expressions.md), которое должно возвращать логическое значение.

</div>

## endIf

#### Краткое описание
Закрывает блок условного выполнения [@if]. Примеры использования см. в руководстве [условное выполнение](/ru/guide/naninovel-scripts.md#conditional-execution).

## finishTrans

#### Краткое описание
Завершает переход между сценами, начатый командой [@startTrans]; см. справку по команде запуска для получения дополнительной информации и примеров использования.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Transition</span> | String | Тип используемого [эффекта перехода](/ru/guide/transition-effects.md) (по умолчанию используется кроссфейд).
params | List&lt;Decimal&gt; | Параметры эффекта перехода.
dissolve | String | Путь к [заказному растворению](/ru/guide/transition-effects.md#custom-transition-effects) текстура (путь должен быть относительно папки `Resources`). Имеет эффект, только если для перехода установлен режим `Пользовательский`.
easing | String |Имя функции замедления, используемой для модификации.  <br /><br />  Available options: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br /> Если не указано, будет использоваться функция ослабления по умолчанию, установленная в настройках конфигурации менеджера актора.
time | Decimal | Продолжительность (в секундах) перехода. Значение по умолчанию: 0.35 секунды.

</div>

## gosub

#### Краткое описание
Переходит при воспроизведении скрипта наниновелл по указанному пути и сохраняет этот путь в глобальном состоянии; команды [@return] используют эту информацию для перенаправления на команду после последней запущенной команды gosub. Предназначен для использования в качестве функции (подпрограммы) в языке программирования, позволяющей повторно использовать фрагмент сценария наниновелл. Полезно для многократного вызова повторяющегося набора команд.

#### Примечание
Можно объявить gosub вне текущего воспроизводимого скрипта и использовать его из любых других скриптов; по умолчанию сброс состояния не происходит, когда вы загружаете другой скрипт для воспроизведения gosub или возвращаетесь назад, чтобы предотвратить задержки и загрузочные экраны. Однако имейте в виду, что все ресурсы, указанные в сценарии gosub, будут храниться до следующего сброса состояния.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | Named&lt;String&gt; | Путь для перехода в следующем формате: `ScriptName.LabelName`. Если имя метки опущено, скрипт будет воспроизводиться с самого начала. Если имя сценария опущено, попытается найти метку в текущем воспроизводимом сценарии.
reset | List&lt;String&gt; | Если указано, сбрасывает состояние служб ядра перед загрузкой сценария (если путь ведет к другому сценарию). Укажите `*`, чтобы сбросить все службы (кроме диспетчера переменных), или укажите имена служб, которые нужно исключить из сброса. По умолчанию состояние не сбрасывается.

</div>

#### Примеры
```
; Перейдите при воспроизведении к метке `VictoryScene` в воспроизводимом в данный момент скрипте, выполняет команды и переходит обратно к команде после `gosub`.
@gosub .VictoryScene
...
@stop

# VictoryScene
@back Victory
@sfx Fireworks
@bgm Fanfares
You are victorious!
@return

; Другой пример с некоторым ветвлением внутри подпрограммы.
@set time=10
; Здесь мы получаем один результат
@gosub .Room
...
@set time=3
; И вот мы получаем еще один
@gosub .Room
...

# Room
@print "It's too early, I should visit this place when it's dark." if:time<21&&time>6
@print "I can sense an ominous presence here!" if:time>21&&time<6
@return
```

## goto

#### Краткое описание
Перемещает воспроизведение сценария наниновеллы по указанному пути. Когда путь ведет к другому (не воспроизводимому в данный момент) скрипту новеллы, он также [сбросит состояние](/api/#resetstate)   перед загрузкой целевого скрипта, если [ResetStateOnLoad](https://naninovel.com/ru/guide/configuration.html#state)  не отключен в комплектации.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | Named&lt;String&gt; |Путь для перехода в следующем формате: `ScriptName.LabelName`. Если имя метки опущено, скрипт будет воспроизводиться с самого начала. Если имя сценария опущено, попытается найти метку в текущем воспроизводимом сценарии.
reset | List&lt;String&gt; | Если указано, сбрасывает состояние служб ядра перед загрузкой сценария (если путь ведет к другому сценарию). Укажите `*`, чтобы сбросить все службы (кроме диспетчера переменных), или укажите имена служб, которые нужно исключить из сброса. Укажите `-`, чтобы не выполнять сброса (даже если он включен по умолчанию в конфигурации). Значение по умолчанию контролируется опцией `Reset State On Load` в меню конфигурации двигателя.
</div>

#### Примеры
```
; Загружает и запускает воспроизведение сценария новеллы с именем `Script001` с самого начала.
@goto Script001

; Сохраните, как указано выше, но начните играть с метки`AfterStorm`.
@goto Script001.AfterStorm

; Перемещает воспроизведение к метке `Epilogue` в воспроизводимом в данный момент скрипте.
@goto .Epilogue

; Загрузите Script001, но не сбрасывайте аудио-менеджер (воспроизведение звука не прерывается). Имейте в виду, что исключение сброса состояния формы службы оставит связанные ресурсы в памяти.
@goto Script001 reset:IAudioManager
```

## hide

#### Краткое описание
Скрывает (делает невидимыми) акторов (персонаж, фон, текстовый принтер, обработчик выбора и т. д.) С указанными идентификаторами. В случае, если найдены несколько акторов с одним и тем же идентификатором (например, персонаж и принтер), это повлияет только на первого найденного.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | ID акторов, которых нужно скрыть.
time | Decimal | Продолжительность (в секундах) анимации затухания. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Если актор с ID `SomeActor` виден, то скройте его в течение 3 секунд.
@hide SomeActor time:3

; Спрячьте акторов `Kohaku` и `Yuko`.
@hide Kohaku,Yuko
```

## hideAll

#### Краткое описание
Скрывает (удаляет) всех действующих лиц (например, персонажей, фоны, текстовые принтеры, обработчики выбора и т. Д.) на сцене.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
time | Decimal | Продолжительность (в секундах) анимации затухания. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
@hideAll
```

## hideChars

#### Краткое описание
Скрывает (удаляет) всех видимых персонажей на сцене.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
time | Decimal | Продолжительность (в секундах) анимации затухания. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
@hideChars
```

## hidePrinter

#### Краткое описание
Скрывает текстовый принтер.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID используемого принтера. Если не указано иное, будет использоваться значение по умолчанию.
time | Decimal | Продолжительность (в секундах) анимации скрытия. Значение по умолчанию для каждого принтера устанавливается в конфигурации актора.

</div>

#### Примеры
```
; Скрыть принтер по умолчанию.
@hidePrinter
; Скрыть принтер с ID `Wide`.
@hidePrinter Wide
```

## hideUI

#### Краткое описание
Делает [элементы пользовательского интерфейса](/ru/guide/user-interface.md#ui-customization) невидимыми с указанными именами. Если имена не указаны, прекратит рендеринг (скроет) весь UI (включая все встроенные пользовательские интерфейсы).

#### Примечание
Если скрыть весь UI с помощью этой команды и параметр `allowToggle`  имеет значение false (по умолчанию), пользователь не сможет повторно отобразить IU с помощью горячих клавиш или щелкнув в любом месте экрана; используйте команду [@showUI], чтобы снова сделать UI ~~ великолепный ~~ видимым.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">UINames</span> | List&lt;String&gt; | Имя скрытых элементов пользовательского интерфейса.
allowToggle | Boolean | При скрытии всего UI, определяет, разрешить ли пользователю повторно отображать UI с помощью горячих клавиш или щелчком в любом месте экрана (по умолчанию false). Не влияет на скрытие определенного UI.
time | Decimal | Продолжительность (в секундах) анимации скрытия. Если не указано, будет использоваться продолжительность, зависящая от UI.

</div>

#### Примеры
```
; При наличии пользовательского UI `Calendar` следующая команда скроет его.
@hideUI Calendar

; Скрыть весь UI, не позволит пользователю повторно показать его
@hideUI
...
; Сделайте UI снова видимым
@showUI

; Скрыть весь UI, но разрешить пользователю переключать его обратно
@hideUI allowToggle:true

; Одновременно скройте встроенный UI `TipsUI` и UI ` Calendar`.
@hideUI TipsUI,Calendar
```

## i

#### Краткое описание
Удерживает выполнение скрипта до тех пор, пока пользователь не активирует вход `continue`. Ярлык для `@wait i`.

#### Примеры
```
; Пользователь должен будет активировать ввод `continue` после первого предложения, чтобы принтер продолжил печать следующий текст.
Lorem ipsum dolor sit amet.[i] Consectetur adipiscing elit.
```

## if

#### Краткое описание
Обозначает начало блока условного выполнения. Всегда должен закрываться командой [@endif]. Примеры использования см. в руководстве [условное выполнение](/ru/guide/naninovel-scripts.md#conditional-execution).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | [Выражение сценария](/ru/guide/script-expressions.md), которое должно возвращать логическое значение.

</div>

## input

#### Краткое описание
Показывает UI поля ввода, где пользователь может ввести произвольный текст. После отправки введенный текст будет присвоен указанной пользовательской переменной.

#### Примечание
Посмотрите этот [видеогид](https://youtu.be/F9meuMzvGJw) на примере использования.  <br /><br />  Чтобы назначить отображаемое имя для символа с помощью этой команды, рассмотрите возможность [привязки имени к пользовательской переменной](/ru/guide/characters.html#display-names).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">VariableName</span> | String | Имя пользовательской переменной, которой будет назначен введенный текст.
summary | String |Необязательный текст сводки для отображения вместе с полем ввода. Если текст содержит пробелы, заключите его в двойные кавычки (`" `). Если вы хотите включить двойные кавычки в сам текст, не используйте их.
value | String | Предопределенное значение для поля ввода.
play | Boolean | Следует ли автоматически возобновлять воспроизведение скрипта, когда пользователь отправляет форму ввода.

</div>

#### Примеры
```
; Разрешить пользователю вводить произвольный текст и назначать его пользовательской переменной состояния `name`
@input name summary:"Choose your name."
; Команда Stop требуется для остановки выполнения скрипта до тех пор, пока пользователь не отправит ввод
@stop

; Затем вы можете ввести присвоенную переменную `name` в скрипты новеллы.
Archibald: Greetings, {name}!
{name}: Yo!

; ...или используйте его внутри множественных и условных выражений
@set score=score+1 if:name=="Felix"
```

## lipSync

#### Краткое описание
Позволяет принудительно остановить анимацию рта с синхронизацией губ для персонажа с предоставленным идентификатором; при остановке анимация не запустится снова, пока эта команда не будет разрешена снова. Персонаж должен иметь возможность получать события синхронизации губ (в настоящее время только общие реализации и реализации Live2D). См. [Руководство персонажей](/ru/guide/characters.md#lip-sync) для получения дополнительной информации о функции синхронизации губ.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">CharIdAndAllow</span> | Named&lt;Boolean&gt; | ID символа, за которым следует логическое значение (true или false), указывающее, следует ли остановить или разрешить анимацию синхронизации губ.

</div>

#### Примеры
```
; Поскольку автоматическое озвучивание отключено, а синхронизация губ управляется текстовыми сообщениями, исключите знаки препинания из анимации рта.
Kohaku: Lorem ipsum dolor sit amet[lipSync Kohaku.false]... [lipSync Kohaku.true]Consectetur adipiscing elit.
```

## loadScene

#### Краткое описание
Загружает [сцену Unity](https://docs.unity3d.com/Manual/CreatingScenes.html) с указанным именем. Не забудьте добавить необходимые сцены в [настройки сборки](https://docs.unity3d.com/Manual/BuildSettings.html), чтобы сделать их доступными для загрузки.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">SceneName</span> | String | Имя загружаемой сцены.
additive | Boolean | Следует ли загружать сцену аддитивно или выгружать текущие загруженные сцены перед загрузкой новой (по умолчанию). См. [Документацию по загрузке сцены](https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.LoadScene.html) для получения дополнительной информации.

</div>

#### Примеры
```
; Загрузить сцену "MyTestScene" в одиночном режиме
@loadScene MyTestScene
; Загрузить сцену "MyTestScene" в аддитивном режиме
@loadScene MyTestScene additive:true
```

## lock

#### Краткое описание
Устанавливает [разблокируемый элемент](/ru/guide/unlockable-items.md) с предоставленным идентификатором в состояние `locked` .

#### Примечание
Разблокированное состояние элементов сохраняется в [global scope](/ru/guide/state-management.md#global-state).<br /> Если элемент с предоставленным идентификатором не зарегистрирован в карте глобального состояния, соответствующая запись будет добавлена автоматически.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Id</span> | String | ID разблокируемого предмета. Используйте `*`, чтобы заблокировать все зарегистрированные разблокируемые элементы.

</div>

#### Примеры
```
@lock CG/FightScene1
```

## look

#### Краткое описание
Включает / выключает режим обзора камеры, когда игрок может смещать основную камеру с помощью устройств ввода (например, перемещая мышь или используя аналоговый джойстик геймпада). Посмотрите [это видео](https://youtu.be/rC6C9mA7Szw) для быстрой демонстрации команды.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
enable | Boolean | Включать или отключать режим просмотра камеры. По умолчанию: true.
zone | List&lt;Decimal&gt; | Связанный прямоугольник с размерами X, Y в единицах от исходного положения камеры, описывающий, как далеко камера может быть перемещена. По умолчанию: 5.3.
speed | List&lt;Decimal&gt; | Скорость движения (чувствительность) камеры по осям X, Y. По умолчанию: 1.5,1.
gravity | Boolean | Следует ли автоматически перемещать камеру в исходное положение, когда вход просмотра не активен (например, мышь не двигается или аналоговый джойстик находится в положении по умолчанию). По умолчанию: false.

</div>

#### Примеры
```
; Активировать режим просмотра камеры с параметрами по умолчанию
@look

; Активируйте режим просмотра камеры с пользовательскими параметрами
@look zone:6.5,4 speed:3,2.5 gravity:true

; Отключить режим обзора камеры и сбросить смещение камеры
@look enabled:false
@camera offset:0,0
```

## movie

#### Краткое описание
Воспроизводит фильм с указанным именем (путем).

#### Примечание
Изображение на экране исчезнет перед воспроизведением фильма и снова исчезнет после воспроизведения. Воспроизведение можно отменить, активировав вход `cancel`  (по умолчанию клавиша `Esc`).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">MovieName</span> | String | Имя ресурса фильма для воспроизведения.

</div>

#### Примеры
```
; Если видеоклип "Opening" добавлен в ресурсы, воспроизводит его.
@movie Opening
```

## print

#### Краткое описание
Печатает (показывает с течением времени) указанное текстовое сообщение с помощью актора текстового принтера.

#### Примечание
Эта команда используется при обработке общих текстовых строк, например, общая строка `Kohaku: Hello World!` будет автоматически преобразована в `@print" Hello World! " author: Kohaku` при разборе скриптов новеллы. <br /> По умолчанию сбросит (очистит) принтер перед печатью нового сообщения; установите для параметра `reset` значение *false* или отключите `Auto Reset` в конфигурации актора принтера, чтобы предотвратить это, и вместо этого добавьте текст. <br /> Сделает принтер по умолчанию и скроет другие принтеры по умолчанию; установите для параметра `default` значение *false* или отключите `Auto Default` в конфигурации актора принтера, чтобы предотвратить это. <br /> Будет ждать ввода данных пользователем перед завершением задачи по умолчанию; установите для параметра `waitInput` значение *false* или отключите `Auto Wait` в конфигурации актора принтера, чтобы он возвращался, как только текст полностью раскрывается. <br />

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Text</span> | String | Текст сообщения для печати. Если текст содержит пробелы, заключите его в двойные кавычки (`"`). Если вы хотите включить двойные кавычки в сам текст, избегайте их.
printer | String | ID используемого принтера. Если не указано иное, будет использоваться значение по умолчанию.
author | String | ID исполнителя, который должен быть связан с напечатанным сообщением.
speed | Decimal | Множитель скорости отображения текста; должно быть положительным или нулевым. Установка на единицу даст скорость по умолчанию.
reset | Boolean | Следует ли сбрасывать текст принтера перед выполнением задания печати. Значение по умолчанию контролируется с помощью свойства `Auto Reset` в меню конфигурации актора принтера.
default | Boolean | Использовать ли принтер по умолчанию и скрывать другие принтеры перед выполнением задания печати. Значение по умолчанию контролируется с помощью свойства `Auto Default` в меню конфигурации актора принтера.
waitInput | Boolean | Следует ли ждать ввода данных пользователем после завершения задания печати. Значение по умолчанию контролируется с помощью свойства `Auto Wait` в меню конфигурации актора принтера.
br | Integer | Количество разрывов строки, добавляемых перед печатным текстом. Значение по умолчанию контролируется с помощью свойства `Auto Line Break` в меню конфигурации актора принтера.
fadeTime | Decimal | Управляет продолжительностью (в секундах) отображения и скрытия принтеров анимации, связанной с этой командой. Значение по умолчанию для каждого принтера устанавливается в конфигурации актора.

</div>

#### Примеры
```
; Распечатает фразу на принтере по умолчанию.
@print "Lorem ipsum dolor sit amet."

; Чтобы включить кавычки в сам текст, экранируйте их.
@print "Saying \"Stop the car\" was a mistake."

; Показывать сообщение с половинной скоростью и не ждать продолжения ввода пользователя.
@print "Lorem ipsum dolor sit amet." speed:0.5 waitInput:false
```

## printer

#### Краткое описание
Изменяет [актора текстового принтера](/ru/guide/text-printers.md).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">IdAndAppearance</span> | Named&lt;String&gt; | ID принтера, который нужно изменить, и внешний вид, который нужно установить. Если идентификатор или внешний вид не указаны, будут использоваться значения по умолчанию.
default | Boolean | Использовать ли этот принтер по умолчанию. Принтер по умолчанию будет подчиняться всем командам, связанным с принтером, если параметр `printer` не указан.
hideOther | Boolean | Скрывать ли все остальные принтеры.
pos | List&lt;Decimal&gt; | Положение (относительно границ экрана, в процентах), устанавливаемое для измененного принтера. Положение описывается следующим образом: «0,0» - это нижний левый угол, «50,50» - центр и «100,100» - это верхний правый угол экрана.
visible | Boolean | Показывать или скрывать принтер.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Сделает принтер `Wide` по умолчанию и скроет все остальные видимые принтеры.
@printer Wide

; Назначит внешний вид `Right` принтеру `Bubble`, по умолчанию будет располагаться в центре экрана и не будет скрывать другие принтеры.
@printer Bubble.Right pos:50,50 hideOther:false
```

## processInput

#### Краткое описание
Позволяет останавливать и возобновлять обработку пользовательского ввода (например, реагировать на нажатие клавиш клавиатуры). Эффект от действия стойкий и сохраняется вместе с игрой.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">InputEnabled</span> | Boolean | Включить ли обработку ввода.

</div>

#### Примеры
```
; Остановить обработку ввода
@processInput false
; Возобновить обработку ввода
@processInput true
```

## purgeRollback

#### Краткое описание
Предотвращает откат игрока к предыдущим снимкам состояния.

#### Примеры
```
; Запретить игроку откатиться назад, чтобы попытаться выбрать другой вариант.

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

#### Краткое описание
Сбрасывает состояние [сервисов движка](https://naninovel.com/ru/guide/engine-services.html) и выгружает (удаляет) все ресурсы, загруженные Naninovel (текстуры, аудио, видео и т.д.); в основном вернется к пустому начальному состоянию двигателя.

#### Примечание
Процесс является асинхронным и маскируется экраном загрузки ([ILoadingUI](https://naninovel.com/ru/guide/user-interface.html#ui-customization)).  <br /><br />  Когда [ResetStateOnLoad](https://naninovel.com/ru/guide/configuration.html#state) отключен в конфигурации, вы можете использовать эту команду, чтобы вручную удалить неиспользуемые ресурсы, чтобы предотвратить проблемы с утечкой памяти.  <br /><br />  Имейте в виду, что эту команду нельзя отменить (перемотать назад).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Exclude</span> | List&lt;String&gt; | Название [сервисов движка](https://naninovel.com/ru/guide/engine-services.html) (интерфейсов), которые нужно исключить из сброса. При указании параметра всегда учитывайте добавление `ICustomVariableManager` для сохранения локальных переменных.

</div>

#### Примеры
```
; Сбросить все службы.
@resetState

; Сбросьте все службы, кроме менеджеров переменных и аудио (текущий звук продолжит воспроизведение).
@resetState ICustomVariableManager,IAudioManager
```

## resetText

#### Краткое описание
Сбрасывает (очищает) содержимое текстового принтера.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID используемого принтера. Если не указано иное, будет использоваться значение по умолчанию.

</div>

#### Примеры
```
; Очистите содержимое принтера по умолчанию.
@resetText
; Очистите содержимое принтера с ID `Fullscreen`.
@resetText Fullscreen
```

## return

#### Краткое описание
Пытается перейти при воспроизведении сценария naninovel к команде после последней использованной [@gosub]. См. Описание команды [@gosub] для получения дополнительной информации и примеров использования.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
reset | List&lt;String&gt; | Если указано, сбросит состояние сервисов движка перед возвратом к исходному сценарию, из которого был введен gosub (в случае, если это не текущий сценарий). Укажите `*`, чтобы сбросить все службы (кроме диспетчера переменных), или укажите имена служб, которые нужно исключить из сброса. По умолчанию состояние не сбрасывается.

</div>

## save

#### Краткое описание
Автоматически сохраняйте игру в слот для быстрого сохранения.

#### Примеры
```
@save
```

## set

#### Краткое описание
Присваивает результат [выражения сценария](/ru/guide/script-expressions.md) [пользовательской переменной](/ru/guide/custom-variables.md).

#### Примечания
Имя переменной должно быть буквенно-цифровым (только латинские символы) и может содержать символы подчеркивания, например: `name`,` Char1Score`, `my_score`; имена регистронезависимы, например: `myscore` равно `MyScore`. Если переменной с указанным именем не существует, она будет создана автоматически. <br /> <br /> Можно определить несколько выражений набора в одной строке, разделив их символом `;`. Выражения будут выполняться последовательно в порядке объявления. <br /> <br /> Пользовательские переменные по умолчанию хранятся в **local scope**. Это означает, что если вы назначите какую-то переменную в процессе игры, и игрок начнет новую игру или загрузит другой сохраненный игровой слот, где эта переменная не была назначена, значение будет потеряно. Если вместо этого вы хотите сохранить переменную в **global scope**, добавьте к ее имени `G_` или` g_`, например: `G_FinishedMainRoute` или` g_total_score`. <br /> <br /> Если имя переменной начинается с `T_` или` t_`, это считается ссылкой на значение, хранящееся в документе 'Script' [управляемый текст](/ru/guide/managed-text.md). Такие переменные не могут быть назначены и в основном используются для ссылки на локализуемые текстовые значения. <br /> <br /> Вы можете получить и установить пользовательские переменные в сценариях C # с помощью `CustomVariableManager` [служба движка](/ru/guide/engine-services.md).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Expression</span> | String | Установить выражение. <br /><br />  Выражение должно быть в следующем формате: `VariableName=ExpressionBody`, где `VariableName` - это имя настраиваемой переменной, которую нужно назначить, а `ExpressionBody` - это [выражение сценария](/ru/guide/script-expressions.md), результат которого следует присвоить переменной.  <br /><br />  Также можно использовать унарные операторы увеличения и уменьшения, например:  `@set foo++`, `@set foo--`.

</div>

#### Примеры
```
; Присвойте переменной `foo` строковое значение` bar`
@set foo="bar"

; Присвойте переменной `foo` одно числовое значение
@set foo=1

; Присвойте переменной `foo` логическое значение` true`
@set foo=true

; Если `foo` - это число, добавьте к его значению 0.5
@set foo=foo+0.5

; Если `angle` - это число, присвойте его косинус переменной `result`.
@set result=Cos(angle)

; Получить случайное целое число от -100 до 100, затем возвести в степень 4 и присвоить переменной `result`
@set "result = Pow(Random(-100, 100), 4)"

; Если `foo` - это число, добавьте к его значению 1
@set foo++

; Если `foo` - число, вычтите 1 из его значения
@set foo--

; Присвойте переменной `foo` значение переменной` bar`, то есть `Hello World!`. Обратите внимание, что переменная `bar` действительно должна существовать, в противном случае вместо этого будет присвоено значение обычного текста `bar`.
@set bar="Hello World!"
@set foo=bar

; Определение нескольких выражений множества в одной строке (результат будет таким же, как указано выше)
@set bar="Hello World!";foo=bar

; Можно вводить переменные в параметры команды скрипта naninovel
@set scale=0
# EnlargeLoop
@char Misaki.Default scale:{scale}
@set scale=scale+0.1
@goto .EnlargeLoop if:scale<1

; ..и общие текстовые строки
@set name="Dr. Stein";drink="Dr. Pepper"
{name}: My favourite drink is {drink}!

; При использовании двойных кавычек внутри самого выражения не забывайте экранировать их дважды.
@set remark="Saying \\"Stop the car\\" was a mistake."
```

## sfx

#### Краткое описание
Воспроизводит или изменяет воспроизводимую в данный момент [SFX (звуковой эффект)](/ru/guide/audio.md#sound-effects) дорожку с указанным именем.

#### Примечание
По умолчанию звуковые эффекты не зацикливаются. Если имя дорожки sfx (SfxPath) не указано, это повлияет на все воспроизводимые в данный момент дорожки. При вызове для дорожки, которая уже воспроизводится, воспроизведение не будет затронуто (дорожка не начнется с начала), но будут применены указанные параметры (громкость и зацикленность дорожки).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">SfxPath</span> | String | Path to the sound effect asset to play.
volume | Decimal | Громкость SFX.
loop | Boolean | Следует ли воспроизводить SFX в цикле.
fade | Decimal |Длительность нарастания громкости при запуске воспроизведения в секундах (по умолчанию 0.0); не действует при изменении воспроизводимой дорожки.
group | String | Аудиомикшер [путь к группе](https://docs.unity3d.com/ScriptReference/Audio.AudioMixer.FindMatchingGroups), который следует использовать при воспроизведении звука.
time | Decimal | Продолжительность (в секундах) модификации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Один раз воспроизводит SFX с названием `Explosion`
@sfx Explosion

; Воспроизводит SFX с названием `Rain` в цикле с плавным переходом в течение 30 секунд
@sfx Rain loop:true fade:30

; Изменяет громкость всех воспроизводимых SFX на 75% за 2.5 секунды и отключает зацикливание для всех из них
@sfx volume:0.75 loop:false time:2.5
```

## show

#### Краткое описание
Показывает (делает видимыми) акторов (персонаж, фон, текстовый принтер, обработчик выбора и т.д.) с указанными идентификаторами. В случае, если найдены несколько акторов с одним и тем же идентификатором (например, персонаж и принтер), это повлияет только на первого найденного.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">ActorIds</span> | List&lt;String&gt; | ID акторов, которых нужно показать.
time | Decimal |Продолжительность (в секундах) анимации затухания. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Если актор с ID `SomeActor` скрыт, покажите его (постепенно) в течение 3 секунд.
@show SomeActor time:3

; Шоу акторов `Kohaku` и `Yuko`.
@show Kohaku,Yuko
```

## showPrinter

#### Краткое описание
Показывает текстовый принтер.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">PrinterId</span> | String | ID спользуемого принтера. Если не указано иное, будет использоваться значение по умолчанию.
time | Decimal | Продолжительность (в секундах) анимации шоу. Значение по умолчанию для каждого принтера устанавливается в конфигурации актора.

</div>

#### Примеры
```
; Показать принтер по умолчанию.
@showPrinter
; Покажи принтер с ID `Wide`.
@showPrinter Wide
```

## showUI

#### Краткое описание
Делает видимыми [элементы UI](/ru/guide/user-interface.md)  с указанными префабами. Если имена не указаны, откроется весь пользовательский интерфейс (в случае, если он был скрыт с помощью [@hideUI]).

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">UINames</span> | List&lt;String&gt; | Имя префаба UI, который нужно сделать видимым.
time | Decimal | Продолжительность (в секундах) анимации шоу. Если не указано, будет использоваться продолжительность, зависящая от UI.

</div>

#### Примеры
```
;Если вы добавили настраиваемый UI с именем префаба `Calendar`, следующее сделает его видимым на сцене.
@showUI Calendar

; Если вы скрыли весь UI с помощью @hideUI, верните его
@showUI

; Одновременно откройте встроенный UI `TipsUI` и UI `Calendar`.
@showUI TipsUI,Calendar
```

## skip

#### Краткое описание
Позволяет включить или отключить режим «пропуска» скрипт-плеера.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">Enable</span> | Boolean | Следует ли включить (по умолчанию) или отключить режим пропуска.

</div>

#### Примеры
```
; Enable skip mode
@skip
; Disable skip mode
@skip false
```

## skipInput

#### Краткое описание
Может использоваться в общих текстовых строках для предотвращения активации режима `wait for input` при печати текста.

#### Примеры
```
; Проигрыватель скриптов не будет ждать ввода `continue` перед выполнением команды `@sfx`.
And the rain starts.[skipInput]
@sfx Rain
```

## slide

#### Краткое описание
Сдвигает (перемещается по оси X) актора (персонаж, фон, текстовый принтер или обработчик выбора) с предоставленным идентификатором и, при необходимости, изменяет внешний вид актора.

#### Примечание
Имейте в виду, что эта команда ищет актора с предоставленным идентификатором среди всех менеджеров акторов, и в случае, если существует несколько акторов с одним и тем же ID (например, персонаж и текстовый принтер), это повлияет только на первого найденного.

#### Parameters

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">IdAndAppearance</span> | Named&lt;String&gt; | ID актора для слайда и (необязательно) внешний вид, который нужно установить.
from | Decimal | Позиционируйте по оси X (в диапазоне от 0 до 100, в процентах от левой границы экрана), от которой следует перемещать актора. Если не указан, будет использоваться текущая позиция актора, если она видна, и случайная позиция за кадром в противном случае.
<span class="command-param-required" title="Required parameter: parameter should always be specified">to</span> | Decimal | Позиционируйте по оси X (в диапазоне от 0 до 100 в процентах от левого края экрана), куда нужно переместить актора.
visible | Boolean | Изменить статус видимости актора (показать или скрыть). Если не установлен и целевой актор скрыт, он все равно будет автоматически отображаться.
easing | String | Имя функции замедления, используемой для изменений.  <br /><br />  Available options: Linear, SmoothStep, Spring, EaseInQuad, EaseOutQuad, EaseInOutQuad, EaseInCubic, EaseOutCubic, EaseInOutCubic, EaseInQuart, EaseOutQuart, EaseInOutQuart, EaseInQuint, EaseOutQuint, EaseInOutQuint, EaseInSine, EaseOutSine, EaseInOutSine, EaseInExpo, EaseOutExpo, EaseInOutExpo, EaseInCirc, EaseOutCirc, EaseInOutCirc, EaseInBounce, EaseOutBounce, EaseInOutBounce, EaseInBack, EaseOutBack, EaseInOutBack, EaseInElastic, EaseOutElastic, EaseInOutElastic.  <br /><br />  Если не указано, будет использоваться функция ослабления по умолчанию, установленная в настройках конфигурации менеджера актора.
time | Decimal | Продолжительность (в секундах) слайд-анимации. Значение по умолчанию: 0.35 секунды.

</div>

#### Примеры
```
; Учитывая, что актор `Jenna` в настоящее время не виден, покажите его с появлением `Angry` и переместите в центр экрана.
@slide Jenna.Angry to:50

; Если актор `Sheba` в настоящее время виден, скройте его и сдвиньте за левую границу экрана.
@slide Sheba to:-10 visible:false

; Переместите актора `Mia` из левой части экрана вправо в течение 5 секунд, используя замедление анимации `EaseOutBounce`.
@slide Sheba from:15 to:85 time:5 easing:EaseOutBounce
```

## spawn

#### Краткое описание
Создает префаб или [специальный эффект](/ru/guide/special-effects.md); когда выполняется над уже созданным объектом, вместо этого обновляет параметры порождения.

#### Примечание
Если в префабе есть компонент `UnityEngine.MonoBehaviour`, прикрепленный к корневому объекту, и компонент реализует интерфейс `Naninovel.Commands.Spawn.IParameterized`, то после порождения передаст указанные значения `params`; если компонент реализует интерфейс `Naninovel.Commands.Spawn.IAwaitable`, выполнение команды будет ожидать выполнения задачи асинхронного завершения, возвращаемой реализацией.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Path</span> | String | Имя (путь) префаба для создания ресурса.
params | List&lt;String&gt; | PПараметры, устанавливаемые при порождении префаба. Требуется, чтобы в префабе к корневому объекту был прикреплен компонент `Naninovel.Commands.Spawn.IParameterized`.

</div>

#### Примеры
```
; Если префаб `Rainbow` назначен в ресурсах спауна, создайте его экземпляр.
@spawn Rainbow
```

## startTrans

#### Краткое описание
Начинает переход сцены, маскируя реальное содержимое сцены всем, что видно в данный момент (кроме пользовательского интерфейса). Когда новая сцена будет готова, закончите с командой [@finishTrans].

#### Примечание
Во время перехода UI будет скрыт, а ввод данных пользователем заблокирован. Вы можете изменить это, переопределив `ISceneTransitionUI`, который обрабатывает процесс перехода. <br /><br /> Список доступных опций эффектов перехода см. в руководстве [transition effects](/ru/guide/transition-effects.md).

#### Примеры
```
; Переход Felix в солнечный день с Jenna в дождливый день
@char Felix
@back SunnyDay
@fx SunShafts
@startTrans
; Следующие изменения не будут видны, пока мы не завершим переход
@hideChars time:0
@char Jenna time:0
@back RainyDay time:0
@stopFx SunShafts params:0
@fx Rain params:,0
; Переход от первоначально захваченной сцены к новой с эффектом `DropFade` в течение 3 секунд.
@finishTrans DropFade time:3
```

## stop

#### Краткое описание
Останавливает выполнение скрипта naninovel.

#### Примеры
```
Show the choices and halt script execution until the player picks one.
@choice "Choice 1"
@choice "Choice 2"
@stop
We'll get here after player will make a choice.
```

## stopBgm

#### Кратое описание
Останавливает воспроизведение трека BGM (фоновой музыки) с указанным именем.

#### Примечание
Если имя музыкальной дорожки (BgmPath) не указано, остановит все текущие воспроизводимые дорожки.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">BgmPath</span> | String | Путь к музыкальной дорожке, котрую нужно остановить
fade | Decimal | Продолжительность затухания громкости перед остановкой воспроизведения в секундах (по умолчанию 0.35).

</div>

#### Примеры
```
; Затухает музыкальный трек `Promenade` на 10 секунд и останавливает воспроизведение
@stopBgm Promenade fade:10

; Останавливает все проигрываемые в данный момент музыкальные треки
@stopBgm
```

## stopSfx

#### Краткое описание
Останавливает воспроизведение дорожки SFX (звукового эффекта) с указанным именем.

#### Примечание
Если имя дорожки звукового эффекта (SfxPath) не указано, все текущие воспроизводимые дорожки будут остановлены.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID">SfxPath</span> | String | Путь к звуковому эффекту, который нужно остановить.
fade | Decimal | Продолжительность затухания громкости перед остановкой воспроизведения в секундах (по умолчанию 0.35).

</div>

#### Примеры
```
; Прекратите проигрывать звуковой эффект с названием `Rain`, затухающий на 15 секунд.
@stopSfx Rain fade:15

; Останавливает все воспроизводимые в данный момент дорожки звуковых эффектов
@stopSfx
```

## stopVoice

#### Краткое описание
Останавливает воспроизведение проигрываемого в данный момент голосового клипа.

## style

#### Краткое описание
Постоянно применяемые [стили текста](/ru/guide/text-printers.md#text-styles) к содержимому текстового принтера.

#### Примечание
Вы также можете использовать теги форматированного текста внутри текстовых сообщений для выборочного применения стилей.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">TextStyles</span> | List&lt;String&gt; | Применяемые теги форматирования текста. Угловые скобки следует опустить, например, используйте `b` для &lt;b&gt; и `size=100` для &lt;size=100&gt;. Используйте ключевое слово `default` 
printer | String | ID используемого принтера. Если не указано иное, будет использоваться значение по умолчанию.

</div>

#### Примеры
```
; Напечатайте первые два предложения жирным красным шрифтом размером 45 пикселей, затем сбросьте стиль и напечатайте последнее предложение, используя стиль по умолчанию.
@style color=#ff0000,b,size=45
Lorem ipsum dolor sit amet.
Cras ut nisi eget ex viverra egestas in nec magna.
@style default
Consectetur adipiscing elit.

; Начальную часть предложения выведите как обычно, а последнюю - жирным шрифтом.
Lorem ipsum sit amet. <b>Consectetur adipiscing elit.</b>
```

## title

#### Краткое описание
Сбросить состояние двигателя и отобразить UI`ITitleUI` (главное меню).

#### Примеры
```
@title
```

## unlock

#### Краткое описание
Устанавливает [разблокируемый элемент](/ru/guide/unlockable-items.md)  с предоставленным идентификатором в состояние `unlocked`.

#### Примечание
Разблокированное состояние элементов сохраняется в [global scope](/ru/guide/state-management.md#global-state).<br />  Если элемент с предоставленным идентификатором не зарегистрирован в карте глобального состояния, соответствующий запись будет добавлена автоматически.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">Id</span> | String | ID разблокируемого предмета. Используйте `*`, чтобы разблокировать все зарегистрированные разблокируемые предметы.

</div>

#### Примеры
```
@unlock CG/FightScene1
```

## voice

#### Краткое описание
Воспроизводит голосовой клип по указанному пути.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">VoicePath</span> | String | Путь к голосовой записи для воспроизведения.
volume | Decimal | Громкость воспроизведения.
group | String | Аудиомикшер [путь к группе](https://docs.unity3d.com/ScriptReference/Audio.AudioMixer.FindMatchingGroups), который следует использовать при воспроизведении звука.

</div>

## wait

#### Краткое описание
Удерживает выполнение скрипта до указанного условия ожидания.

#### Параметры

<div class="config-table">

ID | Тип | Описание
--- | --- | ---
<span class="command-param-nameless command-param-required" title="Nameless parameter: value should be provided after the command identifer without specifying parameter ID  Required parameter: parameter should always be specified">WaitMode</span> | String | Условия ожидания:<br />  - `i` пользователь нажимает кнопку продолжения или пропуска ввода;<br />  - `0.0` таймер (секунды);<br />  - `i0.0` таймер, который можно пропустить клавишами продолжения или пропуска ввода.

</div>

#### Примеры
```
; Звуковой эффект "ThunderSound" будет воспроизводиться через 0.5 секунды после завершения фонового эффекта дрожания.
@fx ShakeBackground
@wait 0.5
@sfx ThunderSound

; Напечатайте первые два слова, затем дождитесь ввода пользователя, прежде чем печатать оставшуюся фразу.
Lorem ipsum[wait i] dolor sit amet.
; Вы также можете использовать следующий ярлык (@i command) для этого режима ожидания.

; Запустите SFX, распечатайте сообщение и подождите 5 секунд с возможностью пропуска, затем остановите SFX.
@sfx Noise loop:true
Jeez, what a disgusting noise. Shut it down![wait i5][skipInput]
@stopSfx Noise
```