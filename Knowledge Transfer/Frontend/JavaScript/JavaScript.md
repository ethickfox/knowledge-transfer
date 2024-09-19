---
Interview graded: false
Last edited time: 2023-08-21T09:49
Needs Rework: false
Status: Not started
Topic:
  - "[[Vanilla JavaScript]]"
---
Javascript is a **single-threaded language**, meaning that just one line of code may be run at once.

**BOM** stands for _Browser Object Model_. It provides interaction with the browser. The default object of a browser is a window. So, you can call all the functions of the window by specifying the window or directly. The window object provides various properties like document, history, screen, navigator, location, innerHeight, innerWidth,

![[Untitled 92.png|Untitled 92.png]]

**DOM** stands for _Document Object Model_. A document object represents the HTML document. It can be used to access and change the content of HTML.

[**Async functions**](https://javascript.info/async-await#async-functions)

The word “async” before a function means one simple thing: a function always returns a promise. Other values are wrapped in a resolved promise automatically.

So, `async` ensures that the function returns a promise, and wraps non-promises in it. Simple enough, right? But not only that. There’s another keyword, `await`, that works only inside `async` functions, and it’s pretty cool.

The keyword `await` makes JavaScript wait until that promise settles and returns its result.

Here’s an example with a promise that resolves in 1 second:

`await` literally suspends the function execution until the promise settles, and then resumes it with the promise result. That doesn’t cost any CPU resources, because the JavaScript engine can do other jobs in the meantime: execute other scripts, handle events, etc.

It’s just a more elegant syntax of getting the promise result than `promise.then`. And, it’s easier to read and write.

  

1. `Promise` (по англ. `promise`, будем называть такой объект «промис») – это специальный объект в JavaScript, который связывает «создающий» и «потребляющий» коды вместе. В терминах нашей аналогии – это «список для подписки». «Создающий» код может выполняться сколько потребуется, чтобы получить результат, а _промис_ делает результат доступным для кода, который подписан на него, когда результат готов.

Функции-потребители могут быть зарегистрированы (подписаны) с помощью методов `.then` и `.catch`.

Первый аргумент метода `.then` – функция, которая выполняется, когда промис переходит в состояние «выполнен успешно», и получает результат.

Второй аргумент `.then` – функция, которая выполняется, когда промис переходит в состояние «выполнен с ошибкой», и получает ошибку.