<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Electricity Bill Calculator</title>
    <style>
        body {
            font-family: 'Segoe UI', Arial, sans-serif;
            background: linear-gradient(135deg, #ff6b6b, #4ecdc4);
            min-height: 100vh;
            margin: 0;
            padding: 20px;
            display: flex;
            justify-content: center;
            align-items: center;
            flex-direction: column;
        }
        
        .main-container {
            display: flex;
            gap: 20px;
            max-width: 800px;
            width: 100%;
            flex-wrap: wrap;
            justify-content: center;
        }

        .container {
            flex: 1;
            min-width: 300px;
            background: rgba(255, 255, 255, 0.95);
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            transition: transform 0.3s;
        }

        .container:hover {
            transform: translateY(-5px);
        }

        .units-to-bill {
            border-left: 4px solid #ff6b6b;
        }

        .bill-to-units {
            border-left: 4px solid #4ecdc4;
        }

        h1 {
            color: #ff3333;
            text-align: center;
            margin-bottom: 35px;
            text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.4), 
                        -2px -2px 4px rgba(255, 255, 255, 0.3);
            font-size: 3em;
            font-weight: 900;
            line-height: 1.3;
            letter-spacing: 1px;
            background: linear-gradient(to right, #ff3333, #ff6666);
            -webkit-background-clip: text;
            background-clip: text;
            padding: 10px 20px;
            border-radius: 10px;
            animation: glow 2s infinite alternate;
        }

        @keyframes glow {
            0% { text-shadow: 3px 3px 6px rgba(0, 0, 0, 0.4), -2px -2px 4px rgba(255, 255, 255, 0.3); }
            100% { text-shadow: 3px 3px 10px rgba(255, 51, 51, 0.7), -2px -2px 6px rgba(255, 255, 255, 0.5); }
        }

        h3 {
            margin: 0 0 15px;
            color: #333;
            font-size: 1.5em;
        }

        label {
            display: block;
            margin: 10px 0 5px;
            color: #666;
            font-weight: bold;
        }

        input {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 16px;
            box-sizing: border-box;
            transition: border-color 0.3s;
        }

        input:focus {
            outline: none;
            border-color: #ff6b6b;
        }

        button {
            width: 100%;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            color: white;
            cursor: pointer;
            transition: transform 0.2s, opacity 0.2s;
        }

        .units-to-bill button {
            background: #ff6b6b;
        }

        .bill-to-units button {
            background: #4ecdc4;
        }

        button:hover {
            transform: scale(1.02);
            opacity: 0.9;
        }

        .result-box {
            margin-top: 20px;
            padding: 15px;
            border-radius: 8px;
            font-weight: bold;
            font-size: 18px;
            background: rgba(0, 0, 0, 0.05);
        }

        .footer {
            margin-top: 20px;
            color: #fff;
            font-size: 14px;
            text-align: center;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.2);
        }

        .footer a {
            color: #fff;
            text-decoration: none;
        }

        .footer a:hover {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <h1>ELECTRICITY BILL CALCULATOR – JAMMU & KASHMIR<br>CREATED BY ZAID (210425)</h1>
    <div class="main-container">
        <div class="container units-to-bill">
            <h3>Units to Bill</h3>
            <label>Enter Units Consumed:</label>
            <input type="number" id="units" placeholder="Enter Units">
            <button onclick="calculateBill()">Calculate Bill</button>
            <div class="result-box" id="billResult"></div>
        </div>

        <div class="container bill-to-units">
            <h3>Bill to Units</h3>
            <label>Enter Bill Amount:</label>
            <input type="number" id="billAmount" placeholder="Enter Amount (₹)">
            <button onclick="calculateUnits()">Estimate Units</button>
            <div class="result-box" id="unitsResult"></div>
        </div>
    </div>
    <div class="footer">
        <a href="https://www.instagram.com/icy_xaid/" target="_blank">@icy_xaid</a>
    </div>

    <script>
        function calculateBill() {
            const unitsInput = document.getElementById("units");
            const units = parseFloat(unitsInput.value);
            const billResult = document.getElementById("billResult");

            if (isNaN(units) || units < 0) {
                billResult.innerText = "Please enter valid units";
                return;
            }
            
            let bill = 8; // Fixed charge
            if (units <= 200) {
                bill += units * 2.3;
            } else if (units <= 400) {
                bill += 200 * 2.3 + (units - 200) * 4;
            } else {
                bill += 200 * 2.3 + 200 * 4 + (units - 400) * 4.35;
            }
            
            billResult.innerText = "Total Bill: ₹" + bill.toFixed(2);
        }

        function calculateUnits() {
            const billInput = document.getElementById("billAmount");
            const bill = parseFloat(billInput.value);
            const unitsResult = document.getElementById("unitsResult");

            if (isNaN(bill) || bill < 8) {
                unitsResult.innerText = "Please enter valid amount (min ₹8)";
                return;
            }
            
            let remainingBill = bill - 8; // Subtract fixed charge
            let units = 0;
            
            if (remainingBill <= 200 * 2.3) {
                units = remainingBill / 2.3;
            } else if (remainingBill <= (200 * 2.3 + 200 * 4)) {
                units = 200 + (remainingBill - 200 * 2.3) / 4;
            } else {
                units = 400 + (remainingBill - (200 * 2.3 + 200 * 4)) / 4.35;
            }
            
            unitsResult.innerText = "Estimated Units: " + Math.floor(units);
        }
    </script>
</body>
</html>
