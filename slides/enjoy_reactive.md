class: center, middle, inverse
## Enjoy Reactive
@chooblarin

2016/02/03
---

class: normal

## Are you new to Rx?

---

class: normal

## Reactive Extension

- An API for asynchronous programming with observable streams

- [http://reactivex.io/] (http://reactivex.io/)

---

## Reactive Extension

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-react/language.png)

---

## Reactive Extension (Google Trend)

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-react/google-trend.png)

---

## Everything is a stream

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-react/everything-is-a-stream.png)

---

## Stream

A sequence of numbers

```
--1--2--3--4--5--6--| // it terminates normally
```

---

Another one with characters

```
--a--b--a--a--a---d---X // it terminates with error
```

---

Sequence of button taps

```
---tap-tap-------tap---> // infinite (maybe..)
```

---

## Introduction

```
--1--2--3--4--5--6--| // it terminates normally
```

```
var stream = Rx.Observable.create(observer -> {
  for (var i = 1; i <= 6; i++) {
    observer.onNext(i);
  }
  observer.onCompleted();
});
```

or

```
var list = [1, 2, 3, 4, 5, 6]
var stream = Rx.Observable.fromArray(array);
```

---

```
--a--b--a--a--a---d---X // it terminates with error
```

```
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

```
---tap-tap-------tap---> // infinite (maybe..)
```

```
var input = $('#inbut');
var source = Rx.Observable.fromEvent(input, 'click');
```

---

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-react/everything-is-a-stream.png)

---

The code calculates the value of the `c`

```Swift:
var c: String
var a = 1
var b = 2

if a + b >= 0 {
  c = "\(a + b) is positive"
}
```

The value of c is now `"3 is positive"`.

---

We change the value of `a` to `4`,

```Swift:
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

## Observer Pattern

`a` や `b` の値の変更が起こったときは，`c` の値が書き換わって欲しい

- `a` , `b` : `Observable`
- `c` : `Observer` (Observableでもある)

---

```java:
BehaviorSubject<Integer> a = BehaviorSubject.create(1);
BehaviorSubject<Integer> b = BehaviorSubject.create(2);
BehaviorSubject<String> c = BehaviorSubject.create();

Observable.combineLatest(a, b, (a, b) -> a + b)
  .filter(x -> x >= 0)
  .map(x -> x + " is positive")
  .subscribe(c)
```

---

![](https://raw.githubusercontent.com/chooblarin/slides/gh-pages/images/enjoy-react/big_bang_theory_season2_screen03.jpg)

---

## Subject

A Subject is acts both as an observer and as an Observable.

---

## Rx in Android

- UIEvents
- ErrorHandling
- AutoComplete

---

## RxAndroid (, RxBinding, RxLifecycle)

```
RxView.clicks(button)
  .flatMap(event -> request())
  .compose(bindUntilEvent(ActivityEvent.STOP))
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe();
```

---

## ErrorHandling

```
request()
  .onErrorResumeNext(t-> empty())
  .subscribeOn(Schedulers.io())
  .observeOn(AndroidSchedulers.mainThread())
  .subscribe(list -> showList(list));
```

---

## AutoComplete Search

```java:
RxTextView.textChanges(searchEditText) // text change events
     .debounce(150, MILLISECONDS) // Reduce network requests
     .switchMap(Api::searchItems) // Request
     .retryWhen(new RetryWithConnectivity()) // Retry when network
     .subscribe(this::updateList, t->showError());
```

---

## Demo

???

---

(GitHublarin)[https://github.com/chooblarin/githublarin-android]

---

## References

- [RxMarbles.com](http://rxmarbles.com/)
- [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)
- [【翻訳】あなたが求めていたリアクティブプログラミング入門](http://ninjinkun.hatenablog.com/entry/introrxja)
- [Improving UX with RxJava](https://medium.com/@diolor/improving-ux-with-rxjava-4440a13b157f#.3mnbbd87g)
