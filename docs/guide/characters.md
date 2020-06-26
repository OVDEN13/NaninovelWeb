# Персонажи 

Персонажи – это акторы, используемые для показа элементов сцены, размещённых поверх [фонов](/guide/backgrounds.md). 

Актор персонажа определяется именем, внешностью, видимостью, трансформацией (что включает в себя положение, поворот, масштаб) и направлением взгляда. Он может изменять внешность, видимость, трансформацию и направление взгляда с течением времени.

Поведение персонажей может быть настроено с помощью контекстного меню `Naninovel -> Configuration -> Characters`; доступные варианты см. в [руководстве по конфигурации](/guide/configuration.md#characters). Доступ к менеджеру ресурсов персонажей можно получить с помощью контекстного меню `Naninovel -> Resources -> Characters`.

![Добавление персонажа](https://i.gyazo.com/c8a4f7f987621831b4a2ecb3145a4a07.png)

В случае, если у вас есть много персонажей и/или внешностей для каждого персонажа, и вам неудобно назначать их все через меню редактора, вы можете просто поместить их в папку `Resources/Naninovel/Characters`, сгруппированными в папки, соответствующие ID их акторов. Например, чтобы добавить внешнность для актора персонажа с ID "Kohaku", сохраните текстуры (спрайты) в папке `Resources/Naninovel/Characters/Kohaku`, и они автоматически будут доступны в сценариях.

Вы можете дополнительно организовать ресурсы внешностей с помощью подпапок, если хотите; в этом случае используйте прямой слеш (`/`) при ссылке на них в скриптах Naninovel. Например, текстура внешности, сохранённая как `Resources/Naninovel/Characters/Kohaku/Casual/Angry` может быть объявлена в сценариях как `Casual/Angry`.

Также можно использовать [систему адрессируемых ассетов?](/guide/resource-providers.md#addressable), чтобы вручную распределить ресурсы. Чтобы предоставить доступ к ассету, назначьте адрес, равный пути, который вы использовали бы для его объявления с помощью метода, описанного выше, за исключением опущенной части "Resources/". Например, для объявления внешности "Happy" для персонажа "Kohaku" назначьте ассету следующий адрес: `Naninovel/Characters/Kohaku/Happy`. Имейте в виду, что адресируемый провайдер по умолчанию не используется в редакторе; вы можете разрешить его, включив свойство `Enable Addressable In Editor` в меню конфигурации провайдера ресурсов.

В сценариях Naninovel персонажи в основном контролируются с помощью команды [@char]:

```
; Вывадение персонажа с именем `Sora` с внешностью по умолчанию.
@char Sora

; То же, что и выше, но с применением внешности `Happy`.
@char Sora.Happy

; То же, что и выше, но также позиционируя персонажа на расстоянии 45% от левой границы
; экрана и на 10% от нижней границы; также заставляя его смотреть влево.
@char Sora.Happy look:left pos:45,10
```

## Позы

Каждый персонаж имеет свойство `Poses`, позволяющее задавать различные именованные состояния("позы").

![](https://i.gyazo.com/5b022d32eddb3e721ed036c96f662f5d.png)

Название позы может быть использовано в качестве внешности в команде [@char] для применения всех параметров, указанных в позе сразу, вместо того, чтобы указывать их по отдельности с помощью команд параметров.

```
; Данная поза `SuperAngry` определена для персонажа `Kohaku`,
; применяет все параметры, указанные в состоянии позы.
@char Kohaku.SuperAngry

; То же, что и выше, но с использованием перехода `DropFade` длительностью в 3 секунды.
@char Kohaku.SuperAngry transition:DropFade time:3
```

Обратите внимание, что когда поза используется в качестве внешности, вы все еще можете переопределить отдельные параметры, например:

```
; Данная поза `SuperAngry` определена для персонажа `Kohaku`,
; применяет все параметры, указанные в состоянии позы,
; за исключением оттенка, который переопределён отдельной командой.
@char Kohaku.SuperAngry tint:#ff45cb
```

## Отображаемые имена

В конфигурации персонажей вы можете установить `Display Name` для определенных персонажей. При установке отображаемое имя будет выводиться в метке имени в принтере вместо ID персонажа. Это позволяет использовать составные имена персонажей, содержащие пробелы и специальные символы (что не допускается для ID).

Для локализации используйте [управляемый текстовый?](/guide/managed-text) документ "CharacterNames", который автоматически создается при запуске задачи генерировать управляемые текстовые ресурсы. Значения из документа "CharacterNames" не будут переопределять значения, заданные в метаданных персонажей, пока они находятся в исходной локали.

Можно привязать отображаемое имя к пользовательской переменной, чтобы динамически изменять его на протяжении всей игры с помощью сценариев Naninovel. Чтобы связать отображаемое имя, укажите имя пользовательской переменной, заключенной в фигурные скобки, в меню конфигурации персонажей.

![](https://i.gyazo.com/9743061df462bd809afc45bff20bbb6d.png)

После этого вы можете изменять значение переменной в сценариях, и это будет также изменять отображаемое имя:

```
@set PlayerName="Mistery Man"
Player: ...

@set PlayerName="Dr. Stein"
Player: You can call me Dr. Stein. ?
```

Также можно использовать функцию привязки имени, чтобы позволить игроку выбрать свое отображаемое имя с помощью команды [@input]:

```
@input PlayerName summary:"Choose your name."
@stop
Player: You can call me {PlayerName}. ?
```

## Цвета сообщений

Если в конфигурации персонажей включена функция `Use Character Color`, текстовые сообщения принтера и метки имен будут окрашены в указанные цвета, если соответствующий ID персонажа указан в команде [@print] или общей текстовой строке.

В этом видео показано, как использовать отображаемые имена и цвета персонажей.

[!!u5B5s]

## Текстуры аватаров

Вы можете назначать текстуры аватаров персонажам, используя параметр `avatar` команды [@char]. Аватары будут отображаться совместимыми текстовыми принтерами, когда они выводят текстовое сообщение, связанное с этим персонажем. В настоящее время функцию аватаров поддерживают только текстовые принтеры `Wide` и `Chat`.

![](https://i.gyazo.com/83c091c08846fa1cab8764a8d4dddeda.png)

Чтобы использовать любой заданный аватар, вы должны сначала добавить его в ресурсы аватара и дать ему имя. Вы можете сделать это с помощью свойства `Avatar Resources` в меню конфигурации персонажей.

![](https://i.gyazo.com/5a0f10d174aa75ed87da1b472567e40b.png)

::: note
Имена аватаров могут быть произвольными и не обязаны содержать существующий ID персонажа или внешность. Это требуется только в том случае, если вы хотите связать аватар с персонажем, чтобы он отображался автоматически.
:::

Теперь вы можете показать определенную текстуру аватара следующим образом:

```
@char CharacaterId avatar:AvatarName
```

Чтобы установить аватар по умолчанию для персонажа, дайте имя ресурсу текстуры аватара, который равен `CharacterID/Default`; например, чтобы установить аватар по умолчанию для персонажа с ID `Kohaku`, дайте имя ресурсу аватара `Kohaku/Default`. Заданные аватары по умолчанию будут отображаться автоматически, даже если параметр `avatar` не указан в командах [@char].

Кроме того, можно связать аватары с определенными внешностями персонажа, так что когда персонаж будет менять внешность, аватар также автоматически будет меняться. Для этого назовите ресурсы аватара в следующем формате: `CharacterID/CharacterAppearance`, где `CharacterAppearance` – это имя внешности, которой сопоставляется ресурс аватара.

Обратите внимание, что **аватары не связаны напрямую с внешностью персонажа** и не должны рассматриваться как способ показать характер на сцене. Внешность, указанная в диспетчере ресурсов персонажа, является фактическим представлением персонажа на сцене. Аватары – это автономная функция, которая "вводит" произвольное изображение в совместимый текстовый принтер.

Можно показать только аватар персонажа внутри текстового принтера, но скрыть самого персонажа, установив параметр `visible` команды [@char] в значение `false`, например:
```
@char CharacaterId visible:false
```

Если вы постоянно меняете аватары, в то время как сам персонаж должен оставаться скрытым, вы можете отключить `Auto Show On Modify` в меню конфигурации персонажей; когда он отключен, вам не нужно будет указывать `visible:false`, чтобы изменить какие-либо параметры персонажа, пока он скрыт.

## Подсветка говорящего

Если этот параметр включен в конфигурации персонажей, он будет окрашивать персонажа в зависимости от того, связано ли с тем последнее напечатанное сообщение.

[!!gobowgagdyE]

## Синхронизация движений губ со звуком

[Универсальные](/guide/characters.md#generic-characters) и [Live2D](/guide/characters.md#live2d-characters) реализации персонажей поддерживают так называемую функцию "синхронизации движений губ со звуком", позволяя управлять анимацией рта персонажа, пока от его имени печатается сообщение, для чего отправляя соответствующие события.

[!!fx_YS2ZQGHI]

Когда включена функция [автоозвучивания?](/guide/voicing.md#auto-voicing), события синхронизации губ будут управляться голосом за кадром; в противном случае печатные текстовые сообщения активируют события. В последнем случае вам, вероятно, иногда захочется вручную запустить или остановить синхронизацию губ (например, чтобы предотвратить движение рта при печати знаков препинания); в таких случаях используйте команду [@lipSync].

См. документацию по реализации [универсальных](/guide/characters.md#generic-characters) и [Live2D-](/guide/characters.md#live2d-characters)персонажей ниже для получения подробной информации о том, как настроить функцию синхронизации губ.

## Привязанный принтер

Существует возможность привязать [текстоый принтер](/guide/text-printers.md) с персонажем, используя параметр `Linked Printer`.

![](https://i.gyazo.com/50ca6b39cd7f708158678339244b1dc4.png)

При подключении принтер будет автоматически использоваться для обработки сообщений, выводимых от имени этого персонажа.

Имейте в виду, что команды [@print] (которые также используются под капотом при печати общих текстовых строк) по умолчанию используют привязанные принтеры по умолчанию и скрывают другие видимые принтеры. Когда принтеры привязаны к персонажам, команды печати автоматически изменят текущий видимый и стандартный текстовый принтер, одновременно печатая текст, связанный с соответствующим персонажем. Такое поведение можно предотвратить, отключив свойство `Auto Default` в меню конфигурации принтера актора; если оно отключено, вам придется вручную показывать/скрывать и переключать принтеры по умолчанию с помощью команд [@printer].

## Sprite Characters 

Sprite implementation of the character actors is the most common and simple one; it uses a set of [sprite](https://docs.unity3d.com/Manual/Sprites) assets to represent appearances of the character. The source of the sprites could be images (textures) of any [formats supported by Unity](https://docs.unity3d.com/Manual/ImportingTextures).

## Diced Sprite Characters

Built with an open source [SpriteDicing](https://github.com/Elringus/SpriteDicing) package, `DicedSpriteCharacter` implementation allows to significantly reduce build size and texture memory by reusing texture areas of the character sprites. 

![Sprite Dicing](https://i.gyazo.com/af08d141e7a08b6a8e2ef60c07332bbf.png)

Install the package via [Unity package manager](https://docs.unity3d.com/Manual/upm-ui.html): open package manager window (Window -> Package Manager), click "+" button, choose "Add package from git URL", enter `https://github.com/Elringus/SpriteDicing.git#package` to the input field and click "Add".

[!b54e9daa9a483d9bf7f74f0e94b2d38a]

`DicedSpriteAtlas` assets containing character appearances are used as the resources for the diced sprite characters. Each appearance is mapped by name to the diced sprites contained in the atlas.

Be aware, that some of diced character metadata properties (eg, pixels per unit, pivot) are controlled by the atlas asset; while the values in the character configuration are applied to a render texture used to represent the actual sprite. When changing the atlas properties, don't forget to rebuild it for changes to take effect.

![](https://i.gyazo.com/3765726bd326bb7a8a03a653f458cd3d.png)

The following video guide covers creating and configuring diced sprite atlas, adding new diced character based on the created atlas and controlling the character from a naninovel script.

[!!6PdOAOsnhio]

## Layered Characters

The layered implementation allows composing characters from multiple sprites (layers) and then toggle them individually or in groups via naninovel scripts at runtime.

To create a layered character prefab, use `Create -> Naninovel -> Character -> Layered` asset context menu. Enter [prefab editing mode](https://docs.unity3d.com/Manual/EditingInPrefabMode.html) to compose the layers. Several layers and groups will be created by default. You can use them or delete and add your own.

Each child game object of the root prefab object with a [sprite renderer](https://docs.unity3d.com/Manual/class-SpriteRenderer.html) component is considered a *layer*; other objects considered *groups*. Aside from organization and transformation purposes, placing layers inside groups will allow you to select a single layer or disable/enable all the layers inside a group with a single expression in naninovel script (more on that later). 

To hide some of the layers from being visible by default, disable sprite renderer components (not the game objects).

The white frame drawn over the prefab is used to describe the actor canvas, which will be rendered to a render texture at runtime. Make sure to minimize the empty areas inside the frame by moving the layers and groups to prevent wasting texture memory and for anchoring to work correctly.

![](https://i.gyazo.com/4ff103c27858ac9671ba3b94ab1ade20.png)

You can scale the root game object to fine-tune the default size of the actor.

When authoring layered character art in Photoshop, consider using Unity's [PSD Importer package](https://docs.unity3d.com/Packages/com.unity.2d.psdimporter@3.0/manual/index.html) to automatically generate character prefab preserving all the layers and their positions. To preserve the layers hierarchy, make sure to enable `Use Layer Grouping` option in the import settings.

Don't forget to add the created layered prefab to the character resources (`Naninovel -> Resources -> Characters`). Choose "Naninovel.LayeredCharacter" implementation and drop prefab to the "Resource" field when configuring the resource record.

To control the layered characters in naninovel scripts, use [@char] command in the same way as with the other character implementations. The only difference is how you set the appearance: instead of a single ID, use the *layer composition expression*. There are three expression types:

 - Enable a single layer in group: `group>layer`
 - Enable a layer: `group+layer`
 - Disable a layer: `group-layer`

For example, consider a "Miho" character, which has a "Body" group with three layers: "Uniform", "SportSuit" and "Pajama". To enable "Uniform" layer and disable all the others, use the following command:

```
@char Miho.Body>Uniform
```

To enable or disable a layer without affecting any other layers in the group, use "+" and "-" respectively instead of ">". You can also specify multiple composition expressions splitting them with commas:

```
; Enable glasses, disable hat, select "Cool" emotion.
@char CharId.Head/Accessories+BlackGlasses,Head-Hat,Head/Emotions>Cool
```

To select a layer outside of any groups (a child of the root prefab object), just skip the group part, eg:

```
; Given "Halo" layer object is placed under the prefab root, disable it
@char CharId.-Halo
```

It's also possible to affect all the layers inside a group (and additionally its neighbors when using select expression) by omitting layer name in composition expression:

```
; Disable all the layers in "Body/Decoration" group
@char CharId.Body/Decoration-

; Enable all the existing layers.
@char CharId.+

; Given `Poses/Light` and `Poses/Dark` groups (each containing multiple layers), 
; enable all the sprites inside `Light` group and disable layers inside `Dark` group
@char CharId.Poses/Light>
```

The above expressions will affect not only the direct descendants of the target groups, but all the layers contained in the underlaying groups, recursively.

When an appearance is not specified (eg, `@char CharId` without previously setting any appearance), a default appearance will be used; default appearance of the layered characters equals to how the layered prefab looks in the editor.

The video below demonstrates how to setup a layered character and control it via naninovel commands.

[!!Bl3kXrg8tiI]

::: note
`@char Miho.Shoes>` command displayed in the video will actually select the "Shoes" group (disabling all the neighbor groups), not hide it. Correct command to hide a group is `@char Miho.Shoes-`.
:::

It's possible to map composition expressions to keys via `Composition Map` property of `Layered Actor Behaviour` component:

![](https://i.gyazo.com/ede5cde3548a3187aa714d3e140750ba.png)

— the keys can then be used to specify layered actor appearance:

```
; Corresponds to `Body>Uniform,Hair/Back>Straight,Hair/Front>Straight,Shoes>Grey`.
@char Miho.Uniform
; Corresponds to `Hair/Back>Straight,Hair/Front>Straight`.
@char Miho.StraightHair
```

While editing layered character prefab, it's possible to preview mapped composition expressions by right-clicking a map record and selecting "Preview Composition".

![](https://i.gyazo.com/9fb0aeccf4f33245d9f975592ee86dbc.gif)

Be aware, that the layer objects are not directly rendered by Unity cameras at runtime; instead, they're rendered once upon each composition (appearance) change to a temporary render texture, which is then fed to a custom mesh visible to the Naninovel camera. This setup is required to prevent semi-transparency overdraw issues and to support transition animation effects.

## Generic Characters

Generic character is the most flexible character actor implementation. It's based on a prefab with a `CharacterActorBehaviour` component attached to the root object. Appearance changes and all the other character parameters are routed as [Unity events](https://docs.unity3d.com/Manual/UnityEvents.html) allowing to implement the behavior of the underlying object in any way you wish.

![](https://i.gyazo.com/9f799f4152782afb6ab86d3c494f4cc4.png)

To create generic character prefab from a template, use `Create -> Naninovel -> Character -> Generic` context asset menu.

To setup lip sync feature for generic characters, use `On Started Speaking` and `On Finished Speaking` Unity events of `CharacterActorBehaviour` component. When the character becomes or ceases to be the author of any printed message (or rather when the message is fully revealed), the events will be invoked allowing you to trigger any custom logic, like starting or stopping mouth animation of the controlled character. This is similar to how UI's `On Show` and `On Hide` events work; find how they can be used to drive a custom animation in the [UI customization guide](/guide/user-interface.md#adding-custom-ui).

Check the following video tutorial for example on setting up a 3D rigged model as a generic character and routing appearance changes to the rig animations via [Animator](https://docs.unity3d.com/Manual/class-AnimatorController.html) component.

[!!HPxhR0I1u2Q]

Be aware, that Unity's `Animator` component could fail to register `SetTrigger` when the game object is enabled/disabled in the same frame; in case you use `GameObject.SetActive` to handle visibility changes (as it's shown in the above tutorial), consider enabling/disabling the child objects with renderers instead.

## Live2D Characters

Live2D character implementation uses assets created with [Live2D Cubism](https://www.live2d.com) 2D modeling and animation software. 

In order to be able to use this implementation you have to first install [Live2D Cubism SDK for Unity](https://live2d.github.io/#unity). Consult official Live2D docs for the installation and usage instructions.

Then download and import [Live2D extension package](https://github.com/Elringus/NaninovelLive2D/raw/master/NaninovelLive2D.unitypackage).

Live2D model prefab used as the resource for the implementation should have a `Live2DController` component attached to the root object. Appearance changes are routed to the animator component as [SetTrigger](https://docs.unity3d.com/ScriptReference/Animator.SetTrigger.html) commands appearance being the trigger name. Eg, if you have a "Kaori" Live2D character prefab and want to invoke a trigger with name "Surprise", use the following command:

```
@char Kaori.Surprise
```

Note, that the above command will only attempt to invoke a [SetTrigger](https://docs.unity3d.com/ScriptReference/Animator.SetTrigger.html) with "Surprise" argument on the animator controller attached to the prefab; you have to compose underlying [animator](https://docs.unity3d.com/Manual/Animator) state machine yourself.

::: warn
Latest version of Cubism SDK for Unity is working directly with `Animator` component; expressions and poses (exported as expression.json and pose.json), that were previously used in Cubism 2.x are now [deprecated](https://docs.live2d.com/cubism-sdk-tutorials/blendexpression) and not supported by Naninovel's extension for Live2D.
:::

When Live2D's `CubismLookController` and `CubismMouthController` components are present and setup on the Live2D model prefab, `Live2DController` can optionally use them to control look direction and mouth animation (aka lip sync feature) of the character.

![](https://i.gyazo.com/498fe948bc5cbdb4dfc5ebc5437ae6b4.png)

Consult Live2D documentation on [eye tracking](https://docs.live2d.com/cubism-sdk-tutorials/lookat) and [lip sync](https://docs.live2d.com/cubism-sdk-tutorials/lipsync) for the setup details.

Be aware, that `Live2DController` expects a "Drawables" gameobject inside the Live2D model prefab (created automatically when importing Live2D models to Unity); the controller will scale this gameobject at runtime in correspondence with "scale" parameter of the [@char] commands. Hence, any local scale values set in the editor will be ignored. To set an initial scale for the Live2D prefabs, please use scale of the parent gameobject as [shown in the video guide](https://youtu.be/rw_Z69z0pAg?t=353).

When Live2D extension is installed a "Live2D" item will appear in the Naninovel configuration menu providing following options:

![](https://i.gyazo.com/435a4824f0ce0dd8c9c3f29d457bab24.png)

Render layer specifies the layer to apply for the Live2D prefabs and culling mask to use for the cameras that will render the prefabs. Render camera field allows to use a custom setup for the render camera (the default render camera is stored inside the packages "Prefabs" folder). Camera offset allows to offset the render camera from the rendered prefab; you can use this parameters to uniformly position all the Live2D prefabs relative to the camera.

Following video guide covers exporting a Live2D character from Cubism Editor, configuring the prefab, creating a simple animator state machine and controlling the character from a naninovel script.

[!!rw_Z69z0pAg]

::: example
Check out an [example project on GitHub](https://github.com/Elringus/NaninovelLive2D), where a Live2D character is used with Naninovel. Be aware, that neither Naninovel, nor Live2D SDK packages are distributed with the project, hence compilation errors will be produced after opening it for the first time; import Naninovel from the Asset Store and Live2D Cubism SDK from their website to resolve the issues.
:::