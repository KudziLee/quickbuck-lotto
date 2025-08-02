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
      font-weight: 900;
      font-size: 36px;
      text-align: center;
      margin-bottom: 10px;
    }
    .logo {
      text-align: center;
      font-size: 40px;
      font-weight: 900;
      color: #4caf50;
      text-shadow: 1px 1px 2px #b28900;
      margin-bottom: 20px;
      letter-spacing: 3px;
      user-select: none;
    }
    input, button {
      padding: 10px;
      margin: 5px 0;
      width: 100%;
      box-sizing: border-box;
      border: 1px solid #ccc;
      border-radius: 4px;}
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
      white-space: pre-line;
    }
  </style>
</head>
<body>
  <div class="logo">QuickBuck Lotto</div>

  <h1>Welcome to QuickBuck Lotto</h1>

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
  <button onclick="resetForm()">Reset</button><div id="result"></div>

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
      }const matches = nums.filter(n => winning.includes(n)).length;
      const prize = getPrize(matches);

      const resultText = `name, you matched{matches} number(s). prize
Winning numbers:{winning.join(", ")}`;
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

    function updateHistory() {
      const container = document.getElementById("ticketList");
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

*server.js* (Node.js + Express)

“`js
const express = require('express');
const fs = require('fs');
const cors = require('cors');
const app = express();
const PORT = 3000;

app.use(cors());
app.use(express.json());

const TICKETS_FILE = './tickets.json';

// Load tickets from file or start empty
let tickets = [];
if (fs.existsSync(TICKETS_FILE)) 
  tickets = JSON.parse(fs.readFileSync(TICKETS_FILE));


// Save tickets to file
function saveTickets() 
  fs.writeFileSync(TICKETS_FILE, JSON.stringify(tickets, null, 2));


// Get all tickets
app.get('/tickets', (req, res) => 
  res.json(tickets);
);

// Add new ticket
app.post('/tickets', (req, res) => 
  const  name, picks, win, matched, prize  = req.body;
  if (!name || !picks || !win || matched === undefined || !prize) 
    return res.status(400).json( error: "Missing fields" );
  
  tickets.unshift( name, picks, win, matched, prize, id: Date.now() );
  saveTickets();
  res.json( message: "Ticket saved" );
);

app.listen(PORT, () => 
  console.log(`Server running on http://localhost:{PORT}`);
});
```

---

Step 3: `tickets.json` (start empty)

```json
[]
```