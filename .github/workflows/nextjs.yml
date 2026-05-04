


-<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Doraemon Adventure Quiz - 10 Questions</title>
    <style>
        :root {
            --dora-blue: #0091d5;
            --dora-red: #e60012;
            --dora-yellow: #fff000;
            --bg-dark: #0a0a0a;
            --text-light: #ffffff;
        }

        body, html {
            margin: 0; padding: 0;
            width: 100%; height: 100%;
            overflow: hidden;
            font-family: 'Segoe UI', Roboto, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-light);
        }

        .wrapper {
            display: flex;
            flex-direction: column;
            height: 100%;
            transition: transform 0.8s cubic-bezier(0.85, 0, 0.15, 1);
        }

        .section {
            height: 100vh; width: 100%;
            flex-shrink: 0;
            display: flex; flex-direction: column;
            justify-content: center; align-items: center;
            position: relative;
        }

        /* --- TIMER BAR --- */
        #timerContainer {
            position: fixed; top: 0; left: 0;
            width: 100%; height: 8px;
            background: rgba(255,255,255,0.1);
            z-index: 1000;
            display: none;
        }
        #timerBar {
            width: 100%; height: 100%;
            background: var(--dora-red);
            transition: width 0.1s linear;
        }

        /* --- CHAR CARDS --- */
        .selection-container { display: flex; gap: 15px; flex-wrap: wrap; justify-content: center; margin-top: 20px; }
        .char-card {
            background: #1e272e;
            border: 3px solid #3d4e5d;
            border-radius: 20px;
            padding: 10px;
            width: 130px;
            cursor: pointer;
            transition: 0.3s;
            text-align: center;
        }
        .char-card:hover { transform: scale(1.1); border-color: var(--dora-yellow); }
        .char-card.selected { border-color: var(--dora-blue); box-shadow: 0 0 20px var(--dora-blue); }
        .char-card img { width: 100%; border-radius: 15px; margin-bottom: 10px; }

        /* --- QUIZ BOX --- */
        .quiz-box {
            background: rgba(30, 39, 46, 0.9);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 30px;
            width: 85%; max-width: 500px;
            border: 2px solid var(--dora-blue);
        }

        .option-btn {
            width: 100%;
            padding: 14px;
            margin: 8px 0;
            background: #2f3542;
            border: 1px solid #57606f;
            border-radius: 12px;
            color: white;
            font-size: 1rem;
            cursor: pointer;
            transition: 0.2s;
        }
        .option-btn:hover { background: var(--dora-blue); transform: translateX(5px); }

        #companionDisplay {
            position: fixed; top: 20px; right: 20px;
            width: 70px; height: 70px;
            border-radius: 50%;
            border: 3px solid var(--dora-blue);
            background: #fff;
            display: none; z-index: 100;
        }

        .btn-main {
            padding: 15px 40px;
            border-radius: 50px;
            border: none;
            background: var(--dora-blue);
            color: white;
            font-size: 1.2rem;
            font-weight: bold;
            cursor: pointer;
            box-shadow: 0 5px 15px rgba(0, 145, 213, 0.4);
        }
    </style>
</head>
<body>

    <div id="timerContainer"><div id="timerBar"></div></div>
    <div id="companionDisplay"><img id="chosenAvatar" src="" style="width: 100%;"></div>

    <div class="wrapper" id="mainWrapper">
        
        <section class="section">
            <h1 style="font-size: 3.5rem; color: var(--dora-blue); text-shadow: 2px 2px #fff;">DORAEMON</h1>
            <h2 style="letter-spacing: 5px; opacity: 0.8;">THỬ THÁCH 10 CÂU HỎI</h2>
            <button class="btn-main" style="margin-top: 40px;" onclick="scrollToSection(1)">PLAY NOW</button>
        </section>

        <section class="section">
            <h2 style="color: var(--dora-yellow);">CHỌN BẠN ĐỒNG HÀNH</h2>
            <div class="selection-container" id="charGrid"></div>
            <button class="btn-main" style="margin-top: 40px;" onclick="startQuiz()">VÀO TRẬN</button>
        </section>

        <section class="section">
            <div class="quiz-box">
                <div id="timerText" style="text-align: right; color: var(--dora-red); font-weight: bold; font-size: 1.2rem;">07.0s</div>
                <p id="questionCount" style="color: var(--dora-blue); font-weight: bold;"></p>
                <h3 id="questionText" style="min-height: 60px;"></h3>
                <div id="options"></div>
            </div>
        </section>

        <section class="section">
            <h2>KẾT QUẢ CUỐI CÙNG</h2>
            <h1 id="finalScore" style="font-size: 5rem; color: var(--dora-yellow); margin: 10px 0;">0</h1>
            <div id="rankTag" style="font-size: 1.5rem; margin-bottom: 20px; font-weight: bold;">RANK: F</div>
            
            <div id="giftArea">
                <div style="font-size: 80px; cursor: pointer;" onclick="openGift()">🎁</div>
                <p>Mở bảo bối bí mật</p>
            </div>
            <div id="surprise" style="display:none; color: var(--dora-red); font-size: 3rem; font-weight: bold;">FUCK YOU!</div>
            
            <button class="btn-main" onclick="restartGame()" style="margin-top: 30px; background: #57606f;">CHƠI LẠI</button>
        </section>

    </div>

    <script>
        const characters = [
            { seed: "Doraemon", name: "Doraemon" },
            { seed: "Nobita", name: "Nobita" },
            { seed: "Shizuka", name: "Shizuka" },
            { seed: "Gian", name: "Chaien" },
            { seed: "Suneo", name: "Xê-kô" }
        ];

        const allQuestionsDB = [
            { q: "Doraemon đến từ thế kỷ nào?", a: ["XX", "XXI", "XXII", "XXIII"], c: 2 },
            { q: "Món ăn yêu thích của Doraemon là gì?", a: ["Bánh mì", "Bánh rán", "Mì ống", "Cơm nắm"], c: 1 },
            { q: "Màu gốc của Doraemon là gì?", a: ["Xanh lá", "Đỏ", "Vàng", "Cam"], c: 2 },
            { q: "Nobita giỏi môn thể thao nào nhất?", a: ["Bóng đá", "Bóng rổ", "Bắn súng", "Bơi lội"], c: 2 },
            { q: "Tên thật của Chaien là gì?", a: ["Takeshi", "Suneo", "Hidetoshi", "Dekisugi"], c: 0 },
            { q: "Shizuka thích ăn món gì nhất?", a: ["Bánh quy", "Khoai lang nướng", "Sushi", "Ramen"], c: 1 },
            { q: "Bảo bối nào giúp đi đến bất cứ đâu?", a: ["Cỗ máy thời gian", "Đèn pin thu nhỏ", "Cánh cửa thần kỳ", "Chong chóng tre"], c: 2 },
            { q: "Doraemon sợ con vật nào nhất?", a: ["Chó", "Gián", "Chuột", "Nhện"], c: 2 },
            { q: "Cỗ máy thời gian nằm ở đâu?", a: ["Tủ áo", "Ngăn kéo bàn", "Gầm giường", "Sân sau"], c: 1 },
            { q: "Em gái của Doraemon tên là gì?", a: ["Dorami", "Dorako", "Doramini", "Doraemonky"], c: 0 },
            { q: "Nobita thường bị điểm mấy?", a: ["0", "1", "5", "10"], c: 0 },
            { q: "Ai là người học giỏi nhất lớp Nobita?", a: ["Suneo", "Chaien", "Dekisugi", "Shizuka"], c: 2 },
            { q: "Bảo bối nào gắn lên đầu để bay?", a: ["Cánh chim", "Chong chóng tre", "Khăn choàng bay", "Tên lửa"], c: 1 },
            { q: "Bố của Nobita tên là gì?", a: ["Nobi Nobirou", "Nobi Nobisuke", "Nobi Nobita", "Nobi Tamako"], c: 1 },
            { q: "Doraemon có bao nhiêu túi thần kỳ?", a: ["1", "2", "3", "4"], c: 1 },
            { q: "Suneo thường khoe khoang về điều gì?", a: ["Học tập", "Sức mạnh", "Sự giàu có", "Lòng tốt"], c: 2 },
            { q: "Chaien có ước mơ trở thành gì?", a: ["Cầu thủ", "Ca sĩ", "Đầu bếp", "Họa sĩ"], c: 1 },
            { q: "Nobita ngủ quên trong bao lâu?", a: ["1 giây", "0.93 giây", "3 giây", "5 giây"], c: 1 },
            { q: "Vợ tương lai của Nobita (ban đầu) là ai?", a: ["Shizuka", "Jaiko", "Dorami", "Lululu"], c: 1 },
            { q: "Cửa hàng nhà Chaien tên gì?", a: ["Tiệm bánh", "Nhà sách", "Tạp hóa", "Cây cảnh"], c: 2 },
            { q: "Doraemon được sản xuất tại đâu?", a: ["Nhà máy robot", "Tương lai", "Trung Quốc", "Mỹ"], c: 0 },
            { q: "Túi thần kỳ chứa được bao nhiêu đồ?", a: ["100 cái", "1000 cái", "Vô hạn", "10 cái"], c: 2 },
            { q: "Nobita chơi dây rất giỏi đúng không?", a: ["Sai", "Đúng", "Tàm tạm", "Rất kém"], c: 1 },
            { q: "Chong chóng tre màu gì?", a: ["Xanh", "Vàng", "Đỏ", "Trắng"], c: 1 },
            { q: "Doraemon mất tai do con gì cắn?", a: ["Chó", "Mèo", "Chuột robot", "Chuột cống"], c: 2 },
            { q: "Màu xanh của Doraemon là do?", a: ["Sơn lại", "Khóc trôi sơn vàng", "Bị nhuộm", "Màu gốc"], c: 1 },
            { q: "Bánh mì giúp học thuộc lòng?", a: ["Bánh mì trí nhớ", "Bánh mì kiến thức", "Bánh mì thông minh", "Bánh mì học tập"], c: 0 },
            { q: "Ai thường xuyên đi tắm?", a: ["Nobita", "Chaien", "Suneo", "Shizuka"], c: 3 },
            { q: "Dekisugi có tên đầy đủ là gì?", a: ["Hidetoshi Dekisugi", "Takeshi Dekisugi", "Suneo Dekisugi", "Nobita Dekisugi"], c: 0 },
            { q: "Tác giả của Doraemon là ai?", a: ["Osamu Tezuka", "Fujiko F. Fujio", "Akira Toriyama", "Eiichiro Oda"], c: 1 }
        ];

        let selectedAvatar = "";
        let currentIdx = 0;
        let score = 0;
        let timer;
        let timeLeft = 70;
        let activeQuestions = [];

        // Khởi tạo thẻ nhân vật
        const grid = document.getElementById('charGrid');
        characters.forEach(char => {
            const div = document.createElement('div');
            div.className = 'char-card';
            const imgUrl = `https://api.dicebear.com/7.x/bottts-neutral/svg?seed=${char.seed}`;
            div.innerHTML = `<img src="${imgUrl}"><h4>${char.name}</h4>`;
            div.onclick = () => {
                document.querySelectorAll('.char-card').forEach(c => c.classList.remove('selected'));
                div.classList.add('selected');
                selectedAvatar = imgUrl;
                document.getElementById('chosenAvatar').src = imgUrl;
            };
            grid.appendChild(div);
        });

        function scrollToSection(idx) {
            document.getElementById('mainWrapper').style.transform = `translateY(-${idx * 100}vh)`;
            document.getElementById('companionDisplay').style.display = (idx >= 2) ? "block" : "none";
            document.getElementById('timerContainer').style.display = (idx === 2) ? "block" : "none";
        }

        function startQuiz() {
            if(!selectedAvatar) return alert("Bạn chưa chọn nhân vật đồng hành kìa!");
            
            // Xử lý random 10 câu từ 30 câu
            activeQuestions = [...allQuestionsDB]
                .sort(() => Math.random() - 0.5) // Trộn ngẫu nhiên
                .slice(0, 10); // Lấy 10 câu đầu tiên
                
            score = 0; 
            currentIdx = 0;
            scrollToSection(2);
            nextQuestion();
        }

        function nextQuestion() {
            clearInterval(timer);
            if(currentIdx >= 10) {
                showResult();
                return;
            }
            
            const q = activeQuestions[currentIdx];
            document.getElementById('questionCount').innerText = `CÂU ${currentIdx + 1}/10`;
            document.getElementById('questionText').innerText = q.q;
            
            const optDiv = document.getElementById('options');
            optDiv.innerHTML = "";
            q.a.forEach((opt, i) => {
                const btn = document.createElement('button');
                btn.className = 'option-btn';
                btn.innerText = opt;
                btn.onclick = () => checkAnswer(i);
                optDiv.appendChild(btn);
            });

            startTimer();
        }

        function startTimer() {
            timeLeft = 70;
            document.getElementById('timerBar').style.width = "100%";
            document.getElementById('timerText').innerText = "07.0s";
            
            timer = setInterval(() => {
                timeLeft--;
                document.getElementById('timerBar').style.width = (timeLeft / 70) * 100 + "%";
                document.getElementById('timerText').innerText = (timeLeft / 10).toFixed(1) + "s";
                
                if(timeLeft <= 0) {
                    clearInterval(timer);
                    checkAnswer(-1); 
                }
            }, 100);
        }

        function checkAnswer(selected) {
            clearInterval(timer);
            const correct = activeQuestions[currentIdx].c;
            if(selected === correct) score += 10;
            
            currentIdx++;
            setTimeout(nextQuestion, 400);
        }

        function showResult() {
            document.getElementById('finalScore').innerText = score;
            let rank = "NOBITA (HẬU ĐẬU)";
            if(score === 100) rank = "DEKISUGI (THIÊN TÀI)";
            else if(score >= 80) rank = "DORAEMON (THÔNG THÁI)";
            else if(score >= 50) rank = "SHIZUKA (CHĂM CHỈ)";
            
            document.getElementById('rankTag').innerText = `RANK: ${rank}`;
            scrollToSection(3);
        }

        function openGift() {
            document.getElementById('giftArea').style.display = 'none';
            document.getElementById('surprise').style.display = 'block';
        }

        function restartGame() {
            scrollToSection(0);
            setTimeout(() => {
                document.getElementById('giftArea').style.display = 'block';
                document.getElementById('surprise').style.display = 'none';
            }, 800);
        }
    </script>
</body>
</html>
