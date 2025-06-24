
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
    .calculator {
      background-color: #ffffff;
      border: 2px solid #5b01fa;
      border-radius: 20px;
      padding: 2rem;
      width: 100%;
      max-width: 600px;
    }
    h1 {
      font-family: 'Unbounded', sans-serif;
      font-weight: 700;
      font-size: 2.2rem;
      text-align: center;
      margin-bottom: 0.25rem;
    }
    h1 em {
      display: block;
      font-family: 'Montserrat', sans-serif;
      font-style: italic;
      font-size: 1.25rem;
      font-weight: 500;
      margin-bottom: 1.5rem;
    }
    label {
      font-weight: 400;
      font-size: 1.2rem;
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
      font-style: italic;
      color: #a6a6a6;
      background-color: white;
    }
    input[type="number"]::placeholder, select:invalid {
      color: #a6a6a6;
      font-style: italic;
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
    }
    .results {
      margin-top: 2rem;
    }
    #sessionInfo {
      margin-top: 2rem;
    }
    #sessionInfo em {
      display: block;
      margin-bottom: 1rem;
      font-size: 1rem;
      color: #333;
    }
    #sessionDetails {
      padding-top: 1rem;
      border-top: 1px solid #ccc;
      font-weight: 500;
    }
  </style>
</head>
<body>
  <div class="calculator">
    <h1>
      Estimated Programme Rollout Cost Calculator
      <em>The Line</em>
    </h1>

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

    <div class="results" id="results"></div>
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
      return Math.min(total, 3500000); // cap at R3.5m
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

      let extrasCost = 0;
      if (kickoff) extrasCost += 15000;
      if (wrapup) extrasCost += 17500;

      const totalCost = contentCost + engagementCost + extrasCost;

      document.getElementById("results").innerHTML = `
        <h2>Estimated Rollout Cost</h2>
        <p><strong>Content Cost:</strong> R${contentCost.toLocaleString()}</p>
        ${sessions ? `<p><strong>Engagement Sessions:</strong> ${sessions} sessions = R${engagementCost.toLocaleString()}</p>` : ""}
        <p><strong>Optional Extras:</strong> R${extrasCost.toLocaleString()}</p>
        <p><strong>Total Estimated Cost:</strong> R${totalCost.toLocaleString()}</p>
      `;
    }
  </script>
</body>
</html>
