[![.github/workflows/build.yml](https://github.com/280Zo/charybdis-wireless-mini-zmk-firmware/actions/workflows/build.yml/badge.svg)](https://github.com/280Zo/charybdis-wireless-mini-zmk-firmware/actions/workflows/build.yml)

## Intro

This repository offers pre-configured ZMK firmware designed for Wireless Charybdis keyboards, supporting both the ubiquitous QWERTY layout and the optimized Colemak DH layout. You can choose from two configurations:

- Bluetooth and USB
- Dongle

Additionally, this repository automatically generates SVG images of all layers in the keymap, and adds it to the README. It also provides high level instructions and resources on how to customize and build the firmware to meet your specific needs.

Check out the [Charybdis Mini Wireless build guide](https://github.com/280Zo/charybdis-wireless-mini-3x6-build-guide?tab=readme-ov-file) if you haven't yet built your own Charybdis keyboard.

## Usage

If you'd like to use the pre-built firmware the files can be found in the [Actions Workflows](https://github.com/280Zo/charybdis-wireless-mini-zmk-firmware/actions?query=is%3Acompleted+branch%3Amain). To download them, log into Github, click the link, select the latest run that passed on the main branch, and download the applicable firmware. There are five firmware artifacts to choose from. If you're unsure which one to use, you probably want the firmware-charybdis-qwerty build.

- **firmware-charybdis-qwerty** - Bluetooth/USB with QWERTY layout
- **firmware-charybdis-qwerty-dongle** - Dongle with QWERTY layout
- **firmware-charybdis-colemak** - Bluetooth/USB with Colemak DH layout
- **firmware-charybdis-colemak-dongle** - Dongle with Colemak DH layout
- **firmware-reset-nanov2** - Reset the firmware completely

There are a few things to note about how the pre-built firmware is configured:

- ZMK has terms for each side of a split keyboard. Central is the half that sends keyboard outputs over USB or advertises to other devices over bluetooth. Peripheral is the half that will only send keystrokes to the central once they are paired and connected through bluetooth. The Bluetooth/USB firmware uses the right side as central.
- The dongle firmware will have much better battery life for the central side, but requires an extra MCU and can only be connected through the dongle.
- The Bluetooth/USB firmware can connect through Bluetooth, but the central side will have a shorter battery life because it needs to maintain that connection.
  - The central side can also be plugged in to USB and the keyboard can be used when Bluetooth on the host computer isn't available (e.g. BIOS navigation)
- To add support for the PMW3610 low power trackball sensor, badjeff's [zmk-pmw3610-driver](https://github.com/badjeff/zmk-pmw3610-driver), [ZMK Input Behavior Listener](https://github.com/badjeff/zmk-input-behavior-listener?tab=readme-ov-file), and [ZMK Split Peripheral Input Relay](https://github.com/badjeff/zmk-split-peripheral-input-relay) modules are included in the firmware.
- eigatech's [zmk-configs](https://github.com/eigatech/zmk-config?tab=readme-ov-file) played a major role in getting badjeff's drivers and modules fully configured and are a great resource
- A separate branch builds the Bluetooth/USB firmware using [inorichi's driver](https://github.com/inorichi/zmk-pmw3610-driver?tab=readme-ov-file) as an alternative to badjeff's driver.
- Pete Johanson (creator and lead of the ZMK firmware) developed a feature ([pointers-move-scroll](https://github.com/zmkfirmware/zmk/pull/2027)) that allows mouse keys to move and scroll. A successor feature ([pointers-with-input-processors](https://github.com/zmkfirmware/zmk/pull/2477)) was then developed that allows more flexibility. This feature is what will eventually be merged into the main ZMK branch, and it's what is used by this repo to build the firmware. Although it's not guranteed to be stable, it hasn't caused any noticible issues. That being said, if you'd prefer to use pointers-move-scroll which is in a stable state, you can update the west.yaml and adapt the config files accordingly.

## Flashing the Firmware

Follow the steps below to flash the firmware

- If you are flashing the firmware for the first time, or if you're switching between the dongle and the Bluetooth/USB configuration, flash the reset firmware to all the devices first
- Unzip the firmware.zip
- Plug the right half info the computer through USB
- Double press the reset button
- The keyboard will mount as a removable storage device
- Copy the applicable uf2 file into the NICENANO storage device (e.g. charybdis_qwerty_dongle.uf2 -> dongle)
- It will take a few seconds, then it will unmount and restart itself.
- Repeat these steps for all devices.
- You should now be able to use your keyboard

> [!NOTE]  
> If the keyboard halves aren't connecting as expected, try pressing the reset button on both halves at the same time. If that doesn't work, follow the [ZMK Connection Issues](https://zmk.dev/docs/troubleshooting/connection-issues#acquiring-a-reset-uf2) documentation for more troubleshooting steps.

## Keymaps & Layers

The base layer uses [home row mods](https://precondition.github.io/home-row-mods) to make typing as efficient and comfortable as possible. To reduce hand movement, extra attention has also been given to making sure cursor, scrolling, and mouse button operations are as seamless as possible.

Review the layer maps below to see how each one functions. Then either modify the keymap to fit your needs, or start using these defaults to become more familiar with them.

Here are a few tips for a quick start:

- The bluetooth keys on the EXTRAS layer allow you to select which bluetooth pairing you want, BT-CLR clears the pairing on the selected profile.

- The left most thumb button has multiple functions
  - When held, the function of the trackball is changed from moving the cursor to scrolling.
  - When double tapped, it will reduce the cursor speed for more precision, and activate the mouse layer.
  - When single tapped it outputs the escape key.

![keymap images](keymap-drawer/charybdis.svg)

## Подробное описание Layout'а

### 🎯 Философия дизайна

Данная конфигурация построена на современных принципах эргономичного набора текста с акцентом на минимизацию движений рук и максимальное удобство. Основой служит раскладка **Colemak-DH** с множественными улучшениями для программистов и активных пользователей компьютера.

### 🔧 Основные особенности

#### **Home Row Mods (Модификаторы на домашнем ряду)**
- **Левая рука**: `Cmd(C) Alt(I) Ctrl(E) Shift(A)`
- **Правая рука**: `Shift(H) Ctrl(T) Alt(S) Cmd(N)`
- **Кросс-хендовая активация**: модификаторы активируются только при нажатии клавиш противоположной руки
- **Адаптивные настройки**: `175ms` tapping-term с балансированным флейвором для максимального комфорта

#### **Умные комбинации (Combos)**
Быстрый доступ к часто используемым клавишам без слоев:

**Базовые действия:**
- `ESC`: позиции 13+14 (средний ряд, левая рука)
- `TAB`: позиции 25+26 (нижний ряд, левая рука) 
- `ENTER`: позиции 33+34 (нижний ряд, правая рука)

**Скобки (билатеральные):**
- `()`: 3+4 и 7+8 (верхний ряд)
- `[]`: 15+16 и 19+20 (средний ряд)
- `{}`: 27+28 и 31+32 (нижний ряд)

**Быстрые команды:**
- `Ctrl+C` (копировать): 26+27
- `Ctrl+V` (вставить): 27+28
- `Ctrl+X` (вырезать): 25+26
- `Ctrl+Z` (отменить): 24+25
- `Ctrl+A` (выделить всё): 2+3

#### **Адаптивные клавиши**
- **Левый Ctrl/Esc**: при удержании — Ctrl, при нажатии — Esc
- **Правый Shift/Enter**: при удержании — Shift, при нажатии — Enter
- **Билатеральный Shift**: автоматически активирует Sticky Shift при нажатии на домашнем ряду

### 📚 Описание слоёв

#### **BASE** (Colemak-DH) - Основной слой
```
TAB  B Y O U '     - L D W V Z
ESC  C I E A ,     . H T S N Q  
SHF  G X J K /     ; R M F P ENT
_    ALT SW CMD LANG    REP SYM NAV
         SPC CLK RCK
```

**Особенности:**
- Оптимизированная Colemak-DH раскладка
- Умная пунктуация: `,` → `;` при Shift, `.` → `:` при Shift
- Переключение языка на большом пальце
- Трекбол интегрирован в layout

#### **LOWER** - Цифры и F-клавиши
```
`    1 2 3 4 5     6 7 8 9 0 DEL
_    F1F2F3F4F5     F6 4 5 6 / *
_    DT TB TB DT =  0 1 2 3 . _
_    _ _ _ _         _ DEL _
       _ _ _
```

**Удобства:**
- Цифровая клавиатура на правой руке
- F-клавиши на левой руке для быстрого доступа
- Управление рабочими столами (DT = Desktop, TB = Tab)

#### **SYMB** - Символы и программирование
```
~    ! @ # $ %     ^ & [ ] % BS
`    | - + = #     => : ( ) ? '
_    ^ / * \ |>    ~ $ { } ! _
_    _ _ _ _         _ _ _
       SPC _ _
```

**Программерские фишки:**
- Стрелочная функция `=>` на одной клавише
- Pipe operator `|>` для функционального программирования
- Все скобки под правой рукой для быстрого доступа
- Математические операторы на левой руке

#### **NAV** - Навигация и медиа
```
SW   PR PP NX VU+ SS   PU WL ↑ WR MAX MIN
_    CMD ALT CTL SHF V-  PD ← ↓ → HOME END
_    SP AP CUT CP PST   UND HM END INS DEL _
_    _ _ _ _             _ _ _
       _ _ _
```

**Продвинутая навигация:**
- Автоповтор для стрелок при удержании
- Управление окнами (WL=влево, WR=вправо, MAX=максимизация)
- Медиа контроль и громкость
- App Switcher (CMD+TAB) и Swapper (ALT+TAB)

#### **MOUSE** - Управление трекболом
```
_    _ _ _ _ _       _ SL ↑ SR _ _
_    _ _ _ _ _       _ ← ↓ ↑ → _
_    _ _ _ _ _       _ _ ↓ _ _ _
_    _ _ _ TGL       RC LC TGL
       LC MC RC
```

**Трекбол возможности:**
- Точное управление курсором
- Скролл в 4 направлениях
- Переключение между режимами курсора и скролла

### 🚀 Макросы и автоматизация

#### **Управление окнами (macOS оптимизация)**
- `win_left/right`: разделение окон пополам
- `win_max`: максимизация окна
- `win_min`: минимизация окна
- `screenshot`: скриншот области (CMD+SHIFT+4)

#### **Навигация по системе**
- `lang_switch`: переключение языка (CMD+SPACE)
- `next/prev_desktop`: переключение рабочих столов
- `next/prev_tab`: навигация между вкладками
- `spotlight`: поиск Spotlight

#### **Программирование**
- `arrow_macro`: `=>` для стрелочных функций
- `pipe_op`: `|>` для pipe операторов
- `fat_arrow`: улучшенная стрелочная функция

### 💡 Продвинутые возможности

#### **Условные слои**
- При одновременной активации LOWER + SYMB автоматически активируется ADJUST
- NAV + FUNC также ведёт к слою ADJUST

#### **Sticky Keys и Oneshot**
- Sticky Shift: одно нажатие активирует Shift для следующей клавиши
- Oneshot layer: быстрый доступ к слоям без удержания
- Caps Word: умная активация заглавных букв для слов

#### **Swapper для Alt-Tab**
- Tri-state поведение для удобной навигации между приложениями
- Автоматическое освобождение модификатора

### 🎮 Практические советы использования

#### **Для программистов:**
1. Используйте home row mods для быстрого доступа к модификаторам
2. Комбо для скобок экономят время при написании кода
3. Символьный слой оптимизирован для популярных языков программирования

#### **Для продуктивной работы:**
1. Навигационный слой позволяет не отрывать руки от клавиатуры
2. Оконный менеджмент через макросы ускоряет workflow
3. Быстрые команды (копировать/вставить) через комбо

#### **Для удобства набора:**
1. Адаптивные клавиши уменьшают необходимость в дополнительных модификаторах
2. Caps Word автоматически отключается после слова
3. Sticky keys для одноразового использования модификаторов

### 🔄 Переключение между раскладками

В конфигурации доступны две основные раскладки:
- **BASE**: Colemak-DH (по умолчанию, оптимизированная)
- **QWERTY**: для совместимости и привычности

Переключение происходит через комбинацию или слой ADJUST.

### ⚡ Оптимизация производительности

- **Требование покоя**: 100ms перед активацией hold-tap поведений
- **Быстрые нажатия**: 125ms для quick-tap
- **Баланс**: flavor "balanced" для оптимального отклика
- **Cross-hand only**: home row mods активируются только противоположной рукой

Данная конфигурация создана для максимального комфорта и производительности, сочетая лучшие практики современного эргономичного набора с мощными возможностями ZMK.

## Modify Key Mappings

### ZMK Studio

[ZMK Studio](https://zmk.studio/) allows users to update functionality during runtime. It's currently in beta, but the physical layout and updated config files are included in the BT/USB firmware for testing. The dongle firmware does not have this integration at the moment.

To change the visual layout of the keys, the physical layout must be updated. This is the charybdis-layouts.dtsi file, which handles the actual physical positions of the keys. Though they may appear to be similar, this is different than the matrix transform file (charybdis.json) which handles the electrical matrix to keymap relationship.

To easily modify the physical layout, or convert a matrix transform file, [caksoylar](https://github.com/caksoylar/zmk-physical-layout-converter) has built the [ZMK physical layouts converter](https://zmk-physical-layout-converter.streamlit.app/).

For more details on how to use ZMK Studio, refer to the [ZMK documentation](https://zmk.dev/docs/features/studio).

### Keymap GUI

Using a GUI to generate the keymap file before building the firmware is another easy way to modify the key mappings. Head over to nickcoutsos' keymap editor and follow the steps below.

- Fork/Clone this repo
- Open a new tab to the [keymap editor](https://nickcoutsos.github.io/keymap-editor/)
- Give it permission to see your repo
- Select the branch you'd like to modify
- Update the keys to match what you'd like to use on your keyboard
- Save
- Wait for the pipeline to run
- Download and flash the new firmware

### Edit Keymap Directly

To change a key layout choose a behavior you'd like to assign to a key, then choose a parameter code. This process is more clearly outlined on ZMK's [Keymaps & Behaviors](https://zmk.dev/docs/features/keymaps) page.

- Behaviors are all documented on the [Behaviors Overview](https://zmk.dev/docs/behaviors)
- Codes are all documented on the [keycodes](https://zmk.dev/docs/codes) page

Open the keymap file and change keys, or add/remove layers, then merge the changes and re-flash the keyboard with the updated firmware.

## Changing the Central and Peripheral Assignments

Follow the ZMK documentation [Kconfig.deconfig](https://zmk.dev/docs/development/new-shield#kconfigdefconfig) to change which keyboard half is the central and which is the peripheral. This does not apply to the dongle configuration.

## Changing the Keyboard Name

Follow the ZMK [Kconfig.defconfig](https://zmk.dev/docs/development/new-shield#kconfigdefconfig) section to update the keyboard name. Make sure to read about the danger in exceeding the 16 character limit.

## Building Your Own Firmware

ZMK provides a comprehensive guide to follow when creating a [New Keyboard Shield](https://zmk.dev/docs/development/new-shield). I'll touch on some of the points here, but their docs should be what you reference when you're building your own firmware.

### File Structure

When building the ZMK firmware, the files need to be located in the correct place. The formats and locations of the files can be found on ZMK's [Configuration Overview](https://zmk.dev/docs/config).

### Mapping GPIO Pins to Keys

To set up some of the configuration files it requires a knowledge of which keys connect to which pins on the MCU (see the [Shield Overlays](https://zmk.dev/docs/development/new-shield#shield-overlays) section), and how the rows and columns are wired.

To get this information, look at the PCB kcad files and follow the traces from key pads, to row and column through holes, to MCU through holes. Once you have that information you can update the applicable dtsi/overlay files.

## Creating Graphical Key Maps

This repo uses the excellent work of caksoylar's [Keymap Drawer](https://keymap-drawer.streamlit.app/) to automatically generate a key mapping of each layer when the Github Actions are run.

## Upcoming ZMK Features

ZMK is actively being developed and there are a few features that will be added to these builds if/when they are approved.

- Layer Lock - [Open PR](https://github.com/zmkfirmware/zmk/pull/1984)
- Unicode Support - [Issue](https://github.com/zmkfirmware/zmk/issues/232)

## Credits

- [eigatech](https://github.com/eigatech)
- [badjeff](https://github.com/badjeff)
- [inorichi](https://github.com/inorichi)
- [manna-harbour](https://github.com/manna-harbour)
- [nickcoutsos](https://github.com/nickcoutsos/keymap-editor)
- [Petejohanson](https://github.com/petejohanson)
- [caksoylar](https://github.com/caksoylar/keymap-drawer)
