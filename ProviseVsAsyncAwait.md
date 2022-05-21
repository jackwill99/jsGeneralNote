## With Async-Await

```js
function makeGetRequest(path) {
    axios.get(path).then(
        (response) => {
            var result = response.data;
            console.log('Processing Request');
            return (result);
        },
        (error) => {
            console.log(error);
        }
    );
}
function main() {
    var response = makeGetRequest('http://127.0.0.1:5000/test'); // this api will return "Statement 1" text
    console.log(response);
    console.log('Statement 2');
}
main();

/*
We want:
Processing Request
Statement 1
Statement 2
*/


/*
Result:
undifined
Statement 2
Processing Request
*/
```

**Explanation**: This happens because JavaScript called for the API, made it a separate process and then proceeded further. It found console.log(response);, but the response was unsigned by the time, so the console shows undefined. The program progressed and printed ‘Statement 2’. By this time, the API had not even reached the processing statement. This is where Javascript behaves as an asynchronous language. This problem is rectified using Async/Await functions. The corresponding code to establish a promise is given below.


```js
function makeGetRequest(path) {
    return new Promise(function (resolve, reject) {
        axios.get(path).then(
            (response) => {
                var result = response.data;
                console.log('Processing Request');
                resolve(result);
            },
                (error) => {
                reject(error);
            }
        );
    });
}
  
async function main() {
    var result = await makeGetRequest('http://127.0.0.1:5000/test');
    console.log(result.result);
    console.log('Statement 2');
}
main();


/*
Result:
Processing Request
Statement 1
Statement 2
*/
```

**Explanation**: The async function made the function to continue execution on the same thread without breaking into a separate event. The await keyword marks the point where the program has to wait for the promise to return. Hence, the program waits for the API to return response and then proceeded with the program, therefore, the output is in perfect order as required.


