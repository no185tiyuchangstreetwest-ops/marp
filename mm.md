<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale-1.0">
    <title>数字媒体导论 - 进制互动学习应用</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutral Tech -->
    <!-- Application Structure Plan: 本应用采用三段式信息架构（“为何学”、“如何学”、“学何用”），而非沿用PPT的线性结构。第一部分“为何学”通过网页颜色这一具体案例激发学习兴趣；第二部分“如何学”是核心，提供了一系列互动工具，让学生通过实践操作掌握进制转换的重难点；第三部分“学何用”则将知识置于课程的宏观背景下，总结其价值与考核方式。这种以“任务和探索”为导向的结构，比被动的幻灯片浏览更能促进自主学习和深度理解。 -->
    <!-- Visualization & Content Choices: 1. 课程评价(目标:信息传达) -> 饼图(Chart.js) -> 交互:悬停提示 -> 理由: 直观展示各部分占比，比文字列表更清晰。 2. 进制转换(目标:技能训练) -> 实时转换器(HTML/JS) -> 交互:实时输入/输出 -> 理由: 即时反馈强化学习效果，是核心练习工具。 3. 十-二进制转换(目标:理解难点) -> 步骤可视化(HTML/CSS/JS) -> 交互:输入数字，动态生成步骤 -> 理由: 将“除基取余”的抽象算法过程具象化，降低理解门槛。 4. 二-十六进制转换(目标:理解难点) -> 动态分组可视化(HTML/CSS/JS) -> 交互:输入二进制串，自动高亮分组 -> 理由: “四位合一”的规则通过视觉分组一目了然。 5. 颜色与进制(目标:建立联系) -> 互动式颜色选择器(HTML/JS) -> 交互:选择颜色，实时显示Hex/RGB值 -> 理由: 建立抽象数字与具体视觉应用间的直接联系，体现“所学即所用”。 -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body { font-family: 'Noto Sans SC', sans-serif; background-color: #FDFBF8; }
        .bg-warm-neutral { background-color: #F5F2ED; }
        .bg-accent { background-color: #3B82F6; }
        .text-accent { color: #3B82F6; }
        .border-accent { border-color: #3B82F6; }
        .shadow-custom { box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -2px rgba(0, 0, 0, 0.05); }
        .nav-link { transition: color 0.3s, border-color 0.3s; }
        .nav-link.active { color: #3B82F6; border-bottom-width: 2px; }
        .nav-link:not(.active) { border-bottom-width: 2px; border-color: transparent; }
        .interactive-card { transition: transform 0.3s, box-shadow 0.3s; }
        .interactive-card:hover { transform: translateY(-4px); box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.07), 0 4px 6px -4px rgba(0, 0, 0, 0.07); }
        .chart-container { position: relative; width: 100%; max-width: 320px; margin-left: auto; margin-right: auto; height: 320px; }
        .step-box { min-width: 150px; }
    </style>
</head>
<body class="text-gray-800">

    <header class="bg-white/80 backdrop-blur-lg sticky top-0 z-50 shadow-sm">
        <nav class="container mx-auto px-6 py-4 flex justify-between items-center">
            <div class="text-2xl font-bold text-gray-800">
                <span class="text-accent">D</span>M <span class="font-light">进制学习</span>
            </div>
            <div class="hidden md:flex space-x-8">
                <a href="#why" class="nav-link pb-1 text-gray-600 hover:text-accent">为何学</a>
                <a href="#how" class="nav-link pb-1 text-gray-600 hover:text-accent">如何学</a>
                <a href="#context" class="nav-link pb-1 text-gray-600 hover:text-accent">学何用</a>
            </div>
        </nav>
    </header>

    <main class="container mx-auto px-6 py-12">

        <section id="why" class="text-center mb-24 scroll-mt-20">
            <h1 class="text-4xl md:text-5xl font-bold mb-4">数字世界的“语言”</h1>
            <p class="text-lg text-gray-600 max-w-3xl mx-auto mb-12">欢迎来到《数字媒体导论》第二单元。本节课我们将探索计算机的母语——进制。理解它，是理解所有数字媒体（图像、声音、视频）如何存储和表达的基石。</p>
            
            <div class="bg-white rounded-xl shadow-custom p-8 max-w-4xl mx-auto interactive-card">
                <h3 class="text-2xl font-semibold mb-4">从一个网页颜色代码说起</h3>
                <p class="text-gray-600 mb-6">你在网页设计中看到的颜色代码，如 <code class="bg-yellow-100 text-yellow-700 font-mono p-1 rounded">#FF5733</code>，就是一种进制——十六进制。它精确地告诉计算机如何混合红(R)、绿(G)、蓝(B)三种光来创造出屏幕上绚丽的色彩。这个代码其实是三个数字的组合：FF、57 和 33。</p>
                <div class="flex flex-col md:flex-row justify-center items-center gap-6">
                    <div id="color-preview" class="w-32 h-32 rounded-full border-8 border-gray-100" style="background-color: #FF5733;"></div>
                    <div class="text-left space-y-2">
                        <p class="flex items-center"><span class="w-6 h-6 rounded-full bg-red-500 mr-3"></span> 红色(R): <code class="font-mono ml-2">FF</code> (十六进制) → <span class="text-accent font-semibold ml-2">255</span> (十进制)</p>
                        <p class="flex items-center"><span class="w-6 h-6 rounded-full bg-green-500 mr-3"></span> 绿色(G): <code class="font-mono ml-2">57</code> (十六进制) → <span class="text-accent font-semibold ml-2">87</span> (十进制)</p>
                        <p class="flex items-center"><span class="w-6 h-6 rounded-full bg-blue-500 mr-3"></span> 蓝色(B): <code class="font-mono ml-2">33</code> (十六进制) → <span class="text-accent font-semibold ml-2">51</span> (十进制)</p>
                    </div>
                </div>
                <p class="mt-6 text-gray-500 text-sm">这一切的底层，都是计算机唯一能懂的0和1（二进制）。这个单元，我们就来揭开这层神秘面纱。</p>
            </div>
        </section>

        <section id="how" class="mb-24 scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold mb-3">核心技能：进制转换互动工具</h2>
                <p class="text-lg text-gray-600 max-w-3xl mx-auto">理论知识需要动手实践才能真正掌握。使用下面的工具，亲自尝试不同进制间的转换，观察其规律。</p>
            </div>
            
            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card">
                    <h3 class="text-2xl font-semibold mb-6 text-center">通用进制转换器</h3>
                    <div class="space-y-4">
                        <div>
                            <label for="decInput" class="block text-sm font-medium text-gray-700 mb-1">十进制 (Decimal)</label>
                            <input type="number" id="decInput" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-accent focus:border-accent transition" placeholder="例如: 29">
                        </div>
                        <div>
                            <label for="binInput" class="block text-sm font-medium text-gray-700 mb-1">二进制 (Binary)</label>
                            <input type="text" id="binInput" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-accent focus:border-accent transition font-mono" placeholder="例如: 11101">
                        </div>
                        <div>
                            <label for="hexInput" class="block text-sm font-medium text-gray-700 mb-1">十六进制 (Hexadecimal)</label>
                            <input type="text" id="hexInput" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-accent focus:border-accent transition font-mono" placeholder="例如: 1D">
                        </div>
                    </div>
                </div>
                
                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card">
                    <h3 class="text-2xl font-semibold mb-6 text-center">颜色-进制转换器</h3>
                     <div class="flex flex-col items-center gap-6">
                        <input type="color" id="colorPicker" value="#FF5733" class="w-32 h-32 rounded-full cursor-pointer border-4 border-white shadow-inner" style="padding: 0; border-radius: 50%; -webkit-appearance: none; appearance: none; background-color: transparent;">
                        <div id="colorValues" class="text-center font-mono bg-warm-neutral p-4 rounded-lg w-full">
                            <p>HEX: <span id="hexValue" class="font-bold">#FF5733</span></p>
                            <p>RGB: <span id="rgbValue" class="font-bold">rgb(255, 87, 51)</span></p>
                        </div>
                    </div>
                </div>
            </div>

            <div class="mt-8 grid grid-cols-1 md:grid-cols-2 gap-8">
                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card">
                    <h3 class="text-2xl font-semibold mb-2 text-center">难点攻克：十进制 → 二进制</h3>
                    <p class="text-center text-gray-500 mb-6">“除基取余法”可视化</p>
                    <div class="flex flex-col items-center">
                        <input type="number" id="decToBinVisualInput" class="w-full max-w-xs p-3 border border-gray-300 rounded-lg mb-4 text-center" placeholder="输入一个十进制数 (如: 29)">
                        <div id="decToBinVisualOutput" class="font-mono text-center p-4 bg-warm-neutral rounded-lg w-full min-h-[100px]">输入数字查看步骤</div>
                    </div>
                </div>

                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card">
                    <h3 class="text-2xl font-semibold mb-2 text-center">难点攻克：二进制 ↔ 十六进制</h3>
                    <p class="text-center text-gray-500 mb-6">“四位分组法”可视化</p>
                    <div class="flex flex-col items-center">
                         <input type="text" id="binToHexVisualInput" class="w-full max-w-xs p-3 border border-gray-300 rounded-lg mb-4 text-center font-mono" placeholder="输入一串二进制 (如: 11110101)">
                        <div id="binToHexVisualOutput" class="p-4 bg-warm-neutral rounded-lg w-full min-h-[100px] text-center">
                            <div class="font-mono text-xl tracking-widest" id="binGroups">...</div>
                            <div class="font-bold text-2xl text-accent mt-2" id="hexResult">...</div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="context" class="scroll-mt-20">
            <div class="text-center mb-12">
                <h2 class="text-3xl md:text-4xl font-bold mb-3">本节课的价值与目标</h2>
                <p class="text-lg text-gray-600 max-w-3xl mx-auto">掌握进制不仅是为了完成计算，更是为了构建对整个数字媒体领域的深层理解，为未来的专业学习铺平道路。</p>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card">
                    <h4 class="font-bold text-xl mb-3 text-accent">课程定位</h4>
                    <p class="text-gray-600"><strong>前承：</strong>计算机硬件基础。我们学习的进制是硬件工作的基本原理。</p>
                    <p class="text-gray-600 mt-2"><strong>后启：</strong>图像、声音、视频处理。所有这些媒体格式的编码、压缩和保护都基于二进制运算。</p>
                </div>
                
                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card col-span-1 lg:col-span-2">
                    <h4 class="font-bold text-xl mb-3 text-accent">四维教学目标</h4>
                    <ul class="space-y-2 text-gray-600 list-disc list-inside">
                        <li><strong>知识目标：</strong>掌握二、十、十六进制的概念与互相转换的方法。</li>
                        <li><strong>能力目标：</strong>能熟练进行跨进制转换，并解释其在颜色代码、ASCII码等场景的应用。</li>
                        <li><strong>素质目标：</strong>培养逻辑思维与细致严谨的习惯，理解“所学即所用”的实践意义。</li>
                        <li><strong>思政目标：</strong>理解算力基础对国家信息技术战略的重要性，增强专业使命感。</li>
                    </ul>
                </div>

                <div class="bg-white rounded-xl shadow-custom p-8 interactive-card col-span-1 md:col-span-2 lg:col-span-3">
                     <div class="flex flex-col lg:flex-row items-center gap-8">
                        <div class="flex-1">
                            <h4 class="font-bold text-xl mb-3 text-accent">教学评价方式</h4>
                            <p class="text-gray-600 mb-4">本节课的评价是课程形成性考核的一部分，旨在全面评估你的学习效果。各部分占比如右图所示，鼓励大家积极参与课堂互动与实践。</p>
                            <ul class="text-gray-600 space-y-1">
                                <li><span class="font-semibold">课堂表现 (20%):</span> 提问与参与讨论。</li>
                                <li><span class="font-semibold">课堂练习 (40%):</span> 互动工具的练习与小组合作成果。</li>
                                <li><span class="font-semibold">课后作业 (40%):</span> 线上平台练习题与拓展学习反馈。</li>
                            </ul>
                        </div>
                        <div class="flex-shrink-0 w-full lg:w-auto">
                            <div class="chart-container">
                                <canvas id="evaluationChart"></canvas>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

    </main>

    <footer class="bg-warm-neutral mt-20">
        <div class="container mx-auto px-6 py-8 text-center text-gray-600">
            <p class="font-bold text-lg">总结：“进制是计算机的母语，理解进制才能真正理解数字媒体世界。”</p>
            <p class="mt-4">授课教师：XXX | 数字媒体技术专业 2023级</p>
        </div>
    </footer>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const decInput = document.getElementById('decInput');
            const binInput = document.getElementById('binInput');
            const hexInput = document.getElementById('hexInput');

            function clearInputs(except) {
                if (except !== 'dec') decInput.value = '';
                if (except !== 'bin') binInput.value = '';
                if (except !== 'hex') hexInput.value = '';
            }

            decInput.addEventListener('input', (e) => {
                const decValue = parseInt(e.target.value, 10);
                if (isNaN(decValue)) {
                    clearInputs('dec');
                    return;
                }
                binInput.value = decValue.toString(2);
                hexInput.value = decValue.toString(16).toUpperCase();
            });

            binInput.addEventListener('input', (e) => {
                const binValue = e.target.value;
                if (!/^[01]*$/.test(binValue) || binValue === '') {
                    clearInputs('bin');
                    return;
                }
                const decValue = parseInt(binValue, 2);
                decInput.value = decValue;
                hexInput.value = decValue.toString(16).toUpperCase();
            });

            hexInput.addEventListener('input', (e) => {
                const hexValue = e.target.value;
                if (!/^[0-9a-fA-F]*$/.test(hexValue) || hexValue === '') {
                    clearInputs('hex');
                    return;
                }
                const decValue = parseInt(hexValue, 16);
                decInput.value = decValue;
                binInput.value = decValue.toString(2);
            });
            
            const colorPicker = document.getElementById('colorPicker');
            const hexValueSpan = document.getElementById('hexValue');
            const rgbValueSpan = document.getElementById('rgbValue');
            
            colorPicker.addEventListener('input', (e) => {
                const hex = e.target.value.toUpperCase();
                hexValueSpan.textContent = hex;
                
                const r = parseInt(hex.slice(1, 3), 16);
                const g = parseInt(hex.slice(3, 5), 16);
                const b = parseInt(hex.slice(5, 7), 16);
                
                rgbValueSpan.textContent = `rgb(${r}, ${g}, ${b})`;
            });

            const decToBinVisualInput = document.getElementById('decToBinVisualInput');
            const decToBinVisualOutput = document.getElementById('decToBinVisualOutput');

            decToBinVisualInput.addEventListener('input', e => {
                let num = parseInt(e.target.value, 10);
                if (isNaN(num) || num < 0) {
                    decToBinVisualOutput.innerHTML = '请输入一个非负整数';
                    return;
                }
                if (num === 0) {
                    decToBinVisualOutput.innerHTML = `<p>结果: <span class="font-bold text-accent">0</span></p>`;
                    return;
                }

                let remainders = [];
                let stepsHtml = '<div class="flex flex-col items-center gap-2">';
                let originalNum = num;

                while (num > 0) {
                    let remainder = num % 2;
                    let quotient = Math.floor(num / 2);
                    stepsHtml += `<div class="flex items-center gap-4 bg-white p-2 rounded-lg shadow-sm step-box">
                                    <span class="text-right flex-1">${num} ÷ 2 = ${quotient}</span>
                                    <span class="text-left flex-1">余 <strong class="text-red-500 text-lg">${remainder}</strong></span>
                                  </div>`;
                    remainders.push(remainder);
                    num = quotient;
                }
                
                const result = remainders.reverse().join('');
                stepsHtml += `</div><p class="mt-4">将余数从下往上排列得到结果：<strong class="text-accent text-xl">${result}</strong></p>`;
                decToBinVisualOutput.innerHTML = stepsHtml;
            });

            const binToHexVisualInput = document.getElementById('binToHexVisualInput');
            const binGroups = document.getElementById('binGroups');
            const hexResult = document.getElementById('hexResult');
            
            binToHexVisualInput.addEventListener('input', e => {
                let binStr = e.target.value.replace(/[^01]/g, '');
                binToHexVisualInput.value = binStr;

                if (binStr === '') {
                    binGroups.innerHTML = '...';
                    hexResult.innerHTML = '...';
                    return;
                }

                let paddedBinStr = binStr.padStart(Math.ceil(binStr.length / 4) * 4, '0');
                
                let groupsHtml = '';
                let hexStr = '';
                for (let i = 0; i < paddedBinStr.length; i += 4) {
                    const group = paddedBinStr.substring(i, i + 4);
                    const hexChar = parseInt(group, 2).toString(16).toUpperCase();
                    groupsHtml += `<span class="inline-block p-2 m-1 bg-white rounded shadow-sm">${group}</span>`;
                    hexStr += hexChar;
                }
                binGroups.innerHTML = groupsHtml;
                hexResult.innerHTML = hexStr;
            });
            
            const ctx = document.getElementById('evaluationChart').getContext('2d');
            new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: ['课堂表现', '课堂练习', '课后作业'],
                    datasets: [{
                        label: '教学评价占比',
                        data: [20, 40, 40],
                        backgroundColor: [
                            'rgba(59, 130, 246, 0.7)',
                            'rgba(34, 197, 94, 0.7)',
                            'rgba(249, 115, 22, 0.7)'
                        ],
                        borderColor: [
                            '#FDFBF8',
                            '#FDFBF8',
                            '#FDFBF8'
                        ],
                        borderWidth: 4,
                        hoverOffset: 8
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom',
                            labels: {
                                padding: 20,
                                font: {
                                    family: "'Noto Sans SC', sans-serif",
                                    size: 14
                                }
                            }
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.label || '';
                                    if (label) {
                                        label += ': ';
                                    }
                                    if (context.parsed !== null) {
                                        label += context.parsed + '%';
                                    }
                                    return label;
                                }
                            }
                        }
                    },
                    cutout: '60%'
                }
            });

            const navLinks = document.querySelectorAll('.nav-link');
            const sections = document.querySelectorAll('main section');

            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        navLinks.forEach(link => {
                            link.classList.remove('active');
                            if (link.getAttribute('href').substring(1) === entry.target.id) {
                                link.classList.add('active');
                            }
                        });
                    }
                });
            }, { threshold: 0.5 }); 
            
            sections.forEach(section => {
                observer.observe(section);
            });
        });
    </script>
</body>
</html>
