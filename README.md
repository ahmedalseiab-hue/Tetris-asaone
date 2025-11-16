# Tetris-asaone
<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<title>Tetris Ancient Buildings</title>
<style>
    body {
        background: #222;
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
        margin: 0;
        font-family: sans-serif;
        color: white;
    }

    #game {
        display: grid;
        grid-template-columns: repeat(10, 30px);
        grid-template-rows: repeat(20, 30px);
        gap: 1px;
        background: #111;
        border: 3px solid #444;
    }

    .cell {
        width: 30px;
        height: 30px;
        background: #000;
    }

    .block {
        background-size: cover !important;
        background-position: center !important;
        background-repeat: no-repeat !important;
    }

    /* مباني الحضارات القديمة */
    .pyramid { background-image: url("https://i.imgur.com/oV3U1Gf.png"); }
    .ziggurat { background-image: url("https://i.imgur.com/JDzHTz4.png"); }
    .assyrian { background-image: url("https://i.imgur.com/WXDo9Uj.png"); }
    .greek   { background-image: url("https://i.imgur.com/fn9L1pH.png"); }
    .babylon { background-image: url("https://i.imgur.com/1Tb6so6.png"); }
</style>
</head>
<body>

<div id="game"></div>

<script>
// إعداد الشبكة
const rows = 20;
const cols = 10;
const game = document.getElementById("game");

// إنشاء 200 خلية
let grid = [];
for (let i = 0; i < rows * cols; i++) {
    const cell = document.createElement("div");
    cell.className = "cell";
    game.appendChild(cell);
    grid.push(cell);
}

// الأشكال (مباني حضارات)
const shapes = [
    { name: "pyramid", blocks: [[1,0], [0,1], [1,1], [2,1]] },
    { name: "ziggurat", blocks: [[0,0], [1,0], [1,1], [2,1]] },
    { name: "assyrian", blocks: [[0,1], [1,1], [1,0], [2,1]] },
    { name: "greek", blocks: [[0,0], [1,0], [2,0], [1,1]] },
    { name: "babylon", blocks: [[1,0], [0,1], [1,1], [2,1]] }
];

let current = null;

// رسم شكل جديد
function newShape() {
    const shape = shapes[Math.floor(Math.random() * shapes.length)];
    current = {
        name: shape.name,
        blocks: shape.blocks.map(b => [...b]),
        x: 4,
        y: 0
    };
    draw();
}

// فحص الاصطدام
function collide(x, y, blocks) {
    return blocks.some(b => {
        let nx = b[0] + x;
        let ny = b[1] + y;
        if (nx < 0 || nx >= cols || ny >= rows) return true;
        let idx = ny * cols + nx;
        return grid[idx].classList.contains("block");
    });
}

// مسح الشكل القديم
function clearDraw() {
    document.querySelectorAll(".active").forEach(el => {
        el.className = "cell";
    });
}

// رسم الشكل الجديد
function draw() {
    clearDraw();
    current.blocks.forEach(b => {
        let x = current.x + b[0];
        let y = current.y + b[1];
        let idx = y * cols + x;
        grid[idx].classList.add("block", "active", current.name);
    });
}

// تجميد البلوك في مكانه
function freeze() {
    document.querySelectorAll(".active").forEach(el => {
        el.classList.remove("active");
    });
}

// خطوة نزول
function drop() {
    if (!collide(current.x, current.y + 1, current.blocks)) {
        current.y++;
    } else {
        freeze();
        newShape();
    }
    draw();
}

document.addEventListener("keydown", e => {
    if (e.key === "ArrowLeft" && !collide(current.x - 1, current.y, current.blocks)) {
        current.x--;
    }
    if (e.key === "ArrowRight" && !collide(current.x + 1, current.y, current.blocks)) {
        current.x++;
    }
    if (e.key === "ArrowDown") {
        drop();
    }
    draw();
});

// بدء اللعبة
newShape();
setInterval(drop, 600);
</script>

</body>
</html>
