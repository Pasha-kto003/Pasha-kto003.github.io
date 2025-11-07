<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Invaders WPF - Космический шутер</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0a0a2a, #1a1a4a, #2a2a6a);
            color: #ffffff;
            line-height: 1.6;
            overflow-x: hidden;
        }

        .stars {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: -1;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 5s infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.2; }
            50% { opacity: 1; }
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }

        /* Header */
        header {
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(10px);
            padding: 20px 0;
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 1000;
            border-bottom: 2px solid #00ffff;
        }

        nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            font-size: 2rem;
            font-weight: bold;
            color: #00ffff;
            text-shadow: 0 0 10px #00ffff;
        }

        .nav-links {
            display: flex;
            list-style: none;
            gap: 30px;
        }

        .nav-links a {
            color: #ffffff;
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s;
            position: relative;
        }

        .nav-links a:hover {
            color: #00ffff;
        }

        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: #00ffff;
            transition: width 0.3s;
        }

        .nav-links a:hover::after {
            width: 100%;
        }

        /* Hero Section */
        .hero {
            padding: 180px 0 100px;
            text-align: center;
            background: radial-gradient(circle at center, rgba(0, 255, 255, 0.1) 0%, transparent 70%);
        }

        .hero h1 {
            font-size: 4rem;
            margin-bottom: 20px;
            background: linear-gradient(45deg, #00ffff, #ff00ff);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(0, 255, 255, 0.5);
        }

        .hero p {
            font-size: 1.3rem;
            margin-bottom: 30px;
            color: #ccccff;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }

        .cta-button {
            display: inline-block;
            padding: 15px 40px;
            background: linear-gradient(45deg, #00ffff, #ff00ff);
            color: #000;
            text-decoration: none;
            border-radius: 50px;
            font-weight: bold;
            font-size: 1.1rem;
            transition: transform 0.3s, box-shadow 0.3s;
            border: none;
            cursor: pointer;
            margin: 10px;
        }

        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px rgba(0, 255, 255, 0.4);
        }

        .cta-button.secondary {
            background: transparent;
            border: 2px solid #00ffff;
            color: #00ffff;
        }

        /* Features Section */
        .features {
            padding: 100px 0;
            background: rgba(0, 0, 0, 0.3);
        }

        .section-title {
            text-align: center;
            font-size: 2.5rem;
            margin-bottom: 50px;
            color: #00ffff;
        }

        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }

        .feature-card {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 15px;
            border: 1px solid rgba(0, 255, 255, 0.3);
            transition: transform 0.3s, border-color 0.3s;
        }

        .feature-card:hover {
            transform: translateY(-10px);
            border-color: #00ffff;
        }

        .feature-icon {
            font-size: 3rem;
            margin-bottom: 20px;
            color: #00ffff;
        }

        .feature-card h3 {
            font-size: 1.5rem;
            margin-bottom: 15px;
            color: #00ffff;
        }

        /* Game Modes */
        .game-modes {
            padding: 100px 0;
        }

        .modes-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 30px;
        }

        .mode-card {
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 40px;
            text-align: center;
            border: 2px solid transparent;
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }

        .mode-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(0, 255, 255, 0.1), transparent);
            transition: left 0.5s;
        }

        .mode-card:hover::before {
            left: 100%;
        }

        .mode-card.campaign {
            border-color: #00ffff;
        }

        .mode-card.infinite {
            border-color: #ff00ff;
        }

        .mode-card h3 {
            font-size: 1.8rem;
            margin-bottom: 20px;
            color: #00ffff;
        }

        .mode-card.infinite h3 {
            color: #ff00ff;
        }

        .mode-card ul {
            list-style: none;
            text-align: left;
            margin-bottom: 25px;
        }

        .mode-card li {
            margin-bottom: 10px;
            padding-left: 25px;
            position: relative;
        }

        .mode-card li::before {
            content: '▶';
            position: absolute;
            left: 0;
            color: #00ffff;
        }

        .mode-card.infinite li::before {
            color: #ff00ff;
        }

        /* Installation */
        .installation {
            padding: 100px 0;
            background: rgba(0, 0, 0, 0.3);
        }

        .steps {
            max-width: 800px;
            margin: 0 auto;
        }

        .step {
            background: rgba(255, 255, 255, 0.05);
            padding: 30px;
            border-radius: 10px;
            margin-bottom: 20px;
            border-left: 4px solid #00ffff;
        }

        .step-number {
            display: inline-block;
            background: #00ffff;
            color: #000;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            text-align: center;
            line-height: 30px;
            font-weight: bold;
            margin-right: 15px;
        }

        .code-block {
            background: #1a1a2a;
            padding: 20px;
            border-radius: 8px;
            margin: 20px 0;
            border: 1px solid #333;
            font-family: 'Courier New', monospace;
            overflow-x: auto;
        }

        /* Screenshots */
        .screenshots {
            padding: 100px 0;
        }

        .screenshot-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .screenshot {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 20px;
            text-align: center;
            transition: transform 0.3s;
        }

        .screenshot:hover {
            transform: scale(1.05);
        }

        .screenshot-placeholder {
            width: 100%;
            height: 150px;
            background: linear-gradient(45deg, #00ffff, #ff00ff);
            border-radius: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-bottom: 15px;
        }

        /* Footer */
        footer {
            background: rgba(0, 0, 0, 0.9);
            padding: 50px 0 20px;
            border-top: 2px solid #00ffff;
        }

        .footer-content {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 40px;
            margin-bottom: 30px;
        }

        .footer-section h3 {
            color: #00ffff;
            margin-bottom: 20px;
        }

        .footer-links {
            list-style: none;
        }

        .footer-links li {
            margin-bottom: 10px;
        }

        .footer-links a {
            color: #ccccff;
            text-decoration: none;
            transition: color 0.3s;
        }

        .footer-links a:hover {
            color: #00ffff;
        }

        .copyright {
            text-align: center;
            padding-top: 20px;
            border-top: 1px solid #333;
            color: #888;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .nav-links {
                display: none;
            }

            .hero h1 {
                font-size: 2.5rem;
            }

            .hero p {
                font-size: 1.1rem;
            }
        }
    </style>
</head>
<body>
    <!-- Анимированные звезды -->
    <div class="stars" id="stars"></div>

    <!-- Header -->
    <header>
        <div class="container">
            <nav>
                <div class="logo">Space Invaders</div>
                <ul class="nav-links">
                    <li><a href="#features">Особенности</a></li>
                    <li><a href="#modes">Режимы</a></li>
                    <li><a href="#installation">Установка</a></li>
                    <li><a href="#screenshots">Скриншоты</a></li>
                </ul>
            </nav>
        </div>
    </header>

    <!-- Hero Section -->
    <section class="hero">
        <div class="container">
            <h1>Space Invaders WPF</h1>
            <p>Классический космический шутер с современной графикой и захватывающим геймплеем. Сразитесь с ордами врагов и станьте героем галактики!</p>
            <div>
                <a href="#installation" class="cta-button">Скачать игру</a>
                <a href="https://github.com/your-username/space-invaders-wpf" class="cta-button secondary" target="_blank">GitHub репозиторий</a>
            </div>
        </div>
    </section>

    <!-- Features Section -->
    <section class="features" id="features">
        <div class="container">
            <h2 class="section-title">Особенности игры</h2>
            <div class="features-grid">
                <div class="feature-card">
                    <div class="feature-icon">🎮</div>
                    <h3>Простое управление</h3>
                    <p>Интуитивное управление с помощью клавиш A/D или стрелок для движения и Space для стрельбы</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">🚀</div>
                    <h3>Захватывающий геймплей</h3>
                    <p>4 уровня кампании + бесконечный режим с постоянно увеличивающейся сложностью</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">🎵</div>
                    <h3>Звуковое сопровождение</h3>
                    <p>Полноценная звуковая система с эффектами выстрелов, взрывов и фоновой музыкой</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">🛡️</div>
                    <h3>Система защиты</h3>
                    <p>Используйте щиты для защиты от вражеских атак в разных режимах игры</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">👾</div>
                    <h3>Уникальные враги</h3>
                    <p>Различные типы противников с уникальным поведением и боссы на каждом уровне</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">⚡</div>
                    <h3>Оптимизация</h3>
                    <p>Плавный геймплей даже на слабых компьютерах благодаря оптимизированному коду</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Game Modes -->
    <section class="game-modes" id="modes">
        <div class="container">
            <h2 class="section-title">Режимы игры</h2>
            <div class="modes-container">
                <div class="mode-card campaign">
                    <h3>Кампания</h3>
                    <p>Пройдите 4 уровня сложности и победите босса!</p>
                    <ul>
                        <li>4 уровня с увеличивающейся сложностью</li>
                        <li>Уникальный босс на 4 уровне</li>
                        <li>2 защитных щита</li>
                        <li>Система очков и жизней</li>
                        <li>Возможность переиграть уровни</li>
                    </ul>
                </div>
                <div class="mode-card infinite">
                    <h3>Бесконечный режим</h3>
                    <p>Сражайтесь до последнего и установите рекорд!</p>
                    <ul>
                        <li>Волны врагов с постоянным усложнением</li>
                        <li>5 усиленных защитных щитов</li>
                        <li>Случайные боссы каждые 3 минуты</li>
                        <li>Боссы уничтожаются за 1 попадание</li>
                        <li>+200 очков за каждого босса</li>
                        <li>Бесконечная система очков</li>
                    </ul>
                </div>
            </div>
        </div>
    </section>

    <!-- Installation -->
    <section class="installation" id="installation">
        <div class="container">
            <h2 class="section-title">Установка и запуск</h2>
            <div class="steps">
                <div class="step">
                    <span class="step-number">1</span>
                    <strong>Скачайте игру с GitHub</strong>
                    <p>Перейдите в репозиторий проекта и скачайте исходный код</p>
                    <div class="code-block">
                        git clone https://github.com/your-username/space-invaders-wpf.git
                    </div>
                </div>
                <div class="step">
                    <span class="step-number">2</span>
                    <strong>Требования к системе</strong>
                    <ul>
                        <li>Windows 7/8/10/11</li>
                        <li>.NET Framework 4.7.2 или выше</li>
                        <li>DirectX 9 совместимая видеокарта</li>
                        <li>512 MB оперативной памяти</li>
                        <li>100 MB свободного места на диске</li>
                    </ul>
                </div>
                <div class="step">
                    <span class="step-number">3</span>
                    <strong>Запуск через Visual Studio</strong>
                    <p>Откройте проект в Visual Studio 2019 или выше</p>
                    <div class="code-block">
                        1. Откройте SpaceInvaders.sln<br>
                        2. Нажмите F5 для запуска<br>
                        3. Наслаждайтесь игрой!
                    </div>
                </div>
                <div class="step">
                    <span class="step-number">4</span>
                    <strong>Сборка релизной версии</strong>
                    <p>Для создания исполняемого файла:</p>
                    <div class="code-block">
                        1. Выберите конфигурацию "Release"<br>
                        2. Build → Build Solution (Ctrl+Shift+B)<br>
                        3. Исполняемый файл будет в папке bin/Release/
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Screenshots -->
    <section class="screenshots" id="screenshots">
        <div class="container">
            <h2 class="section-title">Скриншоты игры</h2>
            <div class="screenshot-grid">
                <div class="screenshot">
                    <div class="screenshot-placeholder">Главное меню</div>
                    <p>Выбор между кампанией и бесконечным режимом</p>
                </div>
                <div class="screenshot">
                    <div class="screenshot-placeholder">Игровой процесс</div>
                    <p>Сражение с волнами врагов</p>
                </div>
                <div class="screenshot">
                    <div class="screenshot-placeholder">Босс-битва</div>
                    <p>Сражение с финальным боссом</p>
                </div>
                <div class="screenshot">
                    <div class="screenshot-placeholder">Система щитов</div>
                    <p>Защита от вражеских атак</p>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container">
            <div class="footer-content">
                <div class="footer-section">
                    <h3>Space Invaders WPF</h3>
                    <p>Классический космический шутер, переосмысленный с использованием современных технологий WPF.</p>
                </div>
                <div class="footer-section">
                    <h3>Ссылки</h3>
                    <ul class="footer-links">
                        <li><a href="https://github.com/your-username/space-invaders-wpf">GitHub репозиторий</a></li>
                        <li><a href="#installation">Инструкция по установке</a></li>
                        <li><a href="#features">Особенности</a></li>
                        <li><a href="#modes">Режимы игры</a></li>
                    </ul>
                </div>
                <div class="footer-section">
                    <h3>Технологии</h3>
                    <ul class="footer-links">
                        <li>WPF (Windows Presentation Foundation)</li>
                        <li>C# .NET Framework</li>
                        <li>XAML для интерфейса</li>
                        <li>SoundPlayer для аудио</li>
                    </ul>
                </div>
            </div>
            <div class="copyright">
                <p>&copy; 2024 Space Invaders WPF. Все права защищены.</p>
            </div>
        </div>
    </footer>

    <script>
        // Создание анимированных звезд
        function createStars() {
            const starsContainer = document.getElementById('stars');
            const starsCount = 200;
            
            for (let i = 0; i < starsCount; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                
                // Случайные позиции и размеры
                const size = Math.random() * 3;
                const left = Math.random() * 100;
                const top = Math.random() * 100;
                const animationDelay = Math.random() * 5;
                
                star.style.width = `${size}px`;
                star.style.height = `${size}px`;
                star.style.left = `${left}%`;
                star.style.top = `${top}%`;
                star.style.animationDelay = `${animationDelay}s`;
                
                starsContainer.appendChild(star);
            }
        }

        // Плавная прокрутка для навигации
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                const target = document.querySelector(this.getAttribute('href'));
                if (target) {
                    target.scrollIntoView({
                        behavior: 'smooth',
                        block: 'start'
                    });
                }
            });
        });

        // Инициализация при загрузке страницы
        document.addEventListener('DOMContentLoaded', function() {
            createStars();
        });
    </script>
</body>
</html>
