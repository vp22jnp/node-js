# Instructions to build a Node JS Calculator App

# NodeJs-App-Deployment-Project

we can create a simple web interface for it using Node.js and Express on the backend, and HTML with some basic JavaScript on the frontend. Here's how you can structure your project:

1. **Setting Up Your Project:**
   First, make sure you have Node.js installed on your machine.

2. **Create a New Directory:**
   Create a new directory for your project.

3. **Initialize Your Node.js Project:**
   Run `npm init -y` in your terminal to initialize a new Node.js project with default settings.

4. **Install Dependencies:**
   Install Express.js and other required packages by running:
   ```
   npm install express
   ```

5. **Create Files:**
   Create the following files in your project directory:
   - `app.js`: This file will contain the backend logic.
   - `index.html`: This file will contain the frontend HTML.
   - `style.css`: This file will contain CSS styles for the frontend.

6. **Write the Backend Logic:**
   Here's how you can implement the backend logic in `app.js` using Express:

```javascript
const express = require('express');
const app = express();

app.use(express.static('public'));
app.use(express.urlencoded({ extended: true }));

app.post('/calculate', (req, res) => {
    const { num1, num2, operation } = req.body;
    let result;

    switch (operation) {
        case '+':
            result = parseFloat(num1) + parseFloat(num2);
            break;
        case '-':
            result = parseFloat(num1) - parseFloat(num2);
            break;
        case '*':
            result = parseFloat(num1) * parseFloat(num2);
            break;
        case '/':
            if (parseFloat(num2) === 0) {
                result = "Error: Division by zero!";
            } else {
                result = parseFloat(num1) / parseFloat(num2);
            }
            break;
        default:
            result = 'Invalid operation';
    }

    res.send({ result });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

7. **Create the Frontend HTML:**
   Here's a basic structure for the `index.html` file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>Calculator</h1>
    <form id="calculatorForm">
        <input type="text" name="num1" placeholder="Enter the first number" required>
        <input type="text" name="num2" placeholder="Enter the second number" required>
        <select name="operation">
            <option value="+">+</option>
            <option value="-">-</option>
            <option value="*">*</option>
            <option value="/">/</option>
        </select>
        <button type="submit">Calculate</button>
    </form>
    <div id="result"></div>

    <script src="script.js"></script>
</body>
</html>
```

8. **Write CSS Styles:**
   You can add styles to the `style.css` file to make your calculator look nicer.

9. **Write Frontend JavaScript:**
   Create a `script.js` file to handle form submission and display the result:

```javascript
document.getElementById('calculatorForm').addEventListener('submit', async (event) => {
    event.preventDefault();

    const formData = new FormData(event.target);
    const num1 = formData.get('num1');
    const num2 = formData.get('num2');
    const operation = formData.get('operation');

    const response = await fetch('/calculate', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({ num1, num2, operation })
    });

    const data = await response.json();
    document.getElementById('result').innerText = `Result: ${data.result}`;
});
```

10. **Run Your Application:**
    Run your application by executing `node app.js` in your terminal. Then open your web browser and go to `http://localhost:3000` to use the Calculator Application.

With these steps, you have created a full-stack Calculator Application where users can perform arithmetic operations through a web interface.
