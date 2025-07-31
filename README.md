<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator App</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
            font-family: Arial, sans-serif;
        }
        .calculator {
            background-color: #333;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            width: 300px;
        }
        .display {
            background-color: #fff;
            color: #000;
            font-size: 2em;
            padding: 10px;
            text-align: right;
            border-radius: 5px;
            margin-bottom: 10px;
            overflow: hidden;
        }
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 5px;
        }
        button {
            padding: 20px;
            font-size: 1.2em;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background-color: #666;
            color: #fff;
            transition: background-color 0.2s;
        }
        button:hover {
            background-color: #888;
        }
        .operator {
            background-color: #ff9500;
        }
        .operator:hover {
            background-color: #ffaa33;
        }
        .equals {
            background-color: #00cc00;
        }
        .equals:hover {
            background-color: #00e600;
        }
        .clear {
            background-color: #ff3333;
        }
        .clear:hover {
            background-color: #ff6666;
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display" id="display">0</div>
        <div class="buttons">
            <button class="clear" onclick="clearDisplay()">C</button>
            <button onclick="appendToDisplay('(')">(</button>
            <button onclick="appendToDisplay(')')">)</button>
            <button class="operator" onclick="appendToDisplay('%')">%</button>
            <button onclick="appendToDisplay('7')">7</button>
            <button onclick="appendToDisplay('8')">8</button>
            <button onclick="appendToDisplay('9')">9</button>
            <button class="operator" onclick="appendToDisplay('/')">÷</button>
            <button onclick="appendToDisplay('4')">4</button>
            <button onclick="appendToDisplay('5')">5</button>
            <button onclick="appendToDisplay('6')">6</button>
            <button class="operator" onclick="appendToDisplay('*')">×</button>
            <button onclick="appendToDisplay('1')">1</button>
            <button onclick="appendToDisplay('2')">2</button>
            <button onclick="appendToDisplay('3')">3</button>
            <button class="operator" onclick="appendToDisplay('-')">-</button>
            <button onclick="appendToDisplay('0')">0</button>
            <button onclick="appendToDisplay('.')">.</button>
            <button class="equals" onclick="calculate()">=</button>
            <button class="operator" onclick="appendToDisplay('+')">+</button>
        </div>
    </div>

    <script>
        let display = document.getElementById('display');
        let currentInput = '0';
        let shouldResetDisplay = false;

        function appendToDisplay(value) {
            if (shouldResetDisplay) {
                currentInput = '0';
                shouldResetDisplay = false;
            }
            if (currentInput === '0' && value !== '.') {
                currentInput = value;
            } else {
                currentInput += value;
            }
            updateDisplay();
        }

        function updateDisplay() {
            display.textContent = currentInput;
        }

        function clearDisplay() {
            currentInput = '0';
            shouldResetDisplay = false;
            updateDisplay();
        }

        function calculate() {
            try {
                let expression = currentInput.replace(/%/g, '/100');
                let result = eval(expression);
                if (isFinite(result)) {
                    currentInput = result.toString();
                } else {
                    currentInput = 'Error';
                }
                shouldResetDisplay = true;
                updateDisplay();
            } catch (e) {
                currentInput = 'Error';
                shouldResetDisplay = true;
                updateDisplay();
            }
        }

        // Keyboard support
        document.addEventListener('keydown', (event) => {
            const key = event.key;
            if (/[0-9]/.test(key)) appendToDisplay(key);
            if (key === '.') appendToDisplay('.');
            if (key === '+') appendToDisplay('+');
            if (key === '-') appendToDisplay('-');
            if (key === '*') appendToDisplay('*');
            if (key === '/') appendToDisplay('/');
            if (key === '%') appendToDisplay('%');
            if (key === 'Enter') calculate();
            if (key === 'Escape') clearDisplay();
            if (key === '(') appendToDisplay('(');
            if (key === ')') appendToDisplay(')');
            if (key === 'Backspace') {
                currentInput = currentInput.slice(0, -1) || '0';
                updateDisplay();
            }
        });
    </script>
</body>
</html>
