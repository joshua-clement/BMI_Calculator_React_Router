# Ex06 BMI Calculator
## Date: 20/3/26

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
```
App.css
body {
  margin: 0;
  font-family: 'Poppins', sans-serif;
}

/* Full screen center */
.main {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(135deg, #ff6a00, #ee0979, #00c6ff);
  background-size: 300% 300%;
  animation: gradientMove 8s infinite alternate;
}

/* Animated background */
@keyframes gradientMove {
  0% { background-position: left; }
  100% { background-position: right; }
}

/* Card */
.card {
  width: 350px;
  padding: 30px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(12px);
  text-align: center;
  color: white;
  box-shadow: 0 10px 40px rgba(0,0,0,0.4);
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Title */
h1 {
  margin-bottom: 20px;
}

/* Input container */
.input-box {
  width: 100%;
  margin-bottom: 15px;
  text-align: left;
}

/* Label */
label {
  font-size: 14px;
  margin-bottom: 5px;
  display: block;
}

/* Input */
input {
  width: 100%;
  padding: 12px;
  border-radius: 10px;
  border: none;
  outline: none;
  font-size: 14px;
}

/* Button */
button {
  margin-top: 15px;
  padding: 12px;
  width: 100%;
  border: none;
  border-radius: 12px;
  background: linear-gradient(to right, #00c6ff, #0072ff);
  color: white;
  font-size: 16px;
  cursor: pointer;
  transition: 0.3s;
}

button:hover {
  transform: scale(1.05);
  box-shadow: 0 5px 20px rgba(0,0,0,0.3);
}

/* Result number */
.result-number {
  font-size: 45px;
  font-weight: bold;
  margin: 10px 0;
}

/* Status */
.status {
  font-size: 20px;
  font-weight: bold;
}

/* Color classes */
.underweight {
  color: #f1c40f;
}

.normal {
  color: #2ecc71;
}

.overweight {
  color: #e67e22;
}

.obese {
  color: #e74c3c;
}
```
```
Result.js
import React from "react";
import { useLocation, useNavigate } from "react-router-dom";

function Result() {
  const location = useLocation();
  const navigate = useNavigate();

  const bmi = location.state?.bmi;

  let status = "";
  let statusClass = "";

  if (bmi < 18.5) {
    status = "Underweight";
    statusClass = "underweight";
  } else if (bmi < 24.9) {
    status = "Normal";
    statusClass = "normal";
  } else if (bmi < 29.9) {
    status = "Overweight";
    statusClass = "overweight";
  } else {
    status = "Obese";
    statusClass = "obese";
  }

  return (
    <div className="main">
      <div className="card">
        <h1>📊 Your BMI</h1>

        <div className="result-number">{bmi}</div>

        <div className={`status ${statusClass}`}>
          {status}
        </div>

        <button onClick={() => navigate("/")}>
          🔄 Try Again
        </button>
      </div>
    </div>
  );
}

export default Result;
```
```
home.js
import React, { useState } from "react";
import { useNavigate } from "react-router-dom";

function Home() {
  const [weight, setWeight] = useState("");
  const [height, setHeight] = useState("");
  const navigate = useNavigate();

  const calculateBMI = () => {
    if (!weight || !height) return;

    const h = height / 100;
    const bmi = (weight / (h * h)).toFixed(2);

    navigate("/result", { state: { bmi } });
  };

  return (
    <div className="main">
      <div className="card">
        <h1>💪 BMI Calculator</h1>

        <div className="input-box">
          <label>⚖️ Weight (kg)</label>
          <input
            type="number"
            placeholder="Enter weight"
            onChange={(e) => setWeight(e.target.value)}
          />
        </div>

        <div className="input-box">
          <label>📏 Height (cm)</label>
          <input
            type="number"
            placeholder="Enter height"
            onChange={(e) => setHeight(e.target.value)}
          />
        </div>

        <button onClick={calculateBMI}>
  🚀 Calculate BMI
</button>
      </div>
    </div>
  );
}

export default Home;
```
```
App.js
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import Result from "./pages/Result";

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/result" element={<Result />} />
      </Routes>
    </Router>
  );
}

export default App;
```


## OUTPUT
<img width="413" height="230" alt="image" src="https://github.com/user-attachments/assets/8264fb8d-0e3d-4e46-ada8-3de72e9ba619" />




## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
