JavaScript Call Stack:

The JS call stack is basically like a stack of plates or stack of cds where it is impossible to take out the last plate or the last cd without taking out the one before it. It follows the Last In First Out rule just like a stack. So basically the call stack keeps track of all the synchronous code in the project.

The call stack is a stack data structure which keeps track of all the synchronous code in the project and since there are microtasks and macrotasks JavaScript keeps track of synchronous codes to run them first using stack. After line by line interpretation of the entire project, the stack is emptied as soon as it is getting filled.

Microtasks and Macrotasks and their scheduling:

Microtasks are the task which is enqueued into a queue and then run directly after the synchronous code. It is like a small to-do list which will only take a few seconds to a few minutes to complete. Where as Macrotasks are the tasks which are a heavy burden and takes a lot of time, effort and energy to complete it. It is like those chores which we want to put off for as long as possible to save energy and time.

Microtasks are scheduled by queueMicrotasks function which instantly adds the function to the queue of the microtasks. Another way is if the solution is promised and then the .then() function is called (promise.resolve().then()). It instantly adds the process to the microtasks queue. The Macrotasks on the other hand is set by the setTimeout function. It instantly adds the function to the Macrotasks queue without looking any further into the body of the function.

Different behaviours of Promises, queueMicrotasks() and setTimeout() during execution

Promises are like objects which has a value but the value is unknown right now. It's like a placeholder value kept in place till the real value has arrived. This can be due to pending state (means the value has not been decided yet), fulfilled/resolved state (the value has been successfully retrieved/has a value) or due to a rejected state (the function has failed to retrieve a value due to an error or failure).

queueMicrotasks() function is used to add a task to the micro tasks queue instantly without any delay or thought about it. Both promises and queueMicrotasks() are for adding microtasks to the queue and neither has any priority, rather it is based on which is registered first that it adds to the queue.

The setTimeout() function is a rather odd one out of the both as it is the only one which adds the function to a macro task queue. A macro task is run only after the synchronous code and the full microtasks queue is emptied and even after that only a single macro task is completed which then reverts back to checking the micro tasks queue.

Event loops the mother who decides

Mothers make the decision when we are children and obviously it will be for our benefit even if we throw a fit to not obey it. Event loop is like a mother which decides what to run. Not saying any of the program throws a fit over who wants to go first but like event loop decides what to run. It does it in ticks which are small sessions event loop takes to run the whole thing. So there is an algorithm like structure event loop takes to decide which event to run.

Event loop loves the synchronous codes and because of that it calls and checks the call stack first to see if there are any synchronous code. The synchronous codes run as they get into the call stack. After running all the synchronous codes only the event loop decides to look into the micro tasks queue. Micro tasks have the highest priority after the synchronous code. Each micro tasks are run from the queue and until the queue is empty it keeps on popping the queue to run all the micro tasks.

Finally comes the Macro tasks which are the heavy burden like tasks which takes time effort and energy to solve. A single macro task is popped from the macro task queue and it is called into the call stack to run. After the macro task has been called and successfully executed the event loop stops looking at the macro tasks queue and goes back to check if there are nay micro tasks in queue.

Exercise 1- Microtasks vs Macrotasks

the output order would be:

1

5

3

4

2

step by step exec:

console.log('1');

console.log('5');

These are the synchronous code which runs instantly from the call stack

setTimeout(() => {

console.log('2');

}, 0);

This is the function which adds this into the macrostack which only runs after all the call stack and microtasks queue has been fully completed. Also the delay has been given as 0 ms

Promise.resolve().then(() => {

console.log('3');

});

This adds the func to the microtasks queue which then keeps it there till the call stack has been completed

queueMicrotask(() => {

console.log('4');

});

This also gets added to the microtasks queue and runs after the previous one has been dequeued from the queue

Identifying synchronous,micro and macro tasks:

1 and 5 are synchronous

3 and 4 are microtasks

5 is a macrotask

Description of how event loop runs this program:

The event loop first runs the whole program line by line checking for the synchronous, micro and macro tasks. Then the event loop runs the synchronous code form the call stack and fully finishes it. Then the micro tasks are run from the queue and finally the macro task. After the first macro task has been completed the event loop checks the call stack and the micro tasks queue and ensure there are no remaining tasks to run and then checks the macro task and since no other task is in the queue it is completed.

Exercise 2 - Nested and Sequential Tasks

The output order is:

start

end

Promise 2

setTimeout 1

Promise 1

setTimeout 2

step by step exec:

console.log('start');

console.log('end');

These are the synchronous code which are called in directly to the call stack and which are ran first.

setTimeout(() => {

console.log('setTimeout 1');

Promise.resolve().then(() => console.log('Promise 1'));

}, 0);

This is a macro task and since it's been decided it is a macro task the body is not checked till the macro task gets a chance to run. So only while running the macro task the micro task in the body is added to the micro task queue.

setTimeout(() => {

console.log('setTimeout 2');

}, 0);

This is also a macro task which is added to the queue right after the previous one. After successfully executing the first macro task the event loop checks the call stack and the micro tasks queue and then after verifying that it is empty then the second macro task is run

Promise.resolve().then(() => console.log('Promise 2'));

This is a micro task which runs instantly after the call stack is emptied as there are no other micro tasks which are in direct contact with the event loop.

Event loop tick explation:

In the first event loop tick (tick 1)

The synchronous code is run instantly and then the micro tasks queues is drained completely into the call stack. After the micro tasks queue has been drained and emptied then a single macro task is completed (that is the one with one micro task in its body. The micro task is then added to the micro task queue.) This completes the first tick

In the second event loop tick (tick 2)

The micro tasks queue is not empty. So the event loop first drains the micro tasks queue and after making sure there is no more in the micro tasks queue, The Macro tasks queue is checked and then it is run. This completes the second tick and since there are no more contents in the call stack, micro stack or macro stack this completes the event loop with just 2 ticks.

identifying synchronous, Micro and macro tasks:

Synchronous: Microtasks:

start Promise.resolve().then(() => console.log('Promise 2'));

end Promise.resolve().then(() => console.log('Promise 1'));

Macro tasks:

setTimeout(() => {

console.log('setTimeout 1');

Promise.resolve().then(() => console.log('Promise 1'));

}, 0);

setTimeout(() => {

console.log('setTimeout 2');

}, 0);

What I learned:

Execution order: Java script always runs in the order of synchronous code first, then drain the entire micro tasks queue and after making sure the entire queue has been emptied, then only it moves to the macro tasks queue. And after running a single macro task it returns back to the call stack and the micro stacks checking if anything is pending. If anything is left in the micro stack queue then it is called into the call stack to run and then again draining the whole micro stack it will finally check the macro stack.

Async behaviour: Scheduling any task or program using Promise.then() or queueMicrotask() doesn't mean they are run next or run instantly. It means that they are added to the micro tasks queue for later reference or running. Also setTimeout() doesn't mean even after a specified time it will surely run. It adds the program to the macro task queue and then the event loop after taking care of the all the available micro tasks in queue that it will do the execution.

Real Projects effect: If the micro tasks and macro tasks are not taken importantly or they are not decided according to the data in the project, if a lot of micro tasks appear continuously and being added to the micro tasks queue, the macro task may not be able to run and it can cause system errors.