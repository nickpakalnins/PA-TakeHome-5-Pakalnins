# Quiz App Demo

## 1. Problem Statement:

This is a small quiz to test your knowledge about the different kinds of languages yuou have learned so far throughout your time at Oakland University. The quiz consists of five questions. You will only have 15 seconds to answer each question. Once you select your answer, it cannot be undone, and you also cannot select an option once the timer runs out. Once you start the quiz, you will not be able to leave it. Your score will be added up and shown to you at the end of the quiz. Once finished, you will be able to retake or leave the quiz. Make sure you read each question carefully. Good luck!

---

## 2. Functional Features:

- Buttons to move through the quiz (Start Quiz, Exit Quiz, Continue, Next Que, Replay Quiz, Quit Quiz)
- Correctness shown after each choice or when time is up (either with a green check for correct, or a red x for incorrect)
- Live Timer & Status Bar for user to track the time left on the current question
- Question number you are currently on out of five total questions
- Score screen with options to replay or quit the quiz

---

## 3. Explanation of the Directory/Structure Setup of the App

Step 1: We always want to make sure when we create our HTML file, we want to link our CSS and JS files, along with any others (in this case, special fonts)

- This also allows all our HTML elements to be referenced when creation objects in our javascript file.

```html
<!-- CSS FILE -->
<link rel="stylesheet" href="css/style.css" />
<!-- This is my personal font awesome kit code. you will have to add your own after you register with email-->
<script
  src="https://kit.fontawesome.com/4a4f4b55b0.js"
  crossorigin="anonymous"
></script>

<!-- Add questions list -->
<script src="js/questions.js" defer></script>

<!-- Main logic of the app -->
<script src="js/quizApp.js" defer></script>
```

- Make sure to have an HTML element to pull dynamic questions/options from questions.js

```html
<section>
  <div class="que_text">
    <!-- Insert questions from ./js/questions.js -->
  </div>
  <div class="option_list">
    <!-- Insert options to questions from ./js/questions.js -->
  </div>
</section>
```

- Make sure to have an HTML element for the Question Count

```html
<div class="total_que">
  <!-- insert Question Count Number dynamically from JavaScript App logic -->
</div>
```

- Lastly make sure you have a HTML element for the dynamic response at the result screen from Javascript

```html
<div class="score_text">
  <!-- insert dynamic user score as Result from JavaScript -->
</div>
```

Step 2: Make sure those objects are set up in your javascript file

```javascript
//selecting all required elements from html document
const start_btn = document.querySelector(".start_btn button");
const info_box = document.querySelector(".info_box");
const exit_btn = info_box.querySelector(".buttons .quit");
const continue_btn = info_box.querySelector(".buttons .restart");
const quiz_box = document.querySelector(".quiz_box");
const result_box = document.querySelector(".result_box");
const restart_quiz = result_box.querySelector(".buttons .restart");
const quit_quiz = result_box.querySelector(".buttons .quit");
const option_list = document.querySelector(".option_list");
const time_line = document.querySelector("header .time_line");
const timeText = document.querySelector(".timer .time_left_txt");
const timeCount = document.querySelector(".timer .timer_sec");
const next_btn = document.querySelector("footer .next_btn");
const bottom_ques_counter = document.querySelector("footer .total_que");
```

Step 3: When you want to pull from the array we made in questions.js and link it through the quizApp.js, we need to make sure it's available to pull from, so we write a function for that

```javascript
function showQuetions(index) {
  const que_text = document.querySelector(".que_text");

  //creating a new span and div tag for question and option and passing the value using array index
  // self exercise: re-write the following using backtick syntax for string in JS instead of concatenation operator
  let que_tag =
    "<span>" +
    questions[index].numb +
    ". " +
    questions[index].question +
    "</span>";
  let option_tag =
    '<div class="option"><span>' +
    questions[index].options[0] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[1] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[2] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[3] +
    "</span></div>";
  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
  option_list.innerHTML = option_tag; //adding new (child) div tag inside option_tag

  const option = option_list.querySelectorAll(".option");

  // set onclick attribute to all available options
  for (i = 0; i < option.length; i++) {
    option[i].setAttribute("onclick", "optionSelected(this)");
  }
}
```

### Always make sure your files can interact with eachother. If you ever have any issues with linking and calling a file, make sure they are set up properly before you submit your code.

---

## 4. Explanation of The Codebase in Each of the Relevant Files:

### Lets Start with our HTML! (Index.html)

Step 1: Start off by making sure your external files are linked properly (CSS, JS, etc.)

```html
<!-- CSS FILE -->
<link rel="stylesheet" href="css/style.css" />
<!-- This is my personal font awesome kit code. you will have to add your own after you register with email-->
<script
  src="https://kit.fontawesome.com/4a4f4b55b0.js"
  crossorigin="anonymous"
></script>

<!-- Add questions list -->
<script src="js/questions.js" defer></script>

<!-- Main logic of the app -->
<script src="js/quizApp.js" defer></script>
```

Step 2: Create the initial starting screen with a button for the user to start the quiz

```html
<!-- Quiz START Button -->
<div class="start_btn"><button>Start Quiz</button></div>
```

Step 3: Create an introduction screen that will explain the rules of the quiz to the user to make sure they understand how to proceed and what they can expect from the quiz

- It is also very important to make sure the user has a button to quit the quiz if they aren't ready, and a button to continue if they are ready.

```html
<!-- Instruction box wrapper -->
<div class="info_box">
  <div class="info-title"><span>Some Rules of this Quiz</span></div>
  <div class="info-list">
    <div class="info">
      1. You will have only <span>15 seconds</span> per each question.
    </div>
    <div class="info">2. Once you select your answer, it can't be undone.</div>
    <div class="info">3. You can't select any option once time goes off.</div>
    <div class="info">
      4. You can't exit from the Quiz while you're playing.
    </div>
    <div class="info">
      5. You'll get points on the basis of your correct answers.
    </div>
  </div>
  <div class="buttons">
    <button class="quit">Exit Quiz</button>
    <button class="restart">Continue</button>
  </div>
</div>
```

Step 4: Create your quiz box that will hold all the contents for each of the five questions of the quiz

- This should be broken down with a header, a body section, and a footer.
- This will help you as a designer breakdown where your elements will go that you implement in later with javascript.

```html
<!-- Quiz Box -->
<div class="quiz_box">
  <header>
    <div class="title">Demo Quiz App in JavaScript</div>
    <div class="timer">
      <div class="time_left_txt">Time Left</div>
      <div class="timer_sec">15</div>
    </div>
    <div class="time_line"></div>
  </header>
  <section>
    <div class="que_text">
      <!-- Insert questions from ./js/questions.js -->
    </div>
    <div class="option_list">
      <!-- Insert options to questions from ./js/questions.js -->
    </div>
  </section>

  <!-- footer of Quiz Box -->
  <footer>
    <div class="total_que">
      <!-- insert Question Count Number dynamically from JavaScript App logic -->
    </div>
    <button class="next_btn">Next Que</button>
  </footer>
</div>
```

Step 5: Create a result box to notify the user of their score at the end of the quiz

- The text you provide based on their score will be added in later with javascript

```html
<!-- Result Box -->
<div class="result_box">
  <div class="icon">
    <i class="fas fa-crown"></i>
  </div>
  <div class="complete_text">You've completed the Quiz!</div>
  <div class="score_text">
    <!-- insert dynamic user score as Result from JavaScript -->
  </div>
  <div class="buttons">
    <button class="restart">Replay Quiz</button>
    <button class="quit">Quit Quiz</button>
  </div>
</div>
```

---

### Styling our project with CSS (style.css)

Step 1: We always want to start off by importing any fonts you would like to use on your project and always reset the browser defaults

```css
/* importing google fonts */
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@200;300;400;500;600;700&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}
```

Step 2: Next we want to style the background color our app will have throughout the full application

- Always make sure when choosing colors to find ones that compliment eachother for readability purposes

```css
body {
  background: #a020f0;
}
```

Step 3: Style the button on the "Start Quiz" screen, again being careful of color choice and how you center it

```css
::selection {
  color: #fff;
  background: #a020f0;
}

.start_btn,
.info_box,
.quiz_box,
.result_box {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
```

Step 4: Moving on to your info box that you will show the user before the quiz start

- Make sure it's easily readable because this is the most important part of the quiz
- Make sure the user knows what button they are choosing by adding some transitions onto them

```css
.info_box.activeInfo,
.quiz_box.activeQuiz,
.result_box.activeResult {
  opacity: 1;
  z-index: 5;
  pointer-events: auto;
  transform: translate(-50%, -50%) scale(1);
}

.start_btn button {
  font-size: 25px;
  font-weight: 500;
  color: #a020f0;
  padding: 15px 30px;
  outline: none;
  border: none;
  border-radius: 5px;
  background: #fff;
  cursor: pointer;
}

.info_box {
  width: 540px;
  background: #fff;
  border-radius: 5px;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.info_box .info-title {
  height: 60px;
  width: 100%;
  border-bottom: 1px solid lightgrey;
  display: flex;
  align-items: center;
  padding: 0 30px;
  border-radius: 5px 5px 0 0;
  font-size: 20px;
  font-weight: 600;
}

.info_box .info-list {
  padding: 15px 30px;
}

.info_box .info-list .info {
  margin: 5px 0;
  font-size: 17px;
}

.info_box .info-list .info span {
  font-weight: 600;
  color: #a020f0;
}
.info_box .buttons {
  height: 60px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding: 0 30px;
  border-top: 1px solid lightgrey;
}

.info_box .buttons button {
  margin: 0 5px;
  height: 40px;
  width: 100px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  border: none;
  outline: none;
  border-radius: 5px;
  border: 1px solid #a020f0;
  transition: all 0.3s ease;
}
```

Step 5: Style how the quiz question screens will look to the user

- Remember to keep things easy to read
- Add effects with the selections and correctness symbols, but nothing too flashy or crazy

- Start with all your header elements

```css
.quiz_box {
  width: 550px;
  background: #fff;
  border-radius: 5px;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.quiz_box header {
  position: relative;
  z-index: 2;
  height: 70px;
  padding: 0 30px;
  background: #fff;
  border-radius: 5px 5px 0 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0px 3px 5px 1px rgba(0, 0, 0, 0.1);
}

.quiz_box header .title {
  font-size: 20px;
  font-weight: 600;
}

.quiz_box header .timer {
  color: #682a8f;
  background: #cce5ff;
  border: 1px solid #b8daff;
  height: 45px;
  padding: 0 8px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 145px;
}

.quiz_box header .timer .time_left_txt {
  font-weight: 400;
  font-size: 17px;
  user-select: none;
}

.quiz_box header .timer .timer_sec {
  font-size: 18px;
  font-weight: 500;
  height: 30px;
  width: 45px;
  color: #fff;
  border-radius: 5px;
  line-height: 30px;
  text-align: center;
  background: #343a40;
  border: 1px solid #343a40;
  user-select: none;
}

.quiz_box header .time_line {
  position: absolute;
  bottom: 0px;
  left: 0px;
  height: 3px;
  background: #a020f0;
}
```

- Then move onto your body section elements

```css
section {
  padding: 25px 30px 20px 30px;
  background: #fff;
}

section .que_text {
  font-size: 25px;
  font-weight: 600;
}

section .option_list {
  padding: 20px 0px;
  display: block;
}

section .option_list .option {
  background: aliceblue;
  border: 1px solid #84c5fe;
  border-radius: 5px;
  padding: 8px 15px;
  font-size: 17px;
  margin-bottom: 15px;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

section .option_list .option:last-child {
  margin-bottom: 0px;
}

section .option_list .option:hover {
  color: #601391;
  background: #cce5ff;
  border: 1px solid #b8daff;
}

section .option_list .option.correct {
  color: #155724;
  background: #d4edda;
  border: 1px solid #c3e6cb;
}

section .option_list .option.incorrect {
  color: #721c24;
  background: #f8d7da;
  border: 1px solid #f5c6cb;
}

section .option_list .option.disabled {
  pointer-events: none;
}

section .option_list .option .icon {
  height: 26px;
  width: 26px;
  border: 2px solid transparent;
  border-radius: 50%;
  text-align: center;
  font-size: 13px;
  pointer-events: none;
  transition: all 0.3s ease;
  line-height: 24px;
}
.option_list .option .icon.tick {
  color: #23903c;
  border-color: #23903c;
  background: #d4edda;
}

.option_list .option .icon.cross {
  color: #a42834;
  background: #f8d7da;
  border-color: #a42834;
}
```

- Lastly move onto your footer elements

```css
footer {
  height: 60px;
  padding: 0 30px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-top: 1px solid lightgrey;
}

footer .total_que span {
  display: flex;
  user-select: none;
}

footer .total_que span p {
  font-weight: 500;
  padding: 0 5px;
}

footer .total_que span p:first-child {
  padding-left: 0px;
}

footer button {
  height: 40px;
  padding: 0 13px;
  font-size: 18px;
  font-weight: 400;
  cursor: pointer;
  border: none;
  outline: none;
  color: #fff;
  border-radius: 5px;
  background: #a020f0;
  border: 1px solid #a020f0;
  line-height: 10px;
  opacity: 0;
  pointer-events: none;
  transform: scale(0.95);
  transition: all 0.3s ease;
}

footer button:hover {
  background: #7803c0;
}

footer button.show {
  opacity: 1;
  pointer-events: auto;
  transform: scale(1);
}
```

Step 6: Style your results page, make sure everything is readable and the buttons are clear they are being pressed

```css
.result_box {
  background: #fff;
  border-radius: 5px;
  display: flex;
  padding: 25px 30px;
  width: 450px;
  align-items: center;
  flex-direction: column;
  justify-content: center;
  transform: translate(-50%, -50%) scale(0.9);
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s ease;
}

.result_box .icon {
  font-size: 100px;
  color: #a020f0;
  margin-bottom: 10px;
}

.result_box .complete_text {
  font-size: 20px;
  font-weight: 500;
}

.result_box .score_text span {
  display: flex;
  margin: 10px 0;
  font-size: 18px;
  font-weight: 500;
}

.result_box .score_text span p {
  padding: 0 4px;
  font-weight: 600;
}

.result_box .buttons {
  display: flex;
  margin: 20px 0;
}

.result_box .buttons button {
  margin: 0 10px;
  height: 45px;
  padding: 0 20px;
  font-size: 18px;
  font-weight: 500;
  cursor: pointer;
  border: none;
  outline: none;
  border-radius: 5px;
  border: 1px solid #a020f0;
  transition: all 0.3s ease;
}

.buttons button.restart {
  color: #fff;
  background: #a020f0;
}

.buttons button.restart:hover {
  background: #8304d1;
}

.buttons button.quit {
  color: #a020f0;
  background: #fff;
}

.buttons button.quit:hover {
  color: #fff;
  background: #8604d6;
}
```

### Creating the questions using Javascript (questions.js)

Step 1: Here we will be creating an array of objects that will be each of the five questions asked to the user

- Each object will consist of the following members: question number, questions, options, and answers.

```javascript
// creating an array of objects
// Each object consists of the following members: question number, questions, options, and answers
let questions = [
  {
    numb: 1,
    question: "What does HTML stand for?",
    answer: "Hyper Text Markup Language",
    options: [
      "Hyper Text Preprocessor",
      "Hyper Text Markup Language",
      "Hyper Text Multiple Language",
      "Hyper Tool Multi Language",
    ],
  },
  {
    numb: 2,
    question: "What does CSS stand for?",
    answer: "Cascading Style Sheet",
    options: [
      "Common Style Sheet",
      "Colorful Style Sheet",
      "Computer Style Sheet",
      "Cascading Style Sheet",
    ],
  },
  {
    numb: 3,
    question: "What does PHP stand for?",
    answer: "Hypertext Preprocessor",
    options: [
      "Hypertext Preprocessor",
      "Hypertext Programming",
      "Hypertext Preprogramming",
      "Hometext Preprocessor",
    ],
  },
  {
    numb: 4,
    question: "What does SQL stand for?",
    answer: "Structured Query Language",
    options: [
      "Stylish Question Language",
      "Stylesheet Query Language",
      "Statement Question Language",
      "Structured Query Language",
    ],
  },
  {
    numb: 5,
    question: "What does XML stand for?",
    answer: "eXtensible Markup Language",
    options: [
      "eXtensible Markup Language",
      "eXecutable Multiple Language",
      "eXTra Multi-Program Language",
      "eXamine Multiple Language",
    ],
  },
  // Duplicate the object declaration and initialization above for more questions
];
```

### Setting up all the dynamic aspects of the app for interaction purposes

Step 1: Set up variables for every single required element from the HTML Document

```javascript
//selecting all required elements from html document
const start_btn = document.querySelector(".start_btn button");
const info_box = document.querySelector(".info_box");
const exit_btn = info_box.querySelector(".buttons .quit");
const continue_btn = info_box.querySelector(".buttons .restart");
const quiz_box = document.querySelector(".quiz_box");
const result_box = document.querySelector(".result_box");
const restart_quiz = result_box.querySelector(".buttons .restart");
const quit_quiz = result_box.querySelector(".buttons .quit");
const option_list = document.querySelector(".option_list");
const time_line = document.querySelector("header .time_line");
const timeText = document.querySelector(".timer .time_left_txt");
const timeCount = document.querySelector(".timer .timer_sec");
const next_btn = document.querySelector("footer .next_btn");
const bottom_ques_counter = document.querySelector("footer .total_que");
```

Step 2: Set up the options for if the user is ready to take the quiz, or not. And if they are and are ready continue, then start the quiz.

- If the user is ready and hits the startQuiz button, we need to show the instructions to the quiz.

```javascript
// if startQuiz button clicked
start_btn.addEventListener("click", (e) => {
  info_box.classList.add("activeInfo"); //show info box
});
```

- Along side that, if the user is not ready and hits the exitQuiz button we will hide the instructions.

```javascript
// if exitQuiz button clicked
exit_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
});
```

- Once the user is ready, they will click the continue button and we will display the first question to them.

```javascript
// if continueQuiz button clicked
continue_btn.addEventListener("click", (e) => {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.add("activeQuiz"); //show quiz box
  showQuetions(0); //calling showQestions function
  queCounter(1); //passing 1 parameter to queCounter
  startTimer(15); //calling startTimer function
  startTimerLine(0); //calling startTimerLine function
});
```

Step 3: Set up the variables for the values you will need to pull from the quiz to control the operations

```javascript
// Variables to control quiz operations
let timeValue = 15;
let que_count = 0;
let que_numb = 1;
let userScore = 0;
let counter;
let counterLine;
let widthValue = 0;
```

Step 4: Once the user is finished with the quiz, let them decide whether they want to continue or quit the quiz.

- If a user wants to reset the quiz, make sure to clear the form, reset the variables, and show the quiz from question one again.

```javascript
// if restartQuiz button clicked
restart_quiz.addEventListener("click", (e) => {
  quiz_box.classList.add("activeQuiz"); //show quiz box
  result_box.classList.remove("activeResult"); //hide result box
  timeValue = 15;
  que_count = 0;
  que_numb = 1;
  userScore = 0;
  widthValue = 0;
  showQuetions(que_count); //calling showQestions function
  queCounter(que_numb); //passing que_numb value to queCounter
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  startTimer(timeValue); //calling startTimer function
  startTimerLine(widthValue); //calling startTimerLine function
  timeText.textContent = "Time Left"; //change the text of timeText to Time Left
  next_btn.classList.remove("show"); //hide the next button
});
```

- If a user wants to quit the quiz, return them back to the original screen with the "Start Quiz" button.

```javascript
// if quitQuiz button clicked
quit_quiz.addEventListener("click", (e) => {
  window.location.reload(); //reload the current window
});
```

Step 5: As the user goes through the quiz, they have to click the "Next Que" button to go onto the next question.

- Each time this button is clicked, we have to check to make sure that the que count is the less than or equal to five since there are only 5 questions
- With that, we have to increment the variables we set for que count, the que number, and show the next question

```javascript
// if Next Question button is clicked
next_btn.addEventListener("click", (e) => {
  //check if it does not exceed max questions
  if (que_count < questions.length - 1) {
    que_count++; //increment the que_count value
    que_numb++; //increment the que_numb value
    showQuetions(que_count); //calling showQestions function
    queCounter(que_numb); //passing que_numb value to queCounter
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    startTimer(timeValue); //calling startTimer function
    startTimerLine(widthValue); //calling startTimerLine function
    timeText.textContent = "Time Left"; //change the timeText to Time Left
    next_btn.classList.remove("show"); //hide the next button
  } else {
    clearInterval(counter); //clear counter
    clearInterval(counterLine); //clear counterLine
    showResult(); //calling showResult function
  }
});
```

Step 5: Now that the basic layout of the app is set up, we need a function that will get the options to our questions that we wrote in our questions.js file

- If at any point you want to increase the number of options in your quiz questions, you must go into this function and add them in manually

```javascript
    // getting questions and options from array
// If you have lesser or more number of options you need to change this code
function showQuetions(index) {
  const que_text = document.querySelector(".que_text");

  //creating a new span and div tag for question and option and passing the value using array index
  // self exercise: re-write the following using backtick syntax for string in JS instead of concatenation operator
  let que_tag =
    "<span>" +
    questions[index].numb +
    ". " +
    questions[index].question +
    "</span>";
  let option_tag =
    '<div class="option"><span>' +
    questions[index].options[0] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[1] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[2] +
    "</span></div>" +
    '<div class="option"><span>' +
    questions[index].options[3] +
    "</span></div>";
  que_text.innerHTML = que_tag; //adding new (child) span tag inside que_tag
  option_list.innerHTML = option_tag; //adding new (child) div tag inside option_tag
```

Step 6: Set up the display for when a user answers the question correctly or incorrectly

```javascript
const option = option_list.querySelectorAll(".option");

// set onclick attribute to all available options
for (i = 0; i < option.length; i++) {
  option[i].setAttribute("onclick", "optionSelected(this)");
}
```

- If the users answer is correct they will receive a green tick, and if it's incorrect they will receive a red cross

```javascript
// create new div tags for right or wrong tick icons
let tickIconTag = '<div class="icon tick"><i class="fas fa-check"></i></div>';
let crossIconTag = '<div class="icon cross"><i class="fas fa-times"></i></div>';
```

Step 7: Create the function to stop the timer and check the users answer once they've selected one

- Firstly you would want to stop the timer and the timer bar and collect their option's value

```javascript
    //if user clicked on option
function optionSelected(answer) {
  clearInterval(counter); //clear counter
  clearInterval(counterLine); //clear counterLine
  let userAns = answer.textContent; //getting user selected option
  let correcAns = questions[que_count].answer; //getting correct answer from array
  const allOptions = option_list.children.length; //getting all option items
```

- Now you want to run their selected option through the array to see if it is equal to the array's correct answer
- If it is correct you will show they've selected the right decision by displaying the green check next to the answer
- If it is incorrect, you will they've selected the wrong decision by displaying the red check

```javascript
//if user selected option is equal to array's correct answer
if (userAns == correcAns) {
  userScore += 1; //update total score value increment by 1
  answer.classList.add("correct"); //add green color to correct selected option
  answer.insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to correct selected option
  console.log("Correct Answer");
  console.log("Your correct answers = " + userScore);
} else {
  answer.classList.add("incorrect"); //add red color to correct selected option
  answer.insertAdjacentHTML("beforeend", crossIconTag); //add cross icon to correct selected option
  console.log("Wrong Answer");

  for (i = 0; i < allOptions; i++) {
    if (option_list.children[i].textContent == correcAns) {
      //if there is an option which is matched to an array answer
      option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
      option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
      console.log("Auto selected correct answer.");
    }
  }
}
```

- Lastly, since we don't want the user to be able to select any options once their decision is made, we want to also disable all the options from being selected

```javascript
   for (i = 0; i < allOptions; i++) {
    option_list.children[i].classList.add("disabled"); //once user select an option, disable all options
  }
  next_btn.classList.add("show"); //show the next button if user selected any option
}
```

Step 8: At the end of the quiz we want to display the results to the user, which we'll need a function for

```javascript
// display for result box based on user performance
function showResult() {
  info_box.classList.remove("activeInfo"); //hide info box
  quiz_box.classList.remove("activeQuiz"); //hide quiz box
  result_box.classList.add("activeResult"); //show result box
  const scoreText = result_box.querySelector(".score_text");
  if (userScore > 3) {
    // if user scored more than 3
    //create a new span tag and pass the user score number and total question number
    let scoreTag =
      "<span>and congrats! , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag; //add new span tag inside score_Text
  } else if (userScore > 1) {
    // if user scored more than 1
    let scoreTag =
      "<span>and nice , You got <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  } else {
    // if user scored less than 1
    let scoreTag =
      "<span>and sorry , You got only <p>" +
      userScore +
      "</p> out of <p>" +
      questions.length +
      "</p></span>";
    scoreText.innerHTML = scoreTag;
  }
}
```

Step 9: During the game, we have information to give the user an update on time left with a timer, and a status bar, along with what question they are on by showing the que

- Setting up the timer

```javascript
// control the timer and actions associated to it
function startTimer(time) {
  counter = setInterval(timer, 1000);
  function timer() {
    timeCount.textContent = time; //change the value of timeCount with time value
    time--; //decrement the time value
    if (time < 9) {
      //if timer is less than 9
      let addZero = timeCount.textContent;
      timeCount.textContent = "0" + addZero; //add a 0 before time value
    }
    if (time < 0) {
      //if timer is less than 0
      clearInterval(counter); //clear counter
      timeText.textContent = "Time Off"; //change the time text to time off
      const allOptions = option_list.children.length; //get all option items
      let correcAns = questions[que_count].answer; //get correct answer from array
      for (i = 0; i < allOptions; i++) {
        if (option_list.children[i].textContent == correcAns) {
          //if there is an option which is matched to an array answer
          option_list.children[i].setAttribute("class", "option correct"); //add green color to matched option
          option_list.children[i].insertAdjacentHTML("beforeend", tickIconTag); //add tick icon to matched option
          console.log("Time Off: Auto selected correct answer.");
        }
      }
      for (i = 0; i < allOptions; i++) {
        option_list.children[i].classList.add("disabled"); //once user select an option then disabled all options
      }
      next_btn.classList.add("show"); //show the next button if user selected any option
    }
  }
}
```

- Setting up the progress bar with the value mirroring the timer

```javascript
// Shows a progress bar mirroring timer value left
function startTimerLine(time) {
  counterLine = setInterval(timer, 29);
  function timer() {
    time += 1; //upgrading time value with 1
    time_line.style.width = time + "px"; //increasing width of time_line with px by time value
    if (time > 549) {
      //if time value is greater than 549
      clearInterval(counterLine); //clear counterLine
    }
  }
}
```

- Setting up the que

```javascript
function queCounter(index) {
  //creating a new span tag and passing the question number and total question
  let totalQueCounTag =
    "<span><p>" +
    index +
    "</p> of <p>" +
    questions.length +
    "</p> Questions</span>";
  bottom_ques_counter.innerHTML = totalQueCounTag; //adding new span tag inside bottom_ques_counter
}
```

## Congratulations. You've successfully set up a Quiz App Demo!
