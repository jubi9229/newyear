<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>New Year Countdown</title>
  <style>
    body {
      display: flex;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
      overflow: hidden;
    }

    h1 {
      font-size: 4rem;
      transition: opacity 1s;
      z-index: 1;
    }

    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-size: 1rem;
      cursor: pointer;
      transition: opacity 1s;
      z-index: 1;
    }

    canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 0;
    }
  </style>
</head>
<body>
  <h1 id="yearDisplay">2023</h1>
  <button onclick="changeYear()">CHANGE</button>

  <canvas id="fireworksCanvas"></canvas>
  <canvas id="secondFireworksCanvas"></canvas>
  <canvas id="thirdFireworksCanvas"></canvas>

  <script>
    function changeYear() {
      var yearDisplay = document.getElementById('yearDisplay');
      var button = document.querySelector('button');

      var currentYear = parseInt(yearDisplay.innerText);
      var newYear = currentYear + 1;

      // Fade out the year and button
      yearDisplay.style.opacity = 0;
      button.style.opacity = 0;

      // Display "HAPPY NEW YEAR" for a moment
      setTimeout(function () {
        yearDisplay.innerText = 'HAPPY NEW YEAR ' + newYear;

        // Change the text color randomly based on fireworks color
        var fireworksCanvas = document.getElementById('fireworksCanvas');
        var context = fireworksCanvas.getContext('2d');
        var sampleParticle = createSampleParticle();
        yearDisplay.style.color = sampleParticle.color;

        yearDisplay.style.opacity = 1;
        button.style.display = 'none';

        // Start continuous fireworks
        startContinuousFireworks(fireworksCanvas);

        // Start continuous fireworks at X+50
        var secondFireworksCanvas = document.getElementById('secondFireworksCanvas');
        secondFireworksCanvas.style.display = 'block';
        secondFireworksCanvas.width = window.innerWidth;
        secondFireworksCanvas.height = window.innerHeight;
        startContinuousSecondFireworks(secondFireworksCanvas);

        // Start continuous fireworks at Y+100
        var thirdFireworksCanvas = document.getElementById('thirdFireworksCanvas');
        thirdFireworksCanvas.style.display = 'block';
        thirdFireworksCanvas.width = window.innerWidth;
        thirdFireworksCanvas.height = window.innerHeight;
        startContinuousThirdFireworks(thirdFireworksCanvas);
      }, 1000); // 1000 milliseconds (1 second)
    }

    function startContinuousFireworks(canvas) {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      var context = canvas.getContext('2d');

      // Start the continuous fireworks
      setInterval(function () {
        // Create a new explosion
        var x = Math.random() * window.innerWidth;
        var y = window.innerHeight;
        startExplosion(x, y, context);
      }, 500); // Create a new explosion every 0.5 seconds
    }

    function startContinuousSecondFireworks(canvas) {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      var context = canvas.getContext('2d');

      // Start the continuous fireworks at X+50
      setInterval(function () {
        // Create a new explosion at X+50
        var x = Math.random() * window.innerWidth + 50;
        var y = window.innerHeight;
        startExplosion(x, y, context);
      }, 500); // Create a new explosion every 0.5 seconds
    }

    function startContinuousThirdFireworks(canvas) {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;

      var context = canvas.getContext('2d');

      // Start the continuous fireworks at Y+100
      setInterval(function () {
        // Create a new explosion at Y+100
        var x = Math.random() * window.innerWidth;
        var y = window.innerHeight + 100;
        startExplosion(x, y, context);
      }, 500); // Create a new explosion every 0.5 seconds
    }

    function startExplosion(x, y, context) {
      var numberOfParticles = 100;

      for (var i = 0; i < numberOfParticles; i++) {
        createParticle(x, y, context);
      }
    }

    function createParticle(x, y, context) {
      var angle = Math.random() * Math.PI * 2;
      var speed = Math.random() * 5 + 1;
      var radius = Math.random() * 3 + 1;
      var color = getRandomColor();

      var particle = {
        x: x,
        y: y,
        vx: Math.cos(angle) * speed,
        vy: Math.sin(angle) * speed,
        radius: radius,
        color: color,
        life: 100
      };

      particles.push(particle);
    }

    function getRandomColor() {
      var letters = '0123456789ABCDEF';
      var color = '#';
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      return color;
    }

    function createSampleParticle() {
      return {
        color: getRandomColor()
      };
    }

    var particles = [];

    function animateParticles() {
      var fireworksCanvas = document.getElementById('fireworksCanvas');
      var context = fireworksCanvas.getContext('2d');
      context.clearRect(0, 0, window.innerWidth, window.innerHeight);

      for (var i = 0; i < particles.length; i++) {
        var particle = particles[i];
        context.beginPath();
        context.arc(particle.x, particle.y, particle.radius, 0, Math.PI * 2);
        context.fillStyle = particle.color;
        context.fill();
        context.closePath();

        particle.x += particle.vx;
        particle.y += particle.vy;
        particle.life--;

        if (particle.life <= 0) {
          particles.splice(i, 1);
          i--;
        }
      }
    }

    setInterval(animateParticles, 1000 / 60);

  </script>
</body>
</html>
