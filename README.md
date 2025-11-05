```html
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
            margin: 0;
            padding: 20px;
        }

        .calculator-container {
            background-color: white;
            border-radius: 16px;
            box-shadow: var(--shadow);
            overflow: hidden;
            width: 100%;
            max-width: 350px;
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
            min-height: 120px;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: flex-end;
            position: relative;
            overflow: hidden;
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
            transition: all 0.2s ease;
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
            <button class="calculator-button special" onclick="clearDisplay()">C</button>
            <button class="calculator-button operator" onclick="appendToDisplay('÷')">÷</button>
            <button class="calculator-button operator" onclick="appendToDisplay('*')">×</button>
            <button class="calculator-button operator" onclick="appendToDisplay('%')">%</button>

            <button class="calculator-button number" onclick="appendToDisplay('7')">7</button>
            <button class="calculator-button number" onclick="appendToDisplay('8')">8</button>
            <button class="calculator-button number" onclick="appendToDisplay('9')">9</button>
            <button class="calculator-button operator" onclick="appendToDisplay('-')">-</button>

            <button class="calculator-button number" onclick="appendToDisplay('4')">4</button>
            <button class="calculator-button number" onclick="appendToDisplay('5')">5</button>
            <button class="calculator-button number" onclick="appendToDisplay('6')">6</button>
            <button class="calculator-button operator" onclick="appendToDisplay('+')">+</button>

            <button class="calculator-button number" onclick="appendToDisplay('1')">1</button>
            <button class="calculator-button number" onclick="appendToDisplay('2')">2</button>
            <button class="calculator-button number" onclick="appendToDisplay('3')">3</button>
            <button class="calculator-button equals" onclick="calculate()">=</button>

            <button class="calculator-button number" onclick="appendToDisplay('0')">0</button>
            <button class="calculator-button number" onclick="appendToDisplay('.')">.</button>
        </div>
    </div>

    <script>
        // Elements
        const currentOperandDisplay = document.querySelector('.current-operand');
        const previousOperandDisplay = document.querySelector('.previous-operand');
        const calculatorButtons = document.querySelectorAll('.calculator-button');

        // Calculator state
        let currentOperand = '0';
        let previousOperand = '';
        let operation = undefined;
        let shouldResetCurrentOperand = false;

        // Function to append a value to the display
        function appendToDisplay(value) {
            if (shouldResetCurrentOperand) {
                currentOperand = value === '.' ? '0.' : value;
                shouldResetCurrentOperand = false;
            } else {
                if (value === '.' && currentOperand.includes('.')) return;
                if (currentOperand === '0' && value !== '.') {
                    currentOperand = value;
                } else {
                    currentOperand += value;
                }
            }
            updateDisplay();
        }

        // Function to clear the display
        function clearDisplay() {
            currentOperand = '0';
            previousOperand = '';
            operation = undefined;
            updateDisplay();
        }

        // Function to calculate the result
        function calculate() {
            if (operation === undefined || shouldResetCurrentOperand) return;

            let result;
            const prev = parseFloat(previousOperand);
            const current = parseFloat(currentOperand);

            if (isNaN(prev) || isNaN(current)) return;

            switch (operation) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '÷':
                    if (current === 0) {
                        alert("Cannot divide by zero");
                        clearDisplay();
                        return;
                    }
                    result = prev / current;
                    break;
                case '%':
                    result = prev % current;
                    break;
                default:
                    return;
            }

            // Format the result
            result = result.toString();
            if (result.length > 12) {
                result = parseFloat(result).toExponential(8);
            }

            previousOperand = '';
            operation = undefined;
            currentOperand = result;
            shouldResetCurrentOperand = true;
            updateDisplay();
        }

        // Function to update the display
        function updateDisplay() {
            currentOperandDisplay.textContent = currentOperand;
            if (operation !== undefined) {
                previousOperandDisplay.textContent = `${previousOperand} ${operation}`;
            } else {
                previousOperandDisplay.textContent = previousOperand;
            }
        }

        // Add event listeners to buttons
        calculatorButtons.forEach(button => {
            button.addEventListener('click', () => {
                if (button.classList.contains('operator')) {
                    if (operation === undefined) {
                        operation = button.textContent;
                        previousOperand = currentOperand;
                        shouldResetCurrentOperand = true;
                    } else {
                        calculate();
                    }
                } else if (button.classList.contains('equals')) {
                    calculate();
                } else {
                    appendToDisplay(button.textContent);
                }
            });
        });
    </script>
</body>
</html>
```
