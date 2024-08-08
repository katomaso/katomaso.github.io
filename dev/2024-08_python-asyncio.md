# Python asyncio

Let's look at a real world example of using python's `asyncio`. The biggest problem when using asyncio is how to react to 
exceptions and signals and how to shut down gracefully. The problem is that exceptions get swallowed and signals simply
shoot down your application without further notice - with exception of `SIGINT` that is translated to `KeyboardInterrupt`.
But since it is an exception - it might get swallowed.

Every coroutine - doesn't matter if you launch it via `asyncio.create_task()` or simply `await` it - it always create task
in the current loop accessible via `asyncio.all_tasks()`.


## Graceful shutdown

```python
async def coro1():
    try:
        work()
    except Exception as e:
        logger.error(e)
```

Isn't enough from two reasons. The first reason is that `asyncio.CancelledError` and `KeyboardInterrupt` don't actually inherit
from the `Exception`. They inherit from `BaseException` and it makes perfect sense because you don't want the signaling (either
canceling or interrupt) to mingle with your runtime exceptions like `IOError` and others.

Catching `KeyboardInterrupt` actually reacts on a `SIGINT` sent by the OS. So you can either catch this exception or attach a signal
handler and do the same logic what you do in the `except KeyboardInterrupt` clause. The Sig int is the only signal that you can actually
catch with accept clause. For all other signals you need to attach a signal handler.

You have two options how to gracefully shut down your application upon receiving a signal. 
 
1. have a global variable that you would loop on in all your coroutines
2. that you would loop over `asyncio.all_tasks()` and you would cancel them one by one. Beware that in this case all coroutines, that need
   to end gracefully, must have explicit `except asyncio.CanceledError` block.

The option 2 is more suitable for larger applications where you can't really make all coroutines dependent on one global kinda lock.
