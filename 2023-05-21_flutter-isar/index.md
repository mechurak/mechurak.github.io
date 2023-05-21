# Flutter Isar db 맛보기


Flutter 에서 local db 용도로 (Android, iOS에다가 나중에 Web까지 지원하고 싶어서) Hive를 쓰고 있었다. 그런데 그 Hive 개발자가 이젠 주로 Isar를 개발하고 있고 또 추천하고 있다. 음.. 갈아타야 되나?
<!--more-->

<br/>

{{< image src="https://isar.dev/isar.svg" caption="Isar logo" width="200" >}}

<br/>
<br/>

## 0. 핵심요약

{{< admonition >}}
- local db를 고르고 있고, 나중에 web도 지원할 계획이라면 **Isar**!
  - 단, 2023년 5월 현재 최신 버전인 3.1.0 에서는 Web 지원 안 됨
  - 지금 당장 Web을 지원해야 한다면 Hive
- db 내용 실시간으로 볼 수 있는 Ispector 도 예쁨
{{< /admonition >}}

<br/>

아래는 [공식 문서의 FAQ](https://isar.dev/faq.html#isar-vs-hive) 내용이다. 
> **Isar vs Hive**  
> The answer is easy: Isar was [started as a replacement for Hive](https://github.com/hivedb/hive/issues/246) and is now at a state where I recommend always using Isar over Hive.

나의 경우엔 아직 Hive를 심각하게 쓰고 있지는 않아서, Isar 로 갈아타기로 했다.  

<br/>

아래는 [공식 페이지](https://isar.dev/)에서 자랑하고 있는 Isar의 장점이다.

{{< image src="/images/flutter/isar_features.jpg" caption="이미지 출처: https://isar.dev/" width="997">}}

<br/>

단, 2023년 5월 현재 최신 버전(3.1.0)에서는 Web을 지원하지 못 한다.  
2.5.0을 사용하는 방법이 있는 것 같긴 한데, 그냥 문제 해결된 4.0을 기다리는 걸로 하자.
- [{GitHub Issue} Support for Isar web in version 3.x](https://github.com/isar/isar/issues/686#issuecomment-1493325747)

<br/>
<br/>

공식 페이지의 [Quickstart](https://isar.dev/tutorials/quickstart.html)와 [GitHub의 README](https://github.com/isar/isar) 기반으로 사용법을 알아보자.

참고로, 실습에 사용한 코드는 [GitHub](https://github.com/mechurak/flutter-practice/tree/main/lib)에서 확인해 볼 수 있다.

<br/>

---

## 1. 샘플 프로젝트 준비
간단하게 프로젝트 하나 만들어 보자.  

[{책} 쉽고 빠른 플러터 앱 개발 / 권태형 / 2022.04](https://product.kyobobook.co.kr/detail/S000001842218)의 "5장. Todo 리스트로 배우는 다양한 데이터 연동법" 의 첫 부분의 코드로 시작해 보자.  
- 출판사 GitHub : https://github.com/bjpublic/flutter-app

<br/>

위 코드를 살짝 수정한 버전의 코드로 아래와 같은 UI를 준비한다.

{{< image src="/images/flutter/todo_app_1.png" width="250" >}}
&emsp;{{< image src="/images/flutter/todo_app_2.png" width="250" >}}

- 그냥 메모리상의 리스트로 아이템 관리
- `+` 눌러서 아이템 추가 가능 (AlertDialog)
- `수정` 버튼 눌러서 아이템 수정 가능 (AlertDialog)
- `삭제` 버튼 눌러서 아이템 삭제 가능 (AlertDialog)

<br/>

---

## 2. package 추가
아래 패키지들이 필요하다.  
터미널에서 아래 커맨드를 입력해서 필요한 패키지들을 추가한다.

```bash
# 의존성 추가
flutter pub add isar isar_flutter_libs path_provider

# 개발단계에서 필요한 의존성 추가
flutter pub add -d isar_generator build_runner
```

<br/>

[Isar 3.1.0](https://github.com/isar/isar/blob/main/packages/isar/CHANGELOG.md#310) 버전으로 올라가면서, db 초기화때 `directory`도 넘겨야 한다.  
요거를 위해 `path_provider` 도 필요함


<br/>

---

## 3. collection 클래스 준비


클래스에 `@collection` 어노테이션을 붙이고  
`id`는 `Isar.autoIncrement` 로 하자.

```dart
import 'package:isar/isar.dart';

part 'todo_isar.g.dart';  // build_runner에서 자동으로 만들어 주는 코드 사용할 거임

@collection
class TodoIsar {
  Id id = Isar.autoIncrement;  // Isar가 자동으로 채워 줌
  late String title;  // late: 읽을 때 초기화 되어 있지 않으면 error
  String? description;  // 요건 null 가능
}
```

<br/>

이제 아래 커맨드를 실행하면, `todo_isar.g.dart` 파일이 생기고, 위 코드에서 `part` 줄의 빨간줄도 사라진다.

```bash
flutter pub run build_runner build
```

<br/>

`todo_isar.g.dart` 파일을 열어 보면 아래 부분이 보이는데, `TodoIsarSchema` 요게 Schema 로 쓰인다.

```dart
const TodoIsarSchema = CollectionSchema(
  name: r'TodoIsar',
  // ...
);
```  

<br/>

자동으로 생성되는 g.dart 파일은 버전 관리 필요 없으므로 `.gitignore`에도 추가하자.
```bash
# ...

# auto generated files
*.g.dart
```

<br/>

---

## 4. Isar instance 준비
이제 Isar 인스턴스를 만들고 초기화 한다.  
적당한 위치에서 `Isar.open()`을 호출한다.  
위 [3. collection 클래스 준비](#3-모델-클래스-준비)에서 정의한 collection 들의 Schema 들을 다 명시해야 한다.


```dart
final dir = await getApplicationDocumentsDirectory();
isar = await Isar.open(
  [TodoIsarSchema],  // 사용할 schema 다 명시
  directory: dir.path,
);
```

<br/>

---

## 5. db에서 Read & Write
**READ**  
모든 row를 다 읽는 건 `where().findAll()`을 사용하면 된다.
```dart
Future<List<TodoIsar>> getTodos() async {
  return await isar.todoIsars.where().findAll();
}
```

조건에 맞는 녀석들만 찾으려면, `filter()`, `where()` 부분의 문서를 좀 더 들여다 보자.  
- [{Isar 공식문서} Queries](https://isar.dev/queries.html)

<br/>

**WRITE**

db에 뭔가 Write 하려면 `isar.writeTxn()` 으로 감싸야 한다.  
Insert, Update 둘 다 `put()` 을 사용하면 된다.
```dart
Future addTodo(TodoIsar todo) async {
  await isar.writeTxn(() async {
    await isar.todoIsars.put(todo);
  });
}

Future updateTodo(TodoIsar todo) async {
  await isar.writeTxn(() async {
    await isar.todoIsars.put(todo);
  });
}
```

<br/>

Delete는 `delete()` 를 사용하면 된다.

```dart
Future deleteTodo(int id) async {
  debugPrint("TodoRepositoryIsar.deleteTodo($id)");
  await isar.writeTxn(() async {
    await isar.todoIsars.delete(id);
  });
}
```

<br/>

---

## 6. Inspector
Dev 빌드를 단말에서 실행 시키면, 로그상에 Isar Inspector를 연결할 수 있는 url 이 찍힌다.  
고 링크를 통해 브라우저 상으로 단말의 db 를 확인할 수 있다.

{{< image src="https://raw.githubusercontent.com/isar/isar/main/.github/assets/inspector.gif" caption="출처: [Isar GitHub](https://github.com/isar/isar#isar-database-inspector)">}}


<br/>

---

## 99. Reference
- [{Isar 공식문서} FAQ - Isar vs Hive](https://isar.dev/faq.html#isar-vs-hive)
- [{Isar 공식문서} Quickstart](https://isar.dev/tutorials/quickstart.html)
- [{GitHub} Isar](https://github.com/isar/isar)
- [{GitHub Issue} Support for Isar web in version 3.x](https://github.com/isar/isar/issues/686)
- [{책} 쉽고 빠른 플러터 앱 개발 / 권태형 / 2022.04.28 / 5. Todo 리스트로 배우는 다양한 데이터 연동법](https://product.kyobobook.co.kr/detail/S000001842218)
- [{GitHub - BJPUBLIC} 쉽고 빠른 플러터 앱 개발](https://github.com/bjpublic/flutter-app) 05_flutter_todo

<br/>

---
