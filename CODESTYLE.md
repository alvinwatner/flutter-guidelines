# STYLE GUIDELINES

_(last updated 30/11/203)_

We are fully aware that there are many ways to achieve any of the specified conventions below. A surprisingly important part of good code is good style. Consistent naming, ordering, and formatting helps code that is the same look the same. If we use a consistent style across the entire Dart ecosystem, it makes it easier for all of us to learn from and contribute to each others’ code. 

And finally, always put yourself on the perspective of other dev, how will they think if they read your code. Build a good legacy my friend. Let’s get started.

## Knowledges requirements

1. Flutter & Dart
2. Bloc State Management
3. Clean Architecture
4. Dependency Injection

## Identifier

Identifiers come in four flavors in our code base.
1. camel case (camelCase) 
    * variable
    * function

 ```dart
✅ good

// variable
final int points = 10.0;

// function
Future<DefaultResponse> reportVacancy();
```

2. pascal case (PascalCase)
    * enum
    * class

 ```dart
✅ good

// enum
enum BuildMode {
  debug,
  profile,
  release,
}

// class 
class DeviceUtils {}

```

3. param case (param-case)
    * route-name
    * route-arguments

 ```dart
✅ good

// route-name
class SomePage extends StatefulWidget {
  static const routeName = 'some-page';
    ...
}


// route-arguments 
case SomePage.routeName:
  final arguments = settings.arguments as Map<String, dynamic>;
  final String argOne = arguments['arg-one'] as String;
  return MaterialPageRoute(
    settings: settings,
    builder: (context) => SomePage(argOne: argOne),
  );

```

4. snake case (snake_case)
    * file_name, name_import_prefixes

 ```dart
✅ good

// name_import_prefixes
import 'package:angular_components/angular_components.dart' as ac;

// file_name
file_name.dart

```


## 1. Define variables

There are many ways to define variables in Dart. Anythings that works, wil works. But to keep everything consistent in this codebase, the convention is as follows.

```dart
final T a = value;
late T b;
T b = value;
```

For examples : 
```dart
✅ good

final int counter = 1;
late String name;
CustomObject b = CustomObject(name: "A", counter: 1);
```


```dart
❌ bad

final counter = 1;
late var name;
final b = CustomObject(name: "A", counter: 1);
```


## 2. Variable placements in Stateful widget 

To keep the variables of your stateful widget tidy, we have a prescribed order for different types of variables with each “section” should be separated by a blank line.

```dart
✅ good

class _MyWidgetState extends State<MyWidget> {
  // All Value Notifier goes to here
  final ValueNotifier<T> _myValueNotifier = ValueNotifier<T>(T());

  // All Bloc goes to here
  final MyBloc _myBloc = sl<MyBloc>();

  // All Controllers goes to here
  final TextEditingController _textController = TextEditingController();
  final ScrollController _scrollController = ScrollController();

  // Others
  final _formState = GlobalKey<FormState>();
  bool _otherVariable = false;
  

  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {    
    return AnotherWidget();
  }

}
```


```dart
❌ bad

class _MyWidgetState extends State<MyWidget> {
  final _formState = GlobalKey<FormState>();
  final MyBloc _myBloc = sl<MyBloc>();
  final ValueNotifier<T> _myValueNotifier = ValueNotifier<T>(T());
  final TextEditingController _textController = TextEditingController();
  final ScrollController _scrollController = ScrollController();  
  bool _otherVariable = false;
  

  @override
  void initState() {
    super.initState();
  }

  @override
  Widget build(BuildContext context) {    
    return AnotherWidget();
  }

}
```


## 3. Add new wordings with Flutter Localization

All of the localization files located inside **lib/l10n** folder. 

```
lib
.
.
.
└─ l10n
  └─ arb
    └─ app_en.arb
    └─ app_id.arb    
.
.
.
```

To add new words for every languages manually (without vscode extention) : 
1. Go to lib/l10n/arb folder 
2. There you will find .arb file per each language.
3. To add a word without arguments:\
    ```"{feature}_{variableName}": "Some words"```
4. To add a word with arguments:
    ```
   "@{feature}_{variableName}": {
     "description": "Some descriptive description",
     "placeholders": {
       "arg": {
         "type": "String"
       }
     }
   },

    "{feature}_{variableName}": "Some words {arg}
    ```

## 4. Named and positioned arguments

Positioned arguments only used for a function with 1 argument, anythings beyond 1 must used named arguments.

 ```dart
✅ good

// 1 argument
Future<DefaultResponse> reportVacancy(int employerId);

// more than 1 arguments
Future<DefaultResponse> removeLikeVacancy({required int employerId, required int vacancyId})
```


## 5. Trailing coma

A function with more than 3 arguments must ended up with trailing coma. 

 ```dart
✅ good

// less than 3 arguments
Future<DefaultResponse> removeLikeVacancy(int employerId, int vacancyId)

// more than 3 arguments
Future<DefaultResponse> reportVacancy(
    int employerId,
    int vacancyId,
    String reason,
);
```

## 6. Model Response 

Every model name that decoded from API response must has suffix ```Response```. Commonly the json structures from API response will be as the following : 
```
{‘message’ : ’string’, ‘data’: {a complicated json structures}}
```

When decoding ‘data’ into a Class, the name of the class supposed to be the core API object. For example, say there is an API to get profile information  ```/profile-info```, with the following response :

```
{‘message’ : ’string’, ‘data’: {'name': 'John'}}
```

Then the model would look like this :

```dart
class ProfileInfoResponse extends Equatable {
  @JsonKey(name: 'message')
  final String? message;
  @JsonKey(name: 'data')
  final ProfileData? data;

  const VacancyResponse({
    this.message,
    this.data,
  });

  static VacancyResponse fromJson(Map<String, dynamic> json) =>
      _$VacancyResponseFromJson(json);

  Map<String?, dynamic> toJson() => _$VacancyResponseToJson(this);

  @override
  List<Object?> get props => [
        message,
        data,
      ];

  @override
  String toString() {
    return 'VacancyResponse{message: $message, data: $data}';
  }
}


class ProfileData extends Equatable {
  @JsonKey(name: 'name')
  final String? name;

  const ProfileData({
    this.name,
  });

  static ProfileData fromJson(Map<String, dynamic> json) =>
      _$ProfileDataFromJson(json);

  Map<String?, dynamic> toJson() => _$ProfileDataToJson(this);

  @override
  List<Object?> get props => [
        name,
      ];

  @override
  String toString() {
    return 'ProfileData{name: $name}';
  }
}

```


## 7. API Request body & Params arguments for datasource, repository, and usecase 

All the neccessary arguments are wrapped by a class with suffix ```Params``` to every call-stack (from datasource function to repository usecases).

The arguments of a usecase are not as simple as speficying all the key-value pair from the request body. Only specify the arguments if : (1) the key-value pair are dynamically changing from the interaction of the presentation layer, or (2) only can be retrieved from the presentation layer. For example, an API with the following request body ```{‘vacancy_id’: int, ‘page’ : int, ‘limit’ : int, ‘device_id’ : int }``` . 

* ```vacancy_id``` argument only can be retrieved from previous API call from the presentation layer (✅ Params argument)
*  ```page``` argument increment when the pagination is triggered. (✅ Params argument)
* ```limit``` is not retrieved from presentation layer (❌ Not Params argument)
* ```device_id``` is not retrieved from presentation layer (❌ Not Params argument)


Hence the Params arguments : 

```dart
class ParamsGetVacancyData extends Equatable {
  final int? vacancyId;
  final int? page;

  const ParamsGetVacancyData({
    this.vacancyId,
    this.page,
  });

  @override
  List<Object?> get props => [
        vacancyId,
        page,
      ];

  @override
  String toString() {
    return 'ParamsGetVacancyData{vacancyId: $vacancyId, page: $page}';
  }
}

```

## 8. BaseUrlConfig

Related From [4], BaseUrlConfig is a helper class to specify certain key-value pair to API request body or headers. For example, when an API require ```lat, long, device_id, and candidate_id ``` for the request body and an authorization token for the header, then specify the following to the Options class : 

```dart
    final Response<dynamic> response = await dio.post(
      path,
      options: Options(
        contentType: Headers.jsonContentType,
        headers: {
          BaseUrlConfig.requiredToken: true,
        },
        extra: {
          BaseUrlConfig.requiredUserID: true,
          BaseUrlConfig.requireLocation: true,
          BaseUrlConfig.requireDeviceID: true,
        },
      ),
      data: data,
    );
```

## 9. Reusable Widgets

To create reusable widgets, it is encouraged to extract it as a class widget  instead of a functions. An interesting discussion of why [here](https://www.reddit.com/r/FlutterDev/comments/avhvco/extracting_widgets_to_a_function_is_not_an/).


### 9.1 Where to put the extracted widget class? 
1. **To separate dart file**, if : 
   1. There have been more than 3 extracted widgets in the same file.
   2. The extracted widget consist of complex logic. Trait of being too complex, could be: (1) more than 250 lines of code (2) has multiple BlocBuilder and BlocListener (3) has more than 2 local functions.

2. **Below parent class widget** with leading underscore (e.g., _MyPrivateWidget), if :
     1. There are no more than 3 extracted widgets in the same file.


### 9.2 If **to separate dart file**, which specific directory it will be placed? Below are the checklist to decide :

☐ Does the widget used across features?\
 📍 put it into core/widgets. 

☐ Does the widget only used for a single feature?\
📍 put it into {features_name}/widgets.

☐ Does the widget being used for multiple products?\
📍 add the reusable widgets to this [mobile_common_widgets](https://github.com/Jobseeker-company/mobile-common-widgets) repository. This will help your peers but don't forget to announce the related engineers, so no one is doing duplicate works.


## 10. Deprecated Annotation

For a block of code that no longer being used, but with a possibility being used in the future, use [deprecated annotation](https://api.flutter.dev/flutter/dart-core/Deprecated-class.html)

## 11.  Bloc

Bloc commonly used as bridge to consume a usecase. To create a bloc, use **[mason](https://docs.brickhub.dev/mason-new/)** ‘bloc’ brick, it will generate all the base templates with a default event for initial load on a single page. More about bloc? visit the official [Documentation](https://dart.dev/effective-dart/style#do-name-extensions-using-uppercamelcase).

To use a bloc with singleton object : 
```dart
// step 1 : instantiate bloc
final CoolBloc _coolBloc = sl<CoolBloc>();

// step 2 : provide bloc to the parent widget
BlocProvider(
  create: (context) => _coolBloc,
  child : ...
)

// step 3 : send an event to the bloc
_coolBloc.add(CoolEvent(
  arg: arg,
));


// step 4 : build widget given state changes
BlocBuilder<CoolBloc, CoolState>(
  builder: (context, state) {
    if (state is LoadingWidget){
      return SuccessWidget()
    }    
    else if (state is ErrorWidget){
      return ErrorWidget()
    }    
    else if (state is SuccessState){
      return SuccessWidget()
    }
    return Container();
  },
);

```
Tips on updating widget per state changes : 
1. For a widget that require API response to be built (for example, a card that shows vacancy name and salary range from API response), use BlocBuilder.

```dart
BlocBuilder<CoolBloc, CoolState>(
  builder: (context, state) {
    if (state is LoadingState){
      return SuccessWidget()
    }    
    else if (state is ErrorState){
      return ErrorWidget()
    }    
    else if (state is SuccessState){
      return SuccessWidget()
    }
    return Container();
  },
);
```

2. For a widget that **only require a bool** from API response (for example, favorite, bookmark) use the combination of BlocListener and ValueListenableBuilder to give a spontaneous effect. The steps would be: (1) when the widget is tapped, update by flipping the bool ValueNotifier and change the widget with ValueListenableBuilder. (2) re-update the ValueNotifier after the API call got a response

```dart
// step 1 : flip the bool value
ElevatedButton(
  onPressed: () {
    _isBookmarked.value = !_isBookmarked.value;
  },
  child: const Text("bookmark"),
)

// step 2 : change the widget using ValueListenableBuilder
ValueListenableBuilder(
  valueListenable: _isBookmark,
  builder: (context, isBookmark, child) {
    if (isBookmark) {
      return Icon(
        JSIcon.bookmark,
        size: 30,
        color: context.color.white,
      );
    }
    return Icon(
      JSIcon.bookmarkoutlined,
      size: 30,
      color: context.color.white,
    );
  },
)

// step 3 : re-update the ValueNotifier with the emitted State
BlocListener<CoolBloc, CoolState>(
  builder: (context, state) {
    if (state is BookmarkSuccessState){
      _isBookmark.value = true;
    }    
    if (state is BookmarkSuccessState){
      _isBookmark.value = false;
    }    
  },
);
```


Tips when dealing with multiple states using Bloc :\
If there is multiple widgets in the same page with each built from different API response. Then to prevent state collision, each widget should has their own Bloc to manage their own state.



### 11.1. Bloc Event Function Naming Convention

`_on + Noun (optional) + Verb 2`

For example :

* _onLoadDataTriggered : _on + LoadData (Noun) + Triggered (Verb 2)
* _onLikePressed : _on + Like (Noun) + Pressed (Verb 2)


```dart
  on<VacancyLikePressed>(_onLikePressed);

  FutureOr<void> _onLikePressed(
    VacancyLikePressed event,
    Emitter<VacancyState> emit,
  ) async {
    
    .
    .
    .
  }


```

### 11.2. Bloc Event Naming Convention
  
  `Subject (Bloc name) + Noun (optional) + Verb 2`

For example :

* VacancyLoadDataTriggered : Vacancy + LoadData (Noun) + Triggered (Verb 2)
* VacancyLikePressed : Vacancy + Like (Noun) + Pressed (Verb 2)

```dart
class VacancyLikePressed extends VacancyEvent {
  final int vacancyId;
  final int employerId;

  const VacancyLikePressed({
    required this.vacancyId,
    required this.employerId,
  });

  @override
  List<Object?> get props => [
        vacancyId,
        employerId,
      ];

  @override
  String toString() {
    return 'VacancyLikePressed{vacancyId: $vacancyId, employerId: $employerId}';
  }
}

```

### 11.3. Bloc State Naming Convention

  `Subject (Bloc name) + Verb + State`

For example : 

* VacancyLoadDataInProgress : Vacancy (Subject) + LoadData (Verb) + InProgress (State)
* VacancyLoadDataSuccess : Vacancy (Subject) + LoadData (Verb) + Success (State)
* VacancyLoadDataFailure : Vacancy (Subject) + LoadData (Verb) + Failure (State)


## 12.  Variable Name

1. bool variable should be is + v2 (e.g., isLoaded, isSaved, isBookmarked)
 ```dart
✅ good

final isLoaded = true;
final isSaved = true;
final isBookmarked = false;
```

```dart
❌ bad

final loaded = true;
final isSave = true;
final bookmark = false;
```
2. For plurar variable add 's' or 'ies' suffix. 

```dart
✅ good

final customers = <T>[];
final products = <T>[];
final items = <T>[];
```

```dart
❌ bad

final listCustomer = <T>[];
final listProducts = <T>[];
final item = <T>[];
```

Finally, some tips on making a good variable name :
* Descriptive : Writing descriptive variable names may take more time but it will result in saving more time in the future in terms of collaboration, maintenance, and readability.
* In english : To stay consistent and and since the majority of external resources also written in english
* Pronounceable : Makes the code easy to read and discuss about
* Avoid noise words : Noise words like Data, Value, Info, Variable, Table, String, Object, etc which are used as a suffix do not offer any meaningful distinction. Noise words are redundant and should be avoided.


## 13.  Usecase or helper functions

No specific rules, just make sure to use consistent prefix. 

```dart
✅ good

getCustomers(){}
getProducts(){}
getUsers(){}
```

```dart
❌ bad

getCustomers(){}
fetchProducts(){}
takeUsers(){}
```

Common prefix with examples :
1. get — digunakan untuk mendapatkan nilai atau informasi dari suatu objek. Contoh: getName(), getAge(), getAddress(), getPhoneNumber(), getEmail()
2. set — digunakan untuk mengatur atau mengubah nilai dari suatu objek. Contoh: setName(), setAge(), setAddress(), setPhoneNumber(), setEmail()
3. is — digunakan untuk memeriksa apakah suatu objek memenuhi kondisi tertentu. Contoh: isEmpty(), isValid(), isPaid(), isCompleted(), isActive()
4. has — digunakan untuk memeriksa apakah suatu objek memiliki sesuatu. Contoh: hasPermission(), hasAccess(), hasError(), hasNext(), hasPrevious()
5. create — digunakan untuk membuat objek baru. Contoh: createAccount(), createOrder(), createInvoice(), createFile(), createDatabase()
6. delete — digunakan untuk menghapus objek. Contoh: deleteAccount(), deleteOrder(), deleteInvoice(), deleteFile(), deleteDatabase()
7. update — digunakan untuk memperbarui objek. Contoh: updateAccount(), updateOrder(), updateInvoice(), updateFile(), updateDatabase()
8. load — digunakan untuk memuat objek dari sumber eksternal. Contoh: loadAccount(), loadOrder(), loadInvoice(), loadFile(), loadDatabase()
9. save — digunakan untuk menyimpan objek ke sumber eksternal. Contoh: saveAccount(), saveOrder(), saveInvoice(), saveFile(), saveDatabase()
10. process — digunakan untuk memproses objek. Contoh: processPayment(), processOrder(), processRequest(), processData(), processInformation()
11. validate — digunakan untuk memvalidasi objek. Contoh: validateInput(), validateCredentials(), validateInformation(), validateData(), validateRequest()
12. generate — digunakan untuk menghasilkan objek baru atau informasi baru. Contoh: generateReport(), generateInvoice(), generateKey(), generateToken(), generatePassword()
13. calculate — digunakan untuk menghitung nilai dari objek. Contoh: calculateTotal(), calculateAverage(), calculateDiscount(), calculateTax(), calculateCommission()
14. convert — digunakan untuk mengubah objek ke bentuk lain. Contoh: convertCurrency(), convertUnits(), convertFormat(), convertCase(), convertEncoding()
15. sort — digunakan untuk mengurutkan objek. Contoh: sortData(), sortList(), sortArray(), sortDictionary(), sortTable()
16. filter — digunakan untuk menyaring objek. Contoh: filterData(), filterList(), filterArray(), filterDictionary(), filterTable()
17. search — digunakan untuk mencari objek. Contoh: searchData(), searchList(), searchArray(), searchDictionary(), searchTable()
18. find — digunakan untuk menemukan objek. Contoh: findData(), findList(), findArray(), findDictionary(), findTable()
19. extract — digunakan untuk mengekstrak informasi dari objek. Contoh: extractText(), extractData(), extractLink(), extractImage(), extractKey()
20. remove — digunakan untuk menghapus bagian dari objek. Contoh: removeItem(), removeData(), removeElement(), removeProperty(), removeAttribute()
21. add — digunakan untuk menambahkan bagian ke objek. Contoh: addItem(), addData(), addElement(), addProperty(), addAttribute()
22. clear — digunakan untuk membersihkan objek. Contoh: clearList(), clearArray(), clearBuffer(), clearCache(), clearLog()
23. reset — digunakan untuk mengembalikan objek ke kondisi awal. Contoh: resetPassword(), resetCounter(), resetFlag(), resetConfig(), resetOptions()
24. print — digunakan untuk mencetak informasi dari objek. Contoh: printReport(), printInvoice(), printReceipt(), printLabel(), printLog()
25. show — digunakan untuk menampilkan informasi dari objek. Contoh: showData(), showImage(), showMenu(), showMessage(), showPage()
26. hide — digunakan untuk menyembunyikan informasi dari objek. Contoh: hideData(), hideImage(), hideMenu(), hideMessage(), hidePage()
27. open — digunakan untuk membuka objek. Contoh: openFile(), openConnection(), openWindow(), openDialog(), openTab()
28. close — digunakan untuk menutup objek. Contoh: closeFile(), closeConnection(), closeWindow(), closeDialog(), closeTab()
29. execute — digunakan untuk menjalankan objek. Contoh: executeQuery(), executeCommand(), executeFunction(), executeScript(), executeProgram()
   


## 14. Page

To create a new page :
1. Use mason ‘page’ brick, it will generate a single file that already take cares of the route name, naming conventions, basic style
2. Register the page to app_route note : - there can be only 2 helper functions per page, anything beyond that should be moved into helpers folder.- any functions or variables instantiate inside the State class, use leading underscore.

## 15. Authentication Status 

To check authentication status : 
1. Instantiate authentication info with injection containerfinal _authenticationInfo = sl<AuthenticationInfo>();
2. use the ‘status’ property, it will return an AuthenticationStatus.
To handle null values : 
1. Use fallback operator ??
2. Use if conditions

## 16. Navigation

```dart
✅ good
Navigator.pushNamed(context, DestinationPage.routeName)            
// Navigate with arguments
Navigator.pushNamed(
              context,
              DestinationPage.routeName,
              arguments: {
                ‘arg1’: arg1,
                'arg2’: arg2,
              },
            );
```


```dart
❌ bad
Navigator.of(context).pushNamed(DestinationPage.routeName)
```

## 17. To add new route

Go to lib/core/routes/app_route.dart.

```dart
class AppRoute {
  static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
        .
        .
        .
      // without arguments
      case NewPage.routeName:
        return MaterialPageRoute(
          settings: settings,
          builder: (context) => const NewPage(),
        );
      // with arguments
      case OtherPage.routeName:
        final arguments = settings.arguments as Map<String, dynamic>;
        final String arg = arguments['arg'];
        return MaterialPageRoute(
          settings: settings,
          builder: (context) => OtherPage(arg: arg),
        );        
        .
        .
        .
    }
  }
}

```



# Dart Effective Style

## 18. Do name types using PascalCase

Classes, enum types, typedefs, and type parameters should capitalize the first letter of each word (including the first word), and use no separators.

```dart
✅ good

class SliderMenu { ... }

class HttpRequest { ... }

typedef Predicate<T> = bool Function(T value);
```

```dart
❌ bad

class Slider_Menu { ... }

class httpRequest { ... }

typedef predicate<T> = bool Function(T value);
```

## 19. DO name packages, directories, and source files using snake_case

```dart
✅ good

my_package
└─ lib
   └─ file_system.dart
   └─ slider_menu.dart
```

```dart
❌ bad

mypackage
└─ lib
   └─ file-system.dart
   └─ SliderMenu.dart
```


## 20. DO name import prefixes using snake_case

```dart
✅ good

import 'dart:math' as math;
import 'package:angular_components/angular_components.dart' as angular_components;
import 'package:js/js.dart' as js;
```

```dart
❌ bad

import 'dart:math' as Math;
import 'package:angular_components/angular_components.dart' as angularComponents;
import 'package:js/js.dart' as JS;
```

## 21. DO name other identifiers using camelCase

Class members, top-level definitions, variables, parameters, and named parameters should capitalize the first letter of each word except the first word, and use no separators.

```dart
✅ good

var count = 3;

HttpRequest httpRequest;

void align(bool clearItems) {
  // ...
}
```
```dart
❌ bad

var COUNT = 3;

HttpRequest http_request;

void align(bool clearItems) {
  // ...
}
```

## 22. Barrel Import

Most of the time, you only need to import `package:jobseekerapp/lib_barrels.dart;`, which located at :

```dart
my_package
└─ lib
   .
   .
   .
  └─ lib_barrels.dart
```

For example :

```dart
✅ good
import 'package:jobseekerapp/lib_barrels.dart';

class Testing{
   final A a;
   final B b;
}

```


```dart
❌ bad
import 'package:jobseekerapp/data/a.dart';
import 'package:jobseekerapp/data/b.dart';

class Testing{
   final A a;
   final B b;
}

```



## 23. Ordering

To keep the preamble of your file tidy, we have a prescribed order that directives should appear in. Each “section” should be separated by a blank line.

### 23.1 DO place dart: imports before other imports

```dart
✅ good

import 'dart:async';
import 'dart:html';

import 'package:bar/bar.dart';
import 'package:foo/foo.dart';
```

### 23.2 DO place package: imports before relative imports

```dart
✅ good

import 'src/stuff.dart';
import 'src/utils.dart';

import 'util.dart';
```

### 23.3 DO specify exports in a separate section after all imports

```dart
✅ good

import 'src/error.dart';
import 'src/foo_bar.dart';

export 'src/error.dart';
```
```dart
❌ bad

import 'src/error.dart';
export 'src/error.dart';
import 'src/foo_bar.dart';
```


## 24 Injection Container
Separating the injection container for the service locator in a separate file in the injection_file folder then defining separate functions with the aim of making it easier to read and write or add new injections in the injection container.

Named file in folder injection_file with format[feature_name]_injection.dart


```dart
✅ good

my_package
└─ lib
  └─ injection_file
    └─ vacancy_injection.dart
    └─ application_injection.dart
```

```dart
❌ bad

mypackage
└─ lib
  └─ injection_file
    └─ VacancyInjection.dart
    └─ ApplicationInjection.dart
```


Named function in folder injection_file with camel_case


```dart
✅ good

Future<void> vacancyInject() {
  // ...
}
```
```dart
❌ bad

Future<void> vacancy_inject() {
  // ...
}
```

## 25 Color

To define a color : 

Register the color value in ```color_palette.dart```

```dart
const colorPallete = ColorPalette(
  .
  .
  .
  yourColorName: Color(0xFFE4007E),
);



class ColorPalette extends ThemeExtension<ColorPalette> {
  .
  .
  .
  final Color yourColorName;

  const ColorPalette({
    .
    .
    .
    required this.yourColorName,
  });

  @override
  ThemeExtension<ColorPalette> copyWith({
    .
    .
    .
    Color? yourColorName,
  }) {
    return ColorPalette(
      .
      .
      .
      yourColorName: yourColorName ?? this.yourColorName,
    );
  }

  @override
  ThemeExtension<ColorPalette> lerp(
      covariant ThemeExtension<ColorPalette>? other, double t) {
    if (other is! ColorPalette) return this;
    return ColorPalette(
      .
      .
      .
      yourColorName: Color.lerp(yourColorName, other.yourColorName, t) ?? yourColorName,
    );
  }
}

```


To use the color : 

```dart
BoxDecoration(color: context.color.primaryPink700)
```


## 26 Show Dialog or Bottom Sheet

Define the function in the helpers class if project-level usage or in {feature_name}_helpers : 

```dart
  static showYourDialog(
    BuildContext context,
    YourCustomData data,
  ) {
    showGeneralDialog(
      context: context,
      barrierDismissible: true,      
      transitionBuilder: (context, animation, secondaryAnimation, child) {
        return ...
      },
      pageBuilder: (BuildContext context, Animation animation,
          Animation secondaryAnimation) {
        return Dialog(
          child: Container(
            decoration: BoxDecoration(
              borderRadius: BorderRadius.circular(25),
              color: Colors.transparent,
            ),
            child: ...,
          ),
        );
      },
    );
  }

```

To use the show dialog function :

```dart
Helpers.showRoundedBottomSheet(
  context,
  child: const YourBottomSheet(),
);
```

## 27 Inline Validation 

When dealing with Flutter widget styling and layout, maintaining readability and simplicity in your code is crucial, especially when validating conditions to decide which widget to display. For straightforward decisions, using a ternary operator (condition ? WidgetA : WidgetB) can keep your code clean and easy to read. However, nesting multiple conditions with ternary operators can quickly lead to complex and hard-to-read code. For instance, using more than two ternary operators in a single statement, such as (isCondition) ? WidgetA() : (isCondition2) ? WidgetB() : WidgetC() : WidgetD(), should be avoided. This pattern can be replaced with a cleaner approach by encapsulating the conditional logic within a separate function. This function can evaluate the conditions and return the appropriate widget, thereby enhancing the code's readability and maintainability.

```dart
✅ good

Widget chooseWidget(bool isCondition, bool isCondition2) {
  if (isCondition) {
    return WidgetA();
  } else if (isCondition2) {
    return WidgetB();
  } else {
    return WidgetC();
  }
}

int determineValue(bool conditionA, bool conditionB) {
  if (conditionA) {
    return 1;
  } else if (conditionB) {
    return 2;
  } else {
    return 3;
  }
}

// Usage
@override
Widget build(BuildContext context) {
  return Scaffold(
    body: Column(
      children:[
            chooseWidget(isCondition, isCondition2),
            WidgetD(a: determineValue(conditionA, conditionB));
               ]
    ),
  );
}
```




```dart
❌ bad

@override
Widget build(BuildContext context) {
  return Scaffold(
    body: Column(
      // Using nested ternary operators directly in the widget tree
      children: [
         (isCondition) ? WidgetA() : (isCondition2) ? WidgetB() : WidgetC(),
         WidgetD(a: (conditionA) ? 1 : (conditionB) ? 2 : 3)
         ]
    ),
  );
}
}
```

## 28 Add Mixpanel Event

The naming convention is `Mixpanel` + `Object` + `Verb 2` + `Event` (for example, MixpanelUserSignedUp). This folows the [official mixpanel docs](https://docs.mixpanel.com/docs/data-structure/events-and-properties) with additional suffix and prefix. The step by step to add mixpanel event is as follows :

1. Create a new file under /core/mixpanel folder (e.g., mixpanel_saved_vacancy_event.dart)
2. Create a class and implements the `MixpanelEvent` class. This class, therefore, must override 3 functions:
   * addAllProperties : To add [event properties](https://docs.mixpanel.com/docs/data-structure/events-and-properties) anywhere before the `track` function is called.
   * track : To track function after event is successfully executed
   * timedEvent : To start stopwatch and stop when `track` called. This will add `duration` property to the event.

```dart
class MixPanelSavedVacancyEvent implements MixpanelEvent {
  final String name = 'Saved Vacancy';
  final Mixpanel _mixpanel;
  final Map<String, dynamic> _properties = {};

  MixPanelJobPostedEvent(this._mixpanel);

  @override
  void addAllProperties(Map<String, dynamic> properties) {
    _properties.addAll(properties);
    log("[MIXPANEL EVENT $name] success add properties = $_properties");
  }

  @override
  void track() {
    _mixpanel.track(name, properties: _properties);
    _properties.clear();
    log("[MIXPANEL EVENT $name] success track event");
  }

  @override
  void timedEvent() {
    _mixpanel.timeEvent(name);
  }
}Ï
```
3. Don't forget to specify the name variable with the event name. Note that this name must follows the [official mixpanel docs](https://docs.mixpanel.com/docs/data-structure/events-and-properties) convention without above suffix and prefix.
```dart
class MixPanelSavedVacancyEvent implements MixpanelEvent {
  final String name = 'Saved Posted';
   .
   .
   .
```

4. You are free to add additional function to the class

```dart
class MixPanelSavedVacancyEvent implements MixpanelEvent {
   .
   .
   .

  void addFrom(OpenFromPage from, ActionButton action) {
    final concatFrom = "${from.name} ${action.name}";
    _properties.addAll({'Pre Signup Action': concatFrom});
    log("[MIXPANEL EVENT $name] success add pre-signup-action = ${json.encode(_properties)}");
  }
   .
   .
   .

}Ï
```
5. Register as singleton to injection container

```dart
   .
   .
   .
  final savedVacancyEvent = MixPanelSavedVacancyEvent(mixpanel);
  sl.registerLazySingleton(() => savedVacancyEvent);
   .
   .
   .

```

6. Add to {feature}_datasource property if the event is called on API success and specify the event with BaseUrlConfig.
```dart
   .
   .
   .
class OnBoardingRemoteDataSourceImpl implements OnBoardingRemoteDataSource {
  .
  final MixPanelUserSignedUpEvent mixPanelUserSignedUpEvent; // add here

  OnBoardingRemoteDataSourceImpl({
    .
    required this.mixPanelUserSignedUpEvent, // add here
  });
   .
   .
   .
    final response = await dio.post(
      path,
      options: Options(
        extra: {         
          BaseUrlConfig.mixPanelEvent: mixPanelUserSignedUpEvent, // add here
        },
      ),
      data: data,
    );
    if (response.statusCode == 200) {
      mixPanelUserSignedUpEvent.track(); // track called
      return RegisterResponse.fromJson(response.data);
    } else {
      throw DioException(requestOptions: RequestOptions(path: path));
    }

```

```dart
  //Remote Datasource
  sl.registerLazySingleton<OnBoardingRemoteDataSource>(
    () => OnBoardingRemoteDataSourceImpl(
      .
      mixPanelUserSignedUpEvent: sl(), // add this
    ),
  );

```


Final tips :

_"Write it fast, focus on making it works, then leave the code for a while. Revisit and you'll be surprised by how many parts of codes that can be improved."_


# References

[1] [Effective Dart: Style](https://dart.dev/effective-dart/style#do-name-extensions-using-uppercamelcase)

[2] [Flutter Handbook: Coding Style](https://infinum.com/handbook/flutter/basics/coding-style)

[3] [CapekNgoding: Clean Code](https://capekngoding.notion.site/Clean-Code-di-Dart-d1e24bd1e8904c8e9eb4b9e93636c4c0)

[4] [Bloc Naming Conventions](https://bloclibrary.dev/#/blocnamingconventions)

[5] [Machine Coding Blog](https://workat.tech/machine-coding/tutorial/writing-meaningful-variable-names-clean-code-za4m83tiesy0)

