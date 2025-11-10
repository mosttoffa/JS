## Callback in JavaScript

🔹 <b>Callback কী?</b> <br> 
👉 Callback হলো এমন একটি ফাংশন, যেটাকে অন্য একটি ফাংশনের আর্গুমেন্ট হিসেবে পাঠানো হয়, এবং সেই ফাংশনের ভেতরে পরে execute (চালানো) করা হয়।
```
            function greet(name, callback) {
                console.log("Hello, " + name);
                callback();   // পরে callback function টা চালানো হচ্ছে
            }
            
            function sayBye() {
                console.log("Goodbye!");
            }
            
            greet("Ajay", sayBye);
            
            Output:
                Hello, Ajay
                Goodbye!
    🧠 এখানে sayBye() ফাংশনটা greet() ফাংশনের মধ্যে callback হিসেবে পাঠানো হয়েছে।
```

🔹 <b>Callback কেন দরকার?</b> <br> 

JavaScript সাধারণভাবে এক লাইন শেষে পরের লাইন চালায় (Synchronous) কিন্তু কখনো কখনো এমন হয় — কোনো কাজ (যেমন সার্ভার থেকে ডেটা আনা) সময় নেয়। <br> 

👉 তখন আমরা চাই বাকী কোডগুলো চলুক, কিন্তু ওই কাজ শেষ হলে একটা নির্দিষ্ট ফাংশন চলুক — এই কাজটাই করে callback। <br> 

🔹 Asynchronous Callback উদাহরণ :
```
            console.log("Start");
            
            setTimeout(function() {
                console.log("Inside setTimeout");
            }, 2000);
            
            console.log("End");
            
            Output:
                    Start
                    End
                    Inside setTimeout   (২ সেকেন্ড পরে)
```
🕐 setTimeout() একটা asynchronous ফাংশন — এটা ২ সেকেন্ড পর callback ফাংশন চালায়, কিন্তু বাকি কোড অপেক্ষা করে না।  <br> 

🔹 <b>কোথায় কোথায় Callback ব্যবহার হয়? </b> <br> 
1️⃣ API / ডেটা ফেচ করার সময় :  <br> 
```
            function fetchData(callback) {
                fetch("https://jsonplaceholder.typicode.com/todos/1")
                    .then(response => response.json())
                    .then(data => callback(data))
                    .catch(error => console.error("Error:", error));
            }
            
            function handleData(data) {
                console.log("Fetched Data:", data);
            }
            
            fetchData(handleData);

    ➡️ ডেটা পাওয়ার পর handleData() callback হিসেবে চলে।
```
2️⃣ Event Listener-এ :
```
            document.getElementById("myButton").addEventListener("click", function () {
                console.log("Button clicked!");
            });

    ➡️ এখানে function() টা callback, যেটা বোতামে ক্লিক হলে চালায়।
```
3️⃣ Function Behavior পরিবর্তনের জন্য :
```
            function calc(a, b, callback) {
                return callback(a, b);
            }
            
            function add(x, y) { return x + y; }
            function mul(x, y) { return x * y; }
            
            console.log(calc(5, 3, add)); // 8
            console.log(calc(5, 3, mul)); // 15

    ➡️ একই ফাংশন calc() দিয়ে দুই কাজ — যোগ ও গুণ।
```
⚡ <b>JavaScript Callback Concepts: </b> <br> 
🧩 1. Callback with Parameters :<br> 
Callback function কেবল “চালানো” নয় — এর মধ্যে data পাঠানো যায়।
```
            function processData(data, callback) {
                let result = data.map(x => x * 2);
                callback(result);
            }
            
            function displayData(output) {
                console.log("Processed Data:", output);
            }
            
            processData([1, 2, 3, 4], displayData);
            
            Output: 
                    Processed Data: [2, 4, 6, 8]

    🧠 এখানে callback-এ result পাঠানো হয়েছে। এটা প্রমাণ করে callback শুধু action না, data flow-ও handle করে।
```
🧠 2. Callback Inside a Loop (Sequential Execution) :<br>
ধরা যাক, তুমি একাধিক asynchronous কাজ একটার পর একটা চালাতে চাও।
```
            function delayLog(number, callback) {
                setTimeout(() => {
                    console.log("Number:", number);
                    callback();
                }, 1000);
            }
            
            function runSequence(numbers) {
                let i = 0;
                function next() {
                    if (i < numbers.length) {
                        delayLog(numbers[i++], next);
                    }
                }
                next();
            }
            
            runSequence([1, 2, 3]);
            
            Output:
                Number: 1
                Number: 2
                Number: 3

    ⚙️ এখানে recursion + callback ব্যবহার করে sequence maintain করা হয়েছে।
```
🕹️ 3. Anonymous & Arrow Callbacks :<br> 

Callback আলাদা function নাম ছাড়াও লেখা যায়:

```
            setTimeout(() => console.log("Arrow Callback Example"), 1000);

এবং functional methods যেমন map(), filter(), forEach()-এ আমরা প্রায়ই callback দিই:

            let nums = [1, 2, 3, 4, 5];
            
            let doubled = nums.map(n => n * 2);
            let even = nums.filter(n => n % 2 === 0);
            
            console.log(doubled); // [2,4,6,8,10]
            console.log(even);    // [2,4]

    ➡️ এগুলোও callback function — JavaScript এর “functional programming” style।
```
⚠️ 4. Callback with Error-First Convention (Node.js style) :<br> 
Node.js এ callback সাধারণত দুইটা parameter নেয়: <br> 
👉 callback(error, result)
```
            function divide(a, b, callback) {
                if (b === 0) callback("Divide by zero!", null);
                else callback(null, a / b);
            }
            
            divide(10, 2, (err, res) => {
                if (err) return console.log("Error:", err);
                console.log("Result:", res);
            });
            
            Output:
                Result: 5

    👉 Error handling consistent থাকে — industry standard pattern (Node.js-এ খুব common)।
```
🧬 5. Callback Chaining :<br> 

তুমি একাধিক callback function ধারাবাহিকভাবে চালাতে পারো।
```
            function first(cb) {
                console.log("First");
                cb();
            }
            
            function second(cb) {
                console.log("Second");
                cb();
            }
            
            function third() {
                console.log("Third");
            }
            
            first(() => second(() => third()));
            
            Output:
                    First
                    Second
                    Third

    ➡️ দেখ, একটার কাজ শেষ হলে পরের callback চালানো হচ্ছে।
```
🧩 6. Higher-Order Callback Pattern :<br> 
Callback শুধুমাত্র async-এর জন্য না — behavior control এর জন্যও।
```
            function doOperation(a, b, operation) {
                return operation(a, b);
            }
            
            function add(x, y) { return x + y; }
            function sub(x, y) { return x - y; }
            
            console.log(doOperation(10, 5, add)); // 15
            console.log(doOperation(10, 5, sub)); // 5

    🧠 এখানে doOperation একটা higher-order function কারণ এটা আরেকটা function কে argument হিসেবে নিচ্ছে।
```
🧩 7. Callback Queue & Event Loop (Deep Concept)  :<br> 
এটা একটু tricky —
Callback asynchronous হলে JavaScript event loop ব্যবহার করে queue manage করে।
```
            console.log("A");
            
            setTimeout(() => console.log("B"), 0);
            
            Promise.resolve().then(() => console.log("C"));
            
            console.log("D");
            
            Output:
                    A
                    D
                    C
                    B
    🧠 ব্যাখ্যা:
    A, D → synchronous (immediately)
    Promise callback (C) → microtask queue (priority বেশি)
    setTimeout callback (B) → macrotask queue (priority কম)
    এটাই JS event loop-এর magic
```
🧩 8. Custom Asynchronous Callback Example (Simulated API) :
```
            function fakeApiCall(data, callback) {
                console.log("Fetching data...");
                setTimeout(() => {
                    if (data) callback(null, { status: "success", data });
                    else callback("No data found", null);
                }, 1500);
            }
            
            fakeApiCall("User Info", (err, res) => {
                if (err) console.log("Error:", err);
                else console.log("Response:", res);
            });
            
            Output:
                Fetching data...
                Response: { status: 'success', data: 'User Info' }

    🎯 একদম বাস্তব API pattern এর মতো — callback দিয়ে success/error handle।
```
⚙️ 9. Custom Retry Logic with Callback :<br> 
এখন একটা complex level callback দেখো — retry logic সহ। 
```
            function doTask(success, callback, attempt = 1) {
                console.log(`Attempt ${attempt}...`);
                setTimeout(() => {
                    if (success || attempt === 3) {
                        callback(null, "Task Done!");
                    } else {
                        console.log("Failed, retrying...");
                        doTask(success, callback, attempt + 1);
                    }
                }, 500);
            }
            
            doTask(false, (err, res) => {
                if (err) console.log("Error:", err);
                else console.log(res);
            });
            
            Output:
                    Attempt 1...
                    Failed, retrying...
                    Attempt 2...
                    Failed, retrying...
                    Attempt 3...
                    Task Done!
    ➡️ এখানে callback recursion + retry logic ব্যবহার করছে — tricky কিন্তু খুব শক্তিশালী। 
```

⚠️ <b>Callback এর সমস্যা -</b> <br> 
1️⃣ Callback Hell (Nested Callback) :<br> 

যখন callback-এর ভিতরে callback, আবার তার ভিতরে callback দিতে হয়, কোডটা অনেক জটিল হয়ে যায়।
```
            step1(() => {
                step2(() => {
                    step3(() => {
                        console.log("All steps completed");
                    });
                });
            });

    ➡️ কোড পড়া, বোঝা, ঠিক করা — খুব কঠিন হয়ে পড়ে।
``` 
2️⃣ Error Handling ঝামেলা -<br> 
Callback-এ error ধরার জন্য আলাদা parameter নিতে হয়।
```
            function divide(a, b, callback) {
                if (b === 0) {
                    callback(new Error("Cannot divide by zero"), null);
                } else {
                    callback(null, a / b);
                }
            }
            
            function result(error, output) {
                if (error) console.log("Error:", error.message);
                else console.log("Result:", output);
            }
            
            divide(10, 2, result);
            divide(10, 0, result);
```
✅ <b>Callback এর বিকল্প (Alternatives) -<b> <br> 
🔸 1. Promise — Callback Hell ঠিক করে :
```
            function step1() {
                return new Promise(resolve => {
                    setTimeout(() => {
                        console.log("Step 1 completed");
                        resolve();
                    }, 1000);
                });
            }
            
            function step2() {
                return new Promise(resolve => {
                    setTimeout(() => {
                        console.log("Step 2 completed");
                        resolve();
                    }, 1000);
                });
            }
            
            function step3() {
                return new Promise(resolve => {
                    setTimeout(() => {
                        console.log("Step 3 completed");
                        resolve();
                    }, 1000);
                });
            }
            
            step1()
                .then(step2)
                .then(step3)
                .then(() => console.log("All steps completed"));

    ➡️ .then() চেইন করে কোড সুন্দরভাবে লেখা যায়।
```
🔸 2. Async / Await — আরও সহজ সিনট্যাক্স :
```
            async function processSteps() {
                await step1();
                await step2();
                await step3();
                console.log("All steps completed");
            }
            
            processSteps();

    ➡️ কোড দেখতে synchronous মনে হয়, কিন্তু async কাজ করে।
```
⚙️ <b>কখন Callback ব্যবহার করবে / করবে না? </b> 
```
| ব্যবহার করো যখন                 | ব্যবহার না করো যখন                   |
| ------------------------------  | ------------------------------------ |
| Event listener বা button click  | অনেক nested callback লাগে            |
| File পড়া, API request          | Error handle করা কঠিন হয়             |
| Higher-order function দরকার    | Promise বা async/await সহজ বিকল্প হয়   |
```

🧭 <b>Summary — এখন তুমি জানো:</b> <br> 

✅ Callback কীভাবে কাজ করে <br>
✅ কীভাবে data pass হয় <br> 
✅ কীভাবে loop / recursion দিয়ে sequence manage হয় <br> 
✅ Error-first convention (Node.js style) <br> 
✅ Event loop priority (Microtask vs Macrotask) <br>
✅ Real-world async pattern (API, retry, chaining) <br>


