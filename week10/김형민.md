# 제약 조건 이해하기 Understanding constraints

Flutter를 학습하는 사람이 "폭이 100인 위젯이 왜 100픽셀이 되지 않나요?"라고 물으면, 기본적으로 그 위젯을 Center 안에 넣으라고 대답하죠, 맞나요?

```
Center 안에 넣는 이유

위젯을 Center 안에 넣으라고 하는 이유는 Center가 자식 위젯을 가운데에 위치시키고, 

자식 위젯이 지정한 크기만큼의 공간을 사용할 수 있도록 하기 때문입니다.

Flutter에서 위젯의 크기는 부모 위젯의 제약 조건에 따라 결정됩니다.

만약 위젯이 특정한 크기를 갖도록 설정되었지만, 부모 위젯이 그것보다 작은 공간만 제공한다면, 

해당 위젯은 원하는 크기를 가지지 못할 수 있습니다. 이때 Center 위젯은 자식 위젯이 가능한 한 

지정된 크기를 유지할 수 있도록 부모로부터 전달받은 공간을 최대한 활용하면서 자식 위젯을 중앙에 배치합니다.

예를 들어, Center 위젯은 자식 위젯의 크기를 제한하지 않기 때문에, 자식 위젯이 width: 100으로 설정되어 있다면 

실제로 100픽셀의 너비를 가질 수 있게 됩니다. 이로 인해 위젯이 기대한 크기를 갖지 않는 문제를 해결할 수 있습니다.
```

그러지 마세요.

그렇게 하면 그들은 계속해서 돌아와 일부 FittedBox가 작동하지 않는 이유, 해당 Column이 오버플로되는 이유 또는 IntrinsicWidth가 수행해야 하는 작업이 무엇인지 묻습니다.

대신, 먼저 Flutter 레이아웃이 HTML 레이아웃과 매우 다르다는 점을 알려주고(아마도 HTML 레이아웃에서 나온 것일 수 있음) 다음 규칙을 기억하도록 하세요.

제약 조건이 내려갑니다. 사이즈가 올라갑니다. 부모는 위치를 설정합니다.

Flutter 레이아웃은 이 규칙을 모르면 실제로 이해할 수 없으므로 Flutter 개발자는 조기에 이를 배워야 합니다.

더 자세히:
  - 위젯은 부모로부터 자체 제약 조건을 받습니다. 제약 조건은 최소 및 최대 너비, 최소 및 최대 높이 등 4개의 double 집합입니다.
  - 그런 다음 위젯은 자신의 자식들 목록을 살펴봅니다. 위젯은 하나씩 자식에게 제약 조건이 무엇인지 알려주고(자식마다 다를 수 있음) 각 자식에게 원하는 크기를 묻습니다.
  - 그런 다음 위젯은 해당 자식(가로로 x축, 세로로 y축)을 하나씩 배치합니다.
  - 그리고 마지막으로 위젯은 자신의 부모에게 자신의 크기에 대해 알려줍니다(물론 원래 제약 조건 내에서).
  - 예를 들어 구성된 위젯에 패딩이 있는 열이 포함되어 있고 두 자식을 다음과 같이 배치하려는 경우:

협상은 다음과 같이 진행됩니다:

위젯: "안녕하세요 부모님, 제 제약사항은 무엇인가요?"

부모: "너비는 0~300픽셀, 높이는 0~85픽셀이어야 한다."

위젯: "흠, 나는 5픽셀의 패딩을 원하므로 내 아이들은 최대 너비가 290픽셀, 높이가 75픽셀을 가질 수 있겠군."

위젯: "안녕 첫째 아이야, 너는 너비가 0~290픽셀, 키가 0~75픽셀이어야 한다."

첫 번째 아이: "좋아요, 그럼 너비가 290픽셀, 높이가 20픽셀이 되었으면 좋겠어요."

위젯: "흠, 둘째 아이를 첫째 아이 아래에 배치하고 싶기 때문에 두 번째 아이의 높이는 55픽셀만 남는군."

위젯: "안녕 둘째 아이야, 너는 너비가 0에서 290 사이이고 키가 0에서 55 사이여야 해."

둘째 아이: "좋아요. 저는 가로가 140픽셀, 세로가 30픽셀이었으면 좋겠어요."

위젯: "아주 좋아. 첫 번째 아이의 위치는 x: 5 및 y: 5이고, 두 번째 아이의 위치는 x: 80 및 y: 25야."

위젯: "안녕하세요 부모님, 제 크기는 가로 300픽셀, 세로 60픽셀로 결정했습니다."

## 제한 Limitations

Flutter의 레이아웃 엔진은 원패스 프로세스로 설계되었습니다. 이는 Flutter가 위젯을 매우 효율적으로 배치하지만 몇 가지 제한 사항이 있음을 의미합니다.

- 위젯은 부모가 제공한 제약 조건 내에서만 자체 크기를 결정할 수 있습니다. 이는 일반적으로 위젯이 원하는 크기를 가질 수 없음을 의미합니다.

- 위젯의 위치를 ​​결정하는 것은 위젯의 부모이기 때문에 위젯은 화면에서 자신의 위치를 ​​알 수 없고 결정하지도 않습니다.

- 부모의 크기와 위치도 자신의 부모에 따라 달라지므로 트리 전체를 고려하지 않고 위젯의 크기와 위치를 정확하게 정의하는 것은 불가능합니다.

- 자식이 부모와 다른 크기를 원하고 부모를 정렬할 정보가 충분하지 않은 경우 자식의 크기가 무시될 수 있습니다. 정렬을 정의할 때는 구체적으로 설명하세요.

Flutter에서 위젯은 기본 RenderBox 객체에 의해 렌더링됩니다. Flutter의 많은 상자, 특히 자식 하나만 가져오는 상자는 제약 조건을 자식에게 전달합니다.

일반적으로 제약 조건을 처리하는 방법에 따라 세 가지 종류의 상자가 있습니다.

- 가능한 한 크게 되려고 시도하는 상자. 예를 들어 Center 및 ListView에서 사용되는 상자입니다.
- 자식과 같은 크기가 되려고 시도하는 상자. 예를 들어 Transform 및 Opacity에서 사용되는 상자입니다.
- 특정 크기가 되려고 시도하는 상자. 예를 들어 Image 및 Text에 사용되는 상자입니다.

Container와 같은 일부 위젯은 생성자 인자를 기반으로 하는 타입마다 다릅니다. Container 생성자는 기본적으로 가능한 한 크게 만들려고 시도하지만,  예를 들어 너비를 지정하면 이를 존중하고 특정 크기로 설정하려고 합니다.

예를 들어 Row 및 Column(flex 상자)과 같은 기타 위젯은 Flex 섹션에 설명된 대로 제공된 제약 조건에 따라 달라집니다.

## 예시 1

```dart
Container(color: red)
```

화면은 Container의 부모이며 Container가 화면과 정확히 같은 크기가 되도록 강제합니다.

따라서 Container는 화면을 채우고 빨간색으로 칠합니다.

## 예시 2

```dart
Container(width: 100, height: 100, color: red)
```

빨간색 Container는 100 × 100이 되기를 원하지만 화면이 화면과 정확히 같은 크기로 강제하기 때문에 그럴 수 없습니다.

따라서 Container가 화면을 채웁니다.

## 예시 3

```dart
Center(
  child: Container(width: 100, height: 100, color: red),
)
```

화면은 Center가 화면과 정확히 같은 크기가 되도록 강제하므로 Center가 화면을 채웁니다.

Center는 Container에게 원하는 크기로 지정할 수 있지만 화면보다 크지는 않음을 알려줍니다. 이제 Container는 실제로 100 × 100이 될 수 있습니다.

## 예시 4

```dart
Align(
  alignment: Alignment.bottomRight,
  child: Container(width: 100, height: 100, color: red),
)
```

Center 대신 Align을 사용한다는 점에서 이전 예제와 다릅니다.

Align은 또한 컨테이너에 원하는 크기를 지정할 수 있음을 알려주지만, 빈 공간이 있으면 컨테이너를 중앙에 배치하지 않습니다. 대신 컨테이너를 사용 가능한 공간의 오른쪽 하단에 정렬합니다.

## 예시 5

```dart
Center(
  child: Container(
      width: double.infinity, height: double.infinity, color: red),
)
```

화면은 Center가 화면과 정확히 같은 크기가 되도록 강제하므로 Center가 화면을 채웁니다.

Center는 Container에게 원하는 크기로 지정할 수 있지만 화면보다 클 수는 없음을 알려줍니다. Container는 무한한 크기를 원하지만 화면보다 클 수 없기 때문에 화면을 채울 뿐입니다.

## 예시 6

```dart
Center(
  child: Container(color: red),
)
```

화면은 Center가 화면과 정확히 같은 크기가 되도록 강제하므로 Center가 화면을 채웁니다.

Center는 Container에게 원하는 크기로 지정할 수 있지만 화면보다 클 수는 없음을 알려줍니다. Container에는 자식이 없고 크기가 고정되어 있지 않기 때문에 가능한 한 크게 크기를 결정하여 전체 화면을 채웁니다.

그런데 왜 Container가 그런 결정을 내리는 걸까요? 이는 Container 위젯을 만든 사람들의 디자인 결정이기 때문입니다. 다르게 생성되었을 수도 있으며 상황에 따라 어떻게 작동하는지 이해하려면 Container API 문서를 읽어야 합니다.

## 예시 7

```dart
Center(
  child: Container(
    color: red,
    child: Container(color: green, width: 30, height: 30),
  ),
)
```

화면은 Center가 화면과 정확히 같은 크기가 되도록 강제하므로 Center가 화면을 채웁니다.

Center는 빨간색 Container에게 원하는 크기로 지정할 수 있지만 화면보다 클 수는 없음을 알려줍니다. 빨간색 Container에는 크기가 없지만 자식이 있으므로 자식과 동일한 크기를 갖기로 결정합니다.

빨간색 Container는 자식에게 원하는 크기로 지정할 수 있지만 화면보다 크지는 않음을 알려줍니다.

자식은 30 × 30이 되기를 원하는 녹색 Container입니다. 빨간색 Container의 크기가 자식의 크기에 맞춰진다는 점을 고려하면 역시 30 × 30입니다. 녹색 Container가 빨간색을 완전히 덮기 때문에 빨간색 Container가 보이지 않습니다.

## 예시 8

```dart
Center(
  child: Container(
    padding: const EdgeInsets.all(20),
    color: red,
    child: Container(color: green, width: 30, height: 30),
  ),
)
```

빨간색 Container는 자식 크기에 맞춰 크기가 조정되지만 자체 패딩도 고려됩니다. 따라서 30 × 30에 패딩을 더한 것이기도 합니다. 패딩으로 인해 빨간색이 보이고, 녹색 Container는 이전 예시와 크기가 동일합니다.


## 예시 9

```dart
ConstrainedBox(
  constraints: const BoxConstraints(
    minWidth: 70,
    minHeight: 70,
    maxWidth: 150,
    maxHeight: 150,
  ),
  child: Container(color: red, width: 10, height: 10),
)
```

Container가 70~150픽셀 사이여야 한다고 추측할 수 있지만 이는 틀렸습니다. ConstrainedBox는 부모로부터 받은 제약 조건에만 추가 제약 조건을 적용합니다.

여기서 화면은 ConstrainedBox가 화면과 정확히 같은 크기가 되도록 강제하므로 자식 Container에도 화면 크기를 가정하도록 지시하여 ConstrainedBox 매개변수를 무시합니다.

## 예시 10

```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 10, height: 10),
  ),
)
```

이제 Center에서는 ConstrainedBox를 화면 크기까지 원하는 크기로 설정할 수 있습니다. ConstrainedBox는 Constraints 매개변수의 추가 제약 조건을 자식에 적용합니다.

Container는 70~150픽셀 사이여야 합니다. 10개의 픽셀을 원하므로 결국 70(최소값)이 됩니다.

## 예시 11

```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 1000, height: 1000),
  ),
)
```

Center를 사용하면 ConstrainedBox를 화면 크기까지 원하는 크기로 설정할 수 있습니다. ConstrainedBox는 Constraints 매개변수의 추가 제약 조건을 자식에 적용합니다.

Container는 70~150픽셀 사이여야 합니다. 1000픽셀을 원하므로 결국 150(최대값)이 됩니다.

## 예시 12

```dart
Center(
  child: ConstrainedBox(
    constraints: const BoxConstraints(
      minWidth: 70,
      minHeight: 70,
      maxWidth: 150,
      maxHeight: 150,
    ),
    child: Container(color: red, width: 100, height: 100),
  ),
)
```

Center를 사용하면 ConstrainedBox를 화면 크기까지 원하는 크기로 설정할 수 있습니다. ConstrainedBox는 Constraints 매개변수의 추가 제약 조건을 자식에 적용합니다.

Container는 70~150픽셀 사이여야 합니다. Container는 100픽셀을 원하고, 그 크기는 70에서 150사이이기 때문에 그 크기를 갖습니다.

## 예시 13

```dart
UnconstrainedBox(
  child: Container(color: red, width: 20, height: 50),
)
```

화면은 UnconstrainedBox의 크기를 화면과 정확히 동일하게 만듭니다. 그러나 UnconstrainedBox는 자식 컨테이너를 원하는 크기로 설정할 수 있습니다.

## 예시 14

```dart
UnconstrainedBox(
  child: Container(color: red, width: 4000, height: 50),
)
```

화면은 UnconstrainedBox가 화면과 정확히 같은 크기가 되도록 강제하고 UnconstrainedBox는 해당 자식 컨테이너를 원하는 크기로 만듭니다.

불행하게도 이 경우 Container의 너비는 4000픽셀이고 너무 커서 UnconstrainedBox에 맞지 않으므로 UnconstrainedBox는 매우 무서운 "오버플로 경고"를 표시합니다.

## 예시 15

```dart
OverflowBox(
  minWidth: 0,
  minHeight: 0,
  maxWidth: double.infinity,
  maxHeight: double.infinity,
  child: Container(color: red, width: 4000, height: 50),
)
```

화면은 OverflowBox를 화면과 정확히 같은 크기로 강제하고 OverflowBox는 자식 컨테이너를 원하는 크기로 만듭니다.

OverflowBox는 UnconstrainedBox와 유사합니다. 차이점은 아이가 공간에 맞지 않으면 경고가 표시되지 않는다는 것입니다.

이 경우 Container의 너비는 4000픽셀이고 너무 커서 OverflowBox에 맞지 않지만 OverflowBox는 경고 없이 가능한 만큼만 표시합니다.

## 예시 16

```dart
UnconstrainedBox(
  child: Container(color: Colors.red, width: double.infinity, height: 100),
)
```

이렇게 하면 아무것도 렌더링되지 않으며 콘솔에 오류가 표시됩니다.

UnconstrainedBox는 자식을 원하는 크기로 만들 수 있지만 자식은 크기가 무한한 컨테이너입니다.

Flutter는 무한한 크기를 렌더링할 수 없으므로 다음 메시지와 함께 오류가 발생합니다. BoxConstraints는 무한한 너비를 강제합니다.

## 예시 17

```dart
UnconstrainedBox(
  child: LimitedBox(
    maxWidth: 100,
    child: Container(
      color: Colors.red,
      width: double.infinity,
      height: 100,
    ),
  ),
)
```

여기서는 더 이상 오류가 발생하지 않습니다. 왜냐하면 UnconstrainedBox에 의해 LimitedBox에 무한한 크기가 주어질 때 최대 너비 100을 자식에게 전달하기 때문입니다.

UnconstrainedBox를 Center 위젯으로 교체하면 LimitedBox는 더 이상 제한을 적용하지 않으며(제한은 무한 제약 조건을 받을 때만 적용되므로) 컨테이너의 너비는 100을 초과할 수 있습니다.

이는 LimitedBox와 ConstrainedBox의 차이점을 설명합니다.

## 예시 18

```dart
const FittedBox(
  child: Text('Some Example Text.'),
)
```

화면은 FittedBox의 크기를 화면과 정확히 동일하게 만듭니다. Text에는 텍스트 양, 글꼴 크기 등에 따라 달라지는 자연스러운 너비(고유 너비라고도 함)가 있습니다.

FittedBox를 사용하면 Text의 크기를 원하는 대로 지정할 수 있지만 Text가 FittedBox에 크기를 알려준 후 FittedBox는 사용 가능한 너비를 모두 채울 때까지 Text의 크기를 조정합니다.

## 예시 19

```dart
const Center(
  child: FittedBox(
    child: Text('Some Example Text.'),
  ),
)
```

하지만 FittedBox를 Center 위젯 안에 넣으면 어떻게 될까요? Center에서는 FittedBox를 화면 크기까지 원하는 크기로 설정할 수 있습니다.

그런 다음 FittedBox는 Text에 맞게 크기를 조정하고 Text를 원하는 크기로 만듭니다. FittedBox와 Text의 크기가 동일하므로 크기 조정이 발생하지 않습니다.

## 예시 20

```dart
const Center(
  child: FittedBox(
    child: Text(
        'This is some very very very large text that is too big to fit a regular screen in a single line.'),
  ),
)
```

그러나 FittedBox가 Center 위젯 내부에 있지만 Text가 너무 커서 화면에 맞지 않으면 어떻게 될까요?

FittedBox는 Text에 맞게 크기를 조정하려고 시도하지만 화면보다 클 수는 없습니다. 그런 다음 화면 크기를 가정하고 Text 크기도 화면에 맞도록 조정합니다.

## 예시 21

```dart
const Center(
  child: Text(
      'This is some very very very large text that is too big to fit a regular screen in a single line.'),
)
```

그러나 FittedBox를 제거하면 Text는 화면에서 최대 너비를 얻고 화면에 맞도록 줄을 바꿉니다.

## 예시 22

```dart
FittedBox(
  child: Container(
    height: 20,
    width: double.infinity,
    color: Colors.red,
  ),
)
```

FittedBox는 제한된(폭과 높이가 무한하지 않은) 위젯의 크기만 조정할 수 있습니다. 그렇지 않으면 아무것도 렌더링되지 않으며 콘솔에 오류가 표시됩니다.

## 예시 23

```dart
Row(
  children: [
    Container(color: red, child: const Text('Hello!', style: big)),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

화면은 Row의 크기를 화면과 정확히 동일하게 만듭니다.

UnconstrainedBox와 마찬가지로 Row는 자식에 어떠한 제약도 적용하지 않고 대신 원하는 크기로 설정할 수 있습니다. 그런 다음 Row는 이를 나란히 배치하고 나머지 공간은 비어 있습니다.

## 예시 24

```dart
Row(
  children: [
    Container(
      color: red,
      child: const Text(
        'This is a very long text that '
        'won\'t fit the line.',
        style: big,
      ),
    ),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

Row는 자식에게 어떠한 제약도 부과하지 않기 때문에 자식이 너무 커서 Row의 사용 가능한 너비에 맞지 않을 가능성이 높습니다. 이 경우 UnconstrainedBox와 마찬가지로 Row에 "오버플로 경고"가 표시됩니다.

## 예시 25

```dart
Row(
  children: [
    Expanded(
      child: Center(
        child: Container(
          color: red,
          child: const Text(
            'This is a very long text that won\'t fit the line.',
            style: big,
          ),
        ),
      ),
    ),
    Container(color: green, child: const Text('Goodbye!', style: big)),
  ],
)
```

Row의 자식이 Expanded 위젯에 래핑되면 Row는 이 자식이 더 이상 자신의 너비를 정의하도록 허용하지 않습니다.

대신, 다른 자식에 따라 Expanded의 너비를 정의한 다음 Expanded 위젯만 원래 자식이 Expanded의 너비를 갖도록 강제합니다.

즉, Expanded를 사용하면 원래 자식의 너비는 관련이 없게 되어 무시됩니다.

## 예시 26

```dart
Row(
  children: [
    Expanded(
      child: Container(
        color: red,
        child: const Text(
          'This is a very long text that won\'t fit the line.',
          style: big,
        ),
      ),
    ),
    Expanded(
      child: Container(
        color: green,
        child: const Text(
          'Goodbye!',
          style: big,
        ),
      ),
    ),
  ],
)
```

Row의 모든 자식이 Expanded 위젯에 포함된 경우 각 Expanded의 크기는 flex 매개변수에 비례하며, 그런 다음에만 각 Expanded 위젯은 해당 자식이 Expanded의 너비를 갖도록 강제합니다.

즉, Expanded는 자식이 선호하는 너비를 무시합니다.

## 예시 27

```dart
Row(
  children: [
    Flexible(
      child: Container(
        color: red,
        child: const Text(
          'This is a very long text that won\'t fit the line.',
          style: big,
        ),
      ),
    ),
    Flexible(
      child: Container(
        color: green,
        child: const Text(
          'Goodbye!',
          style: big,
        ),
      ),
    ),
  ],
)
```

Expanded 대신 Flexible을 사용하는 경우 유일한 차이점은 Flexible의 자식이 Flexible 자체와 같거나 작은 너비를 갖도록 하는 반면 Expanded는 자식이 Expanded의 너비와 정확히 동일하도록 강제한다는 것입니다. 그러나 Expanded와 Flexible 모두 크기를 조정할 때 자식의 너비를 무시합니다.

```
노트

이는 Row 자식을 크기에 비례하여 확장하는 것이 불가능하다는 것을 의미합니다. Row는 정확한 자식 너비를 사용하거나 Expanded 또는 Flexible을 사용할 때 이를 완전히 무시합니다.
```

## 예시 28

```dart
Scaffold(
  body: Container(
    color: blue,
    child: const Column(
      children: [
        Text('Hello!'),
        Text('Goodbye!'),
      ],
    ),
  ),
)
```

화면은 Scaffold가 화면과 정확히 같은 크기가 되도록 강제하므로 Scaffold가 화면을 채웁니다. Scaffold는 컨테이너가 자신이 원하는 크기로 지정하도록 할 수 있지만 화면보다 크지는 않음을 알려줍니다.

```
노트

위젯이 자식에게 특정 크기보다 작을 수 있다고 말하면 위젯이 자식에게 느슨한 제약 조건을 제공한다고 말합니다. 이에 대해서는 나중에 자세히 설명하겠습니다.
```

## 예시 29

```dart
Scaffold(
  body: SizedBox.expand(
    child: Container(
      color: blue,
      child: const Column(
        children: [
          Text('Hello!'),
          Text('Goodbye!'),
        ],
      ),
    ),
  ),
)
```

Scaffold의 자식이 Scaffold 자체와 정확히 동일한 크기가 되도록 하려면 해당 자식을 SizedBox.expand로 래핑할 수 있습니다.

## 엄격한 제약 vs 느슨한 제약 Tight vs loose constraints

어떤 제약 조건이 "엄격하다" 또는 "느슨하다"는 말을 듣는 것은 매우 흔한 일입니다. 그렇다면 이는 무엇을 의미할까요?

## 엄격한 제약 Tight constraints

엄격한 제약 조건은 단일 가능성, 즉 정확한 크기를 제공합니다. 즉, 엄격한 제약 조건의 최대 너비는 최소 너비와 동일하고 최대 높이는 최소 높이와 동일합니다.

이에 대한 예는 RenderView 클래스에 포함된 App 위젯입니다. 애플리케이션의 빌드 함수에서 반환된 자식이 사용하는 상자에는 애플리케이션의 콘텐츠 영역(일반적으로 전체 화면)을 정확히 채우도록 강제하는 제약 조건이 제공됩니다.

또 다른 예: 애플리케이션 렌더 트리의 루트에 여러 개의 상자를 서로 중첩하면 상자의 엄격한 제약으로 인해 모두 서로 정확히 맞을 것입니다.

Flutter의 box.dart 파일로 이동하여 BoxConstraints 생성자를 검색하면 다음을 찾을 수 있습니다.

```dart
BoxConstraints.tight(Size size)
   : minWidth = size.width,
     maxWidth = size.width,
     minHeight = size.height,
     maxHeight = size.height;
```

예시 2를 다시 보면 화면은 빨간색 Container를 화면과 정확히 같은 크기로 만듭니다. 물론 화면은 Container에 엄격한 제약 조건을 전달하여 이를 달성합니다.

## 느슨한 제약

느슨한 제약조건은 최소값이 0이고 최대값이 0이 아닌 제약조건입니다.

일부 상자는 들어오는 제약 조건을 느슨하게 합니다. 즉, 최대값은 유지되지만 최소값은 제거되므로 위젯의 최소 너비와 높이가 모두 0이 될 수 있습니다.

궁극적으로 Center의 목적은 부모(화면)로부터 받은 엄격한 제약을 자식(컨테이너)에 대한 느슨한 제약으로 변환하는 것입니다.

예시 3을 다시 보면 Center에서는 빨간색 Container가 화면보다 더 작을 수는 있지만 더 크지는 않도록 허용합니다.

## 한정되지 않은 제약 Unbounded contraints

```
노트

프레임워크가 상자 제약 조건과 관련된 문제를 감지하면 여기로 이동될 수 있습니다. 아래의 Flex 섹션도 적용될 수 있습니다.
```

특정 상황에서는 상자의 제약 조건이 한정되지 않거나 무한합니다. 이는 최대 너비 또는 최대 높이가 double.infinity로 설정됨을 의미합니다.

가능한 한 크게 만들려고 하는 상자는 한정되지 않은 제약 조건이 주어지면 유용하게 작동하지 않으며 디버그 모드에서는 예외가 발생합니다.

렌더링 상자가 한정되지 않은 제약 조건으로 끝나는 가장 일반적인 경우는 가변 상자(Row 또는 Column) 내부와 스크롤 가능 영역(예: ListView 및 기타 ScrollView 하위 클래스) 내부입니다.

예를 들어 ListView는 교차 방향에서 사용 가능한 공간에 맞게 확장하려고 합니다(아마도 수직 스크롤 블록이고 부모만큼 넓으려고 시도할 수 있음). 가로로 스크롤되는 ListView 내에 세로로 스크롤되는 ListView를 중첩하는 경우 내부 목록은 가능한 한 넓어지려고 합니다. 외부 목록은 해당 방향으로 스크롤 가능하므로 무한히 넓습니다.

다음 섹션에서는 Flex 위젯의 한정되지 않은 제약 조건으로 인해 발생할 수 있는 오류에 대해 설명합니다.

## Flex

Flex 상자(Row 및 Column)는 제약 조건이 기본 방향으로 제한되어 있는지 또는 제한되어 있지 않은지에 따라 다르게 동작합니다.

기본 방향에 제한된 제약 조건이 있는 Flex 상자는 가능한 한 크게 만들려고 합니다.

기본 방향에 한정되지 않은 제약 조건이 있는 플렉스 박스는 해당 공간에 자식을 맞추려고 합니다. 각 자식의 flex 값은 0으로 설정되어야 합니다. 즉, flex 상자가 다른 flex 상자나 스크롤 가능 항목 안에 있을 때 Expanded를 사용할 수 없습니다. 그렇지 않으면 예외가 발생합니다.

교차 방향(Column의 너비 또는 Row의 높이)은 제한이 없어야 하며 그렇지 않으면 자식을 합리적으로 정렬할 수 없습니다.

## 특정 위젯의 레이아웃 규칙 학습 Learning the layout rules for specific widgets

일반적인 레이아웃 규칙을 아는 것이 필요하지만 그것만으로는 충분하지 않습니다.

각 위젯에는 일반 규칙을 적용할 때 많은 자유가 있으므로 위젯 이름만 읽는 것만으로는 어떻게 작동하는지 알 수 없습니다.

추측하려고 하면 아마도 틀린 추측을 하게 될 것입니다. 문서를 읽거나 소스 코드를 연구하지 않으면 위젯이 어떻게 작동하는지 정확히 알 수 없습니다.

레이아웃 소스 코드는 일반적으로 복잡하므로 설명서를 읽는 것이 더 나을 것입니다. 그러나 레이아웃 소스 코드를 연구하기로 결정한 경우 IDE의 탐색 기능을 사용하여 쉽게 찾을 수 있습니다.

예시가 있습니다:

  코드에서 Column을 찾아 해당 소스 코드로 이동합니다. 이렇게 하려면 Android Studio 또는 IntelliJ에서 command+B(macOS) 또는 Ctrl+B(Windows/Linux)를 사용하세요. basic.dart 파일로 이동됩니다. Column은 Flex를 확장하므로 Flex 소스 코드(basic.dart에도 있음)로 이동하세요.

  createRenderObject()라는 메서드를 찾을 때까지 아래로 스크롤합니다. 보시다시피 이 메서드는 RenderFlex를 반환합니다. 이것은 Column의 렌더링 객체입니다. 이제 flex.dart 파일로 이동하는 RenderFlex 소스 코드로 이동하세요.

  PerformLayout()이라는 메서드를 찾을 때까지 아래로 스크롤합니다. 이것은 Column의 레이아웃을 수행하는 방법입니다.


Marcelo Glasberg의 원본 ​​기사

Marcelo는 원래 이 콘텐츠를 Flutter: 초보자도 알아야 하는 고급 레이아웃 규칙을 Medium에 게시했습니다. 우리는 그것이 마음에 들었고 docs.flutter.dev에 게시할 수 있도록 허락해 달라고 요청했고 그는 기꺼이 동의했습니다. 고마워요, 마르셀로! GitHub 및 pub.dev에서 Marcelo를 찾을 수 있습니다.

또한 기사 상단에 헤더 이미지를 만들어 준 Simon Lightfoot에게도 감사드립니다.
