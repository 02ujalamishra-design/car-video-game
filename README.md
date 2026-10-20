<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Turbo Drift - Car Game</title>
<style>
  *{margin:0;padding:0;box-sizing:border-box}
  body{background:#0f1115;color:#fff;font-family:system-ui,sans-serif;display:flex;justify-content:center;align-items:center;min-height:100vh;overflow:hidden}
  #gameWrap{position:relative;width:100%;max-width:420px;height:100vh;max-height:780px;background:#1a1d24;border-radius:16px;overflow:hidden;box-shadow:0 20px 60px rgba(0,0,0,.5)}
  canvas{display:block;width:100%;height:100%}
  #ui{position:absolute;top:0;left:0;right:0;padding:16px;display:flex;justify-content:space-between;pointer-events:none}
  .badge{background:rgba(0,0,0,.6);backdrop-filter:blur(10px);padding:6px 12px;border-radius:20px;font-weight:700;font-size:14px}
  #center{position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);text-align:center;width:90%}
  #center h1{font-size:38px;letter-spacing:2px;font-style:italic}
  #center p{opacity:.7;margin:10px 0 18px}
  button{background:#ff3b30;color:#fff;border:0;padding:12px 28px;border-radius:30px;font-weight:800;font-size:16px;cursor:pointer;box-shadow:0 8px 20px rgba(255,59,48,.4)}
  button:active{transform:scale(.96)}
  #controls{position:absolute;bottom:20px;left:0;right:0;display:flex;justify-content:space-between;padding:0 20px;pointer-events:none}
  .ctrl{pointer-events:auto;width:64px;height:64px;background:rgba(255,255,255,.12);backdrop-filter:blur(10px);border:1px solid rgba(255,255,255,.2);border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:22px;user-select:none}
  .ctrl.right{margin-left:auto}
  #nitro{position:absolute;bottom:24px;left:50%;transform:translateX(-50%);pointer-events:auto}
</style>
</head>
<body>
<div id="gameWrap">
  <canvas id="c"></canvas>
  <div id="ui">
    <div class="badge" id="score">0 km</div>
    <div class="badge" id="speed">0 km/h</div>
  </div>
  <div id="center">
    <h1>TURBO</h1>
    <p>Arrow / A D to steer, W to nitro<br>Don't crash!</p>
    <button id="startBtn">START ENGINE</button>
  </div>
  <div id="controls" style="display:none">
    <div class="ctrl" id="leftBtn">◀</div>
    <div class="ctrl right" id="rightBtn">▶</div>
  </div>
  <div id="nitro" style="display:none">
    <div class="ctrl" id="nitroBtn">⚡</div>
  </div>
</div>

<script>
const canvas = document.getElementById('c');
const ctx = canvas.getContext('2d');
const scoreEl = document.getElementById('score');
const speedEl = document.getElementById('speed');
const centerEl = document.getElementById('center');
const controlsEl = document.getElementById('controls');
const nitroEl = document.getElementById('nitro');

let W, H, playing=false, score=0, speed=0, baseSpeed=6, maxSpeed=18;
let keys = {}, roadOffset=0, car,