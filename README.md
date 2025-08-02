---

```html
<!DOCTYPE html>
<html>
<head>
  <title>QuickBuck Lotto</title>
  <style>
    body { font-family: Arial; margin: 20px; }
    input { padding: 5px; margin: 5px; width: 60px; }
    button { padding: 8px 15px; }
    #result, #winning { margin-top: 20px; font-weight: bold; }
  </style>
</head>
<body>
  <h1>QuickBuck Lotto</h1>
  <p>Enter 6 numbers between 1 and 49:</p>

  <input type="number" id="num1" min="1" max="49">
  <input type="number" id="num2" min="1" max="49">
  <input type="number" id="num3" min="1" max="49">
  <input type="number" id="num4" min="1" max="49">
  <input type="number" id="num5" min="1" max="49">
  <input type="number" id="num6" min="1" max="49"><br>

  <button onclick="checkLotto()">Submit Ticket</button>

  <div id="winning"></div>
  <div id="result"></div>

  <script>
    function checkLotto() {
      let nums = [
        +document.getElementById('num1').value,
        +document.getElementById('num2').value,
        +document.getElementById('num3').value,
        +document.getElementById('num4').value,
        +document.getElementById('num5').value,
        +document.getElementById('num6').value
      ];

      let unique = [...new Set(nums)];if (nums.includes(0) || nums.length !== 6 || unique.length !== 6) {
        alert("Enter 6 unique numbers between 1 and 49");
        return;
      }

      let winning = [];
      while (winning.length < 6) {
        let n = Math.floor(Math.random() * 49) + 1;
        if (!winning.includes(n)) winning.push(n);
      }

      let matches = nums.filter(n => winning.includes(n)).length;

      document.getElementById('winning').innerText = "Winning numbers: " + winning.join(', ');
      document.getElementById('result').innerText = "You matched " + matches + " number(s)";
    }
  </script>
</body>
</html>
```

---
