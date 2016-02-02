class: center, middle, inverse
## Enjoy Reactive
@chooblarin

2016/02/03
---

class: normal, center, middle

# Are you new to Rx?

---
class: normal
## Reactive Extension

- An API for asynchronous programming with observable streams

- [ReactiveX](http://reactivex.io/)

---
class: inverse, center, middle
## Reactive Extension

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-reactive/languages.png)

---
class: normal, center, middle
### Reactive Extension (Google Trend)

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-reactive/google-trend.png)

---
class: inverse, center, middle
## Everything is a stream

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-reactive/everything-is-a-stream.jpg)

---
class: inverse, center, middle

# Stream!

---
class: normal
### Stream

**A Stream of Numbers**

```
--1--2--3--4--5--6--| // it terminates normally
```


**Another one with Characters**

```
--a--b--a--a--a---d---X // it terminates with error
```


**Stream of Button Taps**

```
---tap-tap-------tap---> // infinite (maybe)
```

---
class: normal
### A Stream of Numbers

```
--1--2--3--4--5--6--| // it terminates normally
```

*can be*

```javascript
var stream = Rx.Observable.create(observer -> {
  for (var i = 1; i <= 6; i++) {
    observer.onNext(i);
  }
  observer.onCompleted();
});
```

*or*

```javascript
var list = [1, 2, 3, 4, 5, 6]
var stream = Rx.Observable.fromArray(array);
```

---
class: normal
### A Stream of Characters

```
--a--b--a--a--a---d---X // it terminates with error
```

*can be*

```javascript
var stream = Rx.Observable.create(observer -> {
  observer.onNext('a');
  observer.onNext('b');
  observer.onNext('a');
  observer.onNext('a');
  observer.onNext('a');
  observer.onNext('d');
  observer.onError(new Error("Oops!!"));
});
```

---
class: normal
### A Stream of UI Events

```
---tap-tap-------tap---> // infinite (maybe)
```

*can be*

```javascript
var input = $('#inbut');
var source = Rx.Observable.fromEvent(input, 'click');
```

---
class: inverse, center, middle

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-reactive/everything-is-a-stream.jpg)

---
class: normal
### Basic Example (1)

The code calculates the value of the `c`

```Swift
var c: String
var a = 1
var b = 2

if a + b >= 0 {
  c = "\(a + b) is positive"
}
```

The value of c is now `"3 is positive"`.

---
class: normal
### Basic Example (2)

We change the value of `a` to `4`,

```Swift
var c: String
var a = 1
var b = 2

if a + b >= 0 {
  c = "\(a + b) is positive"
}

a = 4
```

`c` should be equal to `"6 is positive"`.

---
class: normal
## Observer Pattern

`a` や `b` の値の変更が起こったときは，`c` の値が書き換わって欲しい

- `a` , `b` : `Observable`
- `c` : `Observer` (Observableでもある)

---
class: normal
### Implementation with RxJava

```java
BehaviorSubject<Integer> a = BehaviorSubject.create(1);
BehaviorSubject<Integer> b = BehaviorSubject.create(2);
BehaviorSubject<String> c = BehaviorSubject.create();

Observable.combineLatest(a, b, (a, b) -> a + b)
  .filter(x -> x >= 0)
  .map(x -> x + " is positive")
  .subscribe(c)
```

---
class: inverse, center, middle

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-reactive/big_bang_theory_season2_screen03.jpg)

---
class: normal
## Subject

*A Subject is acts both as an observer and as an Observable.*

(Maybe another time...)

---
class: normal
## Rx in Android

- UIEvents
- ErrorHandling
- AutoComplete

---
class: normal
### RxAndroid (, RxBinding, RxLifecycle)

```java
RxView.clicks(button)
  .flatMap(event -> request())
  .compose(bindUntilEvent(ActivityEvent.STOP))
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe();
```

---
class: normal
## ErrorHandling

**onError**

```java
request()
  .doOnError(e -> showErrorMessage())
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe(list -> showList(list));
```

**retry**

```java
request()
  .retry()
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe(list -> showList(list));
```

---
class: normal
### AutoComplete Search

```java
RxTextView.textChanges(searchEditText)
     .debounce(150, MILLISECONDS)
     .switchMap(Api::searchItems)
     .retryWhen(new RetryWithConnectivity())
     .subscribe(this::updateList, t->showError());
```

- Reduce network requests
- Kill previous requests
- Retry mechanism

---
class: inverse, center, middle
# Demo

---
class: middle, center, inverse

[GitHublarin](https://github.com/chooblarin/githublarin-android)

---
class: middle, inverse

# Thank you!
---

## References

- [RxMarbles.com](http://rxmarbles.com/)
- [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)
- [【翻訳】あなたが求めていたリアクティブプログラミング入門](http://ninjinkun.hatenablog.com/entry/introrxja)
- [Improving UX with RxJava](https://medium.com/@diolor/improving-ux-with-rxjava-4440a13b157f#.3mnbbd87g)