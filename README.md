# task-1-alpha-code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simple Calculator</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            padding: 20px;
        }
        
        .calculator {
            background-color: #2c3e50;
            border-radius: 20px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
            overflow: hidden;
            width: 340px;
            max-width: 100%;
        }
        
        .display {
            background-color: #34495e;
            padding: 25px;
            text-align: right;
            border-bottom: 2px solid #3d566e;
        }
        
        .previous-operand {
            color: rgba(255, 255, 255, 0.7);
            font-size: 1.2rem;
            min-height: 24px;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .current-operand {
            color: white;
            font-size: 2.5rem;
            font-weight: 500;
            margin-top: 10px;
            min-height: 40px;
            overflow: hidden;
            text-overflow: ellipsis;
        }
        
        .buttons {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 1px;
            background-color: #3d566e;
        }
        
        button {
            border: none;
            outline: none;
            background-color: #34495e;
            color: white;
            font-size: 1.4rem;
            padding: 20px 0;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        button:hover {
            background-color: #3d566e;
            transform: scale(1.03);
        }
        
        button:active {
            background-color: #2c3e50;
            transform: scale(0.98);
        }
        
        .operator {
            background-color: #3498db;
        }
        
        .operator:hover {
            background-color: #2980b9;
        }
        
        .equals {
            background-color: #2ecc71;
            grid-column: span 2;
        }
        
        .equals:hover {
            background-color: #27ae60;
        }
        
        .clear {
            background-color: #e74c3c;
        }
        
        .clear:hover {
            background-color: #c0392b;
        }
        
        .footer {
            text-align: center;
            color: white;
            padding: 15px;
            font-size: 0.9rem;
            background-color: #34495e;
        }
        
        @media (max-width: 400px) {
            button {
                padding: 15px 0;
                font-size: 1.2rem;
            }
            
            .display {
                padding: 15px;
            }
            
            .current-operand {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator">
        <div class="display">
            <div class="previous-operand" id="previous-operand"></div>
            <div class="current-operand" id="current-operand">0</div>
        </div>
        <div class="buttons">
            <button class="clear" data-action="clear">C</button>
            <button data-action="delete">⌫</button>
            <button class="operator" data-operation="÷">÷</button>
            <button data-number="7">7</button>
            <button data-number="8">8</button>
            <button data-number="9">9</button>
            <button class="operator" data-operation="×">×</button>
            <button data-number="4">4</button>
            <button data-number="5">5</button>
            <button data-number="6">6</button>
            <button class="operator" data-operation="-">−</button>
            <button data-number="1">1</button>
            <button data-number="2">2</button>
            <button data-number="3">3</button>
            <button class="operator" data-operation="+">+</button>
            <button data-number="0">0</button>
            <button data-number=".">.</button>
            <button class="equals" data-action="calculate">=</button>
        </div>
        <div class="footer">
            Simple Calculator | HTML, CSS & JavaScript
        </div>
    </div>

    <script>
        // Calculator state
        let currentOperand = '0';
        let previousOperand = '';
        let operation = null;
        let resetScreen = false;

        // DOM elements
        const currentOperandElement = document.getElementById('current-operand');
        const previousOperandElement = document.getElementById('previous-operand');

        // Update display
        function updateDisplay() {
            currentOperandElement.textContent = currentOperand;
            previousOperandElement.textContent = previousOperand;
        }

        // Append number
        function appendNumber(number) {
            if (currentOperand === '0' || resetScreen) {
                currentOperand = number;
                resetScreen = false;
            } else {
                currentOperand += number;
            }
        }

        // Add decimal point
        function addDecimal() {
            if (resetScreen) {
                currentOperand = '0.';
                resetScreen = false;
                return;
            }
            
            if (!currentOperand.includes('.')) {
                currentOperand += '.';
            }
        }

        // Choose operation
        function chooseOperation(op) {
            if (currentOperand === '') return;
            if (previousOperand !== '') {
                calculate();
            }
            
            operation = op;
            previousOperand = `${currentOperand} ${op}`;
            currentOperand = '';
        }

        // Calculate result
        function calculate() {
            let computation;
            const prev = parseFloat(previousOperand);
            const current = parseFloat(currentOperand);
            
            if (isNaN(prev) || isNaN(current)) return;
            
            switch (operation) {
                case '+':
                    computation = prev + current;
                    break;
                case '−':
                    computation = prev - current;
                    break;
                case '×':
                    computation = prev * current;
                    break;
                case '÷':
                    if (current === 0) {
                        computation = 'Error';
                    } else {
                        computation = prev / current;
                    }
                    break;
                default:
                    return;
            }
            
            currentOperand = computation.toString();
            operation = null;
            previousOperand = '';
            resetScreen = true;
        }

        // Clear calculator
        function clearCalculator() {
            currentOperand = '0';
            previousOperand = '';
            operation = null;
        }

        // Delete last character
        function deleteNumber() {
            if (currentOperand.length === 1) {
                currentOperand = '0';
            } else {
                currentOperand = currentOperand.slice(0, -1);
            }
        }

        // Event listeners for buttons
        document.querySelectorAll('[data-number]').forEach(button => {
            button.addEventListener('click', () => {
                appendNumber(button.dataset.number);
                updateDisplay();
            });
        });

        document.querySelector('[data-number="."]').addEventListener('click', () => {
            addDecimal();
            updateDisplay();
        });

        document.querySelectorAll('[data-operation]').forEach(button => {
            button.addEventListener('click', () => {
                chooseOperation(button.dataset.operation);
                updateDisplay();
            });
        });

        document.querySelector('[data-action="calculate"]').addEventListener('click', () => {
            calculate();
            updateDisplay();
        });

        document.querySelector('[data-action="clear"]').addEventListener('click', () => {
            clearCalculator();
            updateDisplay();
        });

        document.querySelector('[data-action="delete"]').addEventListener('click', () => {
            deleteNumber();
            updateDisplay();
        });

        // Keyboard support
        document.addEventListener('keydown', event => {
            if (event.key >= 0 && event.key <= 9) {
                appendNumber(event.key);
                updateDisplay();
            }
            if (event.key === '.') {
                addDecimal();
                updateDisplay();
            }
            if (event.key === '+' || event.key === '-' || event.key === '*' || event.key === '/') {
                let op = event.key;
                if (op === '*') op = '×';
                if (op === '/') op = '÷';
                chooseOperation(op);
                updateDisplay();
            }
            if (event.key === 'Enter' || event.key === '=') {
                calculate();
                updateDisplay();
            }
            if (event.key === 'Escape') {
                clearCalculator();
                updateDisplay();
            }
            if (event.key === 'Backspace') {
                deleteNumber();
                updateDisplay();
            }
        });

        // Initialize display
        updateDisplay();
    </script>
</body>
</html>
