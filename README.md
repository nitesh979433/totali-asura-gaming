<!DOCTYPE html>
<html>




<head>
<title>my first website </titel>
<meta charset="UTF-8">
<meta name="description" content="This is my first website. It includes lots of information about my life."> 
</head>
<body> 

<style>
body {background-color: powderblue;}
h1   {color: blue;}
p    {color: green;}
</style>

<h1 style="color:blue;">A Blue Heading</h1>

<p style="color:red;">A green paragraph.</p>

 
<h1>Welcome to my webpage</h1> 
<h2>TOTALI ASURA GAMING</h2>
<h3>YOUTUBE CHANNEL</h3>
<a href="https://www.youtube.com/@totaliasuragaming/">SUBSCRIBE NOW</a>

<p>Welcome to <em>my</em> brand new website.</p>
<p>This site will be my <strong>new</strong> home on the web.</p>
<a href="/page2.html">Page2</a>
<img src="FB_IMG_1714973947906.jpg" alt="This is a test image" height="100" width="100">
<p>This website will have the following benefits for my business:</p>

<ul>
<li>Motive to Increased Subscribers </li>
<li>Global Reach</li>
<li>Promotional Opportunities</li>
</ul>

<table> 
<tr> 
<td><img src="download.jfif" alt="this is a test image" height="50" width="50"> - <a href="https://www.instagram.com/heart.is.life__/">INSTAGRAM</a></td>
<td><img src="download (2).png" alt="this is a test image" height="50" widht="50">  - <a href="https://www.youtube.com/channel/UCr4EQ6Im-OrrmKmQeMjAlKQ/">YOUTUBE</a> </td>
</tr>
<tr>
<td><img src="download.png" alt="this is a test image" height="50" widht="50" > - <a href="https://www.facebook.com/sarvan.kummar.16/">FACEBOOK</a> </td>
<td>Row 2 - Column 2</td>
</tr> 

</table> 
<style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html><style>
   * {
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  width: 100%;
}

body {
  margin: 0;
  overflow: hidden;
}
  </style>
<body>
<script>  
   "use strict";
function getFullScreenParams(ctx) {
    const { width, height } = ctx.canvas;
    return [0, 0, width, height];
}
function getCtxUtils(ctx) {
    function clear() {
        const params = getFullScreenParams(ctx);
        ctx.clearRect(...params);
    }
    function save(callback) {
        ctx.save();
        callback();
        ctx.restore();
    }
    function drawPath(callback) {
        ctx.beginPath();
        callback();
        ctx.closePath();
    }
    return {
        clear,
        save,
        drawPath
    };
}
function initCanvas(resizeCallback, parent = document.body) {
    function create() {
        const canvas = document.createElement('canvas');
        const ctx = canvas.getContext('2d');
        return [canvas, ctx];
    }
    function resize() {
        const { clientHeight, clientWidth } = parent;
        canvas.height = clientHeight;
        canvas.width = clientWidth;
        resizeCallback();
    }
    function append() {
        parent.innerHTML = '';
        parent.appendChild(canvas);
    }
    const [canvas, ctx] = create();
    resize();
    append();
    window.addEventListener('resize', resize);
    return ctx;
}
class Curve {
    constructor(origin, angle, length = 400) {
        this.angle = angle;
        this.length = length;
        this._angleOffset = 40;
        this.percentage = 0;
        this.speed = (Math.random() * 0.005) + 0.0005;
        this.randColor = () => {
            const { floor, random } = Math;
            return ['cyan', 'magenta', 'yellow'][floor(random() * 3)];
        };
        this.getEndPoint = () => {
            const { cos, sin } = Math;
            const x = cos(this.angle) * this.length;
            const y = sin(this.angle) * this.length;
            return [x, y];
        };
        this.getStartControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle - (this._angleOffset / 60);
            const length = this.length / 3;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.getEndControlPoint = () => {
            const { cos, sin } = Math;
            const angle = this.angle + (this._angleOffset / 60);
            const length = (this.length / 3) * 2;
            const x = cos(angle) * length;
            const y = sin(angle) * length;
            return [x, y];
        };
        this.animate = () => {
            function getXY() {
                const x = cubicN(percentage, 0, startControlPoint[0], endControlPoint[0], end[0]);
                const y = cubicN(percentage, 0, startControlPoint[1], endControlPoint[1], end[1]);
                return [x, y];
            }
            // cubic helper formula
            function cubicN(T, a, b, c, d) {
                const t2 = T * T;
                const t3 = t2 * T;
                return a + (-a * 3 + T * (3 * a - a * T)) * T + (3 * b + T * (-6 * b + b * 3 * T)) * T + (c * 3 - c * 3 * T) * t2 + d * t3;
            }
            const { startControlPoint, endControlPoint, end, percentage } = this;
            if (!this.isComplete) {
                this.percentage += this.speed;
                this.currentPoint = getXY();
            }
        };
        this.start = origin;
        this.currentPoint = [0, 0];
        this.end = this.getEndPoint();
        this.startControlPoint = this.getStartControlPoint();
        this.endControlPoint = this.getEndControlPoint();
        this.color = this.randColor();
    }
    get isComplete() {
        return this.percentage >= 1;
    }
}
class Burst {
    constructor(x, y) {
        this.curves = [];
        this.render = (ctx) => {
            function renderCurve(curve) {
                const { currentPoint, endControlPoint, startControlPoint, end } = curve;
                save(() => {
                    drawPath(() => {
                        ctx.moveTo(0, 0);
                        ctx.strokeStyle = 'rgba(80,80,80,0.4)';
                        ctx.bezierCurveTo(...startControlPoint, ...endControlPoint, ...end);
                        ctx.stroke();
                    });
                });
                save(() => {
                    ctx.translate(x, y);
                    drawPath(() => {
                        let [xi, yi] = currentPoint;
                        xi -= x;
                        yi -= y;
                        ctx.fillStyle = curve.color;
                        ctx.moveTo(0, 0);
                        ctx.arc(xi, yi, 4, 0, Math.PI * 2);
                        ctx.fill();
                    });
                });
            }
            function renderCurves() {
                save(() => {
                    ctx.translate(x, y);
                    curves.forEach(renderCurve);
                });
            }
            function renderBackground() {
                save(() => {
                    ctx.translate(0, 0);
                    ctx.fillStyle = '#101219';
                    drawPath(() => ctx.fillRect(...getFullScreenParams(ctx)));
                });
            }
            const { clear, save, drawPath } = getCtxUtils(ctx);
            const { curves, x, y } = this;
            clear();
            renderBackground();
            renderCurves();
        };
        this._x = x;
        this._y = y;
        this.create();
    }
    get x() { return this._x; }
    set x(value) {
        this._x = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    get y() { return this._y; }
    set y(value) {
        this._y = value;
        if (this.onPositionChange)
            this.onPositionChange();
    }
    create() {
        const chunkSize = 6 / 32;
        for (let i = 0; i <= 6; i += chunkSize) {
            this.curves.push(new Curve([this.x, this.y], i, 1300));
        }
    }
}
window.onload = init;
function init() {
    function animate() {
        burst.curves.forEach((curve, i) => {
            if (curve.isComplete) {
                const chunkSize = 6 / 32;
                const newCurve = new Curve([burst.x, burst.y], i * chunkSize, 1300);
                burst.curves.splice(i, 1, newCurve);
            }
            else {
                curve.animate();
            }
        });
        burst.render(ctx);
        requestAnimationFrame(animate);
    }
    let [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
    const burst = new Burst(cx, cy);
    const ctx = initCanvas(() => {
        [cx, cy] = [window.innerWidth / 2, window.innerHeight / 2];
        burst.x = cx;
        burst.y = cy;
    });
    burst.render(ctx);
    requestAnimationFrame(animate);
}

  </script>
</body>
</html>

</body> 

</html>



