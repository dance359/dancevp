/// Dance's Optimization Pack V1 - Electron App
/// Project Base Files

// 📁 package.json
{
  "name": "dance-optimization-pack",
  "version": "1.0.0",
  "description": "A powerful FPS, latency, and performance booster for gamers.",
  "main": "main.js",
  "scripts": {
    "start": "electron .",
    "package": "electron-packager . DanceOptimizationPackV1 --platform=win32 --arch=x64 --icon=assets/logo.ico"
  },
  "author": "ChatGPT + Dance",
  "license": "MIT",
  "devDependencies": {
    "electron": "latest",
    "electron-packager": "latest"
  }
}

// 📁 main.js
const { app, BrowserWindow } = require('electron');
const path = require('path');

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 650,
    resizable: false,
    icon: path.join(__dirname, 'assets/logo.png'),
    webPreferences: {
      preload: path.join(__dirname, 'preload.js'),
      contextIsolation: true,
      nodeIntegration: false,
    }
  });

  win.loadFile('index.html');
}

app.whenReady().then(() => {
  createWindow();
  app.on('activate', () => {
    if (BrowserWindow.getAllWindows().length === 0) createWindow();
  });
});

app.on('window-all-closed', () => {
  if (process.platform !== 'darwin') app.quit();
});

// 📁 preload.js
const { contextBridge, exec } = require('electron');
const { execFile } = require('child_process');
const path = require('path');

contextBridge.exposeInMainWorld('optimizer', {
  runScript: (scriptName) => {
    const filePath = path.join(__dirname, 'scripts', `${scriptName}.bat`);
    execFile(filePath, (err) => {
      if (err) {
        console.error('Error running script:', err);
        alert('Failed to apply optimization.');
      } else {
        alert('Optimization applied successfully!');
      }
    });
  }
});

// 📁 index.html (simplified, will expand later)
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Dance's Optimization Pack V1</title>
  <link rel="stylesheet" href="assets/styles.css">
</head>
<body>
  <div class="container">
    <h1>🎮 Dance's Optimization Pack V1</h1>
    <button onclick="window.optimizer.runScript('power_throttle')">Power Throttling Off</button>
    <button onclick="window.optimizer.runScript('disable_gamedvr')">Disable GameDVR</button>
    <button onclick="window.optimizer.runScript('flush_dns')">Flush DNS</button>
    <button onclick="window.optimizer.runScript('optimize_cpu')">Optimize CPU</button>
    <button onclick="window.optimizer.runScript('revert_all')" class="revert">Revert All</button>
  </div>
</body>
</html>

// 📁 assets/styles.css
body {
  margin: 0;
  background-color: #0a0a0a;
  color: #00ffc8;
  font-family: 'Segoe UI', sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.container {
  text-align: center;
  background: #1a1a1a;
  padding: 40px;
  border-radius: 20px;
  box-shadow: 0 0 20px #00ffc8aa;
}
button {
  margin: 10px;
  padding: 12px 25px;
  font-size: 16px;
  background: #00ffc8;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  color: #000;
  transition: all 0.3s ease;
}
button:hover {
  background: #00e6b2;
}
button.revert {
  background: #ff4d4d;
}
button.revert:hover {
  background: #e60000;
}
