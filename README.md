# my-baby
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Para ti, mi amor</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js"></script>
    <style>
        body {
            margin: 0;
            overflow: hidden;
            background-color: pink;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            flex-direction: column;
        }
        .card {
            position: relative;
            width: 90%;
            max-width: 400px;
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.2);
            text-align: center;
        }
        .hearts {
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            font-size: 30px;
            color: red;
        }
        .message {
            font-size: 40px;
            font-weight: bold;
            color: crimson;
            font-family: 'Comic Sans MS', cursive;
            animation: heartbeat 1.5s infinite;
        }
        .name {
            font-size: 50px;
            font-weight: bold;
            color: deeppink;
            animation: appear 3s infinite alternate;
        }

        @keyframes heartbeat {
            0% { transform: scale(1); }
            25% { transform: scale(1.1); }
            50% { transform: scale(1); }
            75% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }

        @keyframes appear {
            0% { opacity: 0; }
            100% { opacity: 1; }
        }
    </style>
</head>
<body>
    <div class="card">
        <div class="hearts">💖💖💖</div>
        <div class="message">TE AMO</div>
        <div class="name">JOENMY ❤️</div>
    </div>
    <script>
        let fireworks = [];
        let gravity;

        function setup() {
            createCanvas(windowWidth, windowHeight);
            colorMode(HSB);
            gravity = createVector(0, 0.15);
            canvas.parent(document.body);
        }

        function draw() {
            clear();
            if (random(1) < 0.07) {
                fireworks.push(new Firework());
            }
            for (let i = fireworks.length - 1; i >= 0; i--) {
                fireworks[i].update();
                fireworks[i].show();
                if (fireworks[i].done()) {
                    fireworks.splice(i, 1);
                }
            }
        }

        class Firework {
            constructor() {
                this.hu = random(360);
                this.firework = new Particle(random(width), height, this.hu, true);
                this.exploded = false;
                this.particles = [];
            }
            update() {
                if (!this.exploded) {
                    this.firework.applyForce(gravity);
                    this.firework.update();
                    if (this.firework.vel.y >= 0) {
                        this.exploded = true;
                        this.explode();
                    }
                }
                for (let i = this.particles.length - 1; i >= 0; i--) {
                    this.particles[i].applyForce(gravity);
                    this.particles[i].update();
                    if (this.particles[i].done()) {
                        this.particles.splice(i, 1);
                    }
                }
            }
            explode() {
                for (let i = 0; i < 150; i++) {
                    let p = new Particle(this.firework.pos.x, this.firework.pos.y, this.hu, false);
                    this.particles.push(p);
                }
            }
            done() {
                return this.exploded && this.particles.length === 0;
            }
            show() {
                if (!this.exploded) {
                    this.firework.show();
                }
                for (let p of this.particles) {
                    p.show();
                }
            }
        }

        class Particle {
            constructor(x, y, hu, firework) {
                this.pos = createVector(x, y);
                this.vel = firework ? createVector(0, random(-18, -12)) : p5.Vector.random2D().mult(random(3, 12));
                this.acc = createVector(0, 0);
                this.lifespan = 255;
                this.hu = hu;
                this.firework = firework;
            }
            applyForce(force) {
                this.acc.add(force);
            }
            update() {
                this.vel.add(this.acc);
                this.pos.add(this.vel);
                this.acc.mult(0);
                if (!this.firework) {
                    this.vel.mult(0.96);
                    this.lifespan -= 2;
                }
            }
            done() {
                return this.lifespan <= 0;
            }
            show() {
                strokeWeight(this.firework ? 5 : 3);
                stroke(this.hu, 255, 255, this.lifespan);
                point(this.pos.x, this.pos.y);
            }
        }
    </script>
</body>
</html>
