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
```




