---
tags: 程式設計教材庫,範例版,互動藝術程式創作入門,Creative Coding

# 自行設計畫面

## 步驟一:畫出你的基本圖

**Create two canvases to display different graphics at the same time**

```javascript=
let canvas1, canvas2;

function setup() {
    canvas1 = createGraphics(200, 200);
    canvas2 = createGraphics(200, 200);

    // Set the main canvas size to 400x200, serving as a container for two smaller canvases
    createCanvas(400, 200).parent('canvas-container');
}

function draw() {
    // Draw the first canvas (circle)
    canvas1.background("#FFFFFF"); // White background
    canvas1.noFill();
    canvas1.stroke("#000000"); // Black stroke
    canvas1.strokeWeight(3);
    canvas1.ellipse(100, 100, 50);

    // Draw the second canvas (square)
    canvas2.background("#FFFFFF"); // White background
    canvas2.noFill();
    canvas2.stroke("#000000"); // Black stroke
    canvas2.strokeWeight(3);
    canvas2.rect(75, 75, 50, 50);

    // Display both canvases on the main canvas
    image(canvas1, 0, 0);
    image(canvas2, 200, 0);
}
```

## 步驟二:基本圖充滿一整行



```javascript=
function setup() {
    createCanvas(400, 400); // Adjust the canvas size to fit shapes, reducing height for a single-row layout
}

function draw() {
    background("#FFFFFF"); // White background
    noFill();
    stroke("#000000"); // Black stroke
    strokeWeight(3); // Set stroke thickness

    for (let i = 0; i < 10; i++) {
        ellipse(25 + i * 50, 25, 50);
        rect(i * 50, 0, 50, 50);
    }
}

```

## 步驟三:基本圖充滿整個畫面



```javascript=
function setup() {
    createCanvas(400, 400); // Adjust the canvas size for vertical layout
}

function draw() {
    background("#FFFFFF"); // White background
    noFill();
    stroke("#000000"); // Black stroke
    strokeWeight(3);

    for (let j = 0; j < 8; j++) {
        for (let i = 0; i < 8; i++) {
            ellipse(25 + i * 50, 25 + j * 50, 50); // Existing circles
            ellipse(25 + i * 50 + 25, 25 + j * 50 + 25, 25); // Additional smaller circles
            rect(i * 50, j * 50, 50, 50);
        }
    }
}

```

## 步驟四:隨著滑鼠互動改變圖的樣式

**Use WEBGL rendering to solve the lag problem**

```javascript=
let mainGraphics;

function setup() {
    createCanvas(400, 400, WEBGL);
    mainGraphics = createGraphics(400, 400);
}

function draw() {
    mainGraphics.background("#FFFFFF");
    mainGraphics.noFill();
    mainGraphics.stroke("#000000"); 
    mainGraphics.strokeWeight(3);

    var r_w = 50;
    var bc_w = 50;
    var sc_w = 25;

    // Remove the first loop, as it would repeatedly draw in the y=0~50 range
    // Use a single nested loop to handle all drawings
    for(let j = 0; j < 8; j++) {
        for(let i = 0; i < 8; i++) {
            let d = dist(mouseX, mouseY, 25 + bc_w * i, 25 + j * 50);
            let size = map(d, 0, 100, 20, bc_w);
            size = constrain(size, 20, bc_w);

            mainGraphics.ellipse(25 + bc_w * i, 25 + j * 50, size);
            mainGraphics.rect(r_w * i, j * 50, size);
            mainGraphics.ellipse(r_w * (i + 1), 50 + j * 50, size / 2);
        }
    }

    // Render the graphics on the WEBGL canvas
    background(255);
    texture(mainGraphics);
    plane(400, 400);
}

```
# 自行設計畫面

![錄製內容 2024-11-05 224408](https://hackmd.io/_uploads/H1itb2wW1x.gif)


```javascript=
let mainGraphics;

function setup() {
    createCanvas(550, 550, WEBGL); // 加大畫布大小
    mainGraphics = createGraphics(550, 550); // 更新繪圖大小
}

function draw() {
    mainGraphics.background("#FFFFFF");
    mainGraphics.noFill();
    mainGraphics.stroke("#000000"); 
    mainGraphics.strokeWeight(3);
            
    var r_w = 75; // 調整矩形寬度
    var bc_w = 75; // 調整圓形寬度
    var sc_w = 38; // 調整小圓形寬度
            
    // 移除第一個迴圈,因為它會在y=0~50的位置重複繪製
    // 改用單一巢狀迴圈處理所有繪製
    for(let j = 0; j < 8; j = j + 1) {
        for(let i = 0; i < 8; i = i + 1) {
            let d = dist(mouseX, mouseY, 38 + bc_w * i, 38 + j * 75); // 更新距離計算
            let size = map(d, 0, 150, 30, bc_w); // 更新大小映射
            size = constrain(size, 30, bc_w); // 更新大小限制
                    
            mainGraphics.ellipse(38 + bc_w * i, 38 + j * 75, size); // 更新圓形位置
            mainGraphics.rect(r_w * i, j * 75, size); // 更新矩形位置
            mainGraphics.ellipse(r_w * (i + 1), 75 + j * 75, size/2); // 更新小圓形位置
        }
    }

    // 在繪圖中間新增文字並使其能互動
    let textX = 275; // 文字的X位置
    let textY = 275; // 文字的Y位置
    let d = dist(mouseX, mouseY, textX, textY); // 計算滑鼠與文字的距離
    let size = map(d, 0, 150, 32, 48); // 根據距離調整文字大小
    mainGraphics.fill(255, 0, 0); // 文字顏色改為紅色
    mainGraphics.textSize(size); // 更新文字大小
    mainGraphics.textAlign(CENTER, CENTER); // 置中對齊
    mainGraphics.text("Jim", textX, textY); // 文字位置 更改文字內容在""中

    // 將繪圖結果渲染到WEBGL畫布
    background(255);
    texture(mainGraphics);
    plane(550, 550); // 更新平面大小

}
```
