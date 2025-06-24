<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Programme Rollout Cost Calculator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: auto;
      padding: 2rem;
    }
    h1, h2 {
      text-align: center;
    }
    label, select, input, button {
      display: block;
      width: 100%;
      margin-top: 1rem;
    }
    .checkboxes label {
      display: block;
      margin-top: 0.5rem;
    }
    .results {
      margin-top: 2rem;
      border-top: 1px solid #ccc;
      padding-top: 1rem;
    }
    button {
      padding: 0.75rem;
      background-color: #005a9c;
      color: white;
      border: none;
      cursor: pointer;
    }
    #sessionInfo {
      margin-top: 1rem;
      font-style: italic;
    }
    #sessionDetails {
      margin-top: 0.5rem;
    }
  </style>
</head>
<body>
  <h1>Programme Rollout Cost Calculator</h1>

  <label for="learners">Number of Learners:</label>
  <input type="number" id="learners" oninput="toggleEngagementOptions()" />

  <label for="engagement">Engagement Approach:</label>
  <select id="engagement" onchange="toggleEngagementOptions()">
    <option value="elearning">Standard eLearning (Free)</option>
    <option value="team">Team Meeting Rollout (Free)</option>
    <option value="internal">Dedicated Sessions - Internal Facilitation (Free)</option>
    <option value="external_virtual">Dedicated Sessions - External (Virtual - R3,500/session)</option>
    <option value="external_inperson">Dedicated Sessions - External (In-person - R4,500/session)</option>
  </select>

  <div id="sessionInfo" style="display:none;">
    <em>We recommend group sizes of 25 for better engagement. Each group attends 5 sessions.</em>
    <div id="sessionDetails"></div>
  </div>

  <label>Optional Extras:</label>
  <div class="checkboxes">
    <label><input type="checkbox" id="kickoff" /> Kick-off Session (R15,000)</label>
    <label><input type="checkbox" id="wrapup" /> Wrap-up Session (R17,500)</label>
  </div>

  <button onclick="calculateTotal()">Calculate</button>

  <div class="results" id="results"></div>

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
        sessionDetails.innerHTML = `<p><strong>Calculation:</strong> ${learners} learners ÷ 25 = ${groups} group(s) × 5 sessions = <strong>${sessions} sessions</strong></p>`;
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
        <p><strong>Extras:</strong> R${extrasCost.toLocaleString()}</p>
        <p><strong>Total Estimated Cost:</strong> R${totalCost.toLocaleString()}</p>
      `;
    }
  </script>
</body>
</html>
