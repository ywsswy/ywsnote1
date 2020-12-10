JavaScript语言的一大特点就是单线程

把所有任务分成两种，一种是同步任务（synchronous），另一种是异步任务（asynchronous）。
同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
异步任务指的是，不进入主线程、而进入”任务队列”（task queue）的任务，只有”任务队列”通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

JavaScript中，任务被分为Task（又称为MacroTask,宏任务）和MicroTask（微任务）两种。
MacroTask: script(整体代码), setTimeout, setInterval, setImmediate（node独有）, I/O, UI rendering
MicroTask: process.nextTick（node独有）, Promises, Object.observe(废弃), MutationObserver
在同一个上下文中，总的执行顺序为同步代码—>microTask—>macroTask

异步任务必须指定回调函数，当主线程开始执行异步任务，就是执行对应的回调函数。

”定时器”功能，仅仅是在基础上，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

主线程从”任务队列”中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）

Node.js的Event Loop 不同于 浏览器！