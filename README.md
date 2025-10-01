<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>巨噬细胞科普 - 免疫系统的守护者</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* 全局变量和重置 */
        :root {
            --primary: #1a5f7a;
            --secondary: #159895;
            --accent: #57c5b6;
            --light: #f8f9fa;
            --dark: #2c3e50;
            --success: #28a745;
            --info: #17a2b8;
            --gradient: linear-gradient(135deg, var(--primary), var(--secondary), var(--accent));
            --shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            color: var(--dark);
            background-color: var(--light);
            overflow-x: hidden;
            position: relative;
        }

        /* 背景Canvas - 最外层，20%透明度 */
        #backgroundCanvas {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 5;
            pointer-events: none;
            opacity: 1.2;
        }

        .container {
            display: flex;
            min-height: 100vh;
            position: relative;
            z-index: 1;
        }

        /* 左侧导航栏 */
        .sidebar {
            width: 250px;
            background: var(--gradient);
            color: white;
            padding: 30px 0;
            box-shadow: var(--shadow);
            position: fixed;
            height: 100vh;
            overflow-y: auto;
            z-index: 100;
        }

        .logo {
            text-align: center;
            padding: 0 20px 30px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
            margin-bottom: 20px;
        }

            .logo h1 {
                font-size: 1.8rem;
                margin-bottom: 5px;
            }

            .logo p {
                font-size: 0.9rem;
                opacity: 0.8;
            }

        .nav-links {
            list-style: none;
        }

            .nav-links li {
                margin-bottom: 5px;
            }

            .nav-links a {
                display: flex;
                align-items: center;
                padding: 15px 30px;
                color: white;
                text-decoration: none;
                transition: var(--transition);
                border-left: 4px solid transparent;
            }

                .nav-links a:hover, .nav-links a.active {
                    background: rgba(255, 255, 255, 0.1);
                    border-left: 4px solid white;
                }

            .nav-links i {
                margin-right: 10px;
                font-size: 1.2rem;
            }

        /* 主内容区域 */
        .main-content {
            flex: 1;
            margin-left: 250px;
            padding: 0;
        }

        .content-section {
            display: none;
            padding: 40px;
            min-height: 100vh;
            background-color: rgba(248, 249, 250, 0.9);
        }

            .content-section.active {
                display: block;
            }

        h2 {
            font-size: 2.5rem;
            color: var(--primary);
            margin-bottom: 1.5rem;
            position: relative;
            padding-bottom: 15px;
        }

            h2::after {
                content: '';
                position: absolute;
                bottom: 0;
                left: 0;
                width: 80px;
                height: 4px;
                background: var(--gradient);
                border-radius: 2px;
            }

        h3 {
            font-size: 1.8rem;
            color: var(--secondary);
            margin: 2rem 0 1rem;
        }

        p {
            margin-bottom: 1.5rem;
            font-size: 1.1rem;
            line-height: 1.7;
        }

        ul {
            margin-left: 20px;
            margin-bottom: 1.5rem;
        }

        li {
            margin-bottom: 0.5rem;
        }

        /* 简介部分 */
        .intro-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 50px;
            align-items: center;
            margin-top: 30px;
        }

        .intro-text {
            padding-right: 20px;
        }

        .intro-image {
            text-align: center;
        }

        .cell-image {
            width: 100%;
            max-width: 400px;
            height: auto;
            border-radius: 10px;
            box-shadow: var(--shadow);
        }

        /* 功能部分 */
        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 30px;
            margin-top: 40px;
        }

        .feature-card {
            background: white;
            padding: 25px;
            border-radius: 10px;
            box-shadow: var(--shadow);
            transition: var(--transition);
            text-align: center;
        }

            .feature-card:hover {
                transform: translateY(-5px);
            }

        .feature-icon {
            font-size: 2.5rem;
            color: var(--primary);
            margin-bottom: 15px;
        }

        /* 科普视频部分 */
        .videos-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
            gap: 30px;
            margin-top: 40px;
        }

        .video-card {
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: var(--shadow);
        }

        .video-container {
            position: relative;
            padding-bottom: 56.25%; /* 16:9 比例 */
            height: 0;
            overflow: hidden;
        }

            .video-container video {
                position: absolute;
                top: 0;
                left: 0;
                width: 100%;
                height: 100%;
                object-fit: cover;
            }

        .video-info {
            padding: 20px;
        }

        .video-title {
            font-size: 1.3rem;
            margin-bottom: 10px;
            color: var(--primary);
        }

        /* 学术前沿部分 */
        .research-articles {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 30px;
            margin-top: 40px;
        }

        .article-card {
            background: white;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

            .article-card:hover {
                transform: translateY(-5px);
            }

        .article-header {
            background: var(--primary);
            color: white;
            padding: 15px 20px;
        }

        .article-title {
            font-size: 1.2rem;
            margin-bottom: 5px;
        }

        .article-journal {
            font-size: 0.9rem;
            opacity: 0.8;
        }

        .article-body {
            padding: 20px;
        }

        .article-authors {
            font-style: italic;
            margin-bottom: 10px;
            color: #666;
        }

        .article-metrics {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            font-size: 0.9rem;
            color: #888;
        }

        /* 游戏部分 */
        .game-container {
            position: relative;
            margin: 30px 0;
            text-align: center;
        }

        #gameCanvas {
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            display: block;
            margin: 0 auto;
        }

        .stats {
            display: flex;
            justify-content: space-between;
            width: 800px;
            margin: 0 auto 15px;
            font-size: 18px;
            font-weight: bold;
            color: var(--primary);
            background: white;
            padding: 10px 20px;
            border-radius: 10px;
            box-shadow: var(--shadow);
        }

        .instructions {
            background-color: rgba(0, 0, 0, 0.6);
            padding: 15px;
            border-radius: 10px;
            margin-top: 20px;
            max-width: 800px;
            line-height: 1.5;
            color: white;
            margin-left: auto;
            margin-right: auto;
        }

        .game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.8);
            padding: 30px;
            border-radius: 15px;
            text-align: center;
            display: none;
            color: white;
            z-index: 10;
        }

        .cell-info {
            display: flex;
            justify-content: space-around;
            margin-top: 15px;
            flex-wrap: wrap;
        }

        .cell-type {
            display: flex;
            align-items: center;
            margin: 0 15px 10px;
        }

        .color-box {
            width: 20px;
            height: 20px;
            border-radius: 50%;
            margin-right: 8px;
        }

        .btn {
            display: inline-block;
            padding: 12px 30px;
            background: var(--gradient);
            color: white;
            border: none;
            border-radius: 50px;
            font-weight: bold;
            text-decoration: none;
            cursor: pointer;
            transition: var(--transition);
            box-shadow: var(--shadow);
        }

            .btn:hover {
                transform: translateY(-3px);
                box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
            }

        /* 响应式设计 */
        @media (max-width: 1024px) {
            .container {
                flex-direction: column;
            }

            .sidebar {
                position: relative;
                width: 100%;
                height: auto;
            }

            .main-content {
                margin-left: 0;
            }

            .nav-links {
                display: flex;
                justify-content: center;
                flex-wrap: wrap;
            }

                .nav-links li {
                    margin: 0 10px;
                }

                .nav-links a {
                    border-left: none;
                    border-bottom: 4px solid transparent;
                    padding: 10px 15px;
                }

                    .nav-links a:hover, .nav-links a.active {
                        border-left: none;
                        border-bottom: 4px solid white;
                    }
        }

        @media (max-width: 768px) {
            .intro-content {
                grid-template-columns: 1fr;
            }

            .stats, #gameCanvas {
                width: 100%;
            }

            .videos-grid, .research-articles {
                grid-template-columns: 1fr;
            }

            .content-section {
                padding: 20px;
            }

            h2 {
                font-size: 2rem;
            }
        }
    </style>
</head>
<body>
    <!-- 背景Canvas - 最外层，20%透明度 -->
    <canvas id="backgroundCanvas"></canvas>

    <div class="container">
        <!-- 左侧导航栏 -->
        <div class="sidebar">
            <div class="logo">
                <h1>巨噬细胞科普</h1>
                <p>免疫系统的守护者</p>
            </div>
            <ul class="nav-links">
                <li><a href="#intro" class="nav-link active" data-section="intro"><i class="fas fa-info-circle"></i> 简介</a></li>
                <li><a href="#functions" class="nav-link" data-section="functions"><i class="fas fa-cogs"></i> 功能</a></li>
                <li><a href="#videos" class="nav-link" data-section="videos"><i class="fas fa-video"></i> 科普视频</a></li>
                <li><a href="#research" class="nav-link" data-section="research"><i class="fas fa-flask"></i> 学术前沿</a></li>
                <li><a href="#game" class="nav-link" data-section="game"><i class="fas fa-gamepad"></i> 游戏体验</a></li>
            </ul>
        </div>

        <!-- 主内容区域 -->
        <div class="main-content">
            <!-- 简介部分 -->
            <section id="intro" class="content-section active">
                <h2>巨噬细胞简介</h2>
                <div class="intro-content">
                    <div class="intro-text">
                        <p>巨噬细胞（macrophage cell）也称组织细胞（histocyte），是由血液中的单核细胞穿出血管后分化而成的。单核细胞进入结缔组织后，体积增大，内质网和线粒体增生，溶酶体增多，吞噬功能增强。</p>
                        <p>巨噬细胞的寿命因所在组织器官而异，一般可存活数月或更长。它们广泛分布于全身各处，是先天免疫系统的重要组成部分。</p>
                        <h3>分化过程中的表型改变</h3>
                        <p>单核细胞向巨噬细胞分化的过程中伴随显著的表型改变：</p>
                        <ul>
                            <li>细胞表面受体如补体iC3b和转铁蛋白的受体表达增加</li>
                            <li>细胞内酶如α-氨基己糖苷酯酶、肌酸激酶、组织谷氨酰胺转移酶和cAMP依赖的蛋白激酶表达增强</li>
                            <li>分泌过氧化氢和超氧离子的能力降低</li>
                        </ul>
                        <p>此过程还明显受IFN-γ和激素的负调节。</p>
                    </div>
                    <div class="intro-image">
                        <img src="picture/macrophage.jpg" alt="巨噬细胞示意图" class="cell-image">
                    </div>
                </div>
            </section>

            <!-- 功能部分 -->
            <section id="functions" class="content-section">
                <h2>巨噬细胞的主要功能</h2>
                <div class="features-grid">
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-utensils"></i>
                        </div>
                        <h3>吞噬作用</h3>
                        <p>巨噬细胞能够识别并吞噬病原体、凋亡细胞和细胞碎片，是机体清除异物的主要细胞。</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-bell"></i>
                        </div>
                        <h3>抗原呈递</h3>
                        <p>作为抗原呈递细胞，巨噬细胞处理抗原并将其呈递给T细胞，启动适应性免疫应答。</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-seedling"></i>
                        </div>
                        <h3>组织修复</h3>
                        <p>巨噬细胞分泌生长因子和细胞因子，促进组织修复和血管生成。</p>
                    </div>
                    <div class="feature-card">
                        <div class="feature-icon">
                            <i class="fas fa-balance-scale"></i>
                        </div>
                        <h3>免疫调节</h3>
                        <p>通过分泌各种细胞因子，巨噬细胞调节免疫反应的强度和持续时间。</p>
                    </div>
                </div>
            </section>

            <!-- 科普视频部分 -->
            <section id="videos" class="content-section">
                <h2>科普视频</h2>
                <p>以下是与巨噬细胞相关的科普视频，帮助您更直观地理解其功能和作用机制。</p>

                <div class="videos-grid">
                    <div class="video-card">
                        <div class="video-container">
                            <video controls poster="picture/video1-poster.jpg">
                                <source src="video1.mp4" type="video/mp4">
                                您的浏览器不支持视频播放。
                            </video>
                        </div>
                        <div class="video-info">
                            <h3 class="video-title">巨噬细胞吞噬过程演示</h3>
                            <p>本视频展示了巨噬细胞如何识别并吞噬病原体的过程，包括趋化、识别、吞噬和消化等关键步骤。</p>
                        </div>
                    </div>

                    <div class="video-card">
                        <div class="video-container">
                            <video controls poster="picture/video2-poster.jpg">
                                <source src="video2.mp4" type="video/mp4">
                                您的浏览器不支持视频播放。
                            </video>
                        </div>
                        <div class="video-info">
                            <h3 class="video-title">巨噬细胞在免疫系统中的作用</h3>
                            <p>本视频详细解释了巨噬细胞在先天免疫和适应性免疫中的关键作用，以及与其他免疫细胞的协同工作。</p>
                        </div>
                    </div>
                </div>
            </section>

            <!-- 学术前沿部分 -->
            <section id="research" class="content-section">
                <h2>学术前沿</h2>
                <p>以下是最新关于巨噬细胞的重要研究进展：</p>

                <div class="research-articles">
                    <div class="article-card">
                        <div class="article-header">
                            <h3 class="article-title">Single-cell compendium of muscle microenvironment in peripheral artery disease reveals altered endothelial diversity and LYVE1 macrophage activation</h3>
                            <div class="article-journal">Nature Cardiovascular Research (2025)</div>
                        </div>
                        <div class="article-body">
                            <div class="article-authors">Guillermo Turiel, Thibaut Desgeorges, Evi Masschelein, Zheng Fan, David Lussi, Christophe M. Capelle, Giulia Bernardini, Raphaela Ardicoglu, Katharina Schönberger, Manuela Birrer, Sandro F. Fucentese, Jing Zhang, Daniela Latorre, Stephan Engelberger & Katrien De Bock</div>
                            <p>本研究通过单细胞技术分析了外周动脉疾病中肌肉微环境的变化，发现了内皮细胞多样性的改变和LYVE1巨噬细胞的激活。</p>
                            <div class="article-metrics">
                                <span><i class="fas fa-eye"></i> 1956 Accesses</span>
                                <span><i class="fas fa-chart-bar"></i> 12 Altmetric</span>
                            </div>
                        </div>
                    </div>

                    <div class="article-card">
                        <div class="article-header">
                            <h3 class="article-title">Macrophage ferroptosis potentiates GCN2 deficiency induced pulmonary venous arterialization</h3>
                            <div class="article-journal">Nature Communications volume 16, Article number: 8335 (2025)</div>
                        </div>
                        <div class="article-body">
                            <div class="article-authors">Jingyuan Zhang, Pei Mao, Tengfei Zhou, Bingqing Yue, Yaning Li, Yuanhua Qiu, Kejing Ying, Fudi Wang, Jingyu Chen & Jun Yang</div>
                            <p>本研究探讨了巨噬细胞铁死亡在GCN2缺陷诱导的肺静脉动脉化中的作用，揭示了新的治疗靶点。</p>
                            <div class="article-metrics">
                                <span><i class="fas fa-eye"></i> 2017 Accesses</span>
                                <span><i class="fas fa-chart-bar"></i> 18 Altmetric</span>
                            </div>
                        </div>
                    </div>

                    <div class="article-card">
                        <div class="article-header">
                            <h3 class="article-title">Macrophage-derived amphiregulin induces myofibroblast transition in adipogenic lineage precursors near Staphylococcus aureus abscess in bone marrow</h3>
                            <div class="article-journal">Nature Communications volume 16, Article number: 8409 (2025)</div>
                        </div>
                        <div class="article-body">
                            <div class="article-authors">Bingsheng Yang, Jianwen Su, Jichang Wu, Zhongwen Wang, Jin Hu, Mankai Yang, Yihuang Lin, Mingchao Jin, Xiaochun Bai, Bin Yu & Xianrong Zhang</div>
                            <p>本研究发现在金黄色葡萄球菌脓肿附近的骨髓中，巨噬细胞来源的双调蛋白诱导脂肪谱系前体细胞向肌成纤维细胞转变。</p>
                            <div class="article-metrics">
                                <span><i class="fas fa-eye"></i> 307 Accesses</span>
                                <span><i class="fas fa-chart-bar"></i> 25 Altmetric</span>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- 游戏部分 -->
            <section id="game" class="content-section">
                <h2>游戏体验：巨噬细胞吞噬病毒</h2>
                <p>通过互动游戏体验巨噬细胞的工作方式！控制巨噬细胞吞噬病原体，但小心不要伤害正常细胞。</p>

                <div class="game-container">
                    <div class="stats">
                        <div>分数: <span id="score">0</span>/100</div>
                        <div>时间: <span id="time">0</span>s</div>
                        <div>巨噬细胞: <span id="macrophageCount">1</span></div>
                    </div>

                    <canvas id="gameCanvas" width="800" height="500"></canvas>

                    <div id="gameOver" class="game-over">
                        <h2 id="gameOverText">游戏结束</h2>
                        <p>你的分数: <span id="finalScore">0</span></p>
                        <button id="restartButton" class="btn">重新开始</button>
                    </div>
                </div>

                <div class="instructions">
                    <h3>游戏说明:</h3>
                    <p>控制巨噬细胞(蓝色)移动吞噬病毒(红色)和感染细胞(紫色)，每吞噬一个得5分。</p>
                    <p>注意：不要吞噬正常细胞(绿色)，否则游戏结束！</p>
                    <p>感染细胞每20秒会破裂变成3个病毒。达到100分获胜！</p>
                    <p>使用鼠标移动控制巨噬细胞。</p>
                </div>

                <div class="cell-info">
                    <div class="cell-type">
                        <div class="color-box" style="background-color: #3498db;"></div>
                        <span>巨噬细胞 (玩家)</span>
                    </div>
                    <div class="cell-type">
                        <div class="color-box" style="background-color: #e74c3c;"></div>
                        <span>病毒 (敌人)</span>
                    </div>
                    <div class="cell-type">
                        <div class="color-box" style="background-color: #9b59b6;"></div>
                        <span>感染细胞 (敌人)</span>
                    </div>
                    <div class="cell-type">
                        <div class="color-box" style="background-color: #2ecc71;"></div>
                        <span>正常细胞 (中立)</span>
                    </div>
                </div>
            </section>
        </div>
    </div>

    <script>
        // 背景Canvas动画 - 最外层，透明度
        document.addEventListener('DOMContentLoaded', function () {
            const canvas = document.getElementById('backgroundCanvas');
            const ctx = canvas.getContext('2d');

            // 设置Canvas大小
            function resizeCanvas() {
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;
            }

            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);

            // 小球类
            class Ball {
                constructor() {
                    this.radius = Math.random() * 10 + 5;
                    this.x = Math.random() * canvas.width;
                    this.y = Math.random() * canvas.height;
                    this.speedX = (Math.random() - 0.5) * 2;
                    this.speedY = (Math.random() - 0.5) * 2;
                    this.alpha = Math.random() * 0.3 + 0.1;
                    this.color = `rgba(${Math.floor(Math.random() * 100 + 155)},
                                                  ${Math.floor(Math.random() * 100 + 155)},
                                                  ${Math.floor(Math.random() * 100 + 155)},
                                                  ${this.alpha})`;
                }

                update() {
                    this.x += this.speedX;
                    this.y += this.speedY;

                    // 边界检测
                    if (this.x - this.radius < 0 || this.x + this.radius > canvas.width) {
                        this.speedX = -this.speedX;
                    }
                    if (this.y - this.radius < 0 || this.y + this.radius > canvas.height) {
                        this.speedY = -this.speedY;
                    }
                }

                draw() {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                    ctx.fillStyle = this.color;
                    ctx.fill();
                }
            }

            // 线段类
            class Line {
                constructor() {
                    this.x1 = Math.random() * canvas.width;
                    this.y1 = Math.random() * canvas.height;
                    this.x2 = Math.random() * canvas.width;
                    this.y2 = Math.random() * canvas.height;
                    this.speedX1 = (Math.random() - 0.5) * 0.5;
                    this.speedY1 = (Math.random() - 0.5) * 0.5;
                    this.speedX2 = (Math.random() - 0.5) * 0.5;
                    this.speedY2 = (Math.random() - 0.5) * 0.5;
                    this.alpha = Math.random() * 0.2 + 0.05;
                    this.color = `rgba(${Math.floor(Math.random() * 255)},
                                                  ${Math.floor(Math.random() * 255)},
                                                  ${Math.floor(Math.random() * 255)},
                                                  ${this.alpha})`;
                    this.width = Math.random() * 2 + 0.5;
                }

                update() {
                    this.x1 += this.speedX1;
                    this.y1 += this.speedY1;
                    this.x2 += this.speedX2;
                    this.y2 += this.speedY2;

                    // 边界检测
                    if (this.x1 < 0 || this.x1 > canvas.width) this.speedX1 = -this.speedX1;
                    if (this.y1 < 0 || this.y1 > canvas.height) this.speedY1 = -this.speedY1;
                    if (this.x2 < 0 || this.x2 > canvas.width) this.speedX2 = -this.speedX2;
                    if (this.y2 < 0 || this.y2 > canvas.height) this.speedY2 = -this.speedY2;
                }

                draw() {
                    ctx.beginPath();
                    ctx.moveTo(this.x1, this.y1);
                    ctx.lineTo(this.x2, this.y2);
                    ctx.strokeStyle = this.color;
                    ctx.lineWidth = this.width;
                    ctx.stroke();
                }
            }

            // 水波纹类
            class Ripple {
                constructor(x, y) {
                    this.x = x;
                    this.y = y;
                    this.radius = 0;
                    this.maxRadius = Math.random() * 50 + 30;
                    this.speed = Math.random() * 2 + 1;
                    this.alpha = 0.5;
                    this.color = `rgba(255, 255, 255, ${this.alpha})`;
                }

                update() {
                    this.radius += this.speed;
                    this.alpha -= 0.02;
                    this.color = `rgba(255, 255, 255, ${this.alpha})`;

                    return this.alpha > 0;
                }

                draw() {
                    ctx.beginPath();
                    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2);
                    ctx.strokeStyle = this.color;
                    ctx.lineWidth = 2;
                    ctx.stroke();
                }
            }

            // 创建小球和线段
            const balls = [];
            const lines = [];
            const ripples = [];

            for (let i = 0; i < 20; i++) {
                balls.push(new Ball());
            }

            for (let i = 0; i < 15; i++) {
                lines.push(new Line());
            }

            // 鼠标移动事件 - 创建水波纹
            document.addEventListener('mousemove', function (e) {
                ripples.push(new Ripple(e.clientX, e.clientY));
            });

            // 动画循环
            function animate() {
                // 清除画布
                ctx.clearRect(0, 0, canvas.width, canvas.height);

                // 更新和绘制线段
                for (let i = 0; i < lines.length; i++) {
                    lines[i].update();
                    lines[i].draw();
                }

                // 更新和绘制小球
                for (let i = 0; i < balls.length; i++) {
                    balls[i].update();
                    balls[i].draw();
                }

                // 更新和绘制水波纹
                for (let i = ripples.length - 1; i >= 0; i--) {
                    if (ripples[i].update()) {
                        ripples[i].draw();
                    } else {
                        ripples.splice(i, 1);
                    }
                }

                requestAnimationFrame(animate);
            }

            animate();

            // 导航切换功能
            const navLinks = document.querySelectorAll('.nav-link');
            const contentSections = document.querySelectorAll('.content-section');

            navLinks.forEach(link => {
                link.addEventListener('click', function (e) {
                    e.preventDefault();

                    // 移除所有活动状态
                    navLinks.forEach(l => l.classList.remove('active'));
                    contentSections.forEach(s => s.classList.remove('active'));

                    // 添加当前活动状态
                    this.classList.add('active');
                    const targetSection = this.getAttribute('data-section');
                    document.getElementById(targetSection).classList.add('active');

                    // 如果切换到游戏部分，重新初始化游戏
                    if (targetSection === 'game') {
                        initGame();
                    }
                });
            });

            // 初始化游戏
            initGame();
        });

        // 游戏代码
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // 游戏状态
        let score = 0;
        let gameTime = 0;
        let gameOver = false;
        let gameWin = false;
        let infectionTime = 25; // 延长感染时间到20秒

        // 游戏元素数组
        let macrophages = [];
        let viruses = [];
        let infectedCells = [];
        let normalCells = [];

        // 玩家控制的巨噬细胞
        let player = {
            x: canvas.width / 2,
            y: canvas.height / 2,
            radius: 15, // 缩小主体半径
            color: '#3498db',
            speed: 5,
            pseudopods: [], // 伪足数组
            shapePoints: [], // 不规则形状点
            shapeVariation: 0.4, // 形状变化程度
            lastShapeUpdate: 0, // 上次形状更新时间
            shapeUpdateInterval: 0.6 // 形状更新间隔（秒）
        };

        // 初始化游戏
        function initGame() {
            score = 0;
            gameTime = 0;
            gameOver = false;
            gameWin = false;

            // 清空所有数组
            macrophages = [player];
            viruses = [];
            infectedCells = [];
            normalCells = [];

            // 初始化玩家巨噬细胞的不规则形状和伪足
            generateMacrophageShape(player);

            // 创建初始病毒
            for (let i = 0; i < 8; i++) {
                createVirus();
            }

            // 创建初始感染细胞
            for (let i = 0; i < 4; i++) {
                createInfectedCell();
            }

            // 创建初始正常细胞
            for (let i = 0; i < 10; i++) {
                createNormalCell();
            }

            // 更新显示
            updateDisplay();

            // 隐藏游戏结束画面
            document.getElementById('gameOver').style.display = 'none';

            // 开始游戏循环
            requestAnimationFrame(gameLoop);
        }

        // 生成巨噬细胞的不规则形状和伪足
        function generateMacrophageShape(macrophage) {
            // 生成不规则形状的点
            macrophage.shapePoints = [];
            const numPoints = 16 + Math.floor(Math.random() * 8); // 16-23个点
            const baseRadius = macrophage.radius;

            for (let i = 0; i < numPoints; i++) {
                const angle = (i / numPoints) * Math.PI * 2;
                const variation = macrophage.shapeVariation * Math.random(); // 形状变化
                const radius = baseRadius * (1 + variation);
                macrophage.shapePoints.push({
                    x: Math.cos(angle) * radius,
                    y: Math.sin(angle) * radius,
                    baseAngle: angle,
                    baseRadius: baseRadius,
                    variation: variation,
                    phase: Math.random() * Math.PI * 2 // 动画相位
                });
            }

            // 生成伪足
            macrophage.pseudopods = [];
            const numPseudopods = 8 + Math.floor(Math.random() * 3); // 3-5个伪足

            for (let i = 0; i < numPseudopods; i++) {
                const angle = (i / numPseudopods) * Math.PI * 2 + (Math.random() - 0.5) * 0.5;
                // 伪足长度随机，最长为细胞直径相同，最短为0
                const maxLength = macrophage.radius * 0.2;
                const minLength = 0;
                const baseLength = minLength + Math.random() * (maxLength - minLength);

                // 创建主要伪足
                const mainPseudopod = createPseudopod(angle, baseLength, 10, 0.05, 0.5);
                macrophage.pseudopods.push(mainPseudopod);
            }
        }

        // 创建伪足（分段式，实现自然弯曲和蠕动）
        function createPseudopod(baseAngle, baseLength, baseWidth, waveSpeed, waveAmplitude) {
            // 增加节段数量（3倍）
            const numSegments = 20 + Math.floor(Math.random() * 10); // 20-30个节段
            const segments = [];

            for (let i = 0; i < numSegments; i++) {
                // 伪足长度逐渐减小
                const segmentLength = baseLength * (1 - i / numSegments);

                // 伪足宽度：根端扩大一倍，末端不变
                const segmentWidth = baseWidth * (1 - i / numSegments * 0.5);

                segments.push({
                    length: segmentLength,
                    width: segmentWidth,
                    angle: 0, // 相对于前一个节段的角度
                    phase: Math.random() * Math.PI * 2, // 动画相位
                    waveSpeed: waveSpeed * (0.8 + Math.random() * 0.1), // 波动速度
                    waveAmplitude: waveAmplitude * (0.8 + Math.random() * 0.4), // 波动幅度
                    contractionPhase: Math.random() * Math.PI * 2, // 收缩相位
                    contractionSpeed: 0.1 + Math.random() * 0.1, // 收缩速度
                    contractionAmplitude: 0.2 + Math.random() * 0.2, // 收缩幅度
                    branches: [] // 分支
                });
            }

            // 随机添加小伪足分支
            const numBranches = Math.floor(Math.random() * 3); // 0-2个分支
            for (let i = 0; i < numBranches; i++) {
                const branchSegmentIndex = Math.floor(Math.random() * (numSegments - 2)) + 1; // 不在第一个或最后一个节段
                const branchAngle = (Math.random() - 0.5) * Math.PI / 2; // 分支角度
                const branchLength = baseLength * 0.4 * (0.7 + Math.random() * 0.6);
                const branchWidth = baseWidth * 0.5 * (0.7 + Math.random() * 0.6);

                segments[branchSegmentIndex].branches.push(
                    createPseudopod(branchAngle, branchLength, branchWidth, waveSpeed * 1.5, waveAmplitude * 0.8)
                );
            }

            return {
                baseAngle: baseAngle,
                segments: segments,
                phase: Math.random() * Math.PI * 2, // 整体相位
                waveSpeed: waveSpeed * 0.2, // 整体波动速度
                waveAmplitude: waveAmplitude * 0.3, // 整体波动幅度
                baseLength: baseLength,
                baseWidth: baseWidth
            };
        }

        // 更新巨噬细胞形状（动态变化）
        function updateMacrophageShape(macrophage) {
            // 定期更新形状
            if (gameTime - macrophage.lastShapeUpdate > macrophage.shapeUpdateInterval) {
                macrophage.lastShapeUpdate = gameTime;

                // 轻微调整形状点
                for (const point of macrophage.shapePoints) {
                    point.variation = macrophage.shapeVariation * (0.7 + Math.random() * 0.6);
                }
            }

            // 更新形状点动画
            for (const point of macrophage.shapePoints) {
                point.phase += 0.05;
                const wave = Math.sin(point.phase) * 0.1;
                point.x = Math.cos(point.baseAngle) * point.baseRadius * (1 + point.variation + wave);
                point.y = Math.sin(point.baseAngle) * point.baseRadius * (1 + point.variation + wave);
            }

            // 更新伪足动画 - 分段式伪足
            for (const pseudopod of macrophage.pseudopods) {
                updatePseudopod(pseudopod);
            }
        }

        // 更新伪足（分段式，实现蠕动效果）
        function updatePseudopod(pseudopod) {
            // 更新整体相位
            pseudopod.phase += pseudopod.waveSpeed;

            // 更新每个节段
            let cumulativeAngle = pseudopod.baseAngle + Math.sin(pseudopod.phase) * pseudopod.waveAmplitude;

            for (let i = 0; i < pseudopod.segments.length; i++) {
                const segment = pseudopod.segments[i];

                // 更新节段相位
                segment.phase += segment.waveSpeed;
                segment.contractionPhase += segment.contractionSpeed;

                // 计算节段角度 - 基于前一个节段的角度加上波动
                // 使用蠕动效果：类似正弦波的传播
                const waveOffset = Math.sin(segment.phase + i * 0.3) * segment.waveAmplitude;

                // 添加收缩效果 - 使伪足产生类似蠕动的运动
                const contraction = Math.sin(segment.contractionPhase) * segment.contractionAmplitude;

                segment.angle = cumulativeAngle + waveOffset + contraction;

                // 为下一个节段累积角度
                cumulativeAngle = segment.angle;

                // 更新分支
                for (const branch of segment.branches) {
                    updatePseudopod(branch);
                }
            }
        }

        // 创建病毒（冠状病毒样式）
        function createVirus() {
            const virus = {
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                radius: 6, // 缩小病毒大小
                color: '#e74c3c',
                speedX: (Math.random() - 0.5) * 2,
                speedY: (Math.random() - 0.5) * 2,
                spikes: [], // 刺突数组
                rotation: Math.random() * Math.PI * 2 // 随机旋转
            };

            // 生成冠状病毒的刺突
            const numSpikes = 12 + Math.floor(Math.random() * 6); // 12-17个刺突

            for (let i = 0; i < numSpikes; i++) {
                const angle = (i / numSpikes) * Math.PI * 2;
                const length = 6 + Math.random() * 4; // 6-10像素长
                const width = 1.5 + Math.random() * 1.5; // 1.5-3像素宽

                virus.spikes.push({
                    angle: angle,
                    length: length,
                    width: width
                });
            }

            viruses.push(virus);
        }

        // 创建感染细胞
        function createInfectedCell() {
            infectedCells.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                radius: 18,
                color: '#9b59b6',
                creationTime: gameTime,
                nucleusRadius: 6, // 细胞核半径
                nucleusColor: '#8e44ad' // 细胞核颜色
            });
        }

        // 创建正常细胞
        function createNormalCell() {
            normalCells.push({
                x: Math.random() * canvas.width,
                y: Math.random() * canvas.height,
                radius: 15,
                color: '#2ecc71',
                speedX: (Math.random() - 0.5) * 1.5,
                speedY: (Math.random() - 0.5) * 1.5,
                nucleusRadius: 5, // 细胞核半径
                nucleusColor: '#27ae60', // 细胞核颜色
                membraneWidth: 2 // 细胞膜宽度
            });
        }

        // 游戏主循环
        function gameLoop(timestamp) {
            if (gameOver || gameWin) {
                showGameOver();
                return;
            }

            // 清除画布
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // 更新游戏时间
            gameTime += 0.0167; // 约60FPS

            // 更新玩家位置（跟随鼠标）
            updatePlayerPosition();

            // 更新巨噬细胞形状
            updateMacrophageShape(player);

            // 更新病毒位置
            updateViruses();

            // 更新正常细胞位置
            updateNormalCells();

            // 检查感染细胞是否破裂
            checkInfectedCells();

            // 检查碰撞
            checkCollisions();

            // 绘制所有元素
            drawGameElements();

            // 更新显示
            updateDisplay();

            // 检查游戏胜利条件
            if (score >= 100) {
                gameWin = true;
                showGameOver();
                return;
            }

            // 继续游戏循环
            requestAnimationFrame(gameLoop);
        }

        // 更新玩家位置（跟随鼠标）
        function updatePlayerPosition() {
            canvas.addEventListener('mousemove', (e) => {
                const rect = canvas.getBoundingClientRect();
                player.x = e.clientX - rect.left;
                player.y = e.clientY - rect.top;

                // 确保玩家不会移出画布
                player.x = Math.max(player.radius, Math.min(canvas.width - player.radius, player.x));
                player.y = Math.max(player.radius, Math.min(canvas.height - player.radius, player.y));
            });
        }

        // 更新病毒位置
        function updateViruses() {
            for (let i = 0; i < viruses.length; i++) {
                const virus = viruses[i];

                // 移动病毒
                virus.x += virus.speedX;
                virus.y += virus.speedY;

                // 边界检测，碰到边界反弹
                if (virus.x - virus.radius < 0 || virus.x + virus.radius > canvas.width) {
                    virus.speedX = -virus.speedX;
                }
                if (virus.y - virus.radius < 0 || virus.y + virus.radius > canvas.height) {
                    virus.speedY = -virus.speedY;
                }

                // 限制在画布内
                virus.x = Math.max(virus.radius, Math.min(canvas.width - virus.radius, virus.x));
                virus.y = Math.max(virus.radius, Math.min(canvas.height - virus.radius, virus.y));

                // 轻微旋转病毒
                virus.rotation += 0.02;
            }
        }

        // 更新正常细胞位置
        function updateNormalCells() {
            for (let i = 0; i < normalCells.length; i++) {
                const cell = normalCells[i];

                // 移动正常细胞
                cell.x += cell.speedX;
                cell.y += cell.speedY;

                // 边界检测，碰到边界反弹
                if (cell.x - cell.radius < 0 || cell.x + cell.radius > canvas.width) {
                    cell.speedX = -cell.speedX;
                }
                if (cell.y - cell.radius < 0 || cell.y + cell.radius > canvas.height) {
                    cell.speedY = -cell.speedY;
                }

                // 限制在画布内
                cell.x = Math.max(cell.radius, Math.min(canvas.width - cell.radius, cell.x));
                cell.y = Math.max(cell.radius, Math.min(canvas.height - cell.radius, cell.y));
            }
        }

        // 检查感染细胞是否破裂
        function checkInfectedCells() {
            for (let i = infectedCells.length - 1; i >= 0; i--) {
                const cell = infectedCells[i];

                // 如果感染细胞存在超过20秒，破裂并产生3个病毒
                if (gameTime - cell.creationTime > infectionTime) {
                    // 移除感染细胞
                    infectedCells.splice(i, 1);

                    // 创建3个新病毒
                    for (let j = 0; j < 3; j++) {
                        createVirus();
                    }

                    // 创建一个新的感染细胞
                    createInfectedCell();
                }
            }
        }

        // 检查碰撞
        function checkCollisions() {
            // 检查玩家与病毒的碰撞
            for (let i = viruses.length - 1; i >= 0; i--) {
                const virus = viruses[i];
                const dx = player.x - virus.x;
                const dy = player.y - virus.y;
                const distance = Math.sqrt(dx * dx + dy * dy);

                if (distance < player.radius + virus.radius) {
                    // 吞噬病毒，得分
                    viruses.splice(i, 1);
                    score += 5;
                    createVirus(); // 创建新的病毒保持游戏难度
                }
            }

            // 检查玩家与感染细胞的碰撞
            for (let i = infectedCells.length - 1; i >= 0; i--) {
                const cell = infectedCells[i];
                const dx = player.x - cell.x;
                const dy = player.y - cell.y;
                const distance = Math.sqrt(dx * dx + dy * dy);

                if (distance < player.radius + cell.radius) {
                    // 吞噬感染细胞，得分
                    infectedCells.splice(i, 1);
                    score += 5;
                    createInfectedCell(); // 创建新的感染细胞
                }
            }

            // 检查玩家与正常细胞的碰撞
            for (let i = normalCells.length - 1; i >= 0; i--) {
                const cell = normalCells[i];
                const dx = player.x - cell.x;
                const dy = player.y - cell.y;
                const distance = Math.sqrt(dx * dx + dy * dy);

                if (distance < player.radius + cell.radius) {
                    // 吞噬正常细胞，游戏结束
                    gameOver = true;
                    return;
                }
            }

            // 检查病毒与正常细胞的碰撞（感染）
            for (let i = normalCells.length - 1; i >= 0; i--) {
                const cell = normalCells[i];

                for (let j = viruses.length - 1; j >= 0; j--) {
                    const virus = viruses[j];
                    const dx = cell.x - virus.x;
                    const dy = cell.y - virus.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);

                    if (distance < cell.radius + virus.radius) {
                        // 病毒感染正常细胞，将其变为感染细胞
                        normalCells.splice(i, 1);
                        createInfectedCell();
                        break;
                    }
                }
            }
        }

        // 绘制所有游戏元素
        function drawGameElements() {
            // 绘制病毒
            for (const virus of viruses) {
                drawVirus(virus);
            }

            // 绘制感染细胞
            for (const cell of infectedCells) {
                drawInfectedCell(cell);

                // 绘制感染细胞的破裂进度条
                const timeLeft = infectionTime - (gameTime - cell.creationTime);
                const progress = timeLeft / infectionTime;

                ctx.beginPath();
                ctx.rect(cell.x - cell.radius, cell.y - cell.radius - 10, cell.radius * 2 * progress, 5);
                ctx.fillStyle = progress > 0.5 ? '#2ecc71' : progress > 0.2 ? '#f39c12' : '#e74c3c';
                ctx.fill();
            }

            // 绘制正常细胞
            for (const cell of normalCells) {
                drawNormalCell(cell);
            }

            // 绘制巨噬细胞（玩家）
            drawMacrophage(player);

            // 绘制分数和时间
            ctx.font = "16px Arial";
            ctx.fillStyle = "white";
            ctx.fillText(`分数: ${score}`, 10, 20);
            ctx.fillText(`时间: ${Math.floor(gameTime)}s`, 10, 40);
        }

        // 绘制巨噬细胞（不规则形状+伪足）
        function drawMacrophage(macrophage) {
            ctx.save();
            ctx.translate(macrophage.x, macrophage.y);

            // 绘制不规则主体
            ctx.beginPath();
            ctx.moveTo(macrophage.shapePoints[0].x, macrophage.shapePoints[0].y);

            for (let i = 1; i < macrophage.shapePoints.length; i++) {
                ctx.lineTo(macrophage.shapePoints[i].x, macrophage.shapePoints[i].y);
            }

            ctx.closePath();
            ctx.fillStyle = macrophage.color;
            ctx.fill();
            ctx.strokeStyle = '#2980b9';
            ctx.lineWidth = 2;
            ctx.stroke();

            // 绘制伪足（分段式）
            for (const pseudopod of macrophage.pseudopods) {
                drawPseudopod(pseudopod, 0, 0);
            }

            // 绘制细胞核 - 缩小细胞核
            ctx.beginPath();
            ctx.arc(0, 0, 5, 0, Math.PI * 2);
            ctx.fillStyle = '#2980b9';
            ctx.fill();

            // 绘制细胞核细节
            ctx.beginPath();
            ctx.arc(0, 0, 2.5, 0, Math.PI * 2);
            ctx.fillStyle = '#1a5276';
            ctx.fill();

            ctx.restore();
        }

        // 绘制伪足（分段式）
        function drawPseudopod(pseudopod, startX, startY) {
            let currentX = startX;
            let currentY = startY;
            let currentAngle = pseudopod.baseAngle;

            // 绘制伪足主体
            for (let i = 0; i < pseudopod.segments.length; i++) {
                const segment = pseudopod.segments[i];

                // 计算节段终点
                const endX = currentX + Math.cos(currentAngle) * segment.length;
                const endY = currentY + Math.sin(currentAngle) * segment.length;

                // 绘制节段
                ctx.beginPath();
                ctx.moveTo(currentX, currentY);
                ctx.lineTo(endX, endY);
                ctx.strokeStyle = '#2980b9';
                ctx.lineWidth = segment.width;
                ctx.lineCap = 'round';
                ctx.stroke();

                // 绘制节段末端
                ctx.beginPath();
                ctx.arc(endX, endY, segment.width / 2, 0, Math.PI * 2);
                ctx.fillStyle = '#2980b9';
                ctx.fill();

                // 更新当前位置和角度
                currentX = endX;
                currentY = endY;
                currentAngle = segment.angle;

                // 绘制分支
                for (const branch of segment.branches) {
                    drawPseudopod(branch, currentX, currentY);
                }
            }
        }

        // 绘制病毒（冠状病毒样式）
        function drawVirus(virus) {
            ctx.save();
            ctx.translate(virus.x, virus.y);
            ctx.rotate(virus.rotation);

            // 绘制病毒主体
            ctx.beginPath();
            ctx.arc(0, 0, virus.radius, 0, Math.PI * 2);
            ctx.fillStyle = virus.color;
            ctx.fill();

            // 绘制病毒刺突
            for (const spike of virus.spikes) {
                const endX = Math.cos(spike.angle) * (virus.radius + spike.length);
                const endY = Math.sin(spike.angle) * (virus.radius + spike.length);

                ctx.beginPath();
                ctx.moveTo(
                    Math.cos(spike.angle) * virus.radius,
                    Math.sin(spike.angle) * virus.radius
                );
                ctx.lineTo(endX, endY);
                ctx.strokeStyle = virus.color;
                ctx.lineWidth = spike.width;
                ctx.lineCap = 'round';
                ctx.stroke();

                // 绘制刺突末端小球
                ctx.beginPath();
                ctx.arc(endX, endY, spike.width, 0, Math.PI * 2);
                ctx.fillStyle = virus.color;
                ctx.fill();
            }

            ctx.restore();
        }

        // 绘制感染细胞
        function drawInfectedCell(cell) {
            ctx.save();
            ctx.translate(cell.x, cell.y);

            // 绘制细胞主体
            ctx.beginPath();
            ctx.arc(0, 0, cell.radius, 0, Math.PI * 2);
            ctx.fillStyle = cell.color;
            ctx.fill();

            // 绘制细胞膜
            ctx.strokeStyle = '#8e44ad';
            ctx.lineWidth = 2;
            ctx.stroke();

            // 绘制细胞核
            ctx.beginPath();
            ctx.arc(0, 0, cell.nucleusRadius, 0, Math.PI * 2);
            ctx.fillStyle = cell.nucleusColor;
            ctx.fill();

            // 绘制感染特征（斑点）
            for (let i = 0; i < 5; i++) {
                const angle = Math.random() * Math.PI * 2;
                const distance = Math.random() * (cell.radius - cell.nucleusRadius - 2) + cell.nucleusRadius + 2;
                const spotX = Math.cos(angle) * distance;
                const spotY = Math.sin(angle) * distance;
                const spotRadius = 1 + Math.random() * 2;

                ctx.beginPath();
                ctx.arc(spotX, spotY, spotRadius, 0, Math.PI * 2);
                ctx.fillStyle = '#e74c3c';
                ctx.fill();
            }

            ctx.restore();
        }

        // 绘制正常细胞
        function drawNormalCell(cell) {
            ctx.save();
            ctx.translate(cell.x, cell.y);

            // 绘制细胞主体
            ctx.beginPath();
            ctx.arc(0, 0, cell.radius, 0, Math.PI * 2);
            ctx.fillStyle = cell.color;
            ctx.fill();

            // 绘制细胞膜
            ctx.strokeStyle = '#27ae60';
            ctx.lineWidth = cell.membraneWidth;
            ctx.stroke();

            // 绘制细胞核
            ctx.beginPath();
            ctx.arc(0, 0, cell.nucleusRadius, 0, Math.PI * 2);
            ctx.fillStyle = cell.nucleusColor;
            ctx.fill();

            ctx.restore();
        }

        // 更新显示
        function updateDisplay() {
            document.getElementById('score').textContent = score;
            document.getElementById('time').textContent = Math.floor(gameTime);
            document.getElementById('macrophageCount').textContent = macrophages.length;
        }

        // 显示游戏结束画面
        function showGameOver() {
            const gameOverDiv = document.getElementById('gameOver');
            const gameOverText = document.getElementById('gameOverText');
            const finalScore = document.getElementById('finalScore');

            gameOverDiv.style.display = 'block';
            finalScore.textContent = score;

            if (gameWin) {
                gameOverText.textContent = '恭喜你获胜了！';
                gameOverText.style.color = '#2ecc71';
            } else {
                gameOverText.textContent = '游戏结束';
                gameOverText.style.color = '#e74c3c';
            }
        }

        // 重新开始游戏
        document.getElementById('restartButton').addEventListener('click', initGame);
    </script>
</body>
</html>
