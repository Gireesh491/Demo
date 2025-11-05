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
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        :root {
            --primary-color: #4a6fff;
            --secondary-color: #344dc0;
            --accent-color: #ff6b6b;
            --background-color: #f5f5f5;
            --text-color: #333;
            --display-bg: #fff;
            --button-hover: #3a55c8;
            --button-active: #2c3da9;
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        body {
            background-color: var(--background-color);
            color: var(--text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            padding: 20px;
        }
        .calculator-container {
            background-color: white;
            border-radius: 16px;
            box-shadow: var(--shadow);
            width: 100%;
            max-width: 350px;
            overflow: hidden;
        }
        .calculator-header {
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            text-align: center;
            padding: 20px;
            font-size: 24px;
            font-weight: 600;
        }
        .calculator-display {
            padding: 20px;
            text-align: right;
            background-color: var(--display-bg);
            min-height: 60px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: flex-end;
            position: relative;
        }
        .previous-operand {
            color: #888;
            font-size: 18px;
            min-height: 24px;
            margin-bottom: 5px;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        .current-operand {
            font-size: 36px;
            font-weight: 600;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        .calculator-buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background-color: #eee;
        }
        .calculator-button {
            border: none;
            outline: none;
            cursor: pointer;
            font-size: 24px;
            padding: 20px;
            transition: background 0.2s ease;
        }
        .calculator-button:active {
            transform: scale(0.95);
            box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        .calculator-button:hover {
            background-color: var(--button-hover);
        }
        .number {
            background-color: white;
            color: var(--text-color);
        }
        .operator {
            background-color: var(--primary-color);
            color: white;
        }
        .special {
            background-color: var(--secondary-color);
            color: white;
        }
        .equals {
            background-color: var(--accent-color);
            color: white;
            grid-column: span 2;
        }
        @media (max-width: 480px) {
            .calculator-container {
                max-width: 100%;
            }
            .calculator-button {
                padding: 15px;
                font-size: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container">
        <div class="calculator-header">
            Simple Calculator
        </div>
        <div class="calculator-display">
            <div class="previous-operand"></div>
            <div class="current-operand">0</div>
        </div>
        <div class="calculator-buttons">
            <button class="calculator-button special" id="clear">C</button>
            <button class="calculator-button operator" data-operator="÷">÷</button>
            <button class="calculator-button operator" data-operator="*">×</button>
            <button class="calculator-button operator" data-operator="%">%</button>
            <button class="calculator-button number" data-number="7">7</button>
            <button class="calculator-button number" data-number="8">8</button>
            <button class="calculator-button number" data-number="9">9</button>
            <button class="calculator-button operator" data-operator="-">-</button>
            <button class="calculator-button number" data-number="4">4</button>
            <button class="calculator-button number" data-number="5">5</button>
            <button class="calculator-button number" data-number="6">6</button>
            <button class="calculator-button operator" data-operator="+">+</button>
            <button class="calculator-button number" data-number="1">1</button>
            <button class="calculator-button number" data-number="2">2</button>
            <button class="calculator-button number" data-number="3">3</button>
            <button class="calculator-button equals" id="equals">=</button>
            <button class="calculator-button number" data-number="0">0</button>
            <button class="calculator-button number" data-number=".">.</button>
        </div>
    </div>
    <script>
        const prevDisplay = document.querySelector('.previous-operand');
        const currDisplay = document.querySelector('.current-operand');
        let current = '0';
        let previous = '';
        let operator = null;
        let justCalculated = false;

        function updateDisplay() {
            currDisplay.textContent = current;
            prevDisplay.textContent = operator ? previous + ' ' + operator : '';
        }
        function inputNumber(num) {
            if (justCalculated) {
                current = num === '.' ? '0.' : num;
                justCalculated = false;
            } else {
                if (num === '.') {
                    if (!current.includes('.')) current += '.';
                } else {
                    current = current === '0' ? num : current + num;
                }
            }
            updateDisplay();
        }
        function inputOperator(op) {
            if (operator && !justCalculated) {
                calculate();
            }
            previous = current;
            operator = op;
            justCalculated = false;
            current = '0';
            updateDisplay();
        }
        function clearAll() {
            current = '0';
            previous = '';
            operator = null;
            justCalculated = false;
            updateDisplay();
        }
        function calculate() {
            let prev = parseFloat(previous);
            let curr = parseFloat(current);
            let result;
            if (operator === '+') result = prev + curr;
            else if (operator === '-') result = prev - curr;
            else if (operator === '*') result = prev * curr;
            else if (operator === '÷') {
                if (curr === 0) {
                    alert("Cannot divide by zero");
                    clearAll();
                    return;
                }
                result = prev / curr;
            }
            else if (operator === '%') result = prev % curr;
            else return;
            current = result.toString().length > 12 ? result.toExponential(8) : result.toString();
            previous = '';
            operator = null;
            justCalculated = true;
            updateDisplay();
        }
        document.querySelectorAll('[data-number]').forEach(btn => {
            btn.addEventListener('click', () => inputNumber(btn.getAttribute('data-number')));
        });
        document.querySelectorAll('[data-operator]').forEach(btn => {
            btn.addEventListener('click', () => inputOperator(btn.getAttribute('data-operator')));
        });
        document.getElementById('clear').addEventListener('click', clearAll);
        document.getElementById('equals').addEventListener('click', calculate);
        updateDisplay();
    </script>
</body>
</html>
