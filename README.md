# Basketball-ve
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>VR Basketball Game</title>
  <script src="https://aframe.io/releases/1.2.0/aframe.min.js"></script>
</head>
<body>
  <a-scene>
    <a-sky color="#4CC3D9"></a-sky>
    <a-camera position="0 2 4" look-controls></a-camera>
    
    <!-- Basketball hoop -->
    <a-cylinder position="0 2.5 -5" radius="1" height="0.2" color="orange" static-body></a-cylinder>
    <a-box position="0 2.5 -4.8" width="2" height="0.1" depth="0.1" color="orange" static-body></a-box>
    
    <!-- Basketball -->
    <a-sphere id="basketball" position="0 1 -3" radius="0.15" color="orange" dynamic-body></a-sphere>
    
    <!-- Ground -->
    <a-plane position="0 0 -4" rotation="-90 0 0" width="50" height="50" color="#7BC8A4" static-body></a-plane>

    <!-- Instructions -->
    <a-text id="instructions" value="Click to shoot!" align="center" position="0 3 -2" color="white"></a-text>

    <!-- Score -->
    <a-text id="score" value="Score: 0" align="center" position="0 2 -2" color="white"></a-text>

    <!-- Timer -->
    <a-text id="timer" value="Time: 60" align="center" position="0 1 -2" color="white"></a-text>
    
    <!-- Game logic -->
    <script>
      let basketball = document.querySelector('#basketball');
      let scene = document.querySelector('a-scene');
      let scoreDisplay = document.querySelector('#score');
      let timerDisplay = document.querySelector('#timer');
      let instructionsDisplay = document.querySelector('#instructions');
      let score = 0;
      let timeLeft = 60;

      scene.addEventListener('click', function() {
        shootBall();
      });

      function shootBall() {
        basketball.setAttribute('position', '0 1 -3');
        basketball.setAttribute('velocity', '0 0 -10');
      }

      function updateScore() {
        score++;
        scoreDisplay.setAttribute('value', `Score: ${score}`);
      }

      function updateTime() {
        timeLeft--;
        timerDisplay.setAttribute('value', `Time: ${timeLeft}`);
        if (timeLeft <= 0) {
          endGame();
        }
      }

      function endGame() {
        instructionsDisplay.setAttribute('value', 'Game Over! Click to restart.');
        scene.removeEventListener('click', shootBall);
        scene.addEventListener('click', resetGame);
      }

      function resetGame() {
        score = 0;
        timeLeft = 60;
        scoreDisplay.setAttribute('value', `Score: ${score}`);
        timerDisplay.setAttribute('value', `Time: ${timeLeft}`);
        instructionsDisplay.setAttribute('value', 'Click to shoot!');
        scene.addEventListener('click', shootBall);
      }

      setInterval(updateTime, 1000);

      basketball.addEventListener('collide', function() {
        updateScore();
      });
    </script>
  </a-scene>
</body>
</html>
