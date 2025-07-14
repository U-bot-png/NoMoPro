# NoMoPro
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>NoMoPro - 终结拖延工具</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4fdf6;
            color: #333;
            margin: 0;
            padding: 20px;
        }
        h1, h2 {
            color: #2b7a4b;
        }
        .timer {
            font-size: 48px;
            margin: 20px 0;
        }
        .btn {
            padding: 10px 20px;
            background-color: #2b7a4b;
            color: white;
            border: none;
            cursor: pointer;
            margin-right: 10px;
        }
        .btn:hover {
            background-color: #25663f;
        }
        .task-list {
            margin-top: 20px;
        }
        .task-list input[type="text"] {
            padding: 8px;
            width: 60%;
        }
        ul {
            list-style: none;
            padding: 0;
        }
        li {
            margin: 8px 0;
        }
        .completed {
            text-decoration: line-through;
            color: gray;
        }
        .stats {
            margin-top: 30px;
            padding: 10px;
            background-color: #e4f9eb;
            border-left: 5px solid #2b7a4b;
        }
        .music-player {
            margin-top: 30px;
        }
        audio {
            width: 100%;
            margin-bottom: 10px;
        }
        iframe {
            width: 100%;
            height: 150px;
            border: none;
        }
    </style>
</head>
<body>
    <h1>🍅 NoMoPro - 番茄钟 & 任务清单</h1>

    <div class="timer" id="timer">25:00</div>
    <button class="btn" onclick="startTimer()">开始专注</button>
    <button class="btn" onclick="resetTimer()">重置</button>

    <h2>📋 今日任务</h2>
    <div class="task-list">
        <input type="text" id="taskInput" placeholder="输入一个小任务...">
        <button class="btn" onclick="addTask()">添加任务</button>
        <ul id="taskList"></ul>
    </div>

    <button class="btn" onclick="recordProcrastination()">我又拖延了 😅</button>

    <div class="stats">
        ✅ 已完成任务：<span id="completedCount">0</span><br>
        💤 拖延次数：<span id="procrastinationCount">0</span>
    </div>

    <div class="music-player">
        <h2>🎵 背景音乐</h2>
        <audio controls loop>
            <source src="https://cdn.pixabay.com/download/audio/2022/03/15/audio_352cfaa4c6.mp3" type="audio/mpeg">
            您的浏览器不支持音频播放。
        </audio>
        <audio controls loop>
            <source src="https://cdn.pixabay.com/download/audio/2023/03/02/audio_4f1e89fa02.mp3" type="audio/mpeg">
            您的浏览器不支持音频播放。
        </audio>
        <p>📺 也可以播放 YouTube 背景音乐：</p>
        <iframe src="https://www.youtube.com/embed/5qap5aO4i9A?autoplay=0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>

    <script>
        let timer;
        let minutes = 25;
        let seconds = 0;
        let isRunning = false;
        let completedTasks = 0;
        let procrastinationCount = 0;

        function updateTimerDisplay() {
            const display = document.getElementById("timer");
            let minStr = minutes < 10 ? "0" + minutes : minutes;
            let secStr = seconds < 10 ? "0" + seconds : seconds;
            display.textContent = `${minStr}:${secStr}`;
        }

        function startTimer() {
            if (isRunning) return;
            isRunning = true;
            timer = setInterval(() => {
                if (seconds === 0) {
                    if (minutes === 0) {
                        clearInterval(timer);
                        isRunning = false;
                        alert("👏 你完成了一个番茄钟！休息5分钟吧！");
                        minutes = 5;
                        seconds = 0;
                        updateTimerDisplay();
                        return;
                    } else {
                        minutes--;
                        seconds = 59;
                    }
                } else {
                    seconds--;
                }
                updateTimerDisplay();
            }, 1000);
        }

        function resetTimer() {
            clearInterval(timer);
            isRunning = false;
            minutes = 25;
            seconds = 0;
            updateTimerDisplay();
        }

        function addTask() {
            const input = document.getElementById("taskInput");
            const taskText = input.value.trim();
            if (taskText === "") return;
            const li = document.createElement("li");
            const checkbox = document.createElement("input");
            checkbox.type = "checkbox";
            checkbox.onchange = function () {
                if (this.checked) {
                    li.classList.add("completed");
                    completedTasks++;
                    document.getElementById("completedCount").textContent = completedTasks;
                    alert("🎉 太棒了！你完成了一个任务！");
                } else {
                    li.classList.remove("completed");
                    completedTasks--;
                    document.getElementById("completedCount").textContent = completedTasks;
                }
            };
            li.appendChild(checkbox);
            li.appendChild(document.createTextNode(" " + taskText));
            document.getElementById("taskList").appendChild(li);
            input.value = "";
        }

        function recordProcrastination() {
            procrastinationCount++;
            document.getElementById("procrastinationCount").textContent = procrastinationCount;
            alert("没关系，下一次我们一起更努力 💪");
        }

        updateTimerDisplay();
    </script>
</body>
</html>
