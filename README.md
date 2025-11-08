<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Space Invaders WPF - Ретро игра</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Tahoma', 'Arial', sans-serif;
            background: #008080 url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" opacity="0.1"><rect fill="%23008080" width="100" height="100"/><path d="M0 0L100 100M100 0L0 100" stroke="%2300ffff" stroke-width="1"/></svg>');
            color: #000000;
            line-height: 1.4;
            font-size: 14px;
        }

        .taskbar {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: #c0c0c0;
            border-top: 2px solid #dfdfdf;
            height: 40px;
            display: flex;
            align-items: center;
            padding: 0 10px;
            z-index: 1000;
            box-shadow: 0 -2px 4px rgba(0,0,0,0.2);
        }

        .start-button {
            background: #c0c0c0;
            border: 2px outset #dfdfdf;
            padding: 4px 20px;
            font-weight: bold;
            font-size: 14px;
            cursor: pointer;
            margin-right: 10px;
        }

        .start-button:active {
            border: 2px inset #dfdfdf;
        }

        .window {
            background: #c0c0c0;
            border: 2px outset #dfdfdf;
            margin: 20px auto;
            max-width: 1000px;
            box-shadow: 4px 4px 8px rgba(0,0,0,0.3);
        }

        .title-bar {
            background: linear-gradient(90deg, #000080, #1084d0);
            color: white;
            padding: 4px 8px;
            font-weight: bold;
            font-size: 13px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 2px outset #dfdfdf;
        }

        .window-controls {
            display: flex;
            gap: 2px;
        }

        .control-button {
            width: 16px;
            height: 14px;
            background: #c0c0c0;
            border: 1px outset #dfdfdf;
            font-size: 10px;
            text-align: center;
            line-height: 12px;
            cursor: pointer;
        }

        .control-button:active {
            border: 1px inset #dfdfdf;
        }

        .content {
            padding: 20px;
            background: #c0c0c0;
        }

        /* Hero Section */
        .hero {
            text-align: center;
            padding: 40px 20px;
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            margin: 20px;
        }

        .hero h1 {
            font-size: 32px;
            color: #000080;
            margin-bottom: 20px;
            text-shadow: 2px 2px 0px #ffffff;
            font-weight: bold;
        }

        .hero p {
            font-size: 16px;
            color: #000000;
            margin-bottom: 30px;
            max-width: 600px;
            margin-left: auto;
            margin-right: auto;
        }

        .button {
            display: inline-block;
            background: #c0c0c0;
            border: 2px outset #dfdfdf;
            padding: 8px 24px;
            margin: 5px;
            cursor: pointer;
            font-size: 14px;
            font-weight: bold;
            color: #000000;
            text-decoration: none;
        }

        .button:active {
            border: 2px inset #dfdfdf;
        }

        .button.primary {
            background: #000080;
            color: white;
            border-color: #ffffff #000000 #000000 #ffffff;
        }

        .button.friend {
            background: #800080;
            color: white;
            border-color: #ffffff #000000 #000000 #ffffff;
        }

        /* Controls Section */
        .controls-section {
            background: #c0c0c0;
            border: 4px double #008000;
            padding: 25px;
            margin: 30px 0;
            position: relative;
        }

        .controls-section::before {
            content: '🎮';
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background: #c0c0c0;
            padding: 0 15px;
            font-size: 20px;
        }

        .controls-section h3 {
            color: #008000;
            font-size: 22px;
            margin-bottom: 20px;
            text-align: center;
            text-shadow: 1px 1px 0px #ffffff;
        }

        .controls-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin: 25px 0;
        }

        .control-group {
            background: rgba(0, 128, 0, 0.1);
            border: 2px inset #dfdfdf;
            padding: 20px;
            text-align: center;
        }

        .control-group h4 {
            color: #008000;
            margin-bottom: 15px;
            font-size: 18px;
            border-bottom: 1px solid #008000;
            padding-bottom: 8px;
        }

        .key {
            display: inline-block;
            background: #000000;
            color: #00ff00;
            padding: 8px 16px;
            margin: 5px;
            border: 2px outset #dfdfdf;
            font-family: 'Courier New', monospace;
            font-weight: bold;
            font-size: 16px;
            min-width: 50px;
            text-align: center;
        }

        .key-description {
            margin-top: 10px;
            font-size: 14px;
            color: #000000;
        }

        .controls-image {
            text-align: center;
            margin: 20px 0;
        }

        .keyboard-placeholder {
            width: 100%;
            max-width: 400px;
            height: 150px;
            background: linear-gradient(45deg, #004400, #008800);
            border: 4px outset #dfdfdf;
            margin: 0 auto;
            display: flex;
            align-items: center;
            justify-content: center;
            color: #00ff00;
            font-weight: bold;
            font-family: 'Courier New', monospace;
            font-size: 18px;
        }

        .controls-tip {
            background: #ffffcc;
            border: 2px inset #dfdfdf;
            padding: 15px;
            margin: 15px 0;
            text-align: center;
            font-style: italic;
            color: #000000;
        }

        /* Features Grid */
        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .feature-card {
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            padding: 15px;
            text-align: center;
        }

        .feature-icon {
            font-size: 32px;
            margin-bottom: 10px;
        }

        .feature-card h3 {
            color: #000080;
            margin-bottom: 10px;
            font-size: 16px;
        }

        /* Game Modes */
        .modes-container {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin: 20px 0;
        }

        .mode-card {
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            padding: 20px;
        }

        .mode-card h3 {
            color: #000080;
            margin-bottom: 15px;
            font-size: 18px;
            border-bottom: 1px solid #808080;
            padding-bottom: 5px;
        }

        .mode-card ul {
            list-style: none;
            margin-left: 0;
            padding-left: 0;
        }

        .mode-card li {
            margin-bottom: 8px;
            padding-left: 20px;
            position: relative;
        }

        .mode-card li::before {
            content: '►';
            position: absolute;
            left: 0;
            color: #000080;
            font-size: 12px;
        }

        /* Friend Recommendation */
        .friend-recommendation {
            background: #c0c0c0;
            border: 4px double #800080;
            padding: 25px;
            margin: 30px 0;
            text-align: center;
            position: relative;
        }

        .friend-recommendation::before {
            content: '🌟';
            position: absolute;
            top: -15px;
            left: 50%;
            transform: translateX(-50%);
            background: #c0c0c0;
            padding: 0 15px;
            font-size: 20px;
        }

        .friend-recommendation h3 {
            color: #800080;
            font-size: 22px;
            margin-bottom: 15px;
            text-shadow: 1px 1px 0px #ffffff;
        }

        .friend-recommendation p {
            margin-bottom: 20px;
            font-size: 15px;
            color: #000000;
        }

        .friend-features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .friend-feature {
            background: rgba(128, 0, 128, 0.1);
            border: 1px solid #800080;
            padding: 12px;
            border-radius: 5px;
        }

        .friend-feature h4 {
            color: #800080;
            margin-bottom: 8px;
            font-size: 14px;
        }

        /* Installation Steps */
        .steps {
            margin: 20px 0;
        }

        .step {
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            padding: 15px;
            margin-bottom: 15px;
            display: flex;
            align-items: flex-start;
        }

        .step-number {
            background: #000080;
            color: white;
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            margin-right: 15px;
            flex-shrink: 0;
            border: 2px outset #dfdfdf;
        }

        .step-content {
            flex: 1;
        }

        .code-block {
            background: #000000;
            color: #00ff00;
            padding: 15px;
            margin: 10px 0;
            border: 2px inset #dfdfdf;
            font-family: 'Courier New', monospace;
            font-size: 12px;
            overflow-x: auto;
        }

        /* Screenshots */
        .screenshot-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin: 20px 0;
        }

        .screenshot {
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            padding: 15px;
            text-align: center;
        }

        .screenshot-placeholder {
            width: 100%;
            height: 120px;
            background: linear-gradient(45deg, #008080, #000080);
            border: 2px inset #dfdfdf;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
            margin-bottom: 10px;
        }

        /* Footer */
        footer {
            background: #c0c0c0;
            border: 2px inset #dfdfdf;
            margin: 20px;
            padding: 20px;
        }

        .footer-content {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 30px;
            margin-bottom: 20px;
        }

        .footer-section h3 {
            color: #000080;
            margin-bottom: 15px;
            font-size: 16px;
            border-bottom: 1px solid #808080;
            padding-bottom: 5px;
        }

        .footer-links {
            list-style: none;
        }

        .footer-links li {
            margin-bottom: 8px;
        }

        .footer-links a {
            color: #000080;
            text-decoration: none;
        }

        .footer-links a:hover {
            text-decoration: underline;
        }

        .copyright {
            text-align: center;
            padding-top: 15px;
            border-top: 1px solid #808080;
            color: #808080;
            font-size: 12px;
        }

        /* Desktop Icons */
        .desktop {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 40px;
            padding: 20px;
            z-index: -1;
        }

        .desktop-icon {
            width: 80px;
            text-align: center;
            margin: 20px;
            cursor: pointer;
            color: white;
            text-shadow: 1px 1px 0px #000000;
        }

        .desktop-icon img {
            width: 48px;
            height: 48px;
            margin-bottom: 5px;
        }

        /* Scrollbar Styling */
        ::-webkit-scrollbar {
            width: 16px;
        }

        ::-webkit-scrollbar-track {
            background: #c0c0c0;
            border: 1px inset #dfdfdf;
        }

        ::-webkit-scrollbar-thumb {
            background: #c0c0c0;
            border: 1px outset #dfdfdf;
        }

        ::-webkit-scrollbar-thumb:hover {
            background: #a0a0a0;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .window {
                margin: 10px;
            }
            
            .features-grid,
            .modes-container,
            .friend-features,
            .controls-grid {
                grid-template-columns: 1fr;
            }
            
            .desktop {
                display: none;
            }
        }
    </style>
</head>
<body>
    <!-- Desktop Icons -->
    <div class="desktop">
        <div class="desktop-icon" style="position: absolute; top: 20px; left: 20px;">
            <div style="width: 48px; height: 48px; background: #000080; border: 2px outset #dfdfdf; margin: 0 auto 5px;"></div>
            <span>Мой компьютер</span>
        </div>
        <div class="desktop-icon" style="position: absolute; top: 20px; left: 120px;">
            <div style="width: 48px; height: 48px; background: #ff0000; border: 2px outset #dfdfdf; margin: 0 auto 5px;"></div>
            <span>Space Invaders</span>
        </div>
        <div class="desktop-icon" style="position: absolute; top: 20px; left: 220px;">
            <div style="width: 48px; height: 48px; background: #800080; border: 2px outset #dfdfdf; margin: 0 auto 5px;"></div>
            <span>Laser Dino</span>
        </div>
    </div>

    <!-- Main Window -->
    <div class="window" style="margin-top: 80px;">
        <div class="title-bar">
            <span>Space Invaders WPF - Ретро игра</span>
            <div class="window-controls">
                <div class="control-button">_</div>
                <div class="control-button">□</div>
                <div class="control-button">×</div>
            </div>
        </div>
        <div class="content">
            <!-- Hero Section -->
            <div class="hero">
                <h1>Space Invaders WPF</h1>
                <p>Классический космический шутер в стиле ретро! Сразитесь с ордами врагов и почувствуйте ностальгию по старым добрым временам.</p>
                <div>
                    <a href="#installation" class="button primary">Скачать игру</a>
                    <a href="https://github.com/your-username/space-invaders-wpf" class="button" target="_blank">GitHub</a>
                </div>
            </div>

            <!-- Controls Section -->
            <div class="controls-section">
                <h3>🎮 Обучение управлению</h3>
                <p style="text-align: center; margin-bottom: 20px; font-size: 16px;">Освойте простые и интуитивные управления для полного контроля над космическим кораблем!</p>
                
                <div class="controls-grid">
                    <div class="control-group">
                        <h4>🚀 Движение</h4>
                        <div class="key">A</div>
                        <div class="key">←</div>
                        <div class="key-description">Движение влево</div>
                        
                        <div style="margin: 15px 0;"></div>
                        
                        <div class="key">D</div>
                        <div class="key">→</div>
                        <div class="key-description">Движение вправо</div>
                    </div>
                    
                    <div class="control-group">
                        <h4>💥 Стрельба</h4>
                        <div class="key" style="background: #ff0000; color: white;">SPACE</div>
                        <div class="key-description">Выстрел лазером</div>
                        
                        <div style="margin: 15px 0;"></div>
                        
                        <div class="key-description">Одновременно можно выпустить до 3 пуль</div>
                    </div>
                    
                    <div class="control-group">
                        <h4>⚙️ Дополнительные клавиши</h4>
                        <div class="key">ESC</div>
                        <div class="key">P</div>
                        <div class="key-description">Пауза / Меню</div>
                        
                        <div style="margin: 10px 0;"></div>
                        
                        <div class="key">M</div>
                        <div class="key-description">Вкл/Выкл звук</div>
                        
                        <div style="margin: 10px 0;"></div>
                        
                        <div class="key">+</div>
                        <div class="key">-</div>
                        <div class="key-description">Громкость звука</div>
                    </div>
                </div>

                <div class="controls-image">
                    <div class="keyboard-placeholder">
                        [ WASD ] или [ ←↑↓→ ]<br>
                        + [ SPACE ] для стрельбы
                    </div>
                </div>

                <div class="controls-tip">
                    💡 <strong>Совет:</strong> Используйте тактику "стрельба и движение" - стреляйте постоянно while двигаясь из стороны в сторону, чтобы избежать вражеских атак!
                </div>

                <div class="controls-tip">
                    🎯 <strong>Продвинутая тактика:</strong> В бесконечном режиме прицеливайтесь в верхний ряд врагов - только они могут стрелять в ответ!
                </div>
            </div>

            <!-- Features -->
            <div class="window">
                <div class="title-bar">
                    <span>Особенности игры</span>
                </div>
                <div class="content">
                    <div class="features-grid">
                        <div class="feature-card">
                            <div class="feature-icon">🎮</div>
                            <h3>Простое управление</h3>
                            <p>Клавиши A/D или стрелки + Space</p>
                        </div>
                        <div class="feature-card">
                            <div class="feature-icon">👾</div>
                            <h3>Классический геймплей</h3>
                            <p>4 уровня + бесконечный режим</p>
                        </div>
                        <div class="feature-card">
                            <div class="feature-icon">🎵</div>
                            <h3>Ретро звуки</h3>
                            <p>Звуковые эффекты в стиле 90-х</p>
                        </div>
                        <div class="feature-card">
                            <div class="feature-icon">🛡️</div>
                            <h3>Система защиты</h3>
                            <p>Щиты для блокировки атак</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Game Modes -->
            <div class="window">
                <div class="title-bar">
                    <span>Режимы игры</span>
                </div>
                <div class="content">
                    <div class="modes-container">
                        <div class="mode-card">
                            <h3>📁 Кампания</h3>
                            <ul>
                                <li>4 уровня сложности</li>
                                <li>Финальный босс на 4 уровне</li>
                                <li>2 защитных щита</li>
                                <li>Система очков и жизней</li>
                                <li>Классический аркадный режим</li>
                            </ul>
                        </div>
                        <div class="mode-card">
                            <h3>♾️ Бесконечный режим</h3>
                            <ul>
                                <li>Волны с усложнением</li>
                                <li>5 усиленных щитов</li>
                                <li>Боссы каждые 3 минуты</li>
                                <li>+200 очков за босса</li>
                                <li>Бесконечная система очков</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Friend Recommendation -->
            <div class="friend-recommendation">
                <h3>🚀 Рекомендуем также попробовать!</h3>
                <p>Наш друг создал потрясающую игру <strong>Laser Dino</strong> - динамичный платформер с динозавром и лазерами!</p>
                
                <div class="friend-features">
                    <div class="friend-feature">
                        <h4>🦖 Уникальный герой</h4>
                        <p>Динозавр с лазерной пушкой</p>
                    </div>
                    <div class="friend-feature">
                        <h4>⚡ Динамичный геймплей</h4>
                        <p>Прыжки, стрельба, уклонения</p>
                    </div>
                    <div class="friend-feature">
                        <h4>🎯 Захватывающие уровни</h4>
                        <p>Платформы, враги, боссы</p>
                    </div>
                    <div class="friend-feature">
                        <h4>🌟 Стильная графика</h4>
                        <p>Яркий ретро-стиль</p>
                    </div>
                </div>

                <div style="margin-top: 20px;">
                    <a href="https://onefreeusername.github.io/LaserDino_Landing/" class="button friend" target="_blank">
                        🌐 Посмотреть лендинг Laser Dino
                    </a>
                    <a href="https://github.com/OneFreeUsername/LaserDino" class="button" target="_blank" style="background: #008000; color: white;">
                        💻 GitHub репозиторий
                    </a>
                </div>

                <p style="margin-top: 15px; font-size: 13px; color: #666;">
                    Поддержите разработчика - поставьте звезду на GitHub! ⭐
                </p>
            </div>

            <!-- Installation -->
            <div class="window">
                <div class="title-bar">
                    <span>Установка игры</span>
                </div>
                <div class="content">
                    <div class="steps">
                        <div class="step">
                            <div class="step-number">1</div>
                            <div class="step-content">
                                <strong>Скачайте с GitHub</strong>
                                <p>Клонируйте репозиторий или скачайте ZIP</p>
                                <div class="code-block">
                                    git clone https://github.com/Pasha-kto003/Space_Invaders_WPF.git
                                </div>
                            </div>
                        </div>
                        <div class="step">
                            <div class="step-number">2</div>
                            <div class="step-content">
                                <strong>Требования к системе</strong>
                                <ul>
                                    <li>Windows XP/Vista/7/8/10/11</li>
                                    <li>.NET Framework 4.7.2+</li>
                                    <li>DirectX 9 совместимая видеокарта</li>
                                    <li>512 MB оперативной памяти</li>
                                    <li>50 MB свободного места</li>
                                </ul>
                            </div>
                        </div>
                        <div class="step">
                            <div class="step-number">3</div>
                            <div class="step-content">
                                <strong>Запуск через Visual Studio</strong>
                                <p>Откройте решение и нажмите F5</p>
                                <div class="code-block">
                                    1. Откройте SpaceInvaders.sln<br>
                                    2. Нажмите F5 для запуска<br>
                                    3. Выберите режим игры!
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Screenshots -->
            <div class="window">
                <div class="title-bar">
                    <span>Скриншоты</span>
                </div>
                <div class="content">
                    <div class="screenshot-grid">
                        <div class="screenshot">
                            <div class="screenshot-placeholder"><img width="353" height="304" alt="image" src="https://github.com/user-attachments/assets/003e17c9-2f99-47ec-8d56-c6359f75a5ce" />
                            </div>
                            <p>Выбор режима игры</p>
                        </div>
                        <div class="screenshot">
                            <div class="screenshot-placeholder"><img width="468" height="685" alt="image" src="https://github.com/user-attachments/assets/d9238da5-c993-4d2b-bed3-02a0519b54ad" />
                            </div>
                            <p>Сражение с врагами</p>
                        </div>
                        <div class="screenshot">
                            <div class="screenshot-placeholder"><img width="464" height="685" alt="image" src="https://github.com/user-attachments/assets/b8698152-7c79-48ab-ad6a-3fb9b7f6aab8" />
                            </div>
                            <p>Финальный босс</p>
                        </div>
                        <div class="screenshot">
                            <div class="screenshot-placeholder"><img width="468" height="685" alt="image" src="https://github.com/user-attachments/assets/eaf4d83e-481c-415a-b0dc-33b2c8f15c50" />
                            </div>
                            <p>Защита от атак</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="window">
        <div class="footer-content">
            <div class="footer-section">
                <h3>Space Invaders WPF</h3>
                <p>Классический космический шутер в стиле ретро для настоящих ценителей!</p>
            </div>
            <div class="footer-section">
                <h3>Ссылки</h3>
                <ul class="footer-links">
                    <li><a href="https://github.com/Pasha-kto003/Space_Invaders_WPF">GitHub репозиторий</a></li>
                    <li><a href="#installation">Установка</a></li>
                    <li><a href="#features">Особенности</a></li>
                </ul>
            </div>
            <div class="footer-section">
                <h3>Рекомендуем</h3>
                <ul class="footer-links">
                    <li><a href="https://onefreeusername.github.io/LaserDino_Landing/" target="_blank">Laser Dino - Лендинг</a></li>
                    <li><a href="https://github.com/OneFreeUsername/LaserDino" target="_blank">Laser Dino - GitHub</a></li>
                </ul>
            </div>
        </div>
        <div class="copyright">
            <p>&copy; 2024 Space Invaders WPF. Все права защищены. | Стиль Windows XP</p>
        </div>
    </footer>

    <!-- Taskbar -->
    <div class="taskbar">
        <button class="start-button">Пуск</button>
        <div style="flex: 1; background: #c0c0c0; border: 1px inset #dfdfdf; height: 24px; margin-right: 10px;"></div>
        <div style="background: #c0c0c0; border: 1px inset #dfdfdf; padding: 2px 8px; font-size: 12px;">
            15:30
        </div>
    </div>

    <script>
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

        // Имитация работы кнопок окон
        document.querySelectorAll('.control-button').forEach(button => {
            button.addEventListener('click', function() {
                if (this.textContent === '×') {
                    this.closest('.window').style.display = 'none';
                } else if (this.textContent === '_') {
                    this.closest('.window').style.minHeight = '40px';
                    this.closest('.window').querySelector('.content').style.display = 'none';
                }
            });
        });

        // Имитация двойного клика по иконкам
        document.querySelectorAll('.desktop-icon').forEach(icon => {
            icon.addEventListener('dblclick', function() {
                const appName = this.querySelector('span').textContent;
                if (appName === 'Laser Dino') {
                    window.open('https://onefreeusername.github.io/LaserDino_Landing/', '_blank');
                } else {
                    alert('Запуск ' + appName);
                }
            });
        });

        // Открытие ссылок Laser Dino в новом окне
        document.querySelectorAll('a[href*="LaserDino"]').forEach(link => {
            link.setAttribute('target', '_blank');
        });
    </script>
</body>
</html>
