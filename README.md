<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .calculator-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            border-radius: 20px;
            padding: 40px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            text-align: center;
            max-width: 400px;
            width: 100%;
            transition: transform 0.3s ease;
        }

        .calculator-container:hover {
            transform: translateY(-5px);
        }

        h1 {
            color: white;
            margin-bottom: 30px;
            font-size: 2.5rem;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }

        .input-group {
            margin-bottom: 20px;
        }

        label {
            display: block;
            color: white;
            margin-bottom: 8px;
            font-size: 1.1rem;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }

        input[type="number"] {
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 10px;
            font-size: 1.2rem;
            background: rgba(255, 255, 255, 0.9);
            text-align: center;
            transition: all 0.3s ease;
            outline: none;
        }

        input[type="number"]:focus {
            background: white;
            transform: scale(1.02);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        button {
            width: 100%;
            padding: 15px;
            background: linear-gradient(45deg, #ff6b6b, #ee5a24);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 1.3rem;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.3s ease;
            margin: 20px 0;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        button:hover {
            background: linear-gradient(45deg, #ee5a24, #ff6b6b);
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0, 0, 0, 0.2);
        }

        button:active {
            transform: translateY(0);
        }

        .result {
            margin-top: 20px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
            border: 1px solid rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(5px);
        }

        .result-label {
            color: white;
            font-size: 1.1rem;
            margin-bottom: 10px;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }

        .result-value {
            color: #ffd700;
            font-size: 2rem;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.8; }
        }

        .error {
            color: #ff4757;
            background: rgba(255, 71, 87, 0.1);
            border: 1px solid rgba(255, 71, 87, 0.3);
        }

        .success {
            animation: slideIn 0.5s ease-out;
        }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        @media (max-width: 480px) {
            .calculator-container {
                padding: 30px 20px;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            input[type="number"], button {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container">
        <h1>✨ Calculator</h1>
        
        <form id="calculatorForm">
            <div class="input-group">
                <label for="number1">First Number:</label>
                <input type="number" id="number1" step="any" required>
            </div>
            
            <div class="input-group">
                <label for="number2">Second Number:</label>
                <input type="number" id="number2" step="any" required>
            </div>
            
            <button type="submit">Add Numbers</button>
        </form>
        
        <div id="result" class="result" style="display: none;">
            <div class="result-label">Result:</div>
            <div id="resultValue" class="result-value">0</div>
        </div>
    </div>

    <script>
        const form = document.getElementById('calculatorForm');
        const resultDiv = document.getElementById('result');
        const resultValue = document.getElementById('resultValue');
        const number1Input = document.getElementById('number1');
        const number2Input = document.getElementById('number2');

        form.addEventListener('submit', function(e) {
            e.preventDefault();
            
            const num1 = parseFloat(number1Input.value);
            const num2 = parseFloat(number2Input.value);
            
            // Validate inputs
            if (isNaN(num1) || isNaN(num2)) {
                showResult('Please enter valid numbers', true);
                return;
            }
            
            const sum = num1 + num2;
            showResult(sum.toString(), false);
        });

        function showResult(value, isError) {
            resultValue.textContent = value;
            resultDiv.style.display = 'block';
            
            // Reset classes
            resultDiv.classList.remove('error', 'success');
            
            if (isError) {
                resultDiv.classList.add('error');
            } else {
                resultDiv.classList.add('success');
            }
        }

        // Add some interactive effects
        document.querySelectorAll('input[type="number"]').forEach(input => {
            input.addEventListener('input', function() {
                if (this.value && !isNaN(this.value)) {
                    this.style.borderLeft = '4px solid #2ecc71';
                } else {
                    this.style.borderLeft = 'none';
                }
            });
        });

        // Clear result when inputs change
        [number1Input, number2Input].forEach(input => {
            input.addEventListener('input', function() {
                if (resultDiv.style.display === 'block') {
                    resultDiv.style.display = 'none';
                }
            });
        });
    </script>
</body>
</html>
