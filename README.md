# Flutter Stream 

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
![Capture](https://user-images.githubusercontent.com/49618856/97795690-c0d2e300-1c11-11eb-8b52-8d87e398e22c.PNG)
