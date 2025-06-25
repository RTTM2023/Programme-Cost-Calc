<!DOCTYPE html>
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
    }
    h1 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 33px;
      text-align: center;
      margin-bottom: 1rem;
    }
    .results-box h2 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 700;
      font-size: 28px;
      text-align: left;
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
      -webkit-appearance: none;
      -moz-appearance: none;
      appearance: none;
      background: white url('data:image/svg+xml;utf8,<svg fill="%235b01fa" height="20" viewBox="0 0 24 24" width="20" xmlns="http://www.w3.org/2000/svg"><path d="M7 10l5 5 5-5z"/></svg>') no-repeat right 1rem center;
      background-size: 1rem;
      padding-right: 2.5rem;
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
      margin-top: 0.5rem;
      display: none;
      flex-direction: column;
      gap: 0.3rem;
    }
    .results-buttons button {
      border-radius: 20px;
      padding: 0.4rem 0.8rem;
      font-size: 1rem;
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
      display: block;
      margin-top: 1rem;
      margin-bottom: 1rem;
    }
    .results-note {
      font-size: 0.9rem;
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
    }
    .form-content {
      background: white;
      padding: 2rem;
      border-radius: 20px;
      width: 400px;
      font-family: 'Montserrat', sans-serif;
    }
    .form-content h3 {
      margin-bottom: 1rem;
      font-size: 1.5rem;
    }
    .form-content input, .form-content textarea {
      width: 100%;
      padding: 0.5rem;
      margin-bottom: 1rem;
      font-family: 'Montserrat', sans-serif;
      border: 1px solid #ccc;
      border-radius: 10px;
    }
    .form-content button {
      background: #F75D36;
      color: white;
      border: none;
      padding: 0.6rem 1rem;
      border-radius: 10px;
      font-family: 'Montserrat', sans-serif;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <div class="form-modal" id="formModal">
    <div class="form-content">
      <h3>Submit Your Interest</h3>
      <form method="POST" action="https://formspree.io/f/YOUR_FORM_ID">
        <input type="text" name="name" placeholder="Your Name" required />
        <input type="email" name="email" placeholder="Your Email" required />
        <input type="text" name="company" placeholder="Company Name" required />
        <textarea name="summary" id="costSummary" rows="6" readonly></textarea>
        <button type="submit">Send</button>
      </form>
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
      if (engagement.startsWith("external")) {
        const groups = Math.ceil(learners / 25);
        const sessions = groups * 5;
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
resultsHTML += `<p class="results-note">We use this estimated pricing as a guideline for clients. This is not indicative of the final price charged, given negotiations on budget, as well as using different rollout options within a single rollout project.</p>`;

      document.getElementById("resultsContent").innerHTML = resultsHTML;
      document.getElementById("resultsButtons").style.display = "flex";
    }

    function showFormWithSummary() {
      const summary = document.getElementById("resultsContent").innerText;
      document.getElementById("costSummary").value = summary;
      document.getElementById("formModal").style.display = "flex";
    }

    document.querySelector('.submit-btn').addEventListener('click', showFormWithSummary);
  </script>
</body>
</html>
