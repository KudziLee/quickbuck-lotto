
```html
<!DOCTYPE html>
<html>
<head>
  <title>QuickBuck Lotto</title>
  <style>
    body {
      font-family: Arial;
      margin: 20px;
      background-color: #f3f9f1;
      color: #333;
      max-width: 500px;
    }
    h1 {
      color: #2e7d32;
    }
    .logo {
      width: 100%;
      text-align: center;
      margin-bottom: 10px;
    }
    .logo img {
      width: 150px;
    }
    input, button {
      padding: 10px;
      margin: 5px 0;
      width: 100%;
      box-sizing: border-box;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    .number-inputs input {
      width: 60px;
      display: inline-block;
      margin-right: 5px;
    }
    button {
      background-color: #388e3c;
      color: white;
      font-weight: bold;
      border: none;
    }
    #history {
      margin-top: 20px;
      border-top: 1px solid #ccc;
      padding-top: 10px;
    }
    .ticket {
      background: #e8f5e9;
      padding: 10px;
      margin-bottom: 10px;
      border-left: 5px solid #4caf50;
    }
    #result {
      font-weight: bold;
      margin-top: 15px;
    }
  </style>
</head>
<body><div class="logo">
    <img src="https://via.placeholder.com/150x50.png?text=QuickBuck+Lotto" alt="QuickBuck Lotto Logo">
  </div>

  <h1>QuickBuck Lotto</h1>

  <label>Enter your name:</label>
  <input type="text" id="playerName" placeholder="e.g. Tinashe" />

  <p>Enter 6 numbers between 1 and 49:</p>
  <div class="number-inputs">
    <input type="number" id="num1" min="1" max="49">
    <input type="number" id="num2" min="1" max="49">
    <input type="number" id="num3" min="1" max="49">
    <input type="number" id="num4" min="1" max="49">
    <input type="number" id="num5" min="1" max="49">
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

    function getPrize(matches) {
      if (matches === 6) return "JACKPOT! You won 100,000!";
      if (matches === 5) return "You won500!";
      if (matches === 4) return "You won 50!";
      if (matches === 3) return "You won5!";
      return "No prize this time.";
    }

    function checkLotto() {
      const name = document.getElementById("playerName").value.trim();if (name === "") {
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

      const matches = nums.filter(n => winning.includes(n)).length;
      const prize = getPrize(matches);

      const resultText = `name, you matched{matches} number(s). prize numbers:{winning.join(", ")}`;
      document.getElementById("result").innerText = resultText;

      const ticket = {
        name,
        picks: nums,
        win: winning,
        matched: matches,
        prize
      };
      history.unshift(ticket);
      updateHistory();
    }

    function updateHistory() {const container = document.getElementById("ticketList");
      container.innerHTML = "";
      history.forEach(t => {
        const div = document.createElement("div");
        div.className = "ticket";
        div.innerHTML = `
          <strong>t.name</strong><br>
          Your picks:{t.picks.join(", ")}<br>
          Winning: t.win.join(", ")<br>
          Matched:{t.matched}<br>
          Prize: ${t.prize}
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