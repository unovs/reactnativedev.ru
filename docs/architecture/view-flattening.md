# View Flattening

**View Flattening** - это оптимизация рендеринга React Native, позволяющая избежать глубоких деревьев компоновки.

API React разработан таким образом, чтобы быть декларативным и многократно используемым посредством композиции. Это обеспечивает отличную модель для интуитивной разработки. Однако при реализации эти качества API приводят к созданию глубоких [React Element Trees](architecture-glossary.md#react-element-tree-and-react-element), где значительное большинство узлов React Element Nodes влияют только на компоновку представления и ничего не отображают на экране. Мы называем эти типы узлов **"Layout-Only"** узлами.

Концептуально, каждый из узлов дерева элементов React Element Tree имеет связь 1:1 с видом на экране, поэтому рендеринг глубокого дерева элементов React Element Tree, состоящего из большого количества узлов "Layout-Only", приводит к низкой производительности при рендеринге.

Вот распространенный вариант использования, на который влияет стоимость представлений "Только макет".

Представьте, что вы хотите вывести изображение и заголовок, которые обрабатываются `TitleComponent`, и вы включаете этот компонент в качестве дочернего компонента `ContainerComponent`, который имеет некоторые стили полей. После декомпозиции компонентов код React будет выглядеть следующим образом:

```jsx
function MyComponent() {
  return (
    <View>                          // ReactAppComponent
      <View style={{margin: 10}} /> // ContainerComponent
        <View style={{margin: 10}}> // TitleComponent
          <Image {...} />
          <Text {...}>This is a title</Text>
        </View>
      </View>
    </View>
  );
}
```

В процессе рендеринга React Native будет создавать следующие деревья:

![Diagram one](diagram-one.png)

Обратите внимание, что представления (2) и (3) являются представлениями "Layout Only", потому что они отображаются на экране, но только с `margin` в `10 px` поверх своих дочерних элементов.

Чтобы улучшить производительность этих типов деревьев элементов React, рендерер реализует механизм View Flattening, который объединяет или сглаживает эти типы узлов, уменьшая глубину иерархии [host view](architecture-glossary.md#host-view-tree-and-host-view), которая отображается на экране. Этот алгоритм учитывает такие реквизиты, как: `margin`, `padding`, `backgroundColor`, `opacity` и т.д.

Алгоритм View Flattening интегрирован по дизайну как часть стадии диффиринга рендерера, что означает, что мы не используем дополнительные циклы процессора для оптимизации React Element Tree, сплющивающего эти типы представлений. Как и остальное ядро, алгоритм сглаживания представлений реализован на C++, и его преимущества используются по умолчанию на всех поддерживаемых платформах.

В случае предыдущего примера, виды (2) и (3) будут сплющены как часть "алгоритма диффинга", и в результате их стили будут объединены в вид (1):

![Диаграмма два](diagram-two.png)

Важно отметить, что эта оптимизация позволяет рендереру избежать создания и рендеринга двух основных представлений. С точки зрения пользователя на экране не происходит никаких видимых изменений.