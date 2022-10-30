## Callback Pattern

In functional programming there is a term called **CPS** or **The Continuous Passing Style**. This is just where a callback is used to return the value rather than a function.

```javascript
function add(a, b, cb) {
	return cb(a + b);
}
```

The above a synchronous. You would then also get asynchronous CPS which just means some fomr of promise or async action occurs.

*You must be careful when mixing sync + async CPS as this can cause unexpected behaviour when it comes to code flow. Check Page **68** for reference*

## Sync vs Async I/O

When it comes to using sync I/O life file read it is generally recommended to avoid sync. As this can possibly lead to blocking the event loop. A good use case is reading config files. Or things that are required for the application to run.

### Guarentee Async Logic Flow

To create defferred execution when you have some logic that is not async (*see page 72*). You can make use of `process.nextTick(cb)`. Which defers the executions of a function until the running operation completes.

There is also `setImmediate()` and `setTimeout(cb,0)`. These have different execution times (*see page 73*)

## Observer Pattern

This is an object that that can notify its observers when change occurs. Node has a core module called the EventEmitter that provides an API to achieve this (*page 78*).
