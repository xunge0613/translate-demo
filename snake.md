大家好👋

欢迎上车。今天我们将开始一场激动人心的冒险，在这里我们将开发属于我们自己的贪食蛇游戏🐍。通过将其分解为一个个简短的步骤来学习如何解决问题。在这段旅程结束时，你会学到一些新东西，并且有信心能独立探索更多。


如果你是编程新手，[freeCodeCamp](https://www.freecodecamp.org/) 了解一下。这是一个很棒的学习网站…没错…完全免费。我就是从那儿入门的。

好了，好了，言归正传 —— 要发车了，准备好了嘛？

> 你可以在[这里](https://github.com/supergoat/snake)找到完整代码，并且可以在[这里](https://snake-cdxejlircg.now.sh)在线预览。

### 开始吧

首先新建一个文件“snake.html”，它将包含全部代码。

因为这是一个 HTML 文件，所以要做的第一件事就是申明 `<!DOCTYPE>`。请在 snake.html 文件中输入以下内容：

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  
  <body>
    Welcome to Snake!
  </body>
</html>
```

不错，接下来在你最喜欢的浏览器中打开 snake.html。你应该能够看到 **Welcome to Snake!**。

![](https://p0.ssl.qhimg.com/t01fd229818a191527a.png)

很好的开始！ 👊

### 创建画布

为了完成游戏，我们需要用到 `<canvas>` 标签，并借助 JavaScript 绘制图像。


用以下代码来替换 snake.html 里的欢迎语。

```
<canvas id="gameCanvas" width="300" height="300"><canvas>
```

id 用于标识画布且始终应该被指定。稍后我们会用它来访问画布。width 和 height 分别是画布的宽和高，同样也需要被指定。在本例中，画布尺寸为 300 px * 300 px。


snake.html 文件此时应该是这样的：

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  <body>
    <canvas id="gameCanvas" width="300" height="300"></canvas>
  </body>
</html>
```

如果你刷新页面，你会发现页面一片空白。这是因为默认情况下，画布是空的并且没有背景。让我们来调整下。🔧

#### 为画布添加背景色和边框

为了看到画布，我们可以写一段 JavaScript 代码给它加一个边框。为此，我们需要在 `</canvas>` 后添加  `<script></script>` 标签，并在其中编写 JavaScript 代码。


> 如果你把 `<script>` 标签放在了 `<canvas>` 标签前面，那么你的 JavaScript 代码将不会生效，因为此时 HTML 还未加载完。

现在我们来写一些 JavaScript 代码，写在闭合的 `<script></script>` 标签之间。代码如下：

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  <body>
    <canvas id="gameCanvas" width="300" height="300"></canvas>
    
    <script>
      /** 常量 **/
      const CANVAS_BORDER_COLOUR = 'black';
      const CANVAS_BACKGROUND_COLOUR = "white";
      
      // 获取 canvas 元素
      var gameCanvas = document.getElementById("gameCanvas");
      // 返回一个二维绘图上下文 Return a two dimensional drawing context
      var ctx = gameCanvas.getContext("2d");
      // 选择画布的背景颜色
      ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
      // 选择画布的边框颜色
      ctx.strokestyle = CANVAS_BORDER_COLOUR;
      // 绘制一个“实心的”长方形来覆盖整个画布
      ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 绘制画布的“边框”
      ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
    </script>
  </body>
</html>

```

首先，使用前面指定的 id（gameCanvas）获取 canvas 元素。然后获取画布的“2d”上下文，这意味着我们将在 2D 空间绘制图像。

最后，我们画了一个 300 x 300 的白色矩形，边框为黑色。这个矩形从左上角（0，0）开始覆盖了整个画布。

如果你在浏览器中重新加载 snake.html，你会看到一个带黑色边框的白块！干得漂亮，现在我们已经有了一个画布，可以用来创建我们的贪食蛇游戏了！ 👏迎接下一个挑战吧！

### 用坐标来表示贪食蛇

为了让游戏能玩，我们需要知道蛇在画布中的位置。为此，我们用一系列坐标来表示蛇。因此，要在画布中间（150,150）画一条横向的蛇，我们可以这样写：

```
let snake = [
  {x: 150, y: 150},
  {x: 140, y: 150},
  {x: 130, y: 150},
  {x: 120, y: 150},
  {x: 110, y: 150},
];
```

注意蛇所有部位的 y 坐标都是 150。每一部位的 x 坐标比（左边）前一个部位多 10 px。数组中的第一对坐标 {x: 150, y: 150} 表示蛇头，位于蛇的最右边。

别急，在下一步我们画蛇的时候，会对此有更清晰的认识。

### 开始画蛇

为了画蛇，我们可以写一个函数为蛇身上的**每一个**部位画一个矩形。

```
function drawSnakePart(snakePart) {
  ctx.fillStyle = 'lightgreen';
  ctx.strokestyle = 'darkgreen';
  ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
  ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
}
```
 
接下来，我们用另一个函数在画布上把蛇展示出来。

```
function drawSnake() {
  snake.forEach(drawSnakePart);
}
```

此时 snake.html 文件应该是这样的。

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  <body>
    <canvas id="gameCanvas" width="300" height="300"></canvas>
    
    <script>
      /** 常量 **/
      const CANVAS_BORDER_COLOUR = 'black';
      const CANVAS_BACKGROUND_COLOUR = "white";
      const SNAKE_COLOUR = 'lightgreen';
      const SNAKE_BORDER_COLOUR = 'darkgreen';
      
      let snake = [
        {x: 150, y: 150},
        {x: 140, y: 150},
        {x: 130, y: 150},
        {x: 120, y: 150},
        {x: 110, y: 150}
      ]
      
      // 获取 canvas 元素
      var gameCanvas = document.getElementById("gameCanvas");
      // 返回一个二维绘制上下文
      var ctx = gameCanvas.getContext("2d");
      //  选择画布的背景颜色
      ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
      //  选择画布的边框颜色
      ctx.strokestyle = CANVAS_BORDER_COLOUR;
      // 绘制一个“实心的”长方形来覆盖整个画布
      ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 绘制画布的“边框”
      ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
      drawSnake();
      
      /**
       * 在画布上画蛇
       */
      function drawSnake() {
        // 循环遍历蛇的每一部分，并将其绘制到画布上
        snake.forEach(drawSnakePart)
      }
      /**
       * 在画布上画蛇的一个部分
       * @param { object } snakePart —— 需要绘制的部位的所在坐标
       */
      function drawSnakePart(snakePart) {
        // 设置蛇身体的背景颜色
        ctx.fillStyle = SNAKE_COLOUR;
        // 设置蛇身的边框色
        ctx.strokestyle = SNAKE_BORDER_COLOUR;
        // 在蛇身坐标所在的位置，绘制“实心”的矩形以表示蛇    
        ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
        // 绘制蛇身的边框
        ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
      }
    </script>
  </body>
</html>
```

刷新页面，你会发现画布中间有一条绿色的蛇。666！ 😎

![](https://p0.ssl.qhimg.com/t0114f43f310d04d6d3.png)

### 让贪食蛇横向移动

接下来我们想要让蛇能动起来。不过我们该怎么做呢？🤔

嗯，为了让蛇能够一步一步（10 px）移动到最右边，我们可以把蛇的**每个**部位的 x 坐标，每次增加 10 px（dx = +10 px）。同理，为了让蛇移动到最左边，每次把蛇的**每个**部位的 x 坐标减少 10 px（dx = -10）。

> **dx** 是蛇的横向移动速度。

蛇向右移动了 10 px 后，坐标如下图所示：

![](https://p0.ssl.qhimg.com/t010a96ab728cf221e6.png)

用 advanceSnake 函数来更新蛇的状态。

```
function advanceSnake() {
  const head = {x: snake[0].x + dx, y: snake[0].y};
  snake.unshift(head);
  snake.pop();
}
```
 
首先，我们为蛇画了个新头。然后使用 [unshift](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) 方法将新头放在**蛇**的第一个部位，然后使用 [pop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) 方法移除**蛇**的最后一部分。这样以后，如上图所示，所有蛇身上其他部位都移动到了对应位置。

Boom 💥，你已经找到窍门了吧。

### 让贪食蛇纵向移动

为了让蛇能够上下移动，我们不能直接将蛇的所有 y 坐标调整 10 px。那会让整条蛇上下移动。

![](https://p0.ssl.qhimg.com/t01f60003f8d1d05646.gif)

相反，我们可以调整蛇头的 y 坐标。减少 10 px 蛇会向下移动，增加 10 px 蛇会向上移动。这样就能让蛇正确地移动了。

幸运的是，因为我们编写的 advanceSnake 函数很棒，所以这很容易做到。在 advanceSnake 函数中，更新 head 让 y 坐标随着 **dy** 变化。

```
const head = {x: snake[0].x + dx, y: snake[0].y + dy};
```

为了验证 advanceSnake 函数是否正确，我们可以临时性地在 drawSnake 函数前调用它。

```
// 向右走一步
advanceSnake()
// 水平速度改为 0
dx = 0;
// 垂直速度改为 10
dy = -10;
// 向上走一步
advanceSnake();
// 在画布上画蛇
drawSnake();

```
 
现在 snake.html 代码如下：

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  <body>
    <canvas id="gameCanvas" width="300" height="300"></canvas>
    
    <script>
      /** 常量 **/
      const CANVAS_BORDER_COLOUR = 'black';
      const CANVAS_BACKGROUND_COLOUR = "white";
      const SNAKE_COLOUR = 'lightgreen';
      const SNAKE_BORDER_COLOUR = 'darkgreen';
      
      let snake = [
        {x: 150, y: 150},
        {x: 140, y: 150},
        {x: 130, y: 150},
        {x: 120, y: 150},
        {x: 110, y: 150}
      ]
      
      // 横向移动速度
      let dx = 10;
      // 纵向移动速度
      let dy = 0;
      
      // 获取 canvas 元素
      var gameCanvas = document.getElementById("gameCanvas");
      // 返回一个二维绘制上下文
      var ctx = gameCanvas.getContext("2d");
      //  选择画布的背景颜色
      ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
      //  选择画布的边框颜色
      ctx.strokestyle = CANVAS_BORDER_COLOUR;
      // 绘制一个“实心的”长方形来覆盖整个画布
      ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 绘制画布的“边框”
      ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
      
      // 向右走一步
      advanceSnake()
      // 水平速度改为 0
      dx = 0;
      // 垂直速度改为 10
      dy = -10;
      // 向上走一步
      advanceSnake();
      // 在画布上画蛇
      drawSnake();
      
      /**
        * 根据蛇的水平移动速度改变蛇的 x 坐标，
        * 根据蛇的垂直移动速度改变蛇的 y 坐标
        */
      function advanceSnake() {
        const head = {x: snake[0].x + dx, y: snake[0].y + dy};
        snake.unshift(head);
        snake.pop();
      }
      
      /**
       * 在画布上画蛇
       */
      function drawSnake() {
        // 循环遍历蛇的每一部分，并将其绘制到画布上
        snake.forEach(drawSnakePart)
      }
      /**
       * 在画布上画蛇的一个部分
       * @param { object } snakePart —— 需要绘制的部位的所在坐标
       */
      function drawSnakePart(snakePart) {
        // 设置蛇身体的背景颜色
        ctx.fillStyle = SNAKE_COLOUR;
        // 设置蛇身的边框色
        ctx.strokestyle = SNAKE_BORDER_COLOUR;
        // 在蛇身坐标所在的位置，绘制“实心”的矩形以表示蛇    
        ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
        // 绘制蛇身的边框
        ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
      }
    </script>
  </body>
</html>
```

刷新页面，蛇动了。恭喜！

![](https://p0.ssl.qhimg.com/t01058946718ea792e4.png)

### 重构代码

在继续下一步之前，让我们重构一下现有代码，将绘制画布的代码封装在一个函数中。这有助于完成下一步。

> **代码重构**是重构现有计算机**代码**的过程，同时不改变其外部行为。” —— [维基百科](https://en.wikipedia.org/wiki/Code_refactoring)

```
function clearCanvas() {
  ctx.fillStyle = "white";
  ctx.strokeStyle = "black";
  ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
  ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
}
```

我们正在大踏步前进！🐾

### 让贪食蛇自动移动

好的，既然我们已经成功地重构了代码，那么接下来让贪食蛇能够自动移动吧。


之前为了测试 advanceSnake 函数是否正确，我们调用了它两次。一次为了让蛇向右移动，一次为了让它向上移动。

因此如果想要让蛇右移五次，那么我们需要连续调用 advanceSnake() 函数五次。

```
clearCanvas();
advanceSnake();
advanceSnake();
advanceSnake();
advanceSnake();
advanceSnake();
drawSnake();
```

但是，如上所示连续五次调用 advanceSnake 函数会让蛇一下子向前跳 50 px。

![](https://p0.ssl.qhimg.com/t01e97ff3c91820df28.gif)

相反，我们想让蛇看起来像是一步一步地前进。

为此，我们用 [setTimeout](https://www.w3schools.com/Jsref/met_win_settimeout.asp) 在每次调用之间添加一个短暂的延时。我们还需要确保每次调用 advanceSnake 函数时都调用 drawSnake 函数。如果我们不这样做，我们将无法看到蛇移动的中间步骤。
 
```
setTimeout(function onTick() {
  clearCanvas();
  advanceSnake();
  drawSnake();
}, 100);
setTimeout(function onTick() {
  clearCanvas();
  advanceSnake();
  drawSnake();
}, 100);
...
drawSnake();

```

注意我们同样在每个 setTimeout 中调用了 clearCanvas() 函数。这是为了移除蛇先前所有的位置，以免留下痕迹。

![](https://p0.ssl.qhimg.com/t0109fcd2ed761bc8b1.png)

尽管如此，上述代码仍然有问题。没有任何代码告诉程序它必须等待一个 **setTimeout** 完成后才能移动到下一个**setTimeout**。也就是说蛇**仍然**会在**一个短暂延迟**后向前跳 50 px。

要解决这个问题，我们必须将代码包装在函数中，每次只调用一个函数。

```
stepOne();
    
function stepOne() {
  setTimeout(function onTick() {
    clearCanvas();
    advanceSnake();
    drawSnake();
   // 调用第二个函数
   stepTwo();
  }, 100)
}
function stepTwo() {
  setTimeout(function onTick() {
    clearCanvas();
    advanceSnake();
    drawSnake();
    // 调用第三个函数
    stepThree();
  }, 100)
}
...
```

如何让贪食蛇一直前进？我们可以改为创建一个 main 函数并递归调用它，而不是创建无数个相互调用的函数。

```
function main() {
  setTimeout(function onTick() {
    clearCanvas();
    advanceSnake();
    drawSnake();
    // 再次调用 main 函数
    main();
  }, 100)
}
```

看啊！贪食蛇现在会一直向右移动。虽然一旦它到达画布的尽头，将继续其无尽的旅程并进入未知的地方😅。我们将在适当的时候解决这个问题，耐心点年轻的朋友们。🙏。

![](https://p0.ssl.qhimg.com/t0105917546eadd03c1.gif)

### 调整蛇的移动方向

我们的下一个任务是在按下任意方向键时更改蛇的移动方向。在 drawSnakePart 函数之后添加以下代码。

```
function changeDirection(event) {
  const LEFT_KEY = 37;
  const RIGHT_KEY = 39;
  const UP_KEY = 38;
  const DOWN_KEY = 40;
  const keyPressed = event.keyCode;
  const goingUp = dy === -10;
  const goingDown = dy === 10;
  const goingRight = dx === 10;
  const goingLeft = dx === -10;
  if (keyPressed === LEFT_KEY && !goingRight) {
    dx = -10;
    dy = 0;
  }
  if (keyPressed === UP_KEY && !goingDown) {
    dx = 0;
    dy = -10;
  }
  if (keyPressed === RIGHT_KEY && !goingLeft) {
    dx = 10;
    dy = 0;
  }
  if (keyPressed === DOWN_KEY && !goingDown) {
    dx = 0;
    dy = 10;
  }
}
```

这没有什么特别的。我们检查按下的键是否与其中一个方向键匹配。如果匹配到，我们如上所述改变垂直和水平速度。

请注意，我们还要检查蛇是否往新预期方向的相反方向上移动。这是为了防止我们的蛇掉头，例如当蛇向**左**移动时按下**右**键。

![](https://p0.ssl.qhimg.com/t0184be9b4be117a78f.gif)

要将 changeDirection  加到游戏代码中，可以在 [document](https://www.w3schools.com/Jsref/dom_obj_document.asp)  上使用  [addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener)  来“监听”是否有键被按下。然后我们随着 [keydown](https://developer.mozilla.org/en-US/docs/Web/Events/keydown)  事件调用 changeDirection 方法。接着在 main 函数之后添加以下代码。

```
document.addEventListener("keydown", changeDirection)
```

你现在应该可以使用四个方向键更改蛇的方向。干得漂亮，你火了🔥！

接下来让我们看看如何生成食物并让我们的蛇长大。

### 为蛇生成食物

为了生成蛇食，我们必须生成一组随机坐标。我们可以使用辅助函数 randomTen 来生成两个数字。一个用于 x 坐标，一个用于 y 坐标。

我们还必须确保食物不与蛇的位置重叠。如果重叠了，我们必须生成一个新的食物位置。

```
function randomTen(min, max) {
  return Math.round((Math.random() * (max-min) + min) / 10) * 10;
}
function createFood() {
  foodX = randomTen(0, gameCanvas.width - 10);
  foodY = randomTen(0, gameCanvas.height - 10);
  snake.forEach(function isFoodOnSnake(part) {
    const foodIsOnSnake = part.x == foodX && part.y == foodY
    if (foodIsOnSnake)
      createFood();
  });
}
```

然后我们需要写一个在画布上绘制食物的函数。

```
function drawFood() {
 ctx.fillStyle = 'red';
 ctx.strokestyle = 'darkred';
 ctx.fillRect(foodX, foodY, 10, 10);
 ctx.strokeRect(foodX, foodY, 10, 10);
}
```

最后我们可以在调用 main 之前调用 createFood。别忘了还要更新 main 函数以调用 drawFood 函数。

```
function main() {
  setTimeout(function onTick() {
    clearCanvas();
    drawFood()
    advanceSnake();
    drawSnake();
    main();
  }, 100)
}
```

### 让蛇长大

让蛇长大很简单。更新 advanceSnake 函数，检查蛇头是否碰到了食物。如果碰到了，我们可以跳过移除蛇的最后部分这一操作，同时创建一个新的食物位置。

```
function advanceSnake() {
  const head = {x: snake[0].x + dx, y: snake[0].y};
  snake.unshift(head);
  const didEatFood = snake[0].x === foodX && snake[0].y === foodY;
  if (didEatFood) {
    createFood();
  } else {
    snake.pop();
  }
}
```

#### 记录游戏分数

为了让游戏更有乐趣，我们还可以添加一个分数，当蛇吃食物时分数增加。

在声明了 snake 后，创建一个新的变量 score，并将其设为 0。

```
let score = 0;
```

接下来在画布前添加一个 id 为“score”的新 div，用于显示分数。


```
<div id="score">0</div>
<canvas id="gameCanvas" width="300" height="300"></canvas>
```

最后更新 advanceSnake， 当蛇吃食物时，增加并显示分数。
 

```
function advanceSnake() {
  ...
  if (didEatFood) {
    score += 10;
    document.getElementById('score').innerHTML = score;
    createFood();
  } else {
    ...
  }
}
```

呼…真不容易，不过胜利就在眼前了 😌


### 结束游戏

还剩下最后一部分，那就是结束游戏🖐。为此创建一个函数 didGameEnd，当游戏结束时返回 **true**，否则返回 **false**。

```
function didGameEnd() {
  for (let i = 4; i < snake.length; i++) {
    const didCollide = snake[i].x === snake[0].x &&
      snake[i].y === snake[0].y
    if (didCollide) return true
  }
  const hitLeftWall = snake[0].x < 0;
  const hitRightWall = snake[0].x > gameCanvas.width - 10;
  const hitToptWall = snake[0].y < 0;
  const hitBottomWall = snake[0].y > gameCanvas.height - 10;
  return hitLeftWall || 
         hitRightWall || 
         hitToptWall ||
         hitBottomWall
} 
```

首先，我们检查蛇的头部是否碰到蛇身上其他部分，如果碰到了那么返回 **true**。

> 请注意，我们从索引值 4 开始循环。这有两个原因：一个是如果索引为 0，**didCollide** 则会立即判断为 true，导致游戏结束。另一个是，蛇的前三个部分不可能相互接触。


接下来我们检查蛇是否在画布上撞墙了，如果是，那么返回 **true**，否则返回 **false **。

现在，如果 didEndGame 返回 true，我们可以在主函数中提前返回，从而结束游戏。

```
function main() {
  if (didGameEnd()) return;
  ...
}
```

snake.html 现在应该是这样的：

```
<!DOCTYPE html>
<html>
  <head>
   <title>Snake Game</title>
  </head>
  <body>
    <div id="score">0</div>
    <canvas id="gameCanvas" width="300" height="300"></canvas>

    <script>
      /** 常量 **/
      const CANVAS_BORDER_COLOUR = 'black';
      const CANVAS_BACKGROUND_COLOUR = "white";
      const SNAKE_COLOUR = 'lightgreen';
      const SNAKE_BORDER_COLOUR = 'darkgreen';
      const FOOD_COLOUR = 'red';
      const FOOD_BORDER_COLOUR = 'darkred';
      let snake = [
        {x: 150, y: 150},
        {x: 140, y: 150},
        {x: 130, y: 150},
        {x: 120, y: 150},
        {x: 110, y: 150}
      ]
      // 玩家的分数
      let score = 0;
      // 横向移动速度
      let dx = 10;
      // 纵向移动速度
      let dy = 0;
      // 获取 canvas 元素
      var gameCanvas = document.getElementById("gameCanvas");
      // 返回一个二维绘制上下文
      var ctx = gameCanvas.getContext("2d");
      //  选择画布的背景颜色
      ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
      //  选择画布的边框颜色
      ctx.strokestyle = CANVAS_BORDER_COLOUR;
      // 绘制一个“实心的”长方形来覆盖整个画布
      ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 绘制画布的“边框”
      ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 开始游戏
      main();
      // 生成第一个食物位置
      createFood();
      // 按下任意一个键，都会调用 changeDirection
      document.addEventListener("keydown", changeDirection);
      function main() {
        if (didGameEnd()) return;
        setTimeout(function onTick() {
          clearCanvas();
          drawFood();
          advanceSnake();
          drawSnake();
          // Call main again
          main();
        }, 100)
      }
      /**
      * 设置画布的背景色为 CANVAS_BACKGROUND_COLOUR 
      * 并绘制画布的边框
      */
      function clearCanvas() {
        //  选择画布的背景颜色
        ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
        //  选择画布的边框颜色
        ctx.strokestyle = CANVAS_BORDER_COLOUR;
        // 绘制一个“实心的”长方形来覆盖整个画布
        ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
        // 绘制画布的“边框”
        ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
      }
      /**
       * 当蛇撞墙或者蛇头碰到蛇身的时候返回 true
       */
      function didGameEnd() {
        for (let i = 4; i < snake.length; i++) {
          const didCollide = snake[i].x === snake[0].x && snake[i].y === snake[0].y
          if (didCollide) return true
        }
        const hitLeftWall = snake[0].x < 0;
        const hitRightWall = snake[0].x > gameCanvas.width - 10;
        const hitToptWall = snake[0].y < 0;
        const hitBottomWall = snake[0].y > gameCanvas.height - 10;
        return hitLeftWall || hitRightWall || hitToptWall || hitBottomWall
      }
      /**
       * 在画布上画食物
       */
      function drawFood() {
        ctx.fillStyle = FOOD_COLOUR;
        ctx.strokestyle = FOOD_BORDER_COLOUR;
        ctx.fillRect(foodX, foodY, 10, 10);
        ctx.strokeRect(foodX, foodY, 10, 10);
      }
      /**
       * 根据蛇的水平移动速度改变蛇的 x 坐标，
       * 根据蛇的垂直移动速度改变蛇的 y 坐标
       */
      function advanceSnake() {
        // 绘制新的头部
        const head = {x: snake[0].x + dx, y: snake[0].y + dy};
        // 将新的头部放到蛇身体的第一个部位
        snake.unshift(head);
        const didEatFood = snake[0].x === foodX && snake[0].y === foodY;
        if (didEatFood) {
          // 增加分数
          score += 10;
          // 在屏幕上显示分数
          document.getElementById('score').innerHTML = score;
          // 生成新的食物
          createFood();
        } else {
          // 移除蛇的最后一个部分
          snake.pop();
        }
      }
      /**
        * 给定一个最大值和最小值，生成一个 10 的倍数的随机数     
        * @param { number } min —— 随机数的下限
        * @param { number } max —— 随机数的上限
       */
      function randomTen(min, max) {
        return Math.round((Math.random() * (max-min) + min) / 10) * 10;
      }
     /**     
      * 随机生成食物坐标
      */
      function createFood() {
        // 随机生成食物的 x 坐标
        foodX = randomTen(0, gameCanvas.width - 10);
        // 随机生成食物的 y 坐标
        foodY = randomTen(0, gameCanvas.height - 10);
        // 如果新生成的食物与蛇当前位置重叠，重新为食物生成一个位置
        snake.forEach(function isOnSnake(part) {
          if (part.x == foodX && part.y == foodY) createFood();
        });
      }
      /**
       * 在画布上画蛇
       */
      function drawSnake() {
        // 循环遍历蛇的每一部分，并将其绘制到画布上
        snake.forEach(drawSnakePart)
      }
      /**
       * 在画布上画蛇的一个部分
       * @param { object } snakePart  —— 需要绘制的部位的所在坐标
       */
      function drawSnakePart(snakePart) {
        // 设置蛇身体的背景颜色
        ctx.fillStyle = SNAKE_COLOUR;
        // 设置蛇身的边框色
        ctx.strokestyle = SNAKE_BORDER_COLOUR;
        // 在蛇身坐标所在的位置，绘制“实心”的矩形以表示蛇      
        ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
        // 绘制蛇身的边框
        ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
      }
      /**     
      * 根据按下的键，改变蛇的水平移动速度和垂直移动速度
      * 为了避免蛇反转，蛇的移动方向不能直接变成相反的方向，
      * 比如说，当前方向是“向右”，那么下一个方向不能是“向左”
      * @param { object } event —— 键盘事件
      */
      function changeDirection(event) {
        const LEFT_KEY = 37;
        const RIGHT_KEY = 39;
        const UP_KEY = 38;
        const DOWN_KEY = 40;
        const keyPressed = event.keyCode;
        const goingUp = dy === -10;
        const goingDown = dy === 10;
        const goingRight = dx === 10;
        const goingLeft = dx === -10;
        if (keyPressed === LEFT_KEY && !goingRight) {
          dx = -10;
          dy = 0;
        }
        if (keyPressed === UP_KEY && !goingDown) {
          dx = 0;
          dy = -10;
        }
        if (keyPressed === RIGHT_KEY && !goingLeft) {
          dx = 10;
          dy = 0;
        }
        if (keyPressed === DOWN_KEY && !goingUp) {
          dx = 0;
          dy = 10;
        }
      }
    </script>
  </body>
</html>
```

现在贪食蛇游戏已经可以玩了，你可以独乐乐或者分享给你的朋友。不过在庆祝之前，我们还是来看看最后一个问题。我保证，这绝对是最后一个。


### 潜藏的 bugs 🐛


如果你玩了足够多次游戏，可能会注意到游戏有时会意外结束。这是一个很好的例子，展示了 bug 会潜入我们的程序并制造麻烦🙄。

发现 bug 的时候，解决它的最好方法是首先找到可靠的方法来重现它。也就是说，找到导致意外行为的精确步骤。然后，需要了解它们导致意外行为的原因，最后寻求解决方案。

#### 重现 bug

在我们的例子中，重现 bug 的步骤如下：

* 蛇正向左移动
*	玩家按下向下键
*	玩家立即按下向右键（在 100 ms 内）
*	游戏结束

![](https://p0.ssl.qhimg.com/t01f5db14e92f238906.gif)

#### 分析导致 bug 的原因

让我们一步步分解 bug 产生的过程。

**蛇正在向左移动**

*	水平速度，dx 等于 -10
*	调用 main 函数
*	调用 advanceSnake 函数，蛇向左移动 10 px。

**玩家按下向下键**

*	调用 changeDirection
*	`keyPressed === DOWN_KEY &&  !goingUp` 的值为 true
*	dx 更改为 0
*	dy 更改为 +10

**玩家立即按下向右键（在 100 ms 内）**

*   调用 changeDirection
*   `keyPressed === RIGHT_KEY && !goingLeft` 的值为 true
*   dx 更改为 +10
*   dy 更改为 0

**游戏结束**

*	main 函数**延时 100 ms 后**被调用
*	调用 advanceSnake，蛇向右移动 10 px。
*   `const didCollide = snake[i].x === snake[0].x && snake[i].y === snake[0].y` 的值为true
*   didGameEnd 返回 true
*   main 函数提前返回
*	游戏结束

#### 解决 bug

在分析了发生的事情之后，我们了解到游戏结束是因为蛇掉头了。

这是因为当玩家按下向下键时，dx 被设置为 0。因此 `keyPressed === RIGHT_KEY && !goingLeft` 值为 true，同时 dx 更改为 10。

重要的是要注意，在 **100 ms 时间内**，方向改变了。如果超过 100 ms，那么蛇首先会向下移而不会掉头。

为了解决这个 bug，必须确保只有在 main 和 advanceSnake 被调用之后，才可以改变蛇的方向。可以创建一个变量 **changingDirection。**当调用 changeDirection 时设置 changingDirection 为 true，并在调用 advanceSnake 时设置为 false。

在 changeDirection 函数中，如果 **changingDirection** 为 true，就提前返回。

```
function changeDirection(event) {
  const LEFT_KEY = 37;
  const RIGHT_KEY = 39;
  const UP_KEY = 38;
  const DOWN_KEY = 40;
  if (changingDirection) return;
  changingDirection = true;
  ...
}
function main() {
  setTimeout(function onTick() {
    changingDirection = false;
    
    ...
  }, 100)
}
```

以下是 snake.html 的最终版本

> 注意我还在 `<style></style>` 标签之间添加了一些样式🎨。这是为了让画布和分数显示在屏幕中间。

```
<!DOCTYPE html>
<html>
  <head>
    <title>Snake Game</title>
    <link href="https://fonts.googleapis.com/css?family=Antic+Slab" rel="stylesheet">

  </head>

  <body>

    <div id="score">0</div>
    <canvas id="gameCanvas" width="300" height="300"></canvas>

    <style>
      #gameCanvas {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
      #score {
        text-align: center;
        font-size: 140px;
        font-family: 'Antic Slab', serif;
      }
    </style>
  </body>

  <script>
    const GAME_SPEED = 100;
    const CANVAS_BORDER_COLOUR = 'black';
    const CANVAS_BACKGROUND_COLOUR = "white";
    const SNAKE_COLOUR = 'lightgreen';
    const SNAKE_BORDER_COLOUR = 'darkgreen';
    const FOOD_COLOUR = 'red';
    const FOOD_BORDER_COLOUR = 'darkred';
    let snake = [
      {x: 150, y: 150},
      {x: 140, y: 150},
      {x: 130, y: 150},
      {x: 120, y: 150},
      {x: 110, y: 150}
    ]
    // 玩家的分数
    let score = 0;
    // changingDirection 为 true 时表示蛇正在改变方向
    let changingDirection = false;
    // 食物的 x 坐标
    let foodX;
    // 食物的 y 坐标
    let foodY;
    // 横向移动速度
    let dx = 10;
    // 纵向移动速度
    let dy = 0;
    // 获取 canvas 元素
    const gameCanvas = document.getElementById("gameCanvas");
    // 返回一个二维绘制上下文
    const ctx = gameCanvas.getContext("2d");
    // 开始游戏
    main();
    // 生成第一个食物位置
    createFood();
    // 按下任意一个键，都会调用 changeDirection
    document.addEventListener("keydown", changeDirection);
    /**
     * 游戏的主函数
     * 递归调用以推进游戏  
     */
    function main() {
      // 判定游戏结束时提前返回从而终止游戏
      if (didGameEnd()) return;
      setTimeout(function onTick() {
        changingDirection = false;
        clearCanvas();
        drawFood();
        advanceSnake();
        drawSnake();
        // 再次调用 main
        main();
      }, GAME_SPEED)
    }
    /**
     * 设置画布的背景色为 CANVAS_BACKGROUND_COLOUR 
     * 并绘制画布的边框
     */
    function clearCanvas() {
      //  选择画布的背景颜色
      ctx.fillStyle = CANVAS_BACKGROUND_COLOUR;
      //  选择画布的边框颜色
      ctx.strokestyle = CANVAS_BORDER_COLOUR;
      // 绘制一个“实心的”长方形来覆盖整个画布
      ctx.fillRect(0, 0, gameCanvas.width, gameCanvas.height);
      // 绘制画布的“边框”
      ctx.strokeRect(0, 0, gameCanvas.width, gameCanvas.height);
    }
    /**
     * 在画布上画食物
     */
    function drawFood() {
      ctx.fillStyle = FOOD_COLOUR;
      ctx.strokestyle = FOOD_BORDER_COLOUR;
      ctx.fillRect(foodX, foodY, 10, 10);
      ctx.strokeRect(foodX, foodY, 10, 10);
    }
    /**
     * 根据蛇的水平移动速度改变蛇的 x 坐标，
     * 根据蛇的垂直移动速度改变蛇的 y 坐标
     */
    function advanceSnake() {
      // 绘制新的头部
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // 将新的头部放到蛇身体的第一个部位
      snake.unshift(head);
      const didEatFood = snake[0].x === foodX && snake[0].y === foodY;
      if (didEatFood) {
        // 增加分数
        score += 10;
        // 在屏幕上显示分数
        document.getElementById('score').innerHTML = score;
        // 生成新的食物
        createFood();
      } else {
        // 移除蛇的最后一个部分
        snake.pop();
      }
    }
    /**
     * 当蛇撞墙或者蛇头碰到蛇身的时候返回 true
     */
    function didGameEnd() {
      for (let i = 4; i < snake.length; i++) {
        if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) return true
      }
      const hitLeftWall = snake[0].x < 0;
      const hitRightWall = snake[0].x > gameCanvas.width - 10;
      const hitToptWall = snake[0].y < 0;
      const hitBottomWall = snake[0].y > gameCanvas.height - 10;
      return hitLeftWall || hitRightWall || hitToptWall || hitBottomWall
    }
    /**
     * 给定一个最大值和最小值，生成一个 10 的倍数的随机数     
     * @param { number } min —— 随机数的下限
     * @param { number } max —— 随机数的上限
     */
    function randomTen(min, max) {
      return Math.round((Math.random() * (max-min) + min) / 10) * 10;
    }
    /**     
     * 随机生成食物坐标
     */
    function createFood() {
      // 随机生成食物的 x 坐标
      foodX = randomTen(0, gameCanvas.width - 10);
      // 随机生成食物的 y 坐标
      foodY = randomTen(0, gameCanvas.height - 10);
      // 如果新生成的食物与蛇当前位置重叠，重新为食物生成一个位置
      snake.forEach(function isFoodOnSnake(part) {
        const foodIsoNsnake = part.x == foodX && part.y == foodY;
        if (foodIsoNsnake) createFood();
      });
    }
    /**
     * 在画布上画蛇
     */
    function drawSnake() {
      // 循环遍历蛇的每一部分，并将其绘制到画布上
      snake.forEach(drawSnakePart)
    }
    /**
     * 在画布上画蛇的一个部分
     * @param { object } snakePart —— 需要绘制的部位的所在坐标
     */
    function drawSnakePart(snakePart) {
      // 设置蛇身体的背景颜色
      ctx.fillStyle = SNAKE_COLOUR;
      // 设置蛇身的边框色
      ctx.strokestyle = SNAKE_BORDER_COLOUR;
      // 在蛇身坐标所在的位置，绘制“实心”的矩形以表示蛇      
      ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
      // 绘制蛇身的边框
      ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
    }
    /**     
     * 根据按下的键，改变蛇的水平移动速度和垂直移动速度
     * 为了避免蛇反转，蛇的移动方向不能直接变成相反的方向，
     * 比如说，当前方向是“向右”，那么下一个方向不能是“向左”
     * @param { object } event —— 键盘事件
     */
    function changeDirection(event) {
      const LEFT_KEY = 37;
      const RIGHT_KEY = 39;
      const UP_KEY = 38;
      const DOWN_KEY = 40;
      /**
       * 避免贪食蛇掉头
       * 举个例子：
       * 蛇正在向右移动。玩家按下向下键然后迅速地按下向左键。
       * 此时蛇不会先向下移动一步，而会马上改变方向
       */
      if (changingDirection) return;
      changingDirection = true;
      
      const keyPressed = event.keyCode;
      const goingUp = dy === -10;
      const goingDown = dy === 10;
      const goingRight = dx === 10;
      const goingLeft = dx === -10;
      if (keyPressed === LEFT_KEY && !goingRight) {
        dx = -10;
        dy = 0;
      }
      
      if (keyPressed === UP_KEY && !goingDown) {
        dx = 0;
        dy = -10;
      }
      
      if (keyPressed === RIGHT_KEY && !goingLeft) {
        dx = 10;
        dy = 0;
      }
      
      if (keyPressed === DOWN_KEY && !goingUp) {
        dx = 0;
        dy = 10;
      }
    }
  </script>
</html>
```
### Conclusion
### 结尾

恭喜！🎉👏

我们已经走完了这次旅程。希望你喜欢和我一起学习，并且有信心继续挑战下一次冒险。

不过现在还不必告别。我的下一篇文章将重点介绍如何帮助您开启**非常**令人兴奋的**开源**世界。

[开源](https://en.wikipedia.org/wiki/Open-source_software)是学习**许多**新知识、结识大佬的好方法。虽然最初未必敢于尝试，但这毫无疑问是非常有价值的。

如果想要收到我下一篇文章的发布通知，可以关注我！📫

很高兴能和你一起踏上这段旅程。

期待下次再见。✨
