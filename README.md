```html
<!DOCTYPE html>
<html>
<head>
  <title>QuickBuck Lotto</title>
  <style>
    body {
      font-family: Arial;
      margin: 20px;
      max-width: 500px;
      line-height: 1.6;
    }
    input, button {
      padding: 8px;
      margin: 5px 0;
      width: 100%;
      box-sizing: border-box;
    }
    .number-inputs input {
      width: 60px;
      display: inline-block;
      margin-right: 5px;
    }
    #history {
      margin-top: 20px;
      border-top: 1px solid #ccc;
      padding-top: 10px;
    }
    .ticket {
      background: #f2f2f2;
      padding: 10px;
      margin-bottom: 10px;
    }
  </style>
</head>
<body>
  <h1>QuickBuck Lotto</h1>

  <label>Enter your name:</label>
  <input type="text" id="playerName" placeholder="e.g. Tinashe" />

  <p>Enter 6 numbers between 1 and 49:</p>
  <div class="number-inputs">
    <input type="number" id="num1" min="1" max="49">
    <input type="number" id="num2" min="1" max="49">
    <input type="number" id="num3" min="1" max="49">
    <input type="number" id="num4" min="1" max="49"><input type="number" id="num5" min="1" max="49">
    <input type="number" id="num6" min="1" max="49">
  </div>

  <button onclick="checkLotto()">Submit Ticket</button>
  <button onclick="resetForm()">Reset</button>

  <div id="result"></div>

  <div id="history">
    <h3>Ticket History</h3>
    <div id="ticketList"></div>
  </div>

  <script>
    let history = [];

    function checkLotto() {
      const name = document.getElementById("playerName").value.trim();
      if (name === "") {
        alert("Please enter your name.");
        return;
      }

      const nums = [
        +document.getElementById("num1").value,
        +document.getElementById("num2").value,
        +document.getElementById("num3").value,
        +document.getElementById("num4").value,
        +document.getElementById("num5").value,
        +document.getElementById("num6").value
      ];

      const unique = [...new Set(nums)];
      if (nums.includes(0) || unique.length !== 6) {
        alert("Enter 6 unique numbers between 1 and 49.");
        return;
      }

      const winning = [];
      while (winning.length < 6) {
        let n = Math.floor(Math.random() * 49) + 1;
        if (!winning.includes(n)) winning.push(n);
      }

      const matches = nums.filter(n => winning.includes(n)).length;const resultText = `name, you matched{matches} number(s). Winning numbers: winning.join(", ")`;
      document.getElementById("result").innerText = resultText;

      const ticket = 
        name,
        picks: nums,
        win: winning,
        matched: matches
      ;
      history.unshift(ticket);
      updateHistory();
    

    function updateHistory() 
      const container = document.getElementById("ticketList");
      container.innerHTML = "";
      history.forEach(t => 
        const div = document.createElement("div");
        div.className = "ticket";
        div.innerHTML = `
          <strong>{t.name}</strong><br>
          Your picks: t.picks.join(", ")<br>
          Winning:{t.win.join(", ")}<br>
          Matched: ${t.matched}
        `;
        container.appendChild(div);
      });
    }

    function resetForm() {
      document.getElementById("playerName").value = "";
      for (let i = 1; i <= 6; i++) {
        document.getElementById("num" + i).value = "";
      }
      document.getElementById("result").innerText = "";
    }
  </script>
</body>
</html>
```
