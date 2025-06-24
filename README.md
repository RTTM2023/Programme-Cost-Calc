<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Estimated Programme Rollout Cost Calculator</title>
  <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;800&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Montserrat', sans-serif;
      background-color: #f1ecec;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .container {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      gap: 40px;
      width: 1280px;
    }

    .calculator {
      background-color: white;
      border: 3px solid #F75C36;
      padding: 30px;
      width: 650px;
      border-radius: 20px;
    }

    .calculator h1 {
      font-size: 33px;
      font-weight: 800;
      margin-bottom: 30px;
    }

    label {
      display: block;
      margin: 20px 0 8px;
      font-size: 26px;
      font-weight: bold;
    }

    input[type="number"],
    select {
      width: 100%;
      padding: 10px;
      font-size: 23px;
      font-style: italic;
      font-family: 'Montserrat', sans-serif;
      color: #a6a6a6;
      border: 2px dotted #ccc;
      border-radius: 5px;
    }

    input[type="number"]:focus,
    select:focus {
      outline: none;
      border-color: #5b01fa;
      color: black;
    }

    .checkboxes label {
      display: block;
      margin: 10px 0;
      font-size: 23px;
      font-weight: normal;
    }

    button {
      margin-top: 25px;
      padding: 10px 20px;
      font-size: 20px;
      background-color: #5b01fa;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }

    .results-box {
      background-color: white;
      padding: 30px;
      border-radius: 20px;
      width: 500px;
      min-height: 400px;
    }

    .results-box h2 {
      font-size: 30px;
      font-weight: 800;
      margin-bottom: 20px;
    }

    .results-line-item {
      display: flex;
      justify-content: space-between;
      font-size: 20px;
      font-weight: 400;
      margin: 10px 0;
    }

    .results-line-item span:last-child {
      font-weight: 700;
    }

    .line {
      border-top: 2px dotted #ccc;
      margin: 15px 0;
    }

    .total-line {
      display: flex;
      justify-content: space-between;
      font-size: 23px;
      font-weight: 800;
    }

    .results-buttons {
      margin-top: 25px;
      display: none;
      flex-direction: column;
      gap: 10px;
    }

    .submit-btn,
    .reset-btn {
      background-color: #5b01fa;
      color: white;
      font-size: 23px;
      padding: 10px 20px;
      border-radius: 5px;
      width: fit-content;
      text-align: left;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="calculator">
      <h1>Estimated Programme Rollout Cost Calculator</h1>

      <label for="employees">Number of Employees to be Trained:</label>
      <input type="number" id="employees" placeholder="Enter total number" />

      <label for="rollout">Rollout Options:</label>
      <select id="rollout">
        <option value="" disabled selected>Please select a rollout option from the dropdown below</option>
        <option value="standard">Standard eLearning (Free)</option>
        <option value="team">Team Meeting Rollout (Free)</option>
        <option value="internal">Dedicated Sessions - Internal Facilitation (Free)</option>
        <option value="external-virtual">Dedicated Sessions - RTTM Facilitation: Virtual (R3,500/session)</option>
        <option value="external-inperson">Dedicated Sessions - RTTM Facilitation: In-person (R4,500/session)</option>
      </select>

      <div id="sessionDetails" style="display: none;">
        <div style="height: 1rem;"></div>
        <p>We recommend group sizes of 25 people to allow for better engagement. Each group would attend 5 sessions, 1 per episode of the programme.</p>
        <div style="height: 1rem;"></div>
        <p id="sessionCalc"></p>
      </div>

      <label>Optional Extras:</label>
      <div class="checkboxes">
        <label><input type="checkbox" id="kickoff" /> Kick-off Session - Virtual (45 min) - R15,000</label>
        <label><input type="checkbox" id="wrapup" /> Wrap-up Session - Virtual (60 min) - R17,500</label>
      </div>

      <button onclick="calculateCost()">Calculate</button>
    </div>

    <div class="results-box">
      <h2>Estimated Rollout Cost</h2>
      <div id="results"></div>
      <div class="results-buttons" id="actionButtons">
        <button class="submit-btn">✔ Submit Interest to RTTM</button>
        <button class="reset-btn" onclick="window.location.reload()">Reset Calculator</button>
      </div>
    </div>
  </div>

  <script>
    function calculateCost() {
      const employees = parseInt(document.getElementById('employees').value);
      const rollout = document.getElementById('rollout').value;
      const kickoff = document.getElementById('kickoff').checked;
      const wrapup = document.getElementById('wrapup').checked;
      const sessionDetails = document.getElementById('sessionDetails');
      const sessionCalc = document.getElementById('sessionCalc');
      const results = document.getElementById('results');
      const actionButtons = document.getElementById('actionButtons');

      if (!employees || !rollout) return;

      let pricePerLearner = 0;
      if (employees <= 1000) pricePerLearner = 450;
      else if (employees <= 2000) pricePerLearner = 420;
      else if (employees <= 3000) pricePerLearner = 390;
      else if (employees <= 4000) pricePerLearner = 360;
      else if (employees <= 5000) pricePerLearner = 330;
      else pricePerLearner = 300;

      let contentCost = pricePerLearner * employees;
      if (contentCost > 3500000) contentCost = 3500000;

      let engagementCost = 0;
      if (rollout.includes("external")) {
        const groups = Math.ceil(employees / 25);
        const totalSessions = groups * 5;
        const sessionRate = rollout === "external-virtual" ? 3500 : 4500;
        engagementCost = totalSessions * sessionRate;
        sessionCalc.textContent = `${employees} learners ÷ 25 pax = ${groups} group(s) × 5 sessions = ${totalSessions} sessions`;
        sessionDetails.style.display = 'block';
      } else {
        sessionDetails.style.display = 'none';
      }

      let extrasCost = 0;
      const extras = [];
      if (kickoff) {
        extras.push(['Kick-off Session', 15000]);
        extrasCost += 15000;
      }
      if (wrapup) {
        extras.push(['Wrap-up Session', 17500]);
        extrasCost += 17500;
      }

      const total = contentCost + engagementCost + extrasCost;

      results.innerHTML = '';
      results.innerHTML += `<div class='results-line-item'><span>Content Cost:</span><span>R${contentCost.toLocaleString()}</span></div>`;
      if (engagementCost > 0) results.innerHTML += `<div class='results-line-item'><span>Engagement Sessions:</span><span>R${engagementCost.toLocaleString()}</span></div>`;
      extras.forEach(([name, cost]) => {
        results.innerHTML += `<div class='results-line-item'><span>${name}:</span><span>R${cost.toLocaleString()}</span></div>`;
      });
      results.innerHTML += `<div class='line'></div>`;
      results.innerHTML += `<div class='total-line'><span>Total Estimated Cost:</span><span>R${total.toLocaleString()}</span></div>`;
      results.innerHTML += `<div class='line'></div>`;

      actionButtons.style.display = 'flex';
    }
  </script>
</body>
</html>
