# heart
Heart animation открытка
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Heart Animation</title>
<style>
    body {
        margin: 0;
        background: black;
        overflow: hidden;
    }
    canvas {
        display: block;
    }
</style>
</head>
<body>
<canvas id="canvas"></canvas>

<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let particles = [];
let pulseTime = 0;

function heartFunction(t, scale) {
    const x = scale * 16 * Math.pow(Math.sin(t), 3);
    const y = -scale * (13 * Math.cos(t)
        - 5 * Math.cos(2 * t)
        - 2 * Math.cos(3 * t)
        - Math.cos(4 * t));
    return {x, y};
}

function generateHeart() {
    particles = [];
    const steps = 300;
    const scale = 15 + Math.sin(pulseTime) * 2;

    for (let i = 0; i < steps; i++) {
        const t = (i / steps) * Math.PI * 2;
        const pos = heartFunction(t, scale);
        particles.push({
            x: canvas.width / 2 + pos.x,
            y: canvas.height / 2 + pos.y
        });
    }
}

function drawLines() {
    ctx.strokeStyle = "rgba(255, 100, 150, 0.2)";
    ctx.lineWidth = 1;

    for (let i = 0; i < particles.length; i++) {
        for (let j = i + 1; j < particles.length; j++) {
            const dx = particles[i].x - particles[j].x;
            const dy = particles[i].y - particles[j].y;
            const dist = Math.sqrt(dx * dx + dy * dy);

            if (dist < 30) {
                ctx.beginPath();
                ctx.moveTo(particles[i].x, particles[i].y);
                ctx.lineTo(particles[j].x, particles[j].y);
                ctx.stroke();
            }
        }
    }
}

function animate() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    pulseTime += 0.05;
    generateHeart();
    drawLines();
    requestAnimationFrame(animate);
}

animate();
</script>
</body>
</html>
