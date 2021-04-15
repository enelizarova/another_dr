<template>
  <div>
    <div class="snake">
      &#129472;
      <div class="header">
        <h1>Snake JavaScript</h1>
      </div>
      <div class="details">
        <span class="new_Score"
          >Current Score: <span class="score_Number">0</span></span
        >
        <span class="best_Score"
          >Best Score: <span class="best_Score_Number">0</span></span
        >
      </div>
      <div class="game" ref="game">
        <div class="container"></div>
      </div>
    </div>
    <div class="end">
      <div class="bkColor"></div>
      <div class="fail">
        <img
          class="img"
          src="https://mohamedelghandour.github.io/Snake/img/game%20over.png"
          alt=""
        />
      </div>
    </div>
    <div class="warning-rotate">
      <h1>try to rotate the phone to play with The best performance.</h1>
    </div>
    <div class="hidden-Sound">
      <audio class="eat-sound" controls>
        <source
          src="https://mohamedelghandour.github.io/Snake/sound/eat.ogg"
          type="audio/ogg"
        />
        <source
          src="https://mohamedelghandour.github.io/Snake/sound/eat.wav"
          type="audio/mpeg"
        />
        Your browser does not support the audio element.
      </audio>
      <audio class="die-sound" controls>
        <source
          src="https://mohamedelghandour.github.io/Snake/sound/gameover.mp3"
          type="audio/mpeg"
        />
        Your browser does not support the audio element.
      </audio>
      <audio class="background-sound" controls loop>
        <source
          src="https://mohamedelghandour.github.io/Snake/sound/backgroundmusic.mp3"
          type="audio/ogg"
        />
        Your browser does not support the audio element.
      </audio>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    this.snakeGame()
  },

  methods: {
    snakeGame() {
      const game = document.querySelector('.container'),
        pexelNumber = (625 / 25) * (400 / 25);

        console.log(game)
      var arr = [],
        gameIsWorked = true;
      var arrSnakeBody = [],
        newNum,
        indexScore = 0;
      //create Pexel
      for (var index = 0; index < pexelNumber; index++) {
        var pexel = document.createElement("div");
        pexel.setAttribute("number", index);
        if (index % 2 == 0)
          // even
          pexel.classList.add("box", "even");
        // odd
        else pexel.classList.add("box", "odd");
        arr.push(pexel);
        game.appendChild(pexel);
      }
      //create snake
      var snake = document.createElement("div"),
        randomArrItem = Math.round(Math.random() * arr.length);
      snake.classList.add("snake_Head");
      function createSnake(snake, randomArrItem) {
        snake.setAttribute("number", randomArrItem);
        snake.classList.add("snake_Box");
        game.appendChild(snake);
        snake.style.left = ` ${arr[snake.getAttribute("number")].offsetLeft}px`;
        snake.style.top = ` ${arr[snake.getAttribute("number")].offsetTop}px`;
        return snake;
      }
      createSnake(snake, randomArrItem);

      //move Snake
      window.addEventListener("keydown", function (e) {
        snakeMove(e);
      });

      // Class Interval
      function Interval(fn, time) {
        var timer = false;
        this.start = function () {
          if (!this.isRunning()) timer = setInterval(fn, time);
        };
        this.stop = function () {
          clearInterval(timer);
          timer = false;
        };
        this.isRunning = function () {
          return timer !== false;
        };
      }

      var arrowValRFN = new Interval(arrowValR, 250),
        arrowValLFN = new Interval(arrowValL, 250),
        arrowValUFN = new Interval(arrowValU, 250),
        arrowValDFN = new Interval(arrowValD, 250),
        arrFN = [arrowValRFN, arrowValLFN, arrowValUFN, arrowValDFN];

      // create fruit
      var fruit = document.createElement("img"),
        testFruit = true;
      function createFruit() {
        var randomfruit = Math.round(Math.random() * arr.length);
        fruit.classList.add("fruit");
        while (testFruit) {
          if (arrSnakeBody.length) {
            if (randomfruit == randomArrItem) {
              testFruit = true;
              randomfruit = Math.round(Math.random() * arr.length);
            } else {
              testFruit = false;
            }
            arrSnakeBody.forEach((element) => {
              testFruit = false;
              if (parseInt(element.getAttribute("number")) == randomfruit) {
                testFruit = true;
                randomfruit = Math.round(Math.random() * arr.length);
              }
            });
          } else {
            if (randomfruit == randomArrItem) {
              testFruit = true;
              randomfruit = Math.round(Math.random() * arr.length);
            } else {
              testFruit = false;
            }
          }
        }
        fruit.setAttribute("number", randomfruit);
        var randomFruitImg = Math.round(Math.random() * 8) + 1,
          imgSrc = `img/${randomFruitImg}.png`;
        fruit.setAttribute("src", imgSrc);
        fruit.setAttribute(
          "src",
          "https://mohamedelghandour.github.io/Snake/" + imgSrc
        );
        game.appendChild(fruit);
        fruit.style.left = ` ${arr[fruit.getAttribute("number")].offsetLeft}px`;
        fruit.style.top = ` ${arr[fruit.getAttribute("number")].offsetTop}px`;
      }
      createFruit();

      function snakeMove(e) {
        if (gameIsWorked) {
          document.querySelector(".background-sound").play();
          document.querySelector(".background-sound").volume = 0.1;
          if (arrSnakeBody.length) {
            switch (e.which) {
              case 39: // Arrow Right
                arrowRightProcessCaseOne();
                break;
              case 37: // Arrow Left
                arrowLeftProcessCaseOne();
                break;
              case 38: // Arrow Up
                arrowUpProcessCaseOne();
                break;
              case 40: // Arrow Down
                arrowDownProcessCaseOne();
                break;
              default:
                break;
            }
          } else {
            switch (e.which) {
              case 39: // Arrow Right
                arrowRightProcessCaseTwo();
                break;
              case 37: // Arrow Left
                arrowLeftProcessCaseTwo();
                break;
              case 38: // Arrow Up
                arrowUpProcessCaseTwo();
                break;
              case 40: // Arrow Down
                arrowDownProcessCaseTwo();
                break;
              default:
                break;
            }
          }
        }
      }

      document.querySelectorAll(".btn").forEach((element) => {
        element.addEventListener("click", function () {
          if (gameIsWorked) {
            document.querySelector(".background-sound").play();
            document.querySelector(".background-sound").volume = 0.1;
            if (arrSnakeBody.length) {
              switch (parseInt(element.getAttribute("data-num"))) {
                case 3: // Arrow Right
                  arrowRightProcessCaseOne();
                  break;
                case 2: // Arrow Left
                  arrowLeftProcessCaseOne();
                  break;
                case 1: // Arrow Up
                  arrowUpProcessCaseOne();
                  break;
                case 4: // Arrow Down
                  arrowDownProcessCaseOne();
                  break;
                default:
                  break;
              }
            } else {
              switch (parseInt(element.getAttribute("data-num"))) {
                case 3: // Arrow Right
                  arrowRightProcessCaseTwo();
                  break;
                case 2: // Arrow Left
                  arrowLeftProcessCaseTwo();
                  break;
                case 1: // Arrow Up
                  arrowUpProcessCaseTwo();
                  break;
                case 4: // Arrow Down
                  arrowDownProcessCaseTwo();
                  break;
                default:
                  break;
              }
            }
          }
        });
      });

      function arrowRightProcessCaseOne() {
        if (!arrowValRFN.isRunning()) {
          if (arrowValLFN.isRunning()) {
            console.log("Dimensions cannot be reversed");
          } else {
            arrFN.forEach((element) => {
              if (element.isRunning()) element.stop();
            });
            arrowValR();
            arrowValRFN.start();
          }
        }
      }
      function arrowLeftProcessCaseOne() {
        if (!arrowValLFN.isRunning()) {
          if (arrowValRFN.isRunning()) {
            console.log("Dimensions cannot be reversed");
          } else {
            arrFN.forEach((element) => {
              if (element.isRunning()) element.stop();
            });
            arrowValL();
            arrowValLFN.start();
          }
        }
      }
      function arrowUpProcessCaseOne() {
        if (!arrowValUFN.isRunning()) {
          if (arrowValDFN.isRunning()) {
            console.log("Dimensions cannot be reversed");
          } else {
            arrFN.forEach((element) => {
              if (element.isRunning()) element.stop();
            });
            arrowValU();
            arrowValUFN.start();
          }
        }
      }
      function arrowDownProcessCaseOne() {
        if (!arrowValDFN.isRunning()) {
          if (arrowValUFN.isRunning()) {
            console.log("Dimensions cannot be reversed");
          } else {
            arrFN.forEach((element) => {
              if (element.isRunning()) element.stop();
            });
            arrowValD();
            arrowValDFN.start();
          }
        }
      }

      function arrowRightProcessCaseTwo() {
        if (!arrowValRFN.isRunning()) {
          arrFN.forEach((element) => {
            if (element.isRunning()) element.stop();
          });
          arrowValR();
          arrowValRFN.start();
        }
      }

      function arrowLeftProcessCaseTwo() {
        if (!arrowValLFN.isRunning()) {
          arrFN.forEach((element) => {
            if (element.isRunning()) element.stop();
          });
          arrowValL();
          arrowValLFN.start();
        }
      }

      function arrowUpProcessCaseTwo() {
        if (!arrowValUFN.isRunning()) {
          arrFN.forEach((element) => {
            if (element.isRunning()) element.stop();
          });
          arrowValU();
          arrowValUFN.start();
        }
      }

      function arrowDownProcessCaseTwo() {
        if (!arrowValDFN.isRunning()) {
          arrFN.forEach((element) => {
            if (element.isRunning()) element.stop();
          });
          arrowValD();
          arrowValDFN.start();
        }
      }

      // create the snake body

      // function setCookie(name, value, expires) {
      //     document.cookie = name + "=" + value + ((expires == null) ? "" : ";expires=" + expires.toGMTString())
      // }
      // function getCookie(name) {
      //     const value = `; ${document.cookie}`;
      //     const parts = value.split(`; ${name}=`);
      //     if (parts.length === 2) return parts.pop().split(';').shift();
      // }
      // if (getCookie("best_Score_Number") == undefined) {
      //     setCookie("best_Score_Number", indexScore);
      //     document.querySelector('.best_Score_Number').innerText = getCookie("best_Score_Number");
      // } else {
      //     document.querySelector('.best_Score_Number').innerText = getCookie("best_Score_Number");
      // }

      if (localStorage.getItem("best_Score_Number") == undefined) {
        localStorage.setItem("best_Score_Number", indexScore);
        document.querySelector(
          ".best_Score_Number"
        ).innerText = localStorage.getItem("best_Score_Number");
        document.querySelector(".score_Number").innerText = indexScore;
      } else {
        document.querySelector(
          ".best_Score_Number"
        ).innerText = localStorage.getItem("best_Score_Number");
        document.querySelector(".score_Number").innerText = indexScore;
      }

      function moved(number) {
        if (arrSnakeBody.length) {
          arrSnakeBody.forEach((element) => {
            if (parseInt(element.getAttribute("number")) == number) {
              console.log("LOSE");
              gameIsWorked = false;
              console.log("from function moved(number)");
              fail();
            }
          });
        }
        snake.style.left = ` ${
          arr[parseInt(snake.getAttribute("number"))].offsetLeft
        }px`;
        snake.style.top = ` ${
          arr[parseInt(snake.getAttribute("number"))].offsetTop
        }px`;
        if (
          arr[parseInt(snake.getAttribute("number"))] ==
          arr[parseInt(fruit.getAttribute("number"))]
        ) {
          document.querySelector(".eat-sound").play();
          createFruit();
          var snakeBody = document.createElement("div"),
            snakeBodyItem = createSnake(
              snakeBody,
              parseInt(snake.getAttribute("number"))
            );
          arrSnakeBody.push(snakeBodyItem);
          document.querySelector(".score_Number").innerText = ++indexScore;
          // if (parseInt(getCookie("best_Score_Number")) < parseInt(document.querySelector('.score_Number').innerText)) {
          //     setCookie("best_Score_Number", indexScore);
          //     document.querySelector('.best_Score_Number').innerText = getCookie("best_Score_Number");
          // }
          if (
            parseInt(localStorage.getItem("best_Score_Number")) <
            parseInt(document.querySelector(".score_Number").innerText)
          ) {
            localStorage.setItem("best_Score_Number", indexScore);
            document.querySelector(
              ".best_Score_Number"
            ).innerText = localStorage.getItem("best_Score_Number");
          }
        }
        if (arrSnakeBody.length) {
          newNum = snakeBodyMove(number, arrSnakeBody[0]);
        }
        if (arrSnakeBody.length > 1) {
          for (var index = 1; index < arrSnakeBody.length; index++) {
            newNum = snakeBodyMove(newNum, arrSnakeBody[index]);
          }
        }
      }
      function snakeBodyMove(number, tail) {
        tail.style.left = ` ${arr[number].offsetLeft}px`;
        tail.style.top = ` ${arr[number].offsetTop}px`;
        var lastNum = parseInt(tail.getAttribute("number"));
        tail.setAttribute("number", number);
        return lastNum;
      }
      const onEndBtnClicK = (e) => {
        document.querySelector(".end").style.transform = "scale(0)";
        document.querySelector(".container").remove();
        var gameEle = document.createElement("div");
        var snakeEle = document.querySelector(".game");
        gameEle.classList.add("container");
        snakeEle.appendChild(gameEle);
        this.snakeGame();
        document
          .querySelector(".end")
          .removeEventListener("click", onEndBtnClicK);
      };
      const onEndKeyClick = (e) => {
        if (e.which == 13) {
          document.querySelector(".end").style.transform = "scale(0)";
          document.querySelector(".container").remove();
          var gameEle = document.createElement("div");
          var snakeEle = document.querySelector(".game");
          gameEle.classList.add("container");
          snakeEle.appendChild(gameEle);
          snakeGame();
          window.removeEventListener("keydown", onEndKeyClick);
        }
      };

      function fail() {
        document.querySelector(".end").style.transform = "scale(1)";
        document.querySelector(".background-sound").pause();
        document.querySelector(".die-sound").play();
        document.querySelector(".end").addEventListener("click", onEndBtnClicK);
        window.addEventListener("keydown", onEndKeyClick);
        arrFN.forEach((element) => {
          if (element.isRunning()) element.stop();
        });
      }

      function arrowValR() {
        if ((parseInt(snake.getAttribute("number")) + 1) % 25 == 0) {
          console.log("LOSE");
          gameIsWorked = false;
          fail();
          console.log("from function arrowValR()");
        } else {
          var lastNum = parseInt(snake.getAttribute("number"));
          snake.setAttribute(
            "number",
            parseInt(snake.getAttribute("number")) + 1
          );
          moved(lastNum);
        }
      }

      function arrowValL() {
        if (parseInt(snake.getAttribute("number")) % 25 == 0) {
          console.log("LOSE");
          gameIsWorked = false;
          fail();
          console.log("from function arrowValL()");
        } else {
          var lastNum = parseInt(snake.getAttribute("number"));
          snake.setAttribute(
            "number",
            parseInt(snake.getAttribute("number")) - 1
          );
          moved(lastNum);
        }
      }

      function arrowValU() {
        if (parseInt(snake.getAttribute("number")) - 25 < 0) {
          console.log("LOSE");
          gameIsWorked = false;
          fail();
          console.log("from function arrowValU()");
        } else {
          var lastNum = parseInt(snake.getAttribute("number"));
          snake.setAttribute(
            "number",
            parseInt(snake.getAttribute("number")) - 25
          );
          moved(lastNum);
        }
      }

      function arrowValD() {
        if (parseInt(snake.getAttribute("number")) + 25 > 399) {
          console.log("LOSE");
          gameIsWorked = false;
          fail();
          console.log("from function arrowValD()");
        } else {
          var lastNum = parseInt(snake.getAttribute("number"));
          snake.setAttribute(
            "number",
            parseInt(snake.getAttribute("number")) + 25
          );
          moved(lastNum);
        }
      }
    },
  },
};
</script>

<style>
/* latin-ext */
@font-face {
  font-family: "Courier Prime";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: local("Courier Prime Regular"), local("CourierPrime-Regular"),
    url(https://fonts.gstatic.com/s/courierprime/v2/u-450q2lgwslOqpF_6gQ8kELaw9pWt_-.woff2)
      format("woff2");
  unicode-range: U+0100-024F, U+0259, U+1E00-1EFF, U+2020, U+20A0-20AB,
    U+20AD-20CF, U+2113, U+2C60-2C7F, U+A720-A7FF;
}
/* latin */
@font-face {
  font-family: "Courier Prime";
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: local("Courier Prime Regular"), local("CourierPrime-Regular"),
    url(https://fonts.gstatic.com/s/courierprime/v2/u-450q2lgwslOqpF_6gQ8kELawFpWg.woff2)
      format("woff2");
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA,
    U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215,
    U+FEFF, U+FFFD;
}

body,
html {
  margin: 0;
  padding: 0;
  background-color: #131417;
  color: white;
  text-align: center;
  position: relative;
}
@media only screen and (max-width: 850px) {
  /* For mobile phones: */
  .game {
    transform: scale(0.75);
  }
}
@media only screen and (max-width: 650px) {
  /* For mobile phones: */
  .warning-rotate {
    transform: scale(1) !important;
  }
  body,
  html {
    overflow: hidden;
  }
}
.snake .header h1 {
  font-family: "Courier Prime", monospace;
  font-size: 50px;
}
.snake .details {
  width: 400px;
  margin: auto;
  padding: 20px 0;
}
.snake .details .new_Score {
  float: left;
}
.snake .details .best_Score {
  float: right;
}
.snake .game {
  width: 627px;
  height: 402px;
  background-color: rgb(37 40 48);
  margin: auto;
  margin-top: 15px;
  overflow: auto;
  border: 1px solid #5a5f73;
  position: relative;
}
.snake .game .box {
  width: 25px;
  height: 25px;
  float: left;
  position: relative;
}
.snake .game .box.even {
  background-color: #131417;
}
.snake .game .box.odd {
  background-color: rgb(37 40 48);
}

.snake .game .snake_Box {
  background-color: #0078d4;
  position: absolute;
  width: 25px;
  height: 25px;
  z-index: 2;
}
.snake .game .snake_Head {
  transition: all 0.1s linear;
}

.snake .game .fruit {
  position: absolute;
  width: 25px;
  height: 25px;
}

.end {
  transform: scale(0);
  transition: all 0.2s linear;
  width: 100%;
  height: 100%;
  top: 0%;
  overflow: hidden;
  position: absolute;
  z-index: 30;
}

.fail {
  width: 400px;
  background-color: #131417;
  height: 250px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
.bkColor {
  width: 100%;
  height: 100%;
  background-color: rgba(19, 20, 23, 0.7);
  position: absolute;
  top: 0%;
  left: 0%;
  overflow: hidden;
}
.fail img {
  width: 70%;
  margin: auto;
  text-align: center;
  display: block;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  cursor: pointer;
}

.hidden-Sound {
  display: none;
}

.buttons {
  position: absolute;
  bottom: 0;
  left: 0;
  opacity: 0.5;
  transform: scale(1.5) translate(21px, -21px);
  z-index: 20;
}
.buttons button {
  padding: 5px;
  background-color: #455f73;
  border: none;
  margin: auto;
  color: white;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  z-index: 40;
}
button:focus {
  outline: none;
}
.buttons svg {
  width: 30px;
}
.buttons .rightBtn {
  margin-left: 15px;
}
.buttons .leftBtn {
  margin-right: 15px;
}

.warning-rotate {
  background-color: #131417;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  z-index: 30;
  transition: all 0.2s linear;
  transform: scale(0);
}

.warning-rotate h1 {
  text-align: center;
  font-size: x-large;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}



/* Tooltip text */
.tooltip .tooltiptext {
  visibility: hidden;
  width: 120px;
  background-color: #555;
  color: #fff;
  text-align: center;
  padding: 5px 0;
  font-family: "Courier Prime", monospace;
  border-radius: 6px;
  position: absolute;
  z-index: 1;
  bottom: 15%;
  right: 114%;
  opacity: 0;
  transition: opacity 0.5s;
}

/* Tooltip arrow */
.tooltip .tooltiptext::after {
  content: "";
  position: absolute;
  top: 39%;
  left: 104%;
  margin-left: -5px;
  border-width: 5px;
  border-style: solid;
  border-color: #555 transparent transparent transparent;
  transform: rotate(-90deg);
}

/* Show the tooltip text when you mouse over the tooltip container */
.tooltip:hover .tooltiptext {
  visibility: visible;
  opacity: 1;
}
</style>