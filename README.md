<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <title>Мистер Вкусно — Roulette</title>
    <style>
        body {
            background-color: #000000ff;
            color: white;
            font-family: 'Arial Black', sans-serif;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            height: 100vh;
            margin: 0;
            overflow: hidden;
        }

        /* Твоя картинка в углу */
        .user-avatar {
            position: absolute;
            top: 20px;
            right: 20px;
            width: 80px;
            height: 80px;
            border-radius: 50%;
            border: 3px solid #00ff00;
        }

        /*Mister Vkusno*/
        h1 {
            font-size: 80px;
            margin: 0;
            text-transform: uppercase;
            letter-spacing: -2px;
            color: #fff;
            text-shadow: 5px 5px 0px #ff4500;
        }

        .hashtags {
            font-size: 18px;
            color: #aaa;
            margin-bottom: 30px;
        }

        /* Рулетка */
        #wheel-container {
            position: relative;
            width: 300px;
            height: 300px;
            border: 10px solid #fff;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            background: radial-gradient(circle, #333, #000);
        }

        #pointer {
            position: absolute;
            top: -20px;
            width: 0; 
            height: 0; 
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-top: 30px solid red;
            z-index: 10;
        }

        button {
            margin-top: 30px;
            padding: 15px 40px;
            font-size: 20px;
            font-weight: bold;
            cursor: pointer;
            background: #00ff00;
            border: none;
            border-radius: 5px;
        }

        #result {
            margin-top: 20px;
            font-size: 24px;
            color: gold;
        }
    </style>
</head>
<body>

    <img src="c:\Users\ntoma\Downloads\SC-Logo-Original-500x545.webp" alt="Avatar" class="user-avatar">

    <h1>МИСТЕР ВКУСНО</h1>
    <div class="hashtags">#GiftedBySupercell #SupercellPartner</div>

    <div id="wheel-container">
        <div id="pointer"></div>
        <div id="wheel-text">КРУТИ!</div>
    </div>

    <button onclick="spin()">ИСПЫТАТЬ УДАЧУ</button>
    <div id="result"></div>

<script>
    // Список призов с количеством (count)
    let prizes = [
        { name: "5 Rare Starr Drop", chance: 0.0899, img: "c:\Users\ntoma\Downloads\photo_5389016748037707085_x.jpg", count: 3 }, // Выпадет 1 раз и исчезнет
        { name: "Retro Nani", chance: 0.2, img: "c:\Users\ntoma\Downloads\photo_5389016748037707084_x.jpg", count: 15 },        // Выпадет 3 раза и исчезнет
        { name: "Pirates Thumbup", chance: 0.1, img: "c:\Users\ntoma\Downloads\photo_5389016748037707083_x.jpg", count: 1 },       
        { name: "Cuddly Kit", chance: 0.01, img: "c:\Users\ntoma\Downloads\photo_5389016748037707081_x.jpg", count: 10 },
        { name: "Monterey Moe", chance: 0.2, img: "c:\Users\ntoma\Downloads\photo_5389016748037707082_x.jpg", count: 14 },
        { name: "Brawl Pass+", chance: 0.0001, img: "c:\Users\ntoma\Downloads\6777ecd078d7b72ee8ed0cb92908b90d.png", count: 1 },
        { name: "-", chance: 0.4, img: "c:\Users\ntoma\Downloads\b7a3ebad-121c-430a-b5c8-686d2bb2768f.jpg", count: 10 }
    ];

    const pass = prompt("Введите код доступа:");
    if (pass !== "5252") { 
        document.body.innerHTML = "<h1>ДОСТУП ЗАПРЕЩЕН</h1>";
    }

    function spin() {
        if (prizes.length === 0) {
            document.getElementById('result').innerText = "Все призы закончились!";
            return;
        }

        const rand = Math.random();
        let cumulativeChance = 0;
        let winningIndex = -1;

        // Логика выбора с учетом шансов
        for (let i = 0; i < prizes.length; i++) {
            cumulativeChance += prizes[i].chance;
            if (rand < cumulativeChance) {
                winningIndex = i;
                break;
            }
        }

        // Если вдруг из-за округления индекс не найден, берем последний
        if (winningIndex === -1) winningIndex = prizes.length - 1;

        const winner = prizes[winningIndex];
        
        // Уменьшаем количество предметов
        winner.count -= 1;

        document.getElementById('result').innerHTML = `
            Выпало: ${winner.img} ${winner.name} <br>
            <span style="font-size: 14px; color: #777;">Осталось: ${winner.count} шт.</span>
        `;
        
        // ЕСЛИ КОЛИЧЕСТВО СТАЛО 0 — УДАЛЯЕМ ИЗ СПИСКА
        if (winner.count <= 0) {
            console.log(winner.name + " закончился и удален.");
            prizes.splice(winningIndex, 1);
            
            // Пересчитываем шансы для оставшихся предметов, чтобы сумма всегда была 1
            if (prizes.length > 0) {
                const totalRemainingChance = prizes.reduce((sum, p) => sum + p.chance, 0);
                prizes.forEach(p => p.chance /= totalRemainingChance);
            }
        }
    }
</script>
</body>
</html>
