<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Estimated Programme Rollout Cost Calculator</title>
  <style>
    body {
      background-color: transparent;
      font-family: 'Montserrat', sans-serif;
      padding: 0rem;
      margin: 0;
      overflow-x: hidden;
    }

    .container {
      display: flex;
      flex-direction: column;
      align-items: flex-start;
      width: 100%;
      max-width: 1600px;
      margin: 0 auto;
      gap: 2rem;
      box-sizing: border-box;
      padding-left: 0;
      padding-right: 0;
    }

    @media (min-width: 1024px) {
      .container {
        flex-direction: row;
        justify-content: space-between;
        align-items: flex-start;
      }
    }

    .calculator {
      background-color: #ffffff;
      border: 2px solid #F75C36;
      border-radius: 20px;
      padding: 2rem;
      width: 1025px;
      box-sizing: border-box;
    }

    .results-box {
      background-color: #F75D36;
      border-radius: 25px;
      padding: 2rem;
      color: white;
      width: 550px;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
      box-sizing: border-box;
    }

    h1 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 33px;
      text-align: center;
      margin-bottom: 1rem;
      display: none;
    }

    .results-box h2 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 18px;
      text-align: left;
      margin-bottom: 1rem;
    }

    label {
      font-weight: bold;
      font-size: 1.05rem;
      margin-top: 1.5rem;
      display: block;
    }

    select {
      margin-top: 0.5rem;
      width: 100%;
      padding: 0.6rem 1rem;
      font-size: 1rem;
      border: 1px dashed #F87171;
      border-radius: 30px;
      font-family: 'Montserrat', sans-serif;
      background-color: white;
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      background: white url('data:image/svg+xml;utf8,<svg fill="%235b01fa" height="20" viewBox="0 0 24 24" width="20" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>') no-repeat right 1rem center;
      background-size: 1rem;
      padding-right: 2.5rem;
    }

    input[type="number"] {
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

    input[type="number"]:focus,
    select:focus {
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
      font-size: 0.8rem;
      font-family: 'Montserrat', sans-serif;
      display: flex;
      justify-content: space-between;
      margin: 0.5rem 0;
    }

    .results-line-item span:last-child {
      font-weight: bold;
    }

    .results-box .line {
      border-top: 1px dotted white;
      margin: 1rem 0;
    }

    .total-line {
      font-size: 1rem;
      font-weight: bold;
      display: flex;
      justify-content: space-between;
    }

    .results-buttons {
      margin-top: 0.5rem;
      display: none;
      flex-direction: column;
      gap: 0.05rem !important;
    }

    .results-buttons button {
      border-radius: 20px;
      padding: 0.4rem 0.8rem;
      font-size: 0.8rem;
      font-family: 'Montserrat', sans-serif;
      font-weight: 400;
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
      text-align: center;
      display: block;
      margin-top: 1rem;
      margin-bottom: 1rem;
    }

    .results-note {
      font-size: 0.7rem;
      margin-top: 1rem;
      font-family: 'Montserrat', sans-serif;
    }

    .form-modal {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.5);
      justify-content: center;
      align-items: center;
      z-index: 1000;
    }

    .form-content {
      background: white;
      padding: 2rem;
      border-radius: 20px;
      width: 90%;
      max-width: 400px;
      font-family: 'Montserrat', sans-serif;
    }

    .form-content h3 {
      margin-bottom: 1rem;
      font-size: 1.5rem;
    }

    .form-content input,
    .form-content textarea {
      width: 100%;
      padding: 0.5rem;
      margin-bottom: 1rem;
      font-family: 'Montserrat', sans-serif;
      border: 1px solid #ccc;
      border-radius: 10px;
    }

.form-content button[type="submit"] {
  background: #F75D36;
  color: white;
  border: none;
  padding: 0.6rem 1rem;
  border-radius: 10px;
  font-family: 'Montserrat', sans-serif;
  cursor: pointer;
}

.close-form {
  position: absolute;
  top: 1rem;
  right: 1rem;
  font-size: 1.25rem;
  cursor: pointer;
  background: none;
  border: none;
  color: #f05824;
  font-weight: bold;
  z-index: 10;
}

.calculation-line {
  text-align: center;
}
  </style>
</head>
<body>
   <div class="container">
    <div class="calculator">
      <h1>Estimated Programme Rollout Cost Calculator</h1>

      <!-- Programme selection dropdown -->
      <label for="programme">Select the Training Programme:</label>
      <select id="programme" onchange="toggleProgramme()" required>
        <option value="" disabled selected hidden>Please select a training programme.</option>
        <option value="theline">The Line (Sexual Harassment)</option>
        <option value="chapter1">The Inclusion Challenge, Chapter 1 (DEIB)</option>
        <option value="chapter2">The Inclusion Challenge, Chapter 2 (DEIB)</option>
      </select>

      <label for="learners">Number of Employees to be Trained:</label>
      <input type="number" id="learners" oninput="toggleEngagementOptions()" placeholder="Enter total number." />

      <label for="engagement">Rollout Options:</label>
      <select id="engagement" onchange="toggleEngagementOptions()" required>
        <option value="" disabled selected hidden>Please select a rollout option.</option>
        <option value="elearning">Standard eLearning - No Engagement (No extra cost)</option>
        <option value="team">Team Meeting Discussion (No extra cost)</option>
        <option value="internal">Dedicated Sessions - Internal Facilitator (No extra cost)</option>
        <option value="external_virtual">Dedicated Sessions - RTTM Facilitator: Virtual (60 min) - R3,500/session</option>
        <option value="external_inperson">Dedicated Sessions - RTTM Facilitator: In-person (60 min) - R4,500/session</option>
      </select>

      <div id="sessionInfo" style="display:none;">
        <em>We recommend groups of 25 people for optimal engagement. Each group will watch 1 episode per session, followed by a discussion. Therefore, the number of sessions per group will match the number of episodes in the programme.</em>
        <div id="sessionDetails"></div>
      </div>

      <label>Optional Extras:</label>
      <div class="checkboxes">
        <label><input type="checkbox" id="kickoff" /> Kick-off Session - Virtual (45 min) - R15,000</label>
        <label><input type="checkbox" id="wrapup" /> Wrap-up Session - Virtual (60 min) - R17,500</label>
      </div>

      <button onclick="calculateTotal()">Calculate</button>
    </div>

    <div class="results-box">
      <h2>Estimated Rollout Cost</h2>
      <div id="resultsContent"></div>
      <div class="results-buttons" id="resultsButtons">
        <button class="submit-btn">Submit Interest to RTTM</button>
        <button class="reset-btn" onclick="window.location.reload()">Reset Calculator</button>
      </div>
    </div>
  </div>

<div class="form-modal" id="formModal">
<div class="form-content">
  <h3>Submit Your Interest</h3>
  <form method="POST" action="https://formspree.io/f/YOUR_FORM_ID">
    <input type="text" name="name" placeholder="Your Name" required />
    <input type="email" name="email" placeholder="Your Email" required />
    <input type="text" name="company" placeholder="Company Name" required />
    <input type="text" name="programme" id="formProgramme" placeholder="Programme" readonly />
    <input type="text" name="headcount" id="formHeadcount" placeholder="Total Headcount" readonly />
    <textarea name="summary" id="costSummary" rows="6" readonly></textarea>
    <button type="submit">Send</button>
  </form>
    </div>
  </div>

  <script>
    function toggleProgramme() {
      toggleEngagementOptions();
    }

    function toggleEngagementOptions() {
      const learners = parseInt(document.getElementById("learners").value) || 0;
      const engagement = document.getElementById("engagement").value;
      const programme = document.getElementById("programme").value;
      const sessionInfo = document.getElementById("sessionInfo");
      const sessionDetails = document.getElementById("sessionDetails");

      const episodes = programme === "chapter1" ? 7 : programme === "chapter2" ? 5 : 5;

      if (engagement.startsWith("external") && learners > 0) {
        const groups = Math.ceil(learners / 25);
        const sessions = groups * episodes;
        sessionInfo.style.display = "block";
        sessionDetails.innerHTML = `<p class="calculation-line"><strong>Calculation:</strong> ${learners} learners ÷ 25 pax = ${groups} group(s) × 5 sessions = <strong>${sessions} sessions</strong></p>`;
      } else {
        sessionInfo.style.display = "none";
        sessionDetails.innerHTML = "";
      }
    }

    function getContentCost(learners, programme) {
      let rate = 450, cap = 3500000;
      if (programme === "chapter1" || programme === "chapter2") {
        cap = 2000000;
        if (learners > 5000) rate = 300;
        else if (learners > 4000) rate = 375;
        else if (learners > 3000) rate = 400;
        else if (learners > 2000) rate = 425;
        else if (learners > 1000) rate = 450;
        else rate = 500;
      }
      const total = learners * rate;
      return Math.min(total, cap);
    }

    function calculateTotal() {
      const learners = parseInt(document.getElementById("learners").value) || 0;
      const engagement = document.getElementById("engagement").value;
      const programme = document.getElementById("programme").value;
      const kickoff = document.getElementById("kickoff").checked;
      const wrapup = document.getElementById("wrapup").checked;

      const contentCost = getContentCost(learners, programme);

      let engagementCost = 0;
      if (engagement.startsWith("external")) {
        const episodes = programme === "chapter1" ? 7 : programme === "chapter2" ? 5 : 5;
        const groups = Math.ceil(learners / 25);
        const sessions = groups * episodes;
        const rate = engagement === "external_virtual" ? 3500 : 4500;
        engagementCost = sessions * rate;
      }

      let resultsHTML = '';
      resultsHTML += `<div class='results-line-item'><span>Content Cost:</span><span>R${contentCost.toLocaleString()}</span></div>`;

      if (engagementCost > 0) {
        resultsHTML += `<div class='results-line-item'><span>Facilitation Cost:</span><span>R${engagementCost.toLocaleString()}</span></div>`;
      }

      if (kickoff) {
        resultsHTML += `<div class='results-line-item'><span>Kick-off Cost:</span><span>R15,000</span></div>`;
      }
      if (wrapup) {
        resultsHTML += `<div class='results-line-item'><span>Wrap-up Cost:</span><span>R17,500</span></div>`;
      }

      const extrasCost = (kickoff ? 15000 : 0) + (wrapup ? 17500 : 0);
      const totalCost = contentCost + engagementCost + extrasCost;

      resultsHTML += `<div class="line"></div>`;
      resultsHTML += `<div class='total-line'><span>Total Estimated Cost</span><span>R${totalCost.toLocaleString()}</span></div>`;
      resultsHTML += `<div class="line"></div>`;
      resultsHTML += `<p class="results-note">This estimated pricing is intended as a guideline only. Final costs may vary based on budget negotiations and the use of different rollout options within the same project.</p>`;

      document.getElementById("resultsContent").innerHTML = resultsHTML;
      document.getElementById("resultsButtons").style.display = "flex";
    }

function showFormWithSummary() {
  const summary = document.getElementById("resultsContent").innerText;
  const programme = document.getElementById("programme").options[document.getElementById("programme").selectedIndex].text;
  const headcount = document.getElementById("learners").value;

  document.getElementById("formProgramme").value = programme;
  document.getElementById("formHeadcount").value = headcount;
  document.getElementById("costSummary").value = summary;
  document.getElementById("formModal").style.display = "flex";
}

    document.querySelector('.submit-btn').addEventListener('click', showFormWithSummary);
  document.getElementById("formModal").addEventListener("click", function (event) {
    const formContent = document.querySelector(".form-content");
    if (!formContent.contains(event.target)) {
      this.style.display = "none";
    }
  });
  </script>
</body>
</html>
