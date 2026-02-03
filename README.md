<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Gửi Em ❤️</title>

<style>
body{
    margin:0;
    overflow:hidden;
    background:black;
    font-family:Arial;
}

/* Text tình cảm */
.message{
    position:absolute;
    top:40%;
    width:100%;
    text-align:center;
    color:white;
    font-size:40px;
    font-weight:bold;
    text-shadow:0 0 20px pink;
    z-index:10;
}

.sub{
    font-size:22px;
    margin-top:10px;
    color:#ffc0cb;
}

button{
    position:absolute;
    bottom:20px;
    left:50%;
    transform:translateX(-50%);
    padding:10px 20px;
    font-size:18px;
    border:none;
    border-radius:10px;
    background:pink;
    cursor:pointer;
}
</style>
</head>

<body>

<div class="message">
    Anh có điều muốn nói...
    <div class="sub">Anh thương em rất nhiều ❤️</div>
</div>

<button onclick="toggleMusic()">🎵 Bật / Tắt Nhạc</button>

<canvas id="canvas"></canvas>

<audio id="music" loop>
    <source src="music.mp3" type="audio/mpeg">
</audio>

<script>
const canvas = document.getElementById("canvas");
const ctx = canvas.getContext("2d");

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

let particles = [];

class Particle{
    constructor(x,y,vx,vy,color){
        this.x=x;
        this.y=y;
        this.vx=vx;
        this.vy=vy;
        this.life=100;
        this.color=color;
    }

    update(){
        this.x+=this.vx;
        this.y+=this.vy;
        this.vy+=0.02;
        this.life--;
    }

    draw(){
        ctx.beginPath();
        ctx.arc(this.x,this.y,2,0,Math.PI*2);
        ctx.fillStyle=this.color;
        ctx.fill();
    }
}

/* Pháo hoa trái tim */
function heartFirework(x,y){

    for(let i=0;i<120;i++){

        let t = Math.random()*Math.PI*2;

        let vx = 16*Math.pow(Math.sin(t),3);
        let vy = -(13*Math.cos(t)-5*Math.cos(2*t)-2*Math.cos(3*t)-Math.cos(4*t));

        particles.push(new Particle(
            x,
            y,
            vx*0.2,
            vy*0.2,
            "pink"
        ));
    }
}

/* Click bắn pháo */
canvas.addEventListener("click", e=>{
    heartFirework(e.clientX,e.clientY);
});

/* Tự bắn pháo */
setInterval(()=>{
    heartFirework(
        Math.random()*canvas.width,
        Math.random()*canvas.height/2
    );
},1500);


/* Animation */
function animate(){

    ctx.fillStyle="rgba(0,0,0,0.2)";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    particles.forEach((p,i)=>{
        p.update();
        p.draw();

        if(p.life<=0) particles.splice(i,1);
    });

    requestAnimationFrame(animate);
}

animate();


/* Music */
let music = document.getElementById("music");

function toggleMusic(){
    if(music.paused) music.play();
    else music.pause();
}
</script>

</body>
</html>
