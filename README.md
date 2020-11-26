# Flutter Stream 
![04-BLoC-diagram-1](https://user-images.githubusercontent.com/49618856/100374862-92c89f00-3015-11eb-96af-238d68e0fb9f.png)

* Factory Example
``` dart
import 'dart:async';

class Cake {}

class Order {
  String type;
  Order(this.type);
}

void main() {
  
  final controller = new StreamController();
  
  final order1 = new Order('banana');
  final order2 = new Order('vanilia');
  final order3 = new Order('choclate');
  
  final baker = new StreamTransformer.fromHandlers(
    handleData:(cakeType, sink) {
      if(cakeType == 'vanilia') {
        sink.add(new Cake());
      } else {
        sink.addError('Sorry we didn\'t have this type of cake right now');
      }
    }
  );
  
  
  controller.sink.add(order1);
  controller.sink.add(order2);
  controller.sink.add(order3);
  
  controller.stream.
    map((order)=> order.type)
    .transform(baker)
    .listen(
    (cake)=> print('here you are $cake'),
    onError:(err) => print(err)
  );
}
``` 


* Button Example 
``` dart
import 'dart:html';

void main() {
  final button = querySelector('button');
  button.onClick.timeout(
    new Duration(seconds: 1),
    onTimeout: (sink) => sink.addError('you Lose!!!!')
  ).listen(
     (event){},
    onError: (err)=> print(err)
  );
}
```

* Guess Example

![Capture](https://user-images.githubusercontent.com/49618856/97795690-c0d2e300-1c11-11eb-8b52-8d87e398e22c.PNG)

```dart
import 'dart:html';

void main() {
  final ButtonElement button = querySelector('button');
  final InputElement input = querySelector('input');
  
  button.onClick
    .take(4)
    .where((event) => input.value == 'flutter')
    .listen(
      (event) => print('Congrats you got it!'),
      onDone: () => print('BAD GUESSES MAN.')
    );
}
```

* Login Validator

![Capture](https://user-images.githubusercontent.com/49618856/97796095-a18a8480-1c16-11eb-8a32-bfe0cd5a5db0.PNG)
```dart
import 'dart:html';
import 'dart:async';

void main() {
  
  final InputElement input = querySelector('input'); 
  final DivElement div = querySelector('div');
  
  final validator = new StreamTransformer
    .fromHandlers(
      handleData: (inputValue, sink) {
        if(inputValue.contains('@')) {
          sink.add(inputValue);
        } else {
          sink.addError('Enter a valid email');
        }
      } 
    );
  
  input.onInput
    .map((dynamic event) => event.target.value)
    .transform(validator)
    .listen(
      (value) => div.innerHtml = '',
      onError: (err) => div.innerHtml = err
    );
}
```
