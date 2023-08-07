# ScrollView

Компонент, оборачивающий платформу `ScrollView` и обеспечивающий интеграцию с системой сенсорной блокировки "responder".

Помните, что для работы `ScrollView` должны иметь ограниченную высоту, поскольку они содержат дочерние элементы неограниченной высоты в ограниченном контейнере (с помощью взаимодействия прокрутки). Чтобы привязать высоту `ScrollView`, либо задайте высоту представления напрямую (не рекомендуется), либо убедитесь, что все родительские представления имеют ограниченную высоту. Забыв передать `{flex: 1}` вниз по стеку представлений, вы можете столкнуться с ошибками, которые быстро отлаживаются с помощью инспектора элементов.

Пока не поддерживает блокировку другими содержащимися ответчиками этого вида прокрутки.

`<ScrollView>` vs [`<FlatList>`](flatlist.md) — какой из них использовать?

`ScrollView` отображает все свои дочерние компоненты react одновременно, но это имеет недостаток производительности.

Представьте, что у вас есть очень длинный список элементов, которые вы хотите отобразить, возможно, содержимое нескольких экранов. Создание JS-компонентов и нативных представлений для всего сразу, большая часть которого может даже не отображаться, приведет к медленному рендерингу и повышенному использованию памяти.

Именно здесь в игру вступает `FlatList`. `FlatList` лениво отображает элементы, когда они вот-вот появятся, и удаляет элементы, которые прокручиваются далеко за пределами экрана, чтобы сэкономить память и время обработки.

`FlatList` также удобен, если вы хотите отобразить разделители между элементами, несколько колонок, бесконечную прокрутку, или любые другие функции, которые он поддерживает из коробки.

## Пример

<div data-snack-id="@bndby/scrollview" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#F9F9F9;border:1px solid var(--color-border);border-radius:4px;height:505px;width:100%"></div>

## пропсы

### [View Props](view.md#props)

Наследует [View Props](view.md#props).

### `StickyHeaderComponent`

React-компонент, который будет использоваться для рендеринга липких заголовков, должен использоваться вместе с `stickyHeaderIndices`. Вам может понадобиться установить этот компонент, если ваш липкий заголовок использует пользовательские преобразования, например, когда вы хотите, чтобы ваш список имел анимированный и скрываемый заголовок. Если компонент не был указан, будет использован компонент по умолчанию [`ScrollViewStickyHeader`](https://github.com/facebook/react-native/blob/main/packages/react-native/Libraries/Components/ScrollView/ScrollViewStickyHeader.js).

| Type               |
| ------------------ |
| component, element |

### `alwaysBounceHorizontal` :simple-ios:

При значении true представление прокрутки отскакивает по горизонтали, когда достигает конца, даже если содержимое меньше, чем само представление прокрутки.

| Type | Default                                               |
| ---- | ----------------------------------------------------- |
| bool | `true` when `horizontal={true}`<hr/>`false` otherwise |

### `alwaysBounceVertical` :simple-ios:

При значении true представление прокрутки отскакивает по вертикали, когда достигает конца, даже если содержимое меньше, чем само представление прокрутки.

| Type | Default                                             |
| ---- | --------------------------------------------------- |
| bool | `false` when `vertical={true}`<hr/>`true` otherwise |

### `automaticallyAdjustContentInsets` :simple-ios:

Управляет тем, должна ли iOS автоматически регулировать вставку содержимого для прокручиваемых представлений, которые размещены за навигационной панелью или панелью вкладок/панелью инструментов.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `automaticallyAdjustKeyboardInsets` :simple-ios:

Управляет тем, должен ли `ScrollView` автоматически корректировать свои `contentInset` и `scrollViewInsets` при изменении размера клавиатуры.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `automaticallyAdjustsScrollIndicatorInsets` :simple-ios:

Управляет тем, должна ли iOS автоматически регулировать вставки индикатора прокрутки. См. [документацию Apple по этому свойству](https://developer.apple.com/documentation/uikit/uiscrollview/3198043-automaticallyadjustsscrollindica).

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `bounces` :simple-ios:

При значении true вид прокрутки отскакивает, когда достигает конца содержимого, если содержимое больше, чем вид прокрутки вдоль оси направления прокрутки. При значении `false` отключает все отскоки, даже если пропс `alwaysBounce*` имеет значение `true`.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `bouncesZoom` :simple-ios:

Если `true`, жесты могут управлять масштабированием, превышая min/max, и масштаб будет анимирован до min/max значения в конце жеста, в противном случае масштаб не будет превышать пределы.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `canCancelContentTouches` :simple-ios:

Если `false`, после начала отслеживания не будет предприниматься попытка перетаскивания, если прикосновение движется.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `centerContent` :simple-ios:

Если `true`, представление прокрутки автоматически центрирует содержимое, когда содержимое меньше границ представления прокрутки; когда содержимое больше границ представления прокрутки, это свойство не имеет эффекта.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `contentContainerStyle`

Эти стили будут применены к контейнеру содержимого представления прокрутки, в который обернуты все дочерние представления. Пример:

```js
return (
    <ScrollView
        contentContainerStyle={styles.contentContainer}
    ></ScrollView>
);
// ...
const styles = StyleSheet.create({
    contentContainer: {
        paddingVertical: 20,
    },
});
```

| Type                              |
| --------------------------------- |
| [View Style](view-style-props.md) |

### `contentInset` :simple-ios:

Величина, на которую содержимое представления прокрутки отступает от краев представления прокрутки.

| Type                                                               | Default                                  |
| ------------------------------------------------------------------ | ---------------------------------------- |
| object: {top: number, left: number, bottom: number, right: number} | `{top: 0, left: 0, bottom: 0, right: 0}` |

### `contentInsetAdjustmentBehavior` :simple-ios:

Это свойство определяет, как вставки безопасной области используются для изменения области содержимого представления прокрутки. Доступно в iOS 11 и более поздних версиях.

| Type                                                           | Default   |
| -------------------------------------------------------------- | --------- |
| enum(`'automatic'`, `'scrollableAxes'`, `'never'`, `'always'`) | `'never'` |

### `contentOffset`

Используется для ручной установки начального смещения прокрутки.

| Type  | Default        |
| ----- | -------------- |
| Point | `{x: 0, y: 0}` |

### `decelerationRate`

Число с плавающей точкой, определяющее, насколько быстро замедляется представление прокрутки после того, как пользователь поднимает палец. Вы также можете использовать строковые сокращения `"normal"` и `"fast"`, которые соответствуют базовым настройкам iOS для `UIScrollViewDecelerationRateNormal` и `UIScrollViewDecelerationRateFast` соответственно.

-   `normal` 0,998 на iOS, 0,985 на Android.
-   `fast`, 0,99 на iOS, 0,9 на Android.

| Type                               | Default    |
| ---------------------------------- | ---------- |
| enum(`'fast'`, `'normal'`), number | `'normal'` |

### `directionalLockEnabled` :simple-ios:

При значении true `ScrollView` будет пытаться зафиксировать только вертикальную или горизонтальную прокрутку при перетаскивании.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `disableIntervalMomentum`

При значении `true` представление прокрутки останавливается на следующем индексе (по отношению к позиции прокрутки при отпускании) независимо от скорости жеста. Это можно использовать для постраничной навигации, когда страница меньше ширины горизонтального `ScrollView` или высоты вертикального `ScrollView`.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `disableScrollViewPanResponder`

При значении `true`, стандартный JS-респондер панорамирования на `ScrollView` отключается, и полный контроль над касаниями внутри `ScrollView` остается за его дочерними компонентами. Это особенно полезно, если включен `snapToInterval`, поскольку он не следует типичным шаблонам касаний. Не используйте эту функцию в обычных `ScrollView` без `snapToInterval`, так как это может привести к неожиданным касаниям при прокрутке.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `endFillColor` :simple-android:

Иногда просмотр прокрутки занимает больше места, чем заполняет его содержимое. В таком случае этот пропс заполнит оставшуюся часть прокрутки цветом, чтобы избежать установки фона и создания ненужной перерисовки. Это расширенная оптимизация, которая не нужна в общем случае.

| Type               |
| ------------------ |
| [color](colors.md) |

### `fadingEdgeLength` :simple-android:

Обесцвечивает края содержимого свитка.

Если значение больше `0`, края затухания будут установлены в соответствии с текущим направлением и положением прокрутки, указывая, есть ли еще содержимое для показа.

| Type   | Default |
| ------ | ------- |
| number | `0`     |

### `horizontal`

Если `true`, дочерние элементы представления прокрутки располагаются горизонтально в ряд, а не вертикально в столбец.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `indicatorStyle` :simple-ios:

Стиль индикаторов прокрутки.

-   `default` то же самое, что и `black`.
-   `black`, индикатор прокрутки — `black`. Этот стиль хорош на светлом фоне.
-   `white`, индикатор прокрутки — `white`. Этот стиль хорош на темном фоне.

| Type                                    | Default     |
| --------------------------------------- | ----------- |
| enum(`'default'`, `'black'`, `'white'`) | `'default'` |

### `invertStickyHeaders`

Если липкие заголовки должны прилипать к нижней, а не к верхней части `ScrollView`. Это обычно используется с инвертированными `ScrollView`.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `keyboardDismissMode`

Определяет, будет ли клавиатура удалена в ответ на перетаскивание.

-   `none`, при перетаскивании клавиатура не отключается.
-   `on-drag`, клавиатура отключается, когда начинается перетаскивание.

**только для iOS**

-   `interactive`, клавиатура отбрасывается интерактивно при перетаскивании и перемещается синхронно с касанием, перетаскивание вверх отменяет отбрасывание. На Android это не поддерживается и будет иметь то же поведение, что и `'none'`.

| Type                                                                                                        | Default  |
| ----------------------------------------------------------------------------------------------------------- | -------- |
| enum(`'none'`, `'on-drag'`) :simple-android:<hr />enum(`'none'`, `'on-drag'`, `'interactive'`) :simple-ios: | `'none'` |

### `keyboardShouldPersistTaps`

Определяет, когда клавиатура должна оставаться видимой после нажатия.

-   `never` Нажатие за пределами сфокусированного ввода текста, когда клавиатура поднята, отменяет клавиатуру. Когда это происходит, дети не получают касания.
-   `always`, клавиатура не будет автоматически отключаться, и вид прокрутки не будет ловить касания, но дети вида прокрутки могут ловить касания.
-   `handled`, клавиатура не будет автоматически отключаться, если касание было обработано детьми вида прокрутки (или перехвачено предком).
-   `false`, **_deprecated_**, вместо этого используйте `'never'`.
-   `true`, **_удалено_**, используйте `'always'` вместо этого

| Type                                                      | Default   |
| --------------------------------------------------------- | --------- |
| enum(`'always'`, `'never'`, `'handled'`, `false`, `true`) | `'never'` |

### `maintainVisibleContentPosition`

Если установлено, представление прокрутки будет регулировать позицию прокрутки так, чтобы первый дочерний элемент, который виден в данный момент и находится на уровне или дальше `minIndexForVisible`, не менял позицию. Это полезно для списков, которые загружают содержимое в обоих направлениях, например, в потоке чата, где поступающие новые сообщения могут привести к скачку позиции прокрутки. Обычно используется значение `0`, но другие значения, например `1`, могут быть использованы для пропуска загрузки спиннеров или другого содержимого, которое не должно сохранять позицию.

Дополнительный параметр `autoscrollToTopThreshold` может использоваться для того, чтобы содержимое автоматически прокручивалось к вершине после выполнения настройки, если пользователь находился в пределах порога вершины до выполнения настройки. Это также полезно для приложений, подобных чату, где вы хотите, чтобы новые сообщения прокручивались на место, но не прокручивались, если пользователь прокрутил несколько сообщений вверх, и прокрутка будет мешать.

Предупреждение 1: Переупорядочивание элементов в прокрутке с включенной функцией, вероятно, приведет к скачкам и заеданиям. Это может быть исправлено, но в настоящее время это не планируется делать. Пока что не переупорядочивайте содержимое любых `ScrollView` или списков, использующих эту функцию.

Предупреждение 2: Для вычисления видимости используется `contentOffset` и `frame.origin` в собственном коде. Окклюзия, трансформации и другие сложности не будут приняты во внимание, чтобы определить, является ли содержимое "видимым" или нет.

| Type                                                                   |
| ---------------------------------------------------------------------- |
| object: {minIndexForVisible: number, autoscrollToTopThreshold: number} |

### `maximumZoomScale` :simple-ios:

Максимально допустимый масштаб масштабирования.

| Type   | Default |
| ------ | ------- |
| number | `1.0`   |

### `minimumZoomScale` :simple-ios:

Минимально допустимый масштаб масштабирования.

| Type   | Default |
| ------ | ------- |
| number | `1.0`   |

### `nestedScrollEnabled` :simple-android:

Включает вложенную прокрутку для Android API уровня 21+.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `onContentSizeChange`

Вызывается при изменении вида прокручиваемого содержимого `ScrollView`.

Функция-обработчик получает два параметра: ширину и высоту содержимого `(contentWidth, contentHeight)`.

Он реализуется с помощью обработчика `onLayout`, прикрепленного к контейнеру содержимого, который отображает этот `ScrollView`.

| Type     |
| -------- |
| function |

### `onMomentumScrollBegin`

Вызывается, когда начинается прокрутка импульса (прокрутка, которая происходит, когда `ScrollView` начинает скользить).

| Type     |
| -------- |
| function |

### `onMomentumScrollEnd`

Вызывается, когда заканчивается прокрутка импульса (прокрутка, которая происходит, когда `ScrollView` скользит до остановки).

| Type     |
| -------- |
| function |

### `onScroll`

Срабатывает не более одного раза за кадр во время прокрутки. Частотой событий можно управлять с помощью параметра `scrollEventThrottle`. Событие имеет следующую форму (все значения — числа):

```js
{
  nativeEvent: {
    contentInset: {bottom, left, right, top},
    contentOffset: {x, y},
    contentSize: {height, width},
    layoutMeasurement: {height, width},
    zoomScale
  }
}
```

| Type     |
| -------- |
| function |

### `onScrollBeginDrag`

Вызывается, когда пользователь начинает перетаскивать представление прокрутки.

| Type     |
| -------- |
| function |

### `onScrollEndDrag`

Вызывается, когда пользователь прекращает перетаскивать представление прокрутки, и оно либо останавливается, либо начинает скользить.

| Type     |
| -------- |
| function |

### `onScrollToTop` :simple-ios:

Срабатывает, когда вид прокрутки прокручивается к вершине после нажатия на строку состояния.

| Type     |
| -------- |
| function |

### `overScrollMode` :simple-android:

Используется для переопределения значения по умолчанию режима `overScroll`.

Возможные значения:

-   `auto` — Разрешить пользователю прокручивать этот вид только в том случае, если содержимое достаточно велико для значимой прокрутки.
-   `always` — Всегда разрешать пользователю прокручивать этот вид.
-   `never` — Никогда не разрешать пользователю прокручивать этот вид.

| Type                                  | Default  |
| ------------------------------------- | -------- |
| enum(`'auto'`, `'always'`, `'never'`) | `'auto'` |

### `pagingEnabled`

При значении true представление прокрутки останавливается на кратных размерах при прокрутке. Это можно использовать для горизонтальной пагинации.

!!!warning ""

    Примечание: Вертикальная пагинация не поддерживается на Android.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `persistentScrollbar` :simple-android:

Заставляет полосы прокрутки не становиться прозрачными, когда они не используются.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `pinchGestureEnabled` :simple-ios:

При значении true ScrollView позволяет использовать жесты щипка для увеличения или уменьшения масштаба.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `refreshControl`

Компонент `RefreshControl`, используемый для обеспечения функциональности pull-to-refresh для `ScrollView`. Работает только для вертикальных `ScrollView` (параметр `horizontal` должен быть `false`).

См. [RefreshControl](refreshcontrol.md).

| Type    |
| ------- |
| element |

### `removeClippedSubviews`

Экспериментально: При значении `true` дочерние представления вне экрана (чье значение `overflow` равно `hidden`) удаляются из их родного подложного супервидения, когда они находятся вне экрана. Это может улучшить производительность прокрутки длинных списков.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `scrollEnabled`

При значении false представление не может быть прокручено с помощью сенсорного взаимодействия.

Обратите внимание, что представление всегда можно прокрутить, вызвав `scrollTo`.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `scrollEventThrottle` :simple-ios:

Этот параметр определяет, как часто будет срабатывать событие прокрутки при прокрутке (в виде временного интервала в мс). Меньшее число обеспечивает лучшую точность для кода, отслеживающего положение прокрутки, но может привести к проблемам с производительностью прокрутки из-за объема информации, передаваемой по мосту. Вы не заметите разницы между значениями, установленными в диапазоне 1-16, поскольку цикл выполнения JS синхронизирован с частотой обновления экрана. Если вам не нужно точное отслеживание позиции прокрутки, установите это значение выше, чтобы ограничить объем информации, передаваемой через мост. По умолчанию установлено значение `0`, в результате чего событие прокрутки отправляется только один раз при каждой прокрутке представления.

| Type   | Default |
| ------ | ------- |
| number | `0`     |

### `scrollIndicatorInsets` :simple-ios:

Величина, на которую индикаторы представления прокрутки отступают от краев представления прокрутки. Обычно это значение должно быть таким же, как `contentInset`.

| Type                                                               | Default                                  |
| ------------------------------------------------------------------ | ---------------------------------------- |
| object: {top: number, left: number, bottom: number, right: number} | `{top: 0, left: 0, bottom: 0, right: 0}` |

### `scrollPerfTag` :simple-android:

Тег, используемый для регистрации производительности прокрутки в данном представлении прокрутки. Принудительно включает события импульса (см. `sendMomentumEvents`). Это не делает ничего из коробки, и вам нужно реализовать собственный встроенный `FpsListener`, чтобы он был полезен.

| Type   |
| ------ |
| string |

### `scrollToOverflowEnabled` :simple-ios:

Когда `true`, представление прокрутки может быть программно прокручено за пределы размера его содержимого.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `scrollsToTop` :simple-ios:

Если `true`, то при нажатии на строку состояния вид прокручивается к верху.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `showsHorizontalScrollIndicator`

Если `true`, показывает индикатор горизонтальной прокрутки.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `showsVerticalScrollIndicator`

Если `true`, показывает индикатор вертикальной прокрутки.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `snapToAlignment` :simple-ios:

Когда `snapToInterval` установлен, `snapToAlignment` определяет связь привязки с видом прокрутки.

Возможные значения:

-   `'start'` выравнивает привязку слева (по горизонтали) или сверху (по вертикали).
-   `'center'` выравнивает привязку по центру.
-   `'end'` выравнивает привязку справа (по горизонтали) или снизу (по вертикали).

| Type                                 | Default   |
| ------------------------------------ | --------- |
| enum(`'start'`, `'center'`, `'end'`) | `'start'` |

### `snapToEnd`

Используется в сочетании с `snapToOffsets`. По умолчанию конец списка считается смещением привязки. Установите `snapToEnd` в `false`, чтобы отключить это поведение и позволить списку свободно прокручиваться между его концом и последним смещением `snapToOffsets`.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `snapToInterval`

Если установлено, заставляет представление прокрутки останавливаться на значениях, кратных значению `snapToInterval`. Это можно использовать для постраничного просмотра дочерних элементов, длина которых меньше, чем длина представления прокрутки. Обычно используется в сочетании с `snapToAlignment` и `decelerationRate="fast"`. Переопределяет менее настраиваемый параметр `pagingEnabled`.

| Type   |
| ------ |
| number |

### `snapToOffsets`

При установке этого параметра вид прокрутки останавливается на заданном смещении. Это можно использовать для постраничного просмотра дочерних элементов разного размера, которые имеют длину меньше, чем вид прокрутки. Обычно используется в сочетании с `decelerationRate="fast"`. Переопределяет менее настраиваемые пропсы `pagingEnabled` и `napToInterval`.

| Type            |
| --------------- |
| array of number |

### `snapToStart`

Используется в сочетании с `snapToOffsets`. По умолчанию начало списка считается смещением привязки. Установите `snapToStart` в `false`, чтобы отключить это поведение и позволить списку свободно прокручиваться между его началом и первым смещением `snapToOffsets`.

| Type | Default |
| ---- | ------- |
| bool | `true`  |

### `stickyHeaderHiddenOnScroll`

Если установлено значение `true`, липкий заголовок будет скрыт при прокрутке списка вниз, а при прокрутке вверх он будет прикреплен к верхней части списка.

| Type | Default |
| ---- | ------- |
| bool | `false` |

### `stickyHeaderIndices`

Массив индексов дочерних элементов, определяющий, какие дочерние элементы будут прикреплены к верхней части экрана при прокрутке. Например, передача `stickyHeaderIndices={[0]}` приведет к тому, что первый дочерний элемент будет прикреплен к верхней части представления прокрутки. Вы также можете использовать `like [x,y,z]`, чтобы сделать несколько элементов липкими, когда они находятся сверху. Это свойство не поддерживается в сочетании с `horizontal={true}`.

| Type            |
| --------------- |
| array of number |

### `zoomScale` :simple-ios:

Текущий масштаб содержимого представления прокрутки.

| Type   | Default |
| ------ | ------- |
| number | `1.0`   |

## Методы

### `flashScrollIndicators()`

```ts
flashScrollIndicators();
```

Кратковременно отображает индикаторы прокрутки.

### `scrollTo()`

```ts
scrollTo(
  options?: {x?: number, y?: number, animated?: boolean} | number,
  deprecatedX?: number,
	deprecatedAnimated?: boolean,
);
```

Прокрутка до заданного смещения `x`, `y`, либо сразу, либо с плавной анимацией.

**Пример:**

```js
scrollTo({ x: 0, y: 0, animated: true });
```

!!!warning ""

    Примечание: Странная сигнатура функции связана с тем, что по историческим причинам функция также принимает отдельные аргументы в качестве альтернативы объекту `options`. Это устарело из-за двусмысленности (`y` перед `x`), и НЕ ДОЛЖНО ИСПОЛЬЗОВАТЬСЯ.

### `scrollToEnd()`

```ts
scrollToEnd(options?: {animated?: boolean});
```

Если это вертикальный `ScrollView`, то прокручивается вниз. Если это горизонтальный `ScrollView`, то прокручивается вправо.

Используйте `scrollToEnd({animated: true})` для плавной анимированной прокрутки, `scrollToEnd({animated: false})` для немедленной прокрутки. Если опции не переданы, `animated` по умолчанию принимает значение `true`.