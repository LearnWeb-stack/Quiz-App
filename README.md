# Quiz-App
# JavaScript Quiz Application - Step-by-Step Explanation

## Step 1: Setting Up the Application

```javascript
// Wait for the HTML document to fully load before running any JavaScript
document.addEventListener("DOMContentLoaded", function() {
    // This function will contain all our quiz code
```

**Explanation:**
This code sets up an event listener that waits for the entire HTML document to load before running any JavaScript. This is important because we need all HTML elements to be available before our code tries to interact with them. The `DOMContentLoaded` event fires when the initial HTML document has been completely loaded, without waiting for stylesheets, images, and other resources to finish loading.

There are several reasons why using `DOMContentLoaded` is the best option here:

1. **Faster than `load`**: The `load` event waits for ALL resources (images, CSS, scripts, etc.) to finish loading, which can significantly delay the start of your application. `DOMContentLoaded` fires as soon as the HTML is parsed, allowing your JavaScript to run much sooner.

2. **Better than separate event listeners**: You could attach separate event listeners to individual HTML elements, but:
   - This would require those elements to exist in the HTML before your script runs
   - You'd need to manage multiple listeners, creating more complex code
   - It would be harder to ensure your initialization happens in the right order

3. **Better than placing script at end of body**: While putting your `<script>` tag at the end of the `<body>` tag also ensures HTML elements are loaded, using `DOMContentLoaded` is more explicit and works regardless of where your script is placed in the HTML document.

4. **Better than `setTimeout`**: Some developers use `setTimeout` to delay script execution, but this is unreliable since it doesn't actually check if the DOM is ready.

By using `DOMContentLoaded`, our quiz application starts as soon as possible while still ensuring all HTML elements are available for manipulation.

## Step 2: Creating the Quiz Data Structure

```javascript
    // Array containing all quiz questions and their properties
    const questions = [
        { 
            question: "What is a regular expression in JavaScript?", 
            answers: [
                "A method that describes a pattern of characters.",
                "A built-in JavaScript method.",
                "A data type that represents sequences of characters.",
                "A syntax and string pattern for searching, matching, and replacing text."
            ], 
            correct: 3,
            feedback: "A regular expression is a sequence of characters that forms a search pattern. It can be used for text search and text replace operations."
        },
        { 
            question: "Which function is used to search for a match in a string?", 
            answers: [
                "search()",
                "match()",
                "splice()",
                "replace()"
            ], 
            correct: 0,
            feedback: "The search() method searches the string for a match against a regular expression pattern and returns the index of the match."
        },
        { 
            question: "What does the g flag in a regular expression stand for?", 
            answers: [
                "Global match",
                "Generate match",
                "Go match",
                "None of the above"
            ], 
            correct: 0,
            feedback: "The g flag stands for global search, meaning it will search for all matches in a string, not just the first."
        },
        { 
            question: "How do you create a case-insensitive regular expression in JavaScript?", 
            answers: [
                "Using the i flag.",
                "Using the c flag.",
                "Using the m flag.",
                "Using the s flag."
            ], 
            correct: 0,
            feedback: "The i flag is used to make the regular expression search case-insensitive."
        },
        { 
            question: "Which method returns an array containing all matched groups?", 
            answers: [
                "exec()",
                "match()",
                "search()",
                "split()"
            ], 
            correct: 1,
            feedback: "The match() method retrieves the result of matching a string against a regular expression."
        },
        { 
            question: "What does the ^ character mean in regular expressions?", 
            answers: [
                "Matches the beginning of the input.",
                "Matches the end of the input.",
                "Matches any character.",
                "Escapes a special character."
            ], 
            correct: 0,
            feedback: "The ^ character matches the position at the beginning of the input."
        },
        { 
            question: "What does the . character represent in regular expressions?", 
            answers: [
                "Any single character, except newline or other line terminators.",
                "Decimal point.",
                "Specific dot character.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "In regular expressions, the . symbol matches any single character except newline characters."
        },
        { 
            question: "Which flag is used for multiline matching?", 
            answers: [
                "m",
                "x",
                "l",
                "u"
            ], 
            correct: 0,
            feedback: "The m flag is used to perform multiline matching."
        },
        { 
            question: "What does \\d match?", 
            answers: [
                "Any non-digit character.",
                "Any digit character.",
                "Any word character.",
                "Any whitespace character."
            ], 
            correct: 1,
            feedback: "\\d matches any digit character (0-9)."
        },
        { 
            question: "Which method is used to replace the matched substring with a replacement substring?", 
            answers: [
                "replace()",
                "replaceAll()",
                "Both A and B",
                "reverse()"
            ], 
            correct: 2,
            feedback: "Both replace() and replaceAll() methods are used to replace text in a string using a regular expression."
        },
        { 
            question: "What does \\b represent in regular expressions?", 
            answers: [
                "A backspace character.",
                "A non-word boundary.",
                "A word boundary.",
                "A whitespace boundary."
            ], 
            correct: 2,
            feedback: "\\b represents a word boundary."
        },
        { 
            question: "How do you specify a set of characters that you wish to match?", 
            answers: [
                "Using {}",
                "Using ()",
                "Using []",
                "Using //"
            ], 
            correct: 2,
            feedback: "Square brackets [] are used to specify a set of characters to match."
        },
        { 
            question: "What is the purpose of parentheses ( ) in regular expressions?", 
            answers: [
                "To define a searchable pattern.",
                "To specify optional characters.",
                "To create a capture group.",
                "To escape special characters."
            ], 
            correct: 2,
            feedback: "Parentheses are used to create a capture group in regular expressions."
        },
        { 
            question: "What does | mean in regular expressions?", 
            answers: [
                "And",
                "Or",
                "Not",
                "Nor"
            ], 
            correct: 1,
            feedback: "The | symbol is used as a logical OR in regular expressions."
        },
        { 
            question: "What does ? signify in regular expressions?", 
            answers: [
                "Zero or one occurrences of the preceding element.",
                "Exactly one occurrence of the preceding element.",
                "At least one occurrence of the preceding element.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "The ? makes the preceding token in the regular expression optional."
        },
        { 
            question: "What does + signify in regular expressions?", 
            answers: [
                "Zero or more occurrences of the preceding element.",
                "One or more occurrences of the preceding element.",
                "Exactly one occurrence of the preceding element.",
                "None of the above."
            ], 
            correct: 1,
            feedback: "The + character matches one or more of the preceding element."
        },
        { 
            question: "What does * signify in regular expressions?", 
            answers: [
                "Zero or more occurrences of the preceding element.",
                "One or more occurrences of the preceding element.",
                "Optional occurrence of the preceding element.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "The * character matches zero or more of the preceding element."
        },
        { 
            question: "What does \\s match?", 
            answers: [
                "Any whitespace character (space, tab, newline).",
                "Any non-whitespace character.",
                "A specific whitespace character.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "\\s matches any whitespace character including space, tab, carriage return, newline, vertical tab, and form feed."
        },
        { 
            question: "What does {2,4} specify in a regular expression?", 
            answers: [
                "Match the preceding element at least 2 times, but not more than 4 times.",
                "Match exactly 2 to 4 characters of any type.",
                "Match the preceding two characters followed by four characters.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "{2,4} is a quantifier that matches the preceding element at least 2 times and no more than 4 times."
        },
        { 
            question: "What does \\w match?", 
            answers: [
                "Any word character (letter, digit, underscore).",
                "Any non-word character.",
                "Any whitespace character.",
                "Any digit character."
            ], 
            correct: 0,
            feedback: "\\w matches any word character including letters, digits, and underscores."
        },
        { 
            question: "What does the \\W represent in a regular expression?", 
            answers: [
                "Any word character (letters, digits, underscores).",
                "Any non-word character.",
                "Any whitespace character.",
                "Any uppercase letter."
            ], 
            correct: 1,
            feedback: "\\W matches any character that is not a word character, which includes punctuation and spaces."
        },
        { 
            question: "How do you make a quantifier non-greedy in JavaScript regular expressions?", 
            answers: [
                "Add ? after the quantifier.",
                "Add ! before the quantifier.",
                "Add + after the quantifier.",
                "Add ^ before the quantifier."
            ], 
            correct: 0,
            feedback: "Adding a ? after a quantifier (like *?, +?, ??, {n,}?) makes it non-greedy, meaning it will match the smallest possible number of characters."
        },
        { 
            question: "What does the regular expression /^a/ match?", 
            answers: [
                "Any string that includes 'a' at any position.",
                "Any string that starts with 'a'.",
                "Any string that ends with 'a'.",
                "Any string that contains the exact sequence 'a'."
            ], 
            correct: 1,
            feedback: "/^a/ matches any string that starts with the letter 'a'."
        },
        { 
            question: "Which character in a regular expression is used to escape special characters?", 
            answers: [
                "\\",
                "/",
                "|",
                "!"
            ], 
            correct: 0,
            feedback: "The backslash \\ is used to escape special characters in regular expressions."
        },
        { 
            question: "What would the regular expression /n+/ match in the string \"ninja\"?", 
            answers: [
                "Matches 'n' and 'i'.",
                "Matches 'n', 'i', and 'j'.",
                "Matches the first 'n' only.",
                "Matches 'nn' as a sequence."
            ], 
            correct: 2,
            feedback: "/n+/ matches one or more 'n's in sequence, which in \"ninja\" would match the first 'n' only."
        },
        { 
            question: "What is the function of the (?=...) syntax in a regular expression?", 
            answers: [
                "Matches any string containing the specified pattern.",
                "Specifies a non-capturing group.",
                "Positive lookahead, asserts what follows the current position.",
                "Negative lookahead, asserts what does not follow the current position."
            ], 
            correct: 2,
            feedback: "(?=...) is a positive lookahead that asserts a condition for what immediately follows the current position in the string must satisfy the pattern inside the lookahead."
        },
        { 
            question: "What does the negative lookahead (?!...) do in a regular expression?", 
            answers: [
                "Matches a group multiple times.",
                "Excludes the pattern specified within the lookahead from matching.",
                "Looks for any characters not specified within the brackets.",
                "Matches any string where the subsequent characters do not form the specified pattern."
            ], 
            correct: 3,
            feedback: "(?!...) is a negative lookahead that asserts that the specified pattern does not occur immediately after the current position in the string."
        },
        { 
            question: "How do you match either 'cat' or 'dog' in a string using regular expressions?", 
            answers: [
                "/[cat|dog]/",
                "/(cat)|(dog)/",
                "/(cat|dog)/",
                "/{cat,dog}/"
            ], 
            correct: 2,
            feedback: "/(cat|dog)/ correctly uses the pipe | to match either 'cat' or 'dog'."
        },
        { 
            question: "What does {2} specify in a regular expression?", 
            answers: [
                "Match exactly two instances of the preceding element.",
                "Match up to two instances of the preceding element.",
                "Match at least two instances of any character.",
                "None of the above."
            ], 
            correct: 0,
            feedback: "{2} is a quantifier that matches exactly two occurrences of the preceding element."
        },
        { 
            question: "Which method would you use to test if a pattern exists within a string?", 
            answers: [
                "search()",
                "test()",
                "match()",
                "find()"
            ], 
            correct: 1,
            feedback: "The test() method tests for a match in a string. It returns true if there is a match, otherwise it returns false."
        }
    ];
```

**Explanation:**
This step creates an array named `questions` that contains objects representing each quiz question. This is a fundamental data structure in JavaScript known as an "array of objects."

**What is an array of objects?**
An array of objects is a collection of objects stored in a single variable. In this case, each object represents one question in our quiz. This structure is extremely common in JavaScript applications because it allows you to:
1. Store related data together in a logical way
2. Access and manipulate that data easily using array methods and object notation

**Structure of each question object:**
Each question object has:
- A `question` property with the text of the question
- An `answers` array containing the four possible answers
- A `correct` property that stores the index of the correct answer (remember, arrays in JavaScript start at index 0)
- A `feedback` property containing an explanation that will be shown after answering

**Why this structure is necessary for the quiz:**
1. **Organization**: Without this structure, we would need separate variables for each question, each set of answers, each correct answer, and each feedback message. That would be incredibly messy and hard to maintain.

2. **Iteration**: This structure allows us to loop through questions easily with methods like `forEach()` or a for loop, making it simple to process each question in turn.

3. **Consistency**: Each question follows the same data format, ensuring we can process all questions using the same code.

4. **Maintainability**: To add, remove, or modify quiz questions, we only need to edit this data array, not the application code itself. This separation of data from functionality is a good programming practice.

5. **Scalability**: This approach can handle any number of questions - the code will work the same way whether we have 10 questions or 1000.

The `questions` array acts as a database for our quiz application. The actual code contains 30 questions about JavaScript regular expressions, but we're showing just the first one as an example.

## Step 3: Initializing Variables

```javascript
    // Track which question the user is currently viewing
    let currentQuestion = 0;
    
    // Track how many questions the user has answered correctly
    let score = 0;
    
    // Will store the timer interval reference
    let countdown;
    
    // Set time limit for each question (in seconds)
    let timeLeft = 30;
    
    // Flag to prevent multiple answer selections
    let answerSelected = false;
    
    // Labels for multiple choice answers
    const labels = ['a', 'b', 'c', 'd'];
```

**Explanation:**
This section initializes all the variables needed to run the quiz:
- `currentQuestion` tracks which question the user is currently viewing (starting with the first question at index 0)
- `score` keeps count of how many questions the user has answered correctly
- `countdown` will store a reference to the timer interval so it can be stopped when needed
- `timeLeft` sets the time limit for each question (30 seconds)
- `answerSelected` is a flag that prevents the user from selecting multiple answers for the same question
- `labels` is an array of letters used to label the multiple choice options (a, b, c, d)

**Why having the `labels` array separate is better than including it in each question object:**

1. **Avoiding redundancy**: If we included the labels in each question object, we would be repeating the same information (a, b, c, d) for every single question. This would be redundant and waste memory.

2. **Single point of change**: If we ever want to change the labeling system (for example, from letters to numbers), we only need to update it in one place rather than in every question object.

3. **Separation of concerns**: The labels are part of how the quiz is displayed to the user (presentation), not part of the core question data (content). Keeping them separate maintains a cleaner separation between content and presentation.

4. **Flexibility**: Having a separate labels array makes it easier to handle questions with varying numbers of answer options. We can simply use as many labels as needed for each question.

5. **Performance**: While minor in this case, not storing the same labels with each question object results in a smaller data footprint, which can be significant with larger datasets.

These variables establish the quiz's state and will be updated as the user progresses through the questions.

## Step 4: Function to Display Questions

```javascript
    // This function displays the current question and answer choices
    function displayQuestion() {
        // Reset answer selection status for the new question
        answerSelected = false;
        
        // Hide any feedback from the previous question
        document.getElementById("feedback").style.display = "none";
        
        // Update the progress indicator text
        document.getElementById("progress").textContent = 
            `Question ${currentQuestion + 1} of ${questions.length}`;
        
        // Hide the "Next" button until an answer is selected
        document.getElementById("next-button").style.display = "none";
        
        // Reset and start the timer
        clearInterval(countdown);
        timeLeft = 30;
        timer();
        
        // Get the current question object
        const questionObj = questions[currentQuestion];
        
        // Update the question text with number and content
        document.getElementById("question").textContent = 
            `${currentQuestion + 1}. ${questionObj.question}`;
        
        // Get and clear the answers list
        const answersUl = document.getElementById("answers");
        answersUl.innerHTML = "";
        
        // Loop through each answer choice
        questionObj.answers.forEach((answer, index) => {
            // Create a new list item for this answer
            const li = document.createElement("li");
            
            // Create the label (a, b, c, or d)
            const labelSpan = document.createElement("span");
            labelSpan.className = "answer-label";
            labelSpan.textContent = labels[index] + ")";
            
            // Add the label and answer text to the list item
            li.appendChild(labelSpan);
            li.appendChild(document.createTextNode(" " + answer));
            
            // Add click handler to check if this answer is correct
            li.onclick = () => {
                if (!answerSelected) {
                    checkAnswer(index);
                }
            };
            
            // Add this answer to the list
            answersUl.appendChild(li);
        });
    }
```

**Explanation:**
The `displayQuestion()` function is responsible for showing the current question and its answer choices to the user. Let's break down what each part does:

1. **Reset answer selection status**:
   ```javascript
   answerSelected = false;
   ```
   This line resets the flag that prevents multiple selections, allowing the user to select an answer for the new question.

2. **Hide previous feedback**:
   ```javascript
   document.getElementById("feedback").style.display = "none";
   ```
   This hides any feedback from the previous question so the user starts with a clean slate.

3. **Update progress indicator**:
   ```javascript
   document.getElementById("progress").textContent = 
       `Question ${currentQuestion + 1} of ${questions.length}`;
   ```
   This updates the text showing the user's progress. Note how it uses `currentQuestion + 1` to show a human-friendly number (starting from 1 instead of 0).

4. **Hide the Next button**:
   ```javascript
   document.getElementById("next-button").style.display = "none";
   ```
   The Next button is hidden until the user selects an answer.

5. **Reset and start timer**:
   ```javascript
   clearInterval(countdown);
   timeLeft = 30;
   timer();
   ```
   These lines clear any existing timer, reset the time to 30 seconds, and start a new timer.

6. **Get current question data**:
   ```javascript
   const questionObj = questions[currentQuestion];
   ```
   This retrieves the current question object from the questions array using the currentQuestion index.

7. **Update question text with dynamic numbering**:
   ```javascript
   document.getElementById("question").textContent = 
       `${currentQuestion + 1}. ${questionObj.question}`;
   ```
   This line is particularly important as it **dynamically generates the question number**. Instead of hardcoding question numbers in the data, it:
   - Uses the current index (`currentQuestion`) plus 1 to create the human-readable question number
   - Concatenates this with the question text
   
   **Why this dynamic approach is better:**
   - If questions are reordered, the numbers automatically update
   - If questions are added or removed, all numbers adjust accordingly
   - It eliminates the risk of having incorrect numbering due to manual errors
   - It makes the code more maintainable since question numbers don't need to be stored in the data
   - Allows for randomly ordering questions (for a shuffle feature) without breaking the numbering

8. **Clear previous answers**:
   ```javascript
   const answersUl = document.getElementById("answers");
   answersUl.innerHTML = "";
   ```
   These lines get the unordered list element for answers and clear any existing content.

9. **Create answer list items**:
   ```javascript
   questionObj.answers.forEach((answer, index) => {
       const li = document.createElement("li");
       
       const labelSpan = document.createElement("span");
       labelSpan.className = "answer-label";
       labelSpan.textContent = labels[index] + ")";
       
       li.appendChild(labelSpan);
       li.appendChild(document.createTextNode(" " + answer));
       
       li.onclick = () => {
           if (!answerSelected) {
               checkAnswer(index);
           }
       };
       // ...
   ```
   This code loops through each answer, creating a list item with a label and the answer text, plus a click handler.

10. **Add answers to the DOM**:
    ```javascript
    answersUl.appendChild(li);
    ```
    Each answer list item is added to the unordered list in the HTML.

This function refreshes the quiz interface for each new question, dynamically generating question numbers and answer choices. The dynamic approach used for question numbering is an excellent example of letting the code handle presentation details rather than hardcoding them in the data.

## Step 5: Timer Function

```javascript
    // Function to manage the countdown timer
    function timer() {
        // Update the timer display
        document.getElementById("timer").textContent = 
            `Time remaining: ${timeLeft} seconds`;
        
        // Set up function to run every second
        countdown = setInterval(function() {
            // Check if time has run out
            if (timeLeft <= 0) {
                // Stop the timer
                clearInterval(countdown);
                
                // Check answer with -1 to indicate time ran out
                checkAnswer(-1);
            } else {
                // Decrement the time and update display
                timeLeft--;
                document.getElementById("timer").textContent = 
                    `Time remaining: ${timeLeft} seconds`;
            }
        }, 1000); // Run every 1000 milliseconds (1 second)
    }
```

**Explanation:**
The `timer()` function manages the countdown for each question:

```javascript
function timer() {
    // Update the timer display
    document.getElementById("timer").textContent = 
        `Time remaining: ${timeLeft} seconds`;
    
    // Set up function to run every second
    countdown = setInterval(function() {
        // Check if time has run out
        if (timeLeft <= 0) {
            // Stop the timer
            clearInterval(countdown);
            
            // Check answer with -1 to indicate time ran out
            checkAnswer(-1);
        } else {
            // Decrement the time and update display
            timeLeft--;
            document.getElementById("timer").textContent = 
                `Time remaining: ${timeLeft} seconds`;
        }
    }, 1000); // Run every 1000 milliseconds (1 second)
}
```

Let's analyze this function in detail:

1. **Initial display update**: 
   ```javascript
   document.getElementById("timer").textContent = `Time remaining: ${timeLeft} seconds`;
   ```
   This line updates the timer display right away, showing the initial time (30 seconds) before the countdown even starts.

2. **Setting up the interval**:
   ```javascript
   countdown = setInterval(function() { ... }, 1000);
   ```
   The `setInterval()` method calls a function repeatedly at specified intervals (1000ms = 1 second). The return value is stored in the `countdown` variable so we can stop it later.

3. **The time comparison**:
   ```javascript
   if (timeLeft <= 0) { ... }
   ```
   This comparison uses `<=` (less than or equal to) instead of just `<` (less than) for several important reasons:
   
   - **Fail-safe mechanism**: It ensures the timer stops even if `timeLeft` somehow becomes negative
   - **Edge case handling**: If `timeLeft` decrements directly to 0, we want to catch that immediately
   - **Preventing timer overrun**: It stops exactly when time is up, not a second after
   - **Defensive programming**: It's more robust against timing or calculation errors

4. **When time runs out**:
   ```javascript
   clearInterval(countdown);
   checkAnswer(-1);
   ```
   These lines:
   - Stop the timer from continuing to run
   - Call `checkAnswer()` with a special value of `-1` that indicates "no answer was selected before time ran out"

5. **When time is still remaining**:
   ```javascript
   timeLeft--;
   document.getElementById("timer").textContent = `Time remaining: ${timeLeft} seconds`;
   ```
   These lines:
   - Decrement the time by 1 second
   - Update the timer display with the new value

**Why using `-1` as a special value works well here:**
- It's a value that cannot be a valid answer index (since array indices start at 0)
- It clearly communicates "no answer" to the `checkAnswer()` function
- It allows the code to distinguish between a wrong answer selection and no selection at all

This timer implementation is a common pattern in JavaScript for creating countdown timers, and understanding it helps build foundations for asynchronous programming and time-based interaction handling.

## Step 6: Answer Checking Function

```javascript
    // Function to check if the selected answer is correct
    function checkAnswer(index) {
        // Prevent multiple answer selections
        if (answerSelected) return;
        
        // Mark that an answer has been selected
        answerSelected = true;
        
        // Stop the countdown timer
        clearInterval(countdown);
        
        // Get the correct answer index from the current question
        const correctIndex = questions[currentQuestion].correct;
        
        // Compare with selected answer index to determine if correct
        const isCorrect = index === correctIndex;
        
        // Get reference to the feedback element
        const feedback = document.getElementById("feedback");
        
        // Set CSS class based on whether answer is correct
        feedback.className = isCorrect ? "correct" : "incorrect";
        
        // Get the label and text of the correct answer
        const correctLabel = labels[correctIndex];
        const correctAnswer = questions[currentQuestion].answers[correctIndex];
        
        if (isCorrect) {
            // For correct answers
            feedback.innerHTML = 
                `<strong>Correct!</strong> The answer is ${correctLabel}) ${correctAnswer}
                <br>${questions[currentQuestion].feedback}`;
            
            // Increment the score
            score++;
        } else {
            if (index === -1) {
                // For time running out
                feedback.innerHTML = 
                    `<strong>Time's up!</strong> The correct answer is ${correctLabel}) ${correctAnswer}
                    <br>${questions[currentQuestion].feedback}`;
            } else {
                // For incorrect answers
                const selectedLabel = labels[index];
                const selectedAnswer = questions[currentQuestion].answers[index];
                
                feedback.innerHTML = 
                    `<strong>Incorrect.</strong> You selected ${selectedLabel}) ${selectedAnswer}.
                    <br>The correct answer is ${correctLabel}) ${correctAnswer}
                    <br>${questions[currentQuestion].feedback}`;
            }
        }
        
        // Make the feedback element visible
        feedback.style.display = "block";
        
        // Get all answer list items
        const answerItems = document.querySelectorAll("#answers li");
        
        // Highlight the correct answer with green
        answerItems[correctIndex].style.backgroundColor = "rgba(0, 128, 0, 0.1)";
        answerItems[correctIndex].style.borderColor = "green";
        
        // If a wrong answer was selected, highlight it with red
        if (index !== -1 && index !== correctIndex) {
            answerItems[index].style.backgroundColor = "rgba(255, 0, 0, 0.1)";
            answerItems[index].style.borderColor = "red";
        }
        
        // Check if there are more questions remaining
        if (currentQuestion < questions.length - 1) {
            // If not the last question, show the "Next" button
            document.getElementById("next-button").style.display = "inline-block";
        } else {
            // If it's the last question, change to "Show Results" button
            const nextButton = document.getElementById("next-button");
            nextButton.textContent = "Show Results";
            nextButton.style.display = "inline-block";
            nextButton.onclick = showResults;
        }
    }
```

**Explanation:**
The `checkAnswer()` function evaluates the user's answer and provides feedback. Let's examine each part of this critical function:

1. **Prevent multiple answer selections**:
   ```javascript
   // Prevent multiple answer selections
   if (answerSelected) return;
   
   // Mark that an answer has been selected
   answerSelected = true;
   ```
   These lines prevent the function from running multiple times if the user clicks rapidly on different answers. It checks if an answer has already been selected, and if so, immediately exits the function. Otherwise, it sets the flag to true to prevent future selections.

2. **Stop the timer**:
   ```javascript
   // Stop the countdown timer
   clearInterval(countdown);
   ```
   This stops the countdown timer by clearing the interval reference that was stored when the timer started. This is important because we don't want the timer to continue running after an answer is selected.

3. **Check if the answer is correct**:
   ```javascript
   // Get the correct answer index from the current question
   const correctIndex = questions[currentQuestion].correct;
   
   // Compare with selected answer index to determine if correct
   const isCorrect = index === correctIndex;
   ```
   The first line retrieves the index of the correct answer from the current question object. The second line compares it to the index of the selected answer (passed as a parameter to the function) to determine if the answer is correct.

4. **Prepare feedback element**:
   ```javascript
   // Get reference to the feedback element
   const feedback = document.getElementById("feedback");
   
   // Set CSS class based on whether answer is correct
   feedback.className = isCorrect ? "correct" : "incorrect";
   ```
   These lines get a reference to the feedback element and set its CSS class based on whether the answer is correct. This will control the styling (like background color) of the feedback message.

5. **Generate appropriate feedback message**:
   ```javascript
   // Get the label and text of the correct answer
   const correctLabel = labels[correctIndex];
   const correctAnswer = questions[currentQuestion].answers[correctIndex];
   
   if (isCorrect) {
       // For correct answers
       feedback.innerHTML = 
           `<strong>Correct!</strong> The answer is ${correctLabel}) ${correctAnswer}
           <br>${questions[currentQuestion].feedback}`;
       
       // Increment the score
       score++;
   } else {
       if (index === -1) {
           // For time running out
           feedback.innerHTML = 
               `<strong>Time's up!</strong> The correct answer is ${correctLabel}) ${correctAnswer}
               <br>${questions[currentQuestion].feedback}`;
       } else {
           // For incorrect answers
           const selectedLabel = labels[index];
           const selectedAnswer = questions[currentQuestion].answers[index];
           
           feedback.innerHTML = 
               `<strong>Incorrect.</strong> You selected ${selectedLabel}) ${selectedAnswer}.
               <br>The correct answer is ${correctLabel}) ${correctAnswer}
               <br>${questions[currentQuestion].feedback}`;
       }
   }
   ```
   This block creates different feedback messages based on what happened:
   - For correct answers: Shows "Correct!" and increments the score
   - For time running out (index === -1): Shows "Time's up!"
   - For incorrect answers: Shows which wrong answer was selected
   
   In all cases, it shows the correct answer and the explanation from the question object.

6. **Display the feedback**:
   ```javascript
   // Make the feedback element visible
   feedback.style.display = "block";
   ```
   This makes the feedback element visible on the page after setting its content.

7. **Highlight answers**:
   ```javascript
   // Get all answer list items
   const answerItems = document.querySelectorAll("#answers li");
   
   // Highlight the correct answer with green
   answerItems[correctIndex].style.backgroundColor = "rgba(0, 128, 0, 0.1)";
   answerItems[correctIndex].style.borderColor = "green";
   
   // If a wrong answer was selected, highlight it with red
   if (index !== -1 && index !== correctIndex) {
       answerItems[index].style.backgroundColor = "rgba(255, 0, 0, 0.1)";
       answerItems[index].style.borderColor = "red";
   }
   ```
   These lines provide visual feedback by changing the styling of the answer choices:
   - The correct answer is always highlighted with a light green background and green border
   - If a wrong answer was selected (not time running out, and not the correct answer), it gets a light red background and red border
   
   This visual feedback reinforces the learning by clearly showing which answer was correct.

8. **Prepare for the next step**:
   ```javascript
   // Check if there are more questions remaining
   if (currentQuestion < questions.length - 1) {
       // If not the last question, show the "Next" button
       document.getElementById("next-button").style.display = "inline-block";
   } else {
       // If it's the last question, change to "Show Results" button
       const nextButton = document.getElementById("next-button");
       nextButton.textContent = "Show Results";
       nextButton.style.display = "inline-block";
       nextButton.onclick = showResults;
   }
   ```
   This code checks if there are more questions remaining:
   - If not the last question: Shows the "Next" button to continue
   - If it's the last question: Changes the button to say "Show Results" and changes its behavior to show the final results screen

**Key design principles in this function:**

1. **User experience focus**: The function provides immediate, clear feedback about the user's answer.

2. **Visual reinforcement**: Color-coding helps the user understand what happened at a glance.

3. **Edge case handling**: The code properly handles all possible scenarios (correct answer, wrong answer, time running out).

4. **State management**: The function manages the quiz state by updating the score and controlling what happens next.

5. **Learning reinforcement**: By showing the correct answer and explanation regardless of the user's choice, the function supports the educational purpose of the quiz.

This function is the core of the quiz's interactivity, handling all the logic needed to process the user's answers and guide them through the learning experience.

## Step 7: Results Display Function

```javascript
    // Function to display the final quiz results
    function showResults() {
        // Replace the quiz content with results screen
        document.getElementById("quiz-container").innerHTML = `
            <div id="result">
                <h2>Quiz Complete!</h2>
                <p>You scored ${score} out of ${questions.length}</p>
                <p>Percentage: ${Math.round((score / questions.length) * 100)}%</p>
                <button onclick="location.reload()">Try Again</button>
            </div>`;
    }
```

**Explanation:**
The `showResults()` function is called after the user completes all questions. It:

1. Replaces the entire quiz container's HTML with a results screen
2. Shows the user's final score (number of correct answers)
3. Calculates and displays the percentage score (rounded to the nearest whole number)
4. Provides a "Try Again" button that reloads the page to restart the quiz

This function creates a simple summary screen that gives the user feedback on their overall performance on the quiz.

## Step 8: Event Listener for Next Button

```javascript
    // Add click handler to the "Next" button
    document.getElementById("next-button").addEventListener("click", () => {
        // Only advance if an answer was selected and there are more questions
        if (currentQuestion < questions.length - 1 && answerSelected) {
            // Go to next question
            currentQuestion++;
            
            // Display the new question
            displayQuestion();
        }
    });
```

**Explanation:**
This code adds a click event listener to the "Next" button that:

1. Checks if an answer has been selected and if there are more questions
2. If both conditions are true:
   - Increments the `currentQuestion` counter to move to the next question
   - Calls the `displayQuestion()` function to show the new question

This event listener allows the user to progress through the quiz after answering each question. The check for `answerSelected` prevents moving to the next question without answering the current one.

## Step 9: Initialize the Quiz

```javascript
    // Display the first question when the page loads
    displayQuestion();
    
// End of DOMContentLoaded event listener function
});
```

**Explanation:**
This final step:

1. Calls the `displayQuestion()` function to display the first question when the page loads
2. Closes the main function that was started by the DOMContentLoaded event listener

This initializes the quiz and makes it ready for the user to start answering questions.

## How the Quiz Works - Summary

The JavaScript quiz application follows this process:

1. **Setup Phase**:
   - Waits for the page to load
   - Sets up quiz data and variables
   - Prepares functions for handling questions and answers

2. **Question Cycle**:
   - Displays a question with multiple-choice answers
   - Starts a 30-second timer
   - Waits for user to select an answer or for time to run out
   - Shows feedback on whether the answer was correct
   - Provides a button to move to the next question

3. **Completion Phase**:
   - After all questions are answered, shows final score
   - Offers an option to try again

The code is organized in a way that separates different responsibilities:
- Data storage (questions array)
- Display logic (displayQuestion function)
- Timing logic (timer function)
- Answer processing (checkAnswer function)
- Navigation (event listeners)

This separation makes the code easier to understand and maintain, as each part has a specific role in the application.
