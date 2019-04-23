##Initial example
The below code tries to log out each value in the array using the map method wtihout properly waiting for the delay in getData to run.

```javascript {cmd="node"}
function test() {
   let arr = [1, 2, 3];
   arr.map((num) => {
       let result = getData(num);
       console.log(result);
   });
   console.log('after foreach');
}

function getData(x) {
   setTimeout(() => {
       return x;
   }, 1000);
};

test();
```

##Async example
The below will successfully return a promise to ensure we are not trying to console.log the result until the await function is finished.
The issue here is that each iteration is not logging individually. Instead, all iterations are starting at once and the 5 second timers are finishing at the same time.
Although we are waiting for the response of each one indivudally, they are started at the same time so finish at the same time.

```
[9:15 PM] MrLeebo: Promise.all will run each promise it receives concurrently. if you want to run them one at a time consider (See example code at bottom)
[9:18 PM] MrLeebo: what you have in your example WILL log each as it finishes, its just that since you're starting all of the promises at the same time and they each wait 500ms they are all going to finish at the same time anyway
```

*NOTE: Promise.all also does not guarantee order is maintained.*

```javascript {cmd="node"}
async function test(){
   let arr = [1,2,3];
   await Promise.all(arr.map(async (num) => {
     let result = await getData(num);
     console.log(result);
   }));
   console.log('after foreach');
 }

function getData(x){
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(x);
      }, 500);
   });
 }

test();
```

##Final solution with "for in"
Here we have our final solution which still awaits the completion of the async function but removes the Promise.all function to ensure each console.log gets reached after the promise is returned.

When asking if this was possible with the .map method:

`[9:24 PM] MrLeebo: no. the async/await mechanism generates a function that returns a promise, so when you try to "await" your map function to stall the Promise array loop, you're actually just wrapping the actual promise inside another promise, but your Promise.all function doesn't care because either way it got a promise back`

```javascript {cmd="node"}
async function test() {
    let arr = [1, 2, 3];
    for (let num in arr) {
        let result = await getData(arr[num]);
        console.log('result', result);
    }
    console.log('after foreach');
}

function getData(x) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(x);
        }, 500);
    });
}

test();
```
**NOTE: Due to Markdown preview - embedded scripts will complete before showing final value so these examples will not correctly be shown**
