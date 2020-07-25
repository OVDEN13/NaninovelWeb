# Сценарии Naninovel

Скрипты Naninovel – это текстовые документы (c расширениеv `.nani`), где вы контролируете то, что происходит в сценах. Ассеты сценариев создаются с помощью контекстного меню ассета `Create -> Naninovel -> Naninovel Script`. Вы можете открывать и редактировать их с помощью встроенного [визуального редактора](#visual-editor) или с помощью внешнего текстового редактора, например блокнот, TextEdit или [Atom](https://atom.io).

![](https://i.gyazo.com/f552c2ef323f9ec1171eba72e0c55432.png)

Каждая строка в сценарии Naninovel представляет собой инструкцию, которая может быть командой, обычным текстом, меткой или комментарием. Тип оператора определяется литералом, который помещается в начале строки:

Литерал | Тип инструкции 
:---: | --- 
@ | [Команда](#command-lines)
# | [Метка](#label-lines)
; | [Комментарий](#comment-lines)

Если в начале строки нет ни одного из вышеуказанных литералов, она воспринимается как строка [общего текста](#generic-text-lines).

## Командные строки

Строка воспринимается как командная, если начинается с литерала `@`. Команда представляет собой одиночную операцию, которая управляет тем, что происходит в сцене; например, она может быть использована для изменения фона, перемещения персонажа или загрузки другого сценария Naninovel.

### Идентификатор команды

Сразу после литерала команды ожидается идентификатор команды. Это может быть либо имя класса C#, реализующего команду, либо псевдоним команды (если он применяется через атрибут `CommandAlias`).

Например, команда [@save] (используемая для автоматического сохранения игры) реализуется через C# класс `AutoSave`. Реализующий класс также имеет атрибут `[CommandAlias("save")]`, поэтому для вызова этой команды в скрипте можно использовать операторы `@save` и `@AutoSave`.

Идентификаторы команд не чувствительны к регистру; все следующие операторы действительны и вызовут одну и ту же команду `AutoSave`:

```
@save
@Save
@AutoSave
@autosave
``` 

### Параметры команд

Большинство команд имеют ряд параметров, определяющих действие команды. Параметр – это выражение типа "ключ-значение", определяемое после командного литерала, разделенного столбцом (`:`). Идентификатор параметра (ключ) может быть либо именем соответствующего поля параметра класса реализации команды, либо псевдонимом параметра (если он определен через свойство `alias` атрибута `CommandParameter`).

```
@commandId paramId:paramValue 
```

Рассмотрим команду [@hideChars], которая используется для скрытия всех видимых персонажей в сцене. Её можно использовать следующим образом:

```
@hideChars
```

Вы можете здесь использовать параметр `time` типа *Decimal* для того, чтобы указать, как долго персонажи будут растворяться, прежде чем полностью скроются (исчезнут из сцены):

```
@hideChars time:5.5
```

Так персонажи будут угасать в течение 5.5 секунд до того, как полностью исчезнут из сцены.

Вы также можете использовать параметр `wait` типа *Boolean*, чтобы указать, следует ли выполнять следующую команду сразу после завершения текущей команды или дождаться ее завершения:

```
@hideChars time:5.5 wait:false
@hidePrinter
```

Так текстовый принтер скроется сразу после того, как персонажи начнут исчезать. Если `wait` будет иметь значение `true` (или значение не будет указано), принтер будет скрыт только тогда, когда [@hideChars] завершит выполнение.

### Типы значений параметров

В зависимости от собственного типа параметры могут ожидать один из следующих типов значений:

Тип | Описание
--- | ---
String | Простое строковое значение, например: `LoremIpsum`. Не забудьте заключить строку в двойные кавычки, если она содержит пробелы, например: `"Lorem ipsum dolor sit amet."`.
Integer | Число без дробей; целочисленное значение, например: `1`, `150`, `-25`.
Decimal | Десятичное число с дробью, отделённой точкой, например: `1.0`, `12.08`, `-0.005`.
Boolean | Имеет два доступных значения: `true` or `false` (нечуствительна к регистру).
Named<> | Строка имени, связанная со значением одного из вышеперечисленных типов. Часть имени отделена точкой. Например, для *Named&lt;Integer&gt;*: `foo.8`, `bar.-20`.
List<>| Список из значений одного из вышеперечисленных типов, разделённых точками. Например, для *List&lt;String&gt;*: `foo,bar,"Lorem ipsum."`, для *List&lt;Decimal&gt;*: `12,-8,0.105,2`

### Безымянные параметры

Большинство команд имеют безымянный параметр. Параметр считается безымянным, если может быть использован без указания его имени. 

Например, команда [@bgm] ожидает безымянный параметр, указывающий имя музыкального трека, который нужно проиграть:

```
@bgm PianoTheme
```
"PianoTheme" здесь – это значение типа *String* параметра "BgmPath".

В каждой команде может быть только один безымянный параметр, и он всегда должен указываться перед любыми другими параметрами.

### Опциональные и обязательные параметры

Большинство параметров команды являются *опциональными*. Это означает, что они либо имеют предопределенное значение, либо просто не требуют никакого значения для выполнения команды. Например, если команда [@resetText] используется без указания каких-либо параметров, она сбросит текст в принтере по умолчанию, но вы также можете установить определенный ID принтера следующим образом: `@resetText printer:Dialogue`.

Однако некоторые параметры являются *обязательными* для выполнения команды и всегда должны быть указаны.

### Справочик команд API

Для списка всех доступных сейчас команд с описанием, параметрами и примерами использования см. [справочник команд API](/ru/api/). 

## Универсальные текстовые строки

Чтобы сделать написание скриптов с большим объемом текста более удобным, используются универсальные текстовые строки. Строка считается универсальным текстовым оператором, если она не начинается ни с одного из вышеуказанных литералов операторов:

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
```

ID говорящего можно указать в начале универсальной текстовой строки, отделив их двоеточием (`:`), чтобы связать печатный текст с [актором персонажа](/ru/guide/characters.md):

```
Felix: Lorem ipsum dolor sit amet, consectetur adipiscing elit.
```

Чтобы сэкономить время и текст при постоянном изменении внешности персонажей, связанных с печатным текстом, вы также можете указать внешность после ID персонажа:

```
Felix.Happy: Lorem ipsum dolor sit amet.
```

Строка выше аналогична следующим двум:

```
@char Felix.Happy wait:false
Felix: Lorem ipsum dolor sit amet.
```

### Встраивание команд

Иногда вам может потребоваться выполнить команду во время отображения (печати) текстового сообщения, сразу после или перед определенным символом. Например, актор изменил бы свою внешность (выражение) при выведении определенного слова; или воспроизведение определенного звукового эффекта в ответ на какое-то событие, описанное в середине печатного сообщения. Функция встраивания команд позволяет обрабатывать подобные ситуации.

Все команды, (как [встроенные](/ru/api/) и [пользовательские](/ru/guide/custom-commands.md)) могут быть встроены в универсальные текстовые строки с использованием квадратных скобок (`[`,`]`):

```
Felix: Lorem ipsum[char Felix.Happy pos:0.75 wait:false] dolor sit amet, consectetur adipiscing elit.[i] Aenean tempus eleifend ante, ac molestie metus condimentum quis.[i][br 2] Morbi nunc magna, consequat posuere consectetur in, dapibus consectetur lorem. Duis consectetur semper augue nec pharetra.
```

Обратите внимание, что синтаксис встроенной команды остаётся точно таким же, за исключением того, что литерал `@` опущен, а тело команды заключено в квадратные скобки. Вы можете взять любую командную строку, встроить ее в общую текстовую строку, и она будет иметь точно такой же эффект, но в другой момент, в зависимости от позиции внутри текстового сообщения.

Под капотом универсальные текстовые строки разбираются на отдельные команды, идентифицируемые встроенным индексом; текст печатается с помощью команды [@print]. Например, следующая универсальная текстовая строка в скрипте Naninovel:

```
Lorem ipsum[char Felix.Happy pos:75 wait:false] dolor sit amet.
```

— в действительности обрабатывается движком как последовательность отдельных команд:

```
@print "Lorem ipsum" waitInput:false
@char Felix.Happy pos:75 wait:false
@print "dolor sit amet."
```

Чтобы напечатать квадратные скобки через универсальную текстовую строку, экранируйте их с помощью обратного слэша, например:
```
Some text \[ text inside brackets \]
```

— напечатает "Some text [ text inside brackets ]" в игре.

## Строки-метки

Метки используются в качестве "якорей" при навигации по сценариям Naninovel с помощью команд [@goto]. Чтобы объявить метку, используйте литерал `#` в начале строки, за которым следует имя метки:

```
# Epilogue
```

Теперь можно использовать команду [@goto], чтобы "прыгнуть" к этой строке:

```
@goto ScriptName.Epilogue
```

В случае, если вы используете команду [@goto] из того же сценария, в котором находится искомая метка, вы можете опустить имя сценария:

```
@goto .Epilogue
```

## Строки-комментарии

Когда строка начинается с литерала точки с запятой (`;`), она считается оператором комментария. Строки комментариев полностью игнорируются движком при выполнении сценариев. Вы можете использовать строки комментариев для добавления заметок или аннотаций для себя или других членов команды, работающих со сценариями Naninovel.

```
; Следующая команда выполнит автосохранение игры.
@save
```

## Условия исполнения

Хотя сценарий по умолчанию выполняется линейно, вы можете ввести ветвление, используя параметры `if`, поддерживаемые всеми командами.

```
; Если значение `level` – это номер, и он больше 9000, добавить выбор.
@choice "It's over 9000!" if:level>9000

; Ессли переменная `dead` – булева, и имеет значение `false`, выполнить команду вывода текста.
@print text:"I'm still alive." if:!dead

; Если переменная `glitch`– булева, и имеет значение `true`, или функция вывода случайного числа в диапазоне от 1 до 10 
; возвращает 5 или больше, выполнить команду `@spawn`.
@spawn GlitchCamera if:"glitch || Random(1, 10) >= 5"

; Если `score` имеет значение от 7 до 13, или переменная `lucky`– булева, 
; и возвращает `true`, загрузить скрипт `LuckyEnd`.
@goto LuckyEnd if:"(score >= 7 && score <= 13) || lucky"

; Вы также можете использовать условия во встроенных командах.
Lorem sit amet. [style bold if:score>=10]Consectetur elit.[style default]

; Если в самом выражении используются двойные кавычки, 
; не забывайте дважды экранировать их.
@print {remark} if:remark=="Saying \\"Stop the car\\" was a mistake."
```

Также возможно указывать многострочные условные блоки с помощью команд [@if], [@else], [@elseif] и [@endif].

```
@if score>10
	Good job, you've passed the test!
	@bgm Victory
	@spawn Fireworks
@elseif attempts>100
	You're hopeless... Need help?
	@choice "Yeah, please!" goto:.GetHelp
	@choice "I'll keep trying." goto:.BeginTest
	@stop
@else
	You've failed. Try again!
	@goto .BeginTest
@endif
```

Обратите внимание, что табуляции полностью опциональны и используются здесь только для лучшей читаемости.

То же самое работает и для универсальных текстовых строк:

```
Lorem ipsum dolor sit amet. [if score>10]Duis efficitur imperdiet nunc. [else]Vestibulum sit amet dolor non dolor placerat vehicula.[endif]
```

Для дополнительной информации о форматах условных выражений и доступных операторах см. гайд по [выражениям сценариев](/guide/script-expressions.md).

## Визуальный редактор

Вы можете использовать визуальный редактор сценариев для редактирования сценариев Naninovel. Выберите ресурс сценария, и вы увидите, что визуальный редактор автоматически откроется в окне инспектора.

[!ba57b9f78116e57408125325bdf66be9]

Чтобы добавить новую строку в сценарий, щелкните правой кнопкой мыши место, куда вы хотите вставить строку, или же нажмите `Ctrl+Space` (вы можете изменить привязки клавиш по умолчанию в меню конфигурации ввода) и выберите нужную строку или тип команды. Чтобы изменить порядок строк, перетащите их, используя их номерные метки. Чтобы удалить строку, щелкните ее правой кнопкой мыши и выберите пункт "Удалить".

To add a new line to the script, either right-click the place, where you want to insert the line, or press `Ctrl+Space` (you can change the default key bindings in the input configuration menu) and select the desired line or command type. To re-order lines, drag them using their number labels. To remove a line, right-click it and choose "Remove".

When you've changed the script using visual editor, you'll see an asterisk (`*`) over the script name in the inspector header. That means the asset is dirty and need to be saved; press `Ctrl+S` to save the asset. In case you'll attempt to select another asset while the script is dirty, a dialogue window will pop-up allowing to either save or revert the changes.

The visual editor will automatically sync edited script if you update it externally, so you can seamlessly work with the scripts in both text and visual editors. In case auto-sync is not working, make sure `Auto Refresh` is enabled in the `Edit -> Preferences -> General` Unity editor menu.

During the playmode, you can use visual editor to track which script line is currently being played and right-click a line to rewind the playback. This feature requires the script to have equal resource ID (when assigned in the resources manager menu) and asset name.

[!b6e04d664ce4b513296b378b7c25be03]

Currently played line will be highlighted with green color; when script playback is halted due waiting for user input, played line will be highlighted with yellow instead.

You can tweak the editor behavior and looks in the scripts configuration menu.

![](https://i.gyazo.com/4b4b2608e7662b02a61b00734910308c.png)

[!!9UmccF9R9xI]

## Script Graph

When working with large amount of scripts and non-linear stories, it could become handy to have some kind of visual representation of the story flow. This is where script graph tool comes in handy.

[!0dd3ec2393807fb03d501028e1526895]

To open the graph window use `Naninovel -> Script Graph` editor menu. You can dock the window as any other editor panel if you like to.

The tool will automatically build graph representation of all the naninovel scripts (represented as nodes) assigned via editor resources (`Naninovel -> Resources -> Scripts`) and connections between them.

The connections are generated based on [@goto] and [@gosub] commands. If the command has a conditional expression assigned (`if` parameter), corresponding port in the node will be highlighted with yellow and you'll be able to see the expression when hovering the port.

You can select script asset and open visual editor by double-clicking nodes or clicking ports. Clicking the ports will also scroll the visual editor to a line containing label (in case there were a label specified).

You can re-position the nodes as you like and their positions will be automatically saved when closing the graph window or exiting Unity; the positions will then be restored when re-open the window. You can also save manually by clicking "Save" button. Clicking "Auto Align" button will reset all the positions.

When changing scripts or adding new ones, click "Rebuild Graph" button to sync it.

## Hot Reload

It's possible to edit scripts at play mode (via both visual and external editors) and have the changes applied immediately, without game restart. The feature is controlled via `Hot Reload Scripts` property in the scripts configuration and is enabled by default.

When modifying, adding or removing a line before the currently played one, state rollback will automatically happen to the modified line to prevent state inconsistency.

In case hot reload is not working, make sure `Auto Refresh` is enabled and `Script Changes While Playing` is set to `Recompile And Continue Playing`. Both properties can be found at `Edit -> Preferences -> General` Unity editor menu.

![](https://i.gyazo.com/5d433783e1a12531c79fe6be80c92da7.png)

To manually initiate hot reload of the currently played naninovel script (eg, when editing script file outside of Unity project), use `reload` [console command](/guide/development-console.md). The command is editor-only (won't work in builds).

## IDE Support

IDE features, like syntax highlighting, error checking, auto-completion and interactive documentation could significantly increase productivity when writing scripts. We've made an extension for a free and open-source [Atom editor](https://atom.io) (available for Windows, MacOS and Linux), which provides the essential IDE support for NaniScript syntax.

![](https://i.gyazo.com/e3de33e372887b0466ea513576beadd0.png)

To use the extension:

1. Install [Atom editor](https://atom.io)
2. Install [language-naniscript](https://atom.io/packages/language-naniscript) extension
3. Install [atom-ide-ui](https://atom.io/packages/atom-ide-ui) extension (required for our extension to provide some of the features)
4. Restart the Atom editor
5. Open a folder with naninovel scripts (opening a single file won't activate the extension)

Check the following video tutorial on activating and using the extension.

[!!njKxsjtewzA]

Support for other editors is possible in the future; check [the issue on GitHub](https://github.com/Elringus/NaninovelWeb/issues/56#issuecomment-492987029) for more information.

## Scripts Debug

When working with large naninovel scripts, it could become tedious to always play them from start in order to check how things work in particular parts of the script. 

Using [development console](/guide/development-console.md) you can instantly "rewind" currently played script to an arbitrary line:

```
rewind 12
```

— will start playing current script from the 12th line; you can rewind forward and backward in the same way. To open the console while game is running, make sure the console is enabled in the engine configuration and press `~` key (can be changed in the configuration) or perform multi-touch (3 or more simultaneous touches) in case the build is running on a touchscreen device.

To find out which script and line is currently playing, use debug window: type `debug` in the development console and press `Enter` to show the window.

![Scripts Debug](https://i.gyazo.com/12772ecc7c14011bcde4a74c81e997b8.png)

Currently played script name, line number and command (inline) index are displayed in the title of the window. When [auto voicing](/guide/voicing.md#auto-voicing) feature is enabled, name of the corresponding voice clip will also be displayed. You can re-position the window by dragging it by the title. "Stop" button will halt script execution; when script player is stopped "Play" button will resume the execution. You can close the debug window by pressing the "Close" button.

Debug window is available in both editor and player builds.
