# Flutter의 레이아웃 Layouts in Flutter

```
핵심은?

- 위젯은 UI를 구축하는 데 사용되는 클래스입니다.
- 위젯은 레이아웃과 UI 요소 모두에 사용됩니다.
- 간단한 위젯을 구성하여 복잡한 위젯을 구축하세요.
```

Flutter 레이아웃 메커니즘의 핵심은 위젯입니다. Flutter에서는 거의 모든 것이 위젯입니다. 심지어 레이아웃 모델도 위젯입니다. Flutter 앱에 표시되는 이미지, 아이콘, 텍스트는 모두 위젯입니다. 그러나 보이지 않는 것들은 보이는 위젯을 정렬, 제한, 정렬하는 행, 열, 그리드와 같은 위젯이기도 합니다.

더 복잡한 위젯을 만들기 위해 위젯을 구성하여 레이아웃을 만듭니다. 예를 들어 아래 첫 번째 스크린샷에는 각 아이콘 아래에 레이블이 있는 3개의 아이콘이 표시됩니다.

두 번째 스크린샷은 시각적 레이아웃을 표시하며 각 열에는 아이콘과 레이블이 포함된 3개 열의 행을 보여줍니다.

```
노트

이 튜토리얼의 스크린샷 대부분은 debugPaintSizeEnabled가 true로 설정된 상태로 표시되므로 시각적 레이아웃을 볼 수 있습니다. 자세한 내용은 Flutter 검사기 사용의 섹션인 시각적으로 레이아웃 문제 디버깅을 참조하세요.
```

다음은 이 UI의 위젯 트리 다이어그램입니다.

대부분은 예상한 대로 보이지만 컨테이너(분홍색으로 표시)가 궁금할 수도 있습니다. Container는 하위 위젯을 사용자 정의할 수 있는 위젯 클래스입니다. 패딩, 여백, 테두리 또는 배경색을 추가하고 일부 기능의 이름을 지정하려면 Container를 사용하세요.

이 예에서 각 Text 위젯은 여백을 추가하기 위해 Container에 배치됩니다. 행 주위에 패딩을 추가하기 위해 전체 Row도 Container에 배치됩니다.

이 예에서 UI의 나머지 부분은 속성에 의해 제어됩니다. color 속성을 사용하여 Icon의 색상을 설정합니다. Text.style 속성을 사용하여 글꼴, 색상, 두께 등을 설정합니다. Column과 Row에는 하위 항목이 수직 또는 수평으로 정렬되는 방식과 하위 항목이 차지해야 하는 공간의 양을 지정할 수 있는 속성이 있습니다.

## 위젯 배치 Lay out a widget

Flutter에서 단일 위젯을 어떻게 배치할까요? 이 섹션에서는 간단한 위젯을 만들고 표시하는 방법을 보여줍니다. 또한 간단한 Hello World 앱의 전체 코드도 표시됩니다.

Flutter에서는 몇 단계만 거치면 화면에 텍스트, 아이콘, 이미지를 넣을 수 있습니다.

## 1. 레이아웃 위젯을 선택하세요 Select a layout widget

표시되는 위젯을 정렬하거나 제한하려는 방법에 따라 다양한 레이아웃 위젯 중에서 선택하세요. 이러한 특성은 일반적으로 포함된 위젯에 전달됩니다.

이 예에서는 콘텐츠를 가로 및 세로 중앙에 배치하는 Center를 사용합니다.

## 2. 보이는 위젯 만들기 Create a visible widget

예를 들어 텍스트 위젯을 만듭니다.

```dart
Text('Hello World'),
```

이미지 위젯을 만듭니다.

```dart
return Image.asset(
  image,
  fit: BoxFit.cover,
);
```

아이콘 위젯을 만듭니다.

```dart
Icon(
  Icons.star,
  color: Colors.red[500],
),
```

## 3. 레이아웃 위젯에 보이는 위젯을 추가하세요

모든 레이아웃 위젯에는 다음 중 하나가 있습니다.

- 단일 하위 항목을 취하는 경우 child 속성(예: Center 또는 Container)
- Row, Column, ListView 또는 Stack과 같은 위젯 목록을 사용하는 경우 children

Center 위젯에 Text 위젯을 추가합니다.

```dart
const Center(
  child: Text('Hello World'),
),
```

## 4. 페이지에 레이아웃 위젯 추가

Flutter 앱은 그 자체가 위젯이며 대부분의 위젯에는 build() 메서드가 있습니다. 앱의 build() 메서드에서 위젯을 인스턴스화하고 반환하면 위젯이 표시됩니다.


## Material 앱

Material 앱의 경우 Scaffold 위젯을 사용할 수 있습니다. 기본 배너와 배경색을 제공하고 drawer, 스낵바, 바텀시트 추가를 위한 API를 갖추고 있습니다. 그런 다음 홈 페이지의 본문 속성에 Center 위젯을 직접 추가할 수 있습니다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    const String appTitle = 'Flutter layout demo';
    return MaterialApp(
      title: appTitle,
      home: Scaffold(
        appBar: AppBar(
          title: const Text(appTitle),
        ),
        body: const Center(
          child: Text('Hello World'),
        ),
      ),
    );
  }
}
```

```
노트

머티리얼 라이브러리는 머티리얼 디자인 원칙을 따르는 위젯을 구현합니다. UI를 디자인할 때 표준 위젯 라이브러리의 위젯만 사용하거나 Material 라이브러리의 위젯을 사용할 수 있습니다. 두 라이브러리의 위젯을 혼합하거나, 기존 위젯을 사용자 정의하거나, 사용자 정의 위젯 세트를 직접 구축할 수 있습니다.
```

## Cupertino 앱

Cupertino 앱을 만들려면 CupertinoApp 및 CupertinoPageScaffold 위젯을 사용하세요.

Material과 달리 기본 배너나 배경색을 제공하지 않습니다. 이는 직접 설정해야 합니다.

- 기본 색상을 설정하려면 구성된 CupertinoThemeData를 앱의 테마 속성에 전달하세요.

- 앱 상단에 iOS 스타일 탐색 모음을 추가하려면 스캐폴드의 NavigationBar 속성에 CupertinoNavigationBar 위젯을 추가하세요. CupertinoColors가 제공하는 색상을 사용하여 iOS 디자인과 일치하도록 위젯을 구성할 수 있습니다.

- 앱 본문을 배치하려면 Center 또는 Column과 같은 원하는 위젯을 값으로 사용하여 스캐폴드의 하위 속성을 설정하세요.

추가할 수 있는 다른 UI 구성요소를 알아보려면 Cupertino 라이브러리를 확인하세요.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return const CupertinoApp(
      title: 'Flutter layout demo',
      theme: CupertinoThemeData(
        brightness: Brightness.light,
        primaryColor: CupertinoColors.systemBlue,
      ),
      home: CupertinoPageScaffold(
        navigationBar: CupertinoNavigationBar(
          backgroundColor: CupertinoColors.systemGrey,
          middle: Text('Flutter layout demo'),
        ),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Text('Hello World'),
            ],
          ),
        ),
      ),
    );
  }
}
```

```
노트

Cupertino 라이브러리는 Apple의 iOS용 휴먼 인터페이스 지침을 따르는 위젯을 구현합니다. UI를 디자인할 때 표준 위젯 라이브러리 또는 Cupertino 라이브러리의 위젯을 사용할 수 있습니다. 두 라이브러리의 위젯을 혼합하거나, 기존 위젯을 사용자 정의하거나, 사용자 정의 위젯 세트를 직접 구축할 수 있습니다.
```

## Non-Material 앱

Material 앱이 아닌 경우 앱의 build() 메서드에 Center 위젯을 추가할 수 있습니다.

```dart
class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: const BoxDecoration(color: Colors.white),
      child: const Center(
        child: Text(
          'Hello World',
          textDirection: TextDirection.ltr,
          style: TextStyle(
            fontSize: 32,
            color: Colors.black87,
          ),
        ),
      ),
    );
  }
}
```

기본적으로 비 Material 앱에는 AppBar, 제목 또는 배경색이 포함되지 않습니다. Material 앱이 아닌 앱에서 이러한 기능을 사용하려면 직접 빌드해야 합니다. 이 앱은 Material 앱을 모방하기 위해 배경색을 흰색으로, 텍스트를 어두운 회색으로 변경합니다.

그게 전부입니다! 앱을 실행하면 Hello World가 표시됩니다.

앱 소스 코드:
- Material app
- non-Material app

## 여러 위젯을 수직 및 수평으로 배치

가장 일반적인 레이아웃 패턴 중 하나는 위젯을 수직 또는 수평으로 배열하는 것입니다. Row 위젯을 사용하여 위젯을 가로로 정렬하고, Column 위젯을 사용하여 위젯을 세로로 정렬할 수 있습니다.

```
핵심은?

- Row과 Column은 가장 일반적으로 사용되는 두 가지 레이아웃 패턴입니다.
- Row과 Column은 각각 하위 위젯 목록을 가져옵니다.
- 하위 위젯은 그 자체가 Row, Column 또는 기타 복잡한 위젯일 수 있습니다.
- Row 또는 Column이 하위 요소를 수직 및 수평으로 정렬하는 방법을 지정할 수 있습니다.
- 특정 하위 위젯을 늘리거나 제한할 수 있습니다.
- 하위 위젯이 Row 또는 Column의 사용 가능한 공간을 사용하는 방법을 지정할 수 있습니다.
```

Flutter에서 행이나 열을 만들려면 행 또는 열 위젯에 하위 위젯 목록을 추가하세요. 차례로 각 하위 항목 자체가 행이나 열이 될 수 있습니다. 다음 예에서는 행이나 열 내부에 행이나 열을 중첩하는 방법을 보여줍니다.

이 레이아웃은 행으로 구성됩니다. 행에는 두 개의 하위 항목(왼쪽 열, 오른쪽 이미지)이 포함되어 있습니다.

왼쪽 열의 위젯 트리는 행과 열을 중첩합니다.

행과 열을 중첩하여 Pavlova의 레이아웃 코드 중 일부를 구현합니다.

```
노트

Row과 Column은 가로 및 세로 레이아웃을 위한 기본 위젯입니다. 이러한 하위 수준 위젯을 사용하면 최대 사용자 정의가 가능합니다. Flutter는 또한 요구 사항에 충분할 수 있는 전문적이고 높은 수준의 위젯을 제공합니다. 예를 들어 Row 대신 선행 및 후행 아이콘 속성과 최대 3줄의 텍스트가 포함된 사용하기 쉬운 위젯인 ListTile을 선호할 수 있습니다. Column 대신, 내용이 너무 길어 사용 가능한 공간에 맞지 않을 경우 자동으로 스크롤되는 열 형식의 레이아웃인 ListView를 선호할 수 있습니다. 자세한 내용은 일반 레이아웃 위젯을 참조하세요.
```

## 위젯 정렬 Aligning widgets

mainAxisAlignment 및 crossAxisAlignment 속성을 사용하여 행이나 열이 하위 항목을 정렬하는 방법을 제어합니다. Row행의 경우 기본 축은 수평으로 실행되고 교차 축은 수직으로 실행됩니다. Column의 경우 주축은 수직으로 실행되고 교차 축은 수평으로 실행됩니다.

MainAxisAlignment 및 CrossAxisAlignment enum은 정렬 제어를 위한 다양한 상수를 제공합니다.

```
노트

프로젝트에 이미지를 추가할 때 해당 이미지에 액세스하려면 pubspec.yaml 파일을 업데이트해야 합니다. 이 예에서는 Image.asset을 사용하여 이미지를 표시합니다. 자세한 내용은 이 예제의 pubspec.yaml 파일 또는 자산 및 이미지 추가를 참조하세요. Image.network를 사용하여 온라인 이미지를 참조하는 경우에는 이 작업을 수행할 필요가 없습니다.
```

다음 예에서는 3개의 이미지 각각의 너비가 100픽셀입니다. 렌더링 상자(이 경우 전체 화면)의 너비는 300픽셀을 초과하므로 주축 정렬을 spaceEvenly로 설정하면 각 이미지 사이, 앞, 뒤의 여유 수평 공간을 고르게 나눕니다.

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    Image.asset('images/pic1.jpg'),
    Image.asset('images/pic2.jpg'),
    Image.asset('images/pic3.jpg'),
  ],
);
```

앱 소스: row_column

Column은 Row와 동일한 방식으로 작동합니다. 다음 예에서는 각각 높이가 100픽셀인 3개 이미지의 열을 보여줍니다. 렌더링 상자(이 경우 전체 화면)의 높이가 300픽셀을 초과하므로 주축 정렬을 spaceEvenly로 설정하면 각 이미지 사이, 위, 아래에 수직 여유 공간이 고르게 나뉩니다.

```dart
Column(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    Image.asset('images/pic1.jpg'),
    Image.asset('images/pic2.jpg'),
    Image.asset('images/pic3.jpg'),
  ],
);
```

## 위젯 크기 조정 Sizing widgets

레이아웃이 너무 커서 장치에 맞지 않으면 영향을 받은 가장자리를 따라 노란색과 검은색 줄무늬 패턴이 나타납니다. 다음은 너무 넓은 행의 예입니다.

Expanded 위젯을 사용하면 위젯의 크기를 행이나 열에 맞게 조정할 수 있습니다. 렌더링 상자에 비해 이미지 행이 너무 넓은 이전 예를 수정하려면 각 이미지를 Expanded 위젯으로 래핑하세요.

```dart
Row(
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Expanded(
      child: Image.asset('images/pic1.jpg'),
    ),
    Expanded(
      child: Image.asset('images/pic2.jpg'),
    ),
    Expanded(
      child: Image.asset('images/pic3.jpg'),
    ),
  ],
);
```

앱 소스: sizing

아마도 위젯이 형제 위젯보다 두 배나 많은 공간을 차지하기를 원할 것입니다. 이를 위해 위젯의 플렉스 요소를 결정하는 정수인 Expanded 위젯 flex 속성을 사용하세요. 기본 플렉스 인자는 1입니다. 다음 코드는 중간 이미지의 플렉스 인자를 2로 설정합니다.

```dart
Row(
  crossAxisAlignment: CrossAxisAlignment.center,
  children: [
    Expanded(
      child: Image.asset('images/pic1.jpg'),
    ),
    Expanded(
      flex: 2,
      child: Image.asset('images/pic2.jpg'),
    ),
    Expanded(
      child: Image.asset('images/pic3.jpg'),
    ),
  ],
);
```

앱 소스: sizing

## 위젯 패킹 Packing widgets

기본적으로 행이나 열은 주축을 따라 가능한 한 많은 공간을 차지하지만 하위 요소를 서로 가깝게 묶으려면 해당 mainAxisSize를 MainAxisSize.min으로 설정하세요. 다음 예에서는 이 속성을 사용하여 별표 아이콘을 함께 묶습니다.

```dart
Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    const Icon(Icons.star, color: Colors.black),
    const Icon(Icons.star, color: Colors.black),
  ],
)
```

앱 소스: pavlova

## 행과 열 중첩 Nesting rows and columns

레이아웃 프레임워크를 사용하면 행과 열 내부에 행과 열을 필요한 만큼 중첩할 수 있습니다. 다음 레이아웃의 개요 섹션에 대한 코드를 살펴보겠습니다.

윤곽선이 표시된 섹션은 두 개의 행으로 구현됩니다. 평점 행에는 별 5개와 리뷰 수가 포함됩니다. 아이콘 행에는 아이콘과 텍스트로 구성된 세 개의 열이 포함되어 있습니다.

평점 행의 위젯 트리:

평점 변수는 별 5개 아이콘의 작은 행과 텍스트를 포함하는 행을 만듭니다.

```dart
final stars = Row(
  mainAxisSize: MainAxisSize.min,
  children: [
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    Icon(Icons.star, color: Colors.green[500]),
    const Icon(Icons.star, color: Colors.black),
    const Icon(Icons.star, color: Colors.black),
  ],
);

final ratings = Container(
  padding: const EdgeInsets.all(20),
  child: Row(
    mainAxisAlignment: MainAxisAlignment.spaceEvenly,
    children: [
      stars,
      const Text(
        '170 Reviews',
        style: TextStyle(
          color: Colors.black,
          fontWeight: FontWeight.w800,
          fontFamily: 'Roboto',
          letterSpacing: 0.5,
          fontSize: 20,
        ),
      ),
    ],
  ),
);
```

```
팁

심하게 중첩된 레이아웃 코드로 인해 발생할 수 있는 시각적 혼란을 최소화하려면 변수와 함수에 UI 부분을 구현하세요.
```

평점 행 아래의 아이콘 행에는 3개의 열이 있습니다. 각 열에는 위젯 트리에서 볼 수 있듯이 아이콘과 두 줄의 텍스트가 포함되어 있습니다.

iconList 변수는 아이콘 행을 정의합니다.

```dart
const descTextStyle = TextStyle(
  color: Colors.black,
  fontWeight: FontWeight.w800,
  fontFamily: 'Roboto',
  letterSpacing: 0.5,
  fontSize: 18,
  height: 2,
);

// DefaultTextStyle.merge()를 사용하면 해당 하위 항목과 
// 모든 후속 하위 항목에 상속되는 기본 텍스트 스타일을 만들 수 있습니다.

final iconList = DefaultTextStyle.merge(
  style: descTextStyle,
  child: Container(-
    padding: const EdgeInsets.all(20),
    child: Row(
      mainAxisAlignment: MainAxisAlignment.spaceEvenly,
      children: [
        Column(
          children: [
            Icon(Icons.kitchen, color: Colors.green[500]),
            const Text('PREP:'),
            const Text('25 min'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.timer, color: Colors.green[500]),
            const Text('COOK:'),
            const Text('1 hr'),
          ],
        ),
        Column(
          children: [
            Icon(Icons.restaurant, color: Colors.green[500]),
            const Text('FEEDS:'),
            const Text('4-6'),
          ],
        ),
      ],
    ),
  ),
);
```

leftColumn 변수에는 등급 및 아이콘 행은 물론 Pavlova를 설명하는 제목 및 텍스트가 포함됩니다.

```dart
final leftColumn = Container(
  padding: const EdgeInsets.fromLTRB(20, 30, 20, 20),
  child: Column(
    children: [
      titleText,
      subTitle,
      ratings,
      iconList,
    ],
  ),
);
```

왼쪽 열은 너비를 제한하기 위해 SizedBox에 배치됩니다. 마지막으로 UI는 카드 내부의 전체 행(왼쪽 열과 이미지 포함)으로 구성됩니다.

파블로바 이미지는 Pixabay에서 가져온 것입니다. Image.network()를 사용하여 넷에서 이미지를 포함할 수 있지만 이 예에서는 이미지가 프로젝트의 이미지 디렉터리에 저장되고 pubspec 파일에 추가되며 Images.asset()을 사용하여 액세스됩니다. 자세한 내용은 자산 및 이미지 추가를 참조하세요.

```dart
body: Center(
  child: Container(
    margin: const EdgeInsets.fromLTRB(0, 40, 0, 30),
    height: 600,
    child: Card(
      child: Row(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          SizedBox(
            width: 440,
            child: leftColumn,
          ),
          mainImage,
        ],
      ),
    ),
  ),
),
```

```
팁

Pavlova 예제는 태블릿과 같은 넓은 장치에서 수평으로 가장 잘 실행됩니다. iOS 시뮬레이터에서 이 예제를 실행하는 경우 하드웨어 > 장치 메뉴를 사용하여 다른 장치를 선택할 수 있습니다. 이 예에서는 iPad Pro를 권장합니다. 하드웨어 > 회전을 사용하여 방향을 가로 모드로 변경할 수 있습니다. Window > Scale을 사용하여 (논리적 픽셀 수를 변경하지 않고) 시뮬레이터 창의 크기를 변경할 수도 있습니다.
```

앱 소스: pavlova

## 일반적인 레이아웃 위젯 Common layout widgets

Flutter에는 풍부한 레이아웃 위젯 라이브러리가 있습니다. 다음은 가장 일반적으로 사용되는 몇 가지 사항입니다. 전체 목록으로 인해 부담을 느끼기보다는 가능한 한 빨리 시작하고 실행할 수 있도록 하는 것이 목적입니다. 사용 가능한 다른 위젯에 대한 자세한 내용은 위젯 카탈로그를 참조하거나 API 참조 문서의 검색 상자를 사용하세요. 또한 API 문서의 위젯 페이지에서는 필요에 더 잘 맞는 유사한 위젯에 대한 제안을 제공하는 경우가 많습니다.

다음 위젯은 두 가지 범주, 즉 위젯 라이브러리의 표준 위젯과 Material 라이브러리의 특수 위젯으로 분류됩니다. 모든 앱은 위젯 라이브러리를 사용할 수 있지만 Material 앱만 Material 구성 요소 라이브러리를 사용할 수 있습니다.

## 표준 위젯 Standard widgets

- Container: 위젯에 패딩, 여백, 테두리, 배경색 또는 기타 장식을 추가합니다.
- GridView: 위젯을 스크롤 가능한 그리드로 배치합니다.
- ListView: 위젯을 스크롤 가능한 목록으로 배치합니다.
- Stack: 위젯을 다른 위젯 위에 겹칩니다.

## Material 위젯 Material widgets

- Card: 둥근 모서리와 그림자가 있는 상자에 관련 정보를 구성합니다.
- ListTile: 최대 3줄의 텍스트와 선택적 선행 및 후행 아이콘을 행으로 구성합니다.

## 컨테이너 Container

많은 레이아웃에서는 패딩을 사용하여 위젯을 분리하거나 테두리나 여백을 추가하기 위해 Container를 자유롭게 사용합니다. 전체 레이아웃을 Container에 배치하고 배경색이나 이미지를 변경하여 기기의 배경을 변경할 수 있습니다.

## 요약 (컨테이너) Summary (Container)

- 패딩, 여백, 테두리 추가
- 배경색이나 이미지 변경
- 단일 child 위젯을 포함하지만 해당 child는 Row, Column 또는 위젯 트리의 루트일 수도 있습니다.

# 예시 (컨테이너) Examples (Container)

이 레이아웃은 각각 2개의 이미지를 포함하는 2개의 행이 있는 열로 구성됩니다. 컨테이너는 열의 배경색을 밝은 회색으로 변경하는 데 사용됩니다.

```dart
Widget _buildImageColumn() {
  return Container(
    decoration: const BoxDecoration(
      color: Colors.black26,
    ),
    child: Column(
      children: [
        _buildImageRow(1),
        _buildImageRow(3),
      ],
    ),
  );
}
```

Container는 각 이미지에 둥근 테두리와 여백을 추가하는 데에도 사용됩니다.

```dart
Widget _buildDecoratedImage(int imageIndex) => Expanded(
      child: Container(
        decoration: BoxDecoration(
          border: Border.all(width: 10, color: Colors.black38),
          borderRadius: const BorderRadius.all(Radius.circular(8)),
        ),
        margin: const EdgeInsets.all(4),
        child: Image.asset('images/pic$imageIndex.jpg'),
      ),
    );

Widget _buildImageRow(int imageIndex) => Row(
      children: [
        _buildDecoratedImage(imageIndex),
        _buildDecoratedImage(imageIndex + 1),
      ],
    );
```

튜토리얼에서 더 많은 Container 예제를 찾을 수 있습니다.

앱 소스: container

## 그리드뷰 GridView

GridView를 사용하여 위젯을 2차원 목록으로 배치합니다. GridView는 사전 제작된 두 개의 목록을 제공하거나 사용자 지정 그리드를 직접 구축할 수 있습니다. GridView는 콘텐츠가 렌더링 상자에 비해 너무 길다는 것을 감지하면 자동으로 스크롤됩니다.

## 요약 (그리드뷰) Summary (GridView)

- 그리드에 위젯을 배치합니다.
- 열 내용이 렌더링 상자를 초과하는 경우를 감지하고 자동으로 스크롤 제공
- 자신만의 맞춤형 그리드를 구축하거나 제공된 그리드 중 하나를 사용하세요.
- GridView.count를 사용하면 열 수를 지정할 수 있습니다.
- GridView.extent를 사용하면 타일의 최대 픽셀 너비를 지정할 수 있습니다.

```
노트

셀이 차지하는 행과 열이 중요한 2차원 목록을 표시하는 경우(예: "아보카도" 행의 "칼로리" 열 항목) Table 또는 DataTable을 사용합니다.
```

## 예시 (그리드뷰) Examples (GridView)

GridView.extent를 사용하여 최대 150픽셀 너비의 타일로 그리드를 만듭니다.

앱 소스: grid_and_list

GridView.count를 사용하여 세로 모드에서는 타일 2개 너비, 가로 모드에서는 타일 3개 너비의 그리드를 만듭니다. 제목은 각 GridTile의 footer 속성을 설정하여 생성됩니다.

dart 코드: grid_list_demo.dart

```dart
Widget _buildGrid() => GridView.extent(
    maxCrossAxisExtent: 150,
    padding: const EdgeInsets.all(4),
    mainAxisSpacing: 4,
    crossAxisSpacing: 4,
    children: _buildGridTileList(30));

// 이미지는 pic0.jpg, pic1.jpg...pic29.jpg라는 이름으로 저장됩니다. 
// List.generate() 생성자를 사용하면 객체에 예측 가능한 이름 지정 패턴이 
// 있는 경우 목록을 쉽게 생성할 수 있습니다.

List<Container> _buildGridTileList(int count) => List.generate(
    count, (i) => Container(child: Image.asset('images/pic$i.jpg')));
```

## 리스트뷰 ListView

열과 유사한 위젯인 ListView는 콘텐츠가 렌더링 상자에 비해 너무 길면 자동으로 스크롤 기능을 제공합니다.

## 요약 (리스트뷰) Summary (ListView)

- 상자 목록을 정리하기 위한 특화된 컬럼
- 가로 또는 세로로 배치 가능
- 콘텐츠가 맞지 않는 경우를 감지하고 스크롤 기능을 제공합니다.
- Column보다 구성이 덜하지만 사용하기 쉽고 스크롤을 지원합니다.

## 예시 (리스트뷰) Examples (ListView)

ListView를 사용하여 ListTiles를 사용하는 업체 목록을 표시합니다. Divider는 극장과 레스토랑을 분리합니다.

앱 소스: grid_and_list

ListView를 사용하여 특정 색상 계열에 대한 Material 2 디자인 팔레트의 색상을 표시합니다.

Dart 코드: colors_demo.dart

```dart
Widget _buildList() {
  return ListView(
    children: [
      _tile('CineArts at the Empire', '85 W Portal Ave', Icons.theaters),
      _tile('The Castro Theater', '429 Castro St', Icons.theaters),
      _tile('Alamo Drafthouse Cinema', '2550 Mission St', Icons.theaters),
      _tile('Roxie Theater', '3117 16th St', Icons.theaters),
      _tile('United Artists Stonestown Twin', '501 Buckingham Way',
          Icons.theaters),
      _tile('AMC Metreon 16', '135 4th St #3000', Icons.theaters),
      const Divider(),
      _tile('K\'s Kitchen', '757 Monterey Blvd', Icons.restaurant),
      _tile('Emmy\'s Restaurant', '1923 Ocean Ave', Icons.restaurant),
      _tile('Chaiya Thai Restaurant', '272 Claremont Blvd', Icons.restaurant),
      _tile('La Ciccia', '291 30th St', Icons.restaurant),
    ],
  );
}

ListTile _tile(String title, String subtitle, IconData icon) {
  return ListTile(
    title: Text(title,
        style: const TextStyle(
          fontWeight: FontWeight.w500,
          fontSize: 20,
        )),
    subtitle: Text(subtitle),
    leading: Icon(
      icon,
      color: Colors.blue[500],
    ),
  );
}
```

## 스택 Stack

Stack을 사용하여 기본 위젯(종종 이미지) 위에 위젯을 정렬합니다. 위젯은 기본 위젯과 완전히 또는 부분적으로 겹칠 수 있습니다.


## 요약 (스택) Summary (Stack)

- 다른 위젯과 겹치는 위젯에 사용
- children 목록의 첫 번째 위젯은 기본 위젯입니다. 후속 children은 해당 기본 위젯 위에 오버레이됩니다.
- Stack의 콘텐츠는 스크롤할 수 없습니다.
- 렌더링 상자를 초과하는 하위 항목을 자르도록 선택할 수 있습니다.

## 예시 (스택) Exaples (Stack)

Stack을 사용하여 CircleAvatar 위에 컨테이너(반투명 검정색 배경에 텍스트를 표시함)를 오버레이합니다. Stack은 정렬 속성과 정렬을 사용하여 텍스트를 오프셋합니다.

앱 소스: card_and_stack

스택을 사용하여 이미지 위에 아이콘을 오버레이합니다.

Dart 코드: bottom_navigation_demo.dart

```dart
Widget _buildStack() {
  return Stack(
    alignment: const Alignment(0.6, 0.6),
    children: [
      const CircleAvatar(
        backgroundImage: AssetImage('images/pic.jpg'),
        radius: 100,
      ),
      Container(
        decoration: const BoxDecoration(
          color: Colors.black45,
        ),
        child: const Text(
          'Mia B',
          style: TextStyle(
            fontSize: 20,
            fontWeight: FontWeight.bold,
            color: Colors.white,
          ),
        ),
      ),
    ],
  );
}
```

## 카드 Card

Material 라이브러리의 Card에는 관련 정보 덩어리가 포함되어 있으며 거의 ​​모든 위젯으로 구성될 수 있지만 ListTile과 함께 자주 사용됩니다. Card에는 단일 child 항목이 있지만 해당 child 항목은 열, 행, 목록, 그리드 또는 여러 하위 child를 지원하는 기타 위젯일 수 있습니다. 기본적으로 Card는 크기를 0 x 0 픽셀로 축소합니다. SizedBox를 사용하여 카드 크기를 제한할 수 있습니다.

Flutter에서 Card에는 약간 둥근 모서리와 그림자가 있어 3D 효과를 줍니다. Card의 elevation 속성을 변경하면 그림자 효과를 제어할 수 있습니다. 예를 들어 elevation을 24로 설정하면 시각적으로 카드가 표면에서 더 멀리 올라가고 그림자가 더 분산됩니다. 지원되는 elevation 값 목록은 Material 가이드라인의 elevation을 참조하세요. 지원되지 않는 값을 지정하면 그림자가 완전히 비활성화됩니다.

## 요약 (카드) Summary (Card)

- Material 카드 구현
- 관련된 정보 덩어리를 표시하는 데 사용됩니다.
- 단일 child를 허용하지만 해당 child는 행, 열 또는 자식 목록을 보유하는 기타 위젯일 수 있습니다.
- 둥근 모서리와 그림자로 표시됨
- 카드 내용은 스크롤할 수 없습니다.
- Material 라이브러리

## 예시 (카드) Examples (Card)

3개의 ListTiles을 포함하고 SizedBox로 포장하여 크기를 조정한 카드입니다. 구분선은 첫 번째와 두 번째 ListTiles를 분리합니다.

앱 소스: card_and_stack

이미지와 텍스트가 포함된 카드입니다.

Dart 코드: cards_demo.dart

```dart
Widget _buildCard() {
  return SizedBox(
    height: 210,
    child: Card(
      child: Column(
        children: [
          ListTile(
            title: const Text(
              '1625 Main Street',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            subtitle: const Text('My City, CA 99984'),
            leading: Icon(
              Icons.restaurant_menu,
              color: Colors.blue[500],
            ),
          ),
          const Divider(),
          ListTile(
            title: const Text(
              '(408) 555-1212',
              style: TextStyle(fontWeight: FontWeight.w500),
            ),
            leading: Icon(
              Icons.contact_phone,
              color: Colors.blue[500],
            ),
          ),
          ListTile(
            title: const Text('costa@example.com'),
            leading: Icon(
              Icons.contact_mail,
              color: Colors.blue[500],
            ),
          ),
        ],
      ),
    ),
  );
}
```

## 리스트타일 ListTile

최대 3줄의 텍스트와 선택적 선행 및 후행 아이콘을 포함하는 행을 쉽게 생성하려면 Material 라이브러리의 특수 행 위젯인 ListTile을 사용하세요. ListTile은 Card나 ListView에서 가장 일반적으로 사용되지만 다른 곳에서도 사용할 수 있습니다.

## 요약(ListTile) Summary (ListTile)

- 최대 3줄의 텍스트와 선택적 아이콘을 포함하는 특수 행
- Row보다 구성이 덜하지만 사용하기 쉽습니다.
- Material 라이브러리에서

## 예시 (리스트타일) Examples (ListTile)

3개의 ListTiles를 포함하는 카드입니다.

앱 소스: card_and_stack

주요 위젯과 함께 ListTile을 사용합니다.

Dart 코드: list_demo.dart

## 제약 조건 Constraints

Flutter의 레이아웃 시스템을 완전히 이해하려면 Flutter가 레이아웃에서 구성 요소의 위치를 ​​지정하고 크기를 조정하는 방법을 알아야 합니다. 자세한 내용은 제약 조건 이해를 참조하세요.

## 비디오 Videos

Flutter in Focus 시리즈의 일부인 다음 동영상에서는 Stateless 및 Stateful 위젯을 설명합니다.

Flutter in Focus playlist

금주의 위젯 시리즈의 각 에피소드는 위젯에 중점을 둡니다. 그 중 일부에는 레이아웃 위젯이 포함되어 있습니다.

Flutter Widget of the Week playlist

## 기타 리소스 Other resources

다음 리소스는 레이아웃 코드를 작성할 때 도움이 될 수 있습니다.

레이아웃 튜토리얼
    
    레이아웃을 작성하는 방법을 알아보세요.

위젯 카탈로그
    
    Flutter에서 사용할 수 있는 다양한 위젯을 설명합니다.

Flutter의 HTML/CSS 아날로그


    웹 프로그래밍에 익숙한 사람들을 위해 이 페이지에서는 HTML/CSS 기능을 Flutter 기능에 매핑합니다.

API 참조 문서

    모든 Flutter 라이브러리에 대한 참조 문서입니다.

자산 및 이미지 추가

    앱 패키지에 이미지 및 기타 자산을 추가하는 방법을 설명합니다.

Flutter를 사용한 0부터 1

    첫 번째 Flutter 앱을 작성해 본 한 사람의 경험입니다.
