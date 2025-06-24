
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
      width: 650px;
      flex-shrink: 0;
    }
    .results-box {
      background-color: #F75D36;
      border-radius: 25px;
      padding: 2rem;
      color: white;
      width: 480px;
      display: flex;
      flex-direction: column;
      justify-content: flex-start;
    }
    h1 {
      font-family: 'Montserrat', sans-serif;
      font-weight: 800;
      font-size: 2.06rem;
      text-align: center;
      margin-bottom: 1.5rem;
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
    }
    .results-box h2 {
      font-size: 1.9rem;
      margin-bottom: 1rem;
      font-family: 'Montserrat', sans-serif;
      font-weight: 800;
    }
    .results-line-item {
      font-size: 1.1rem;
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
      font-size: 1.3rem;
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
      padding: 0.65rem 1rem;
      font-size: 1.15rem;
      font-family: 'Montserrat', sans-serif;
      text-align: left;
    }
    .submit-btn {
      background: white;
      color: #000;
      border: 1px dashed #5b01fa;
    }
    .submit-btn::after {
      content: '\2713';
      float: right;
      margin-left: 10px;
      color: #5b01fa;
      font-weight: bold;
    }
    .reset-btn {
      background: #5b01fa;
      color: white;
    }
    #sessionDetails {
      margin-top: 1rem;
    }
  </style>
</head>
<body>
  <!-- Body content remains unchanged -->
</body>
</html>
