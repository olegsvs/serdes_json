# serdes_json

serdes_json - is a simple and clean json serializer and deserializer.
To create serializable model just describe your model, add `Scheme` suffix to the end of class name,
and annotate with `@SerdesJson`. That's it!

### NOTE

This package is used only as a data holder and validation unit. You can't add additional methods to a model, at least for now. If you need a more flexible library, you can use [json_serializable](https://github.com/google/json_serializable.dart).

## Install

To use serdes_json you need install serdes_json and serdes_json_generator:

```yaml
dependencies:
  serdes_json: <version>
```

```yaml
dev_dependencies:
  serdes_json_generator: <version>
```

## Generate models

```sh
flutter packages pub run build_runner build
```

## Examples

### Model with all required fields

```dart
import 'package:serdes_json/serdes_json.dart';

part 'schemes.g.dart';

@SerdesJson()
class NewsListScheme {
  late List<NewsItemScheme> news;
}

// You can define private schemes.
@SerdesJson()
class _UserScheme {
  late String name;
}
```

```dart
@SerdesJson()
class NewsItemScheme {
  late int id;
  late String title;
  late String description;
  late int commentsCount;
}
```

### Snake case models

To convert all names to snake case, add `convertToSnakeCase: true` to the annotation:

```dart
@SerdesJson(convertToSnakeCase: true)
class NewsItemScheme {
  late int id;
  late String title;
  late String description;
  late int commentsCount;
}
```

### Rename field

You can manually specify names for field:

```dart
@SerdesJson()
class PayloadScheme {
  late int id;
  late String title;
  late String description;
  @SerdesJsonField(name: 'comments_count')
  late int commentsCount;
}
```

### Use custom suffix

```dart
@SerdesJson(endsWith: 'Response')
class NewsItemResponse {
  late int id;
  late String title;
  late String description;
  late int commentsCount;
}
```

### Optional

By default all fields are required, and if there will not be a field, then parser will throw the `SchemeConsistencyException`.
To describe optional fields:

```dart
@SerdesJson()
class PayloadScheme {
  late Map<String, dynamic>? result;
  late ErrorScheme? error;
}
```

### Enums

WIP

### Custom validations

WIP

### To json

```dart
final news = NewsItem(
  id: 1,
  title: 'Some new',
  description: 'Description',
  commentsCount: 12,
);

final json = news.toJson();
final stringJson = news.toStringJson();
```

### From json

```dart
final json = _client.getJsonBody(response);
final jsonString = _client.getStringJsonBody(response);

final news = NewsItem.fromJson(json);
final news = NewsItem.fromStringJson(jsonString);
```
