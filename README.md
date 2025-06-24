<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Estimated Programme Rollout Cost Calculator</title>
  <style>
    body {
      background-color: #F1ECEC;
      font-family: 'Montserrat', sans-serif;
      display: flex;
      justify-content: center;
      padding: 3rem;
    }
    .container {
      display: flex;
      gap: 2rem;
      align-items: flex-start;
      width: 1280px;
      height: 720px;
      margin: 0 auto;
    }
    .calculator {
      background-color: #ffffff;
      border: 2px solid #F75C36;
      border-radius: 20px;
      padding: 2rem;
      width: 700px;
      flex-shrink: 0;
    }
    .results-box {
      background-color: #F75D36;
      border-radius: 25px;
      padding: 2rem;
      color: white;
      width: 450px;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      font-size: 1.875rem;
      font-weight: 800;
    }
    h1, .results-box h2 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 800;
      font-size: 2rem;
      text-align: center;
      margin-bottom: 1rem;
    }
    label {
      font-weight: bold;
      font-size: 1.05rem;
      margin-top: 1.5rem;
      display: block;
    }
    input[type="number"], select {
      margin-top: 0.5rem;
      width: 100%;
      padding: 0.6rem 1rem;
      font-size: 1rem;
      border: 1px dashed #F87171;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
      background-color: white;
    }
    input[type="number"]::placeholder {
      color: #a6a6a6 !important;
      font-style: italic;
      font-family: 'Montserrat', sans-serif;
    }
    select:invalid {
      color: #a6a6a6 !important;
      font-style: italic;
      font-family: 'Montserrat', sans-serif;
    }
    input[type="number"]:focus, select:focus {
      outline: none;
      border: 2px solid #5b01fa;
      color: #000;
      font-style: normal;
    }
    .checkboxes label {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      margin-top: 0.5rem;
      font-weight: 400;
      font-size: 1rem;
    }
    .checkboxes input[type="checkbox"] {
      accent-color: #5b01fa;
      transform: scale(1.2);
    }
    button {
      margin-top: 2rem;
      width: 100%;
      padding: 1rem;
      font-size: 1.2rem;
      background-color: #F75D36;
      color: white;
      border: none;
      border-radius: 30px;
      cursor: pointer;
      font-family: 'Montserrat', sans-serif;
      text-align: center;
    }
    .results-line-item {
      font-size: 1.25rem;
      font-family: 'Montserrat', sans-serif;
      display: flex;
      justify-content: space-between;
      margin: 0.5rem 0;
    }
    .results-line-item span {
      font-weight: normal;
    }
    .results-line-item span:last-child {
      font-weight: bold;
    }
    .results-box .line {
      border-top: 1px dotted white;
      margin: 1rem 0;
    }
    .total-line {
      font-size: 1.4rem;
      font-weight: bold;
      display: flex;
      justify-content: space-between;
    }
    .results-buttons {
      margin-top: 1rem;
      display: none;
      flex-direction: column;
      gap: 0.5rem;
    }
    .results-buttons button {
      border-radius: 25px;
      padding: 1rem;
      font-size: 1.2rem;
      font-family: 'Montserrat', sans-serif;
      text-align: left;
      width: 100%;
    }
    .submit-btn {
      background: white;
      color: #000;
      border: 1px dashed #5b01fa;
    }
    .reset-btn {
      background: #5b01fa;
      color: white;
    }
    #sessionInfo em {
      display: block;
      margin-top: 1rem;
      margin-bottom: 1rem;
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="calculator">
      <h1>Estimated Programme Rollout Cost Calculator</h1>
      <label for="learners">Number of Employees to be Trained:</label>
      <input type="number" id="learners" oninput="toggleEngagementOptions()" placeholder="Enter total number" />
      <label for="engagement">Rollout Options:</label>
      <select id="engagement" onchange="toggleEngagementOptions()" required>
        <option value="" disabled selected hidden>Please select a rollout option from the dropdown below</option>
        <option value="elearning">Standard eLearning (Free)</option>
        <option value="team">Team Meeting Rollout (Free)</option>
        <option value="internal">Dedicated Sessions - Internal Facilitation (Free)</option>
        <option value="external_virtual">Dedicated Sessions - RTTM Facilitation: Virtual (60 min) - R3,500/session</option>
        <option value="external_inperson">Dedicated Sessions - RTTM Facilitation: In-person (60 min) - R4,500/session</option>
      </select>
      <div id="sessionInfo" style="display:none;">
        <em>We recommend group sizes of 25 people to allow for better engagement. Each group would attend 5 sessions, 1 per episode of the programme.</em>
        <div id="sessionDetails"></div>
      </div>
      <label>Optional Extras:</label>
      <div class="checkboxes">
        <label><input type="checkbox" id="kickoff" /> Kick-off Session - Virtual (45 min) - R15,000</label>
        <label><input type="checkbox" id="wrapup" /> Wrap-up Session - Virtual (60 min) - R17,500</label>
      </div>
      <button onclick="calculateTotal()">Calculate</button>
    </div>

    <div class="results-box" id="results">
      <h2>Estimated Rollout Cost</h2>
      <div id="resultsContent"></div>
      <div class="results-buttons" id="resultsButtons">
        <button class="submit-btn"> Submit Interest to RTTM</button>
        <button class="reset-btn" onclick="window.location.reload()">Reset Calculator</button>
      </div>
    </div>
  </div>

  <script>
    function toggleEngagementOptions() {
      const learners = parseInt(document.getElementById("learners").value) || 0;
      const engagement = document.getElementById("engagement").value;
      const sessionInfo = document.getElementById("sessionInfo");
      const sessionDetails = document.getElementById("sessionDetails");

      if (engagement.startsWith("external") && learners > 0) {
        const groups = Math.ceil(learners / 25);
        const sessions = groups * 5;
        sessionInfo.style.display = "block";
        sessionDetails.innerHTML = `<p><strong>Calculation:</strong> ${learners} learners ÷ 25 pax = ${groups} group(s) × 5 sessions = <strong>${sessions} sessions</strong></p>`;
      } else {
        sessionInfo.style.display = "none";
        sessionDetails.innerHTML = "";
      }
    }

    function getContentCost(learners) {
      let rate = 450;
      if (learners > 5000) rate = 300;
      else if (learners > 4000) rate = 330;
      else if (learners > 3000) rate = 360;
      else if (learners > 2000) rate = 390;
      else if (learners > 1000) rate = 420;
      const total = learners * rate;
      return Math.min(total, 3500000);
    }

    function calculateTotal() {
      const learners = parseInt(document.getElementById("learners").value) || 0;
      const engagement = document.getElementById("engagement").value;
      const kickoff = document.getElementById("kickoff").checked;
      const wrapup = document.getElementById("wrapup").checked;

      const contentCost = getContentCost(learners);

      let engagementCost = 0;
      let sessions = 0;
      if (engagement.startsWith("external")) {
        const groups = Math.ceil(learners / 25);
        sessions = groups * 5;
        const rate = engagement === "external_virtual" ? 3500 : 4500;
        engagementCost = sessions * rate;
      }

      let resultsHTML = '';
      resultsHTML += `<div class='results-line-item'><span>Content Cost:</span><span>R${contentCost.toLocaleString()}</span></div>`;

      if (engagementCost > 0) {
        resultsHTML += `<div class='results-line-item'><span>Engagement Session:</span><span>R${engagementCost.toLocaleString()}</span></div>`;
      }

      if (kickoff) {
        resultsHTML += `<div class='results-line-item'><span>Kick-off</span><span>R15,000</span></div>`;
      }
      if (wrapup) {
        resultsHTML += `<div class='results-line-item'><span>Wrap-up</span><span>R17,500</span></div>`;
      }

      const extrasCost = (kickoff ? 15000 : 0) + (wrapup ? 17500 : 0);
      const totalCost = contentCost + engagementCost + extrasCost;

      resultsHTML += `<div class="line"></div>`;
      resultsHTML += `<div class='total-line'><span>Total Estimated Cost</span><span>R${totalCost.toLocaleString()}</span></div>`;
      resultsHTML += `<div class="line"></div>`;

      document.getElementById("resultsContent").innerHTML = resultsHTML;
      document.getElementById("resultsButtons").style.display = "flex";
    }
  </script>
</body>
</html>
