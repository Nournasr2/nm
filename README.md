<!DOCTYPE html>
<html lang="ar">
<head>
  <meta charset="UTF-8">
  <title>لعبة كرة السلة</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: #f0f8ff;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      direction: rtl;
      font-family: 'Arial', sans-serif;
      overflow: hidden;
    }

    .court {
      width: 400px;
      height: 600px;
      background: #ffe6e6;
      border: 5px solid #d63384;
      border-radius: 20px;
      position: relative;
    }

    .basket {
      width: 100px;
      height: 20px;
      background: #d63384;
      position: absolute;
      top: 20px;
      left: 150px;
      border-radius: 10px;
    }

    .ball {
      width: 60px;
      height: 60px;
      background: orange;
      border-radius: 50%;
      position: absolute;
      bottom: 40px;
      left: 170px;
      cursor: grab;
      box-shadow: 0 0 10px #aaa;
    }

    #message {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(255, 255, 255, 0.9);
      padding: 30px;
      border-radius: 15px;
      font-size: 2em;
      color: #d63384;
      display: none;
      text-align: center;
    }
  </style>
</head>
<body>
  <div class="court">
    <div class="basket"></div>
    <div class="ball" draggable="true"></div>
    <div id="message">بحبك يا بنوتي</div>
  </div>

  <audio id="scoreSound">
    <source src="https://www.fesliyanstudios.com/play-mp3/387" type="audio/mpeg">
  </audio>

  <script>
    const ball = document.querySelector('.ball');
    const basket = document.querySelector('.basket');
    const message = document.getElementById('message');
    const sound = document.getElementById('scoreSound');

    let score = 0;

    ball.addEventListener('dragstart', (e) => {
      const style = window.getComputedStyle(e.target, null);
      e.dataTransfer.setData("text/plain",
        (parseInt(style.getPropertyValue("left"),10) - e.clientX) + ',' + 
        (parseInt(style.getPropertyValue("top"),10) - e.clientY));
    });

    document.querySelector('.court').addEventListener('dragover', (e) => {
      e.preventDefault();
      return false;
    });

    document.querySelector('.court').addEventListener('drop', (e) => {
      e.preventDefault();
      const offset = e.dataTransfer.getData("text/plain").split(',');
      const newLeft = e.clientX + parseInt(offset[0], 10);
      const newTop = e.clientY + parseInt(offset[1], 10);

      ball.style.left = newLeft + 'px';
      ball.style.top = newTop + 'px';

      checkGoal(newLeft, newTop);
    });

    function checkGoal(x, y) {
      const basketRect = document.querySelector('.basket').getBoundingClientRect();
      const ballRect = ball.getBoundingClientRect();

      if (
        ballRect.left + 30 > basketRect.left &&
        ballRect.right - 30 < basketRect.right &&
        ballRect.top < basketRect.bottom + 20
      ) {
        score++;
        sound.play();
        ball.style.left = '170px';
        ball.style.top = '500px';

        if (score >= 3) {
          message.style.display = 'block';
        }
      }
    }
  </script>
</body>
</html>
