<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Mansour E4 - تطبيق تصميم التطريز</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        :root {
            --navy: #0A0E27;
            --deep-blue: #0D1B3E;
            --dark-gray: #1E1E2E;
            --toolbar: #161B22;
            --border: #2D333B;
            --accent: #FF9500;
            --gold: #FFB800;
            --orange: #FF6B00;
            --cyan: #00B4D8;
            --red: #FF4757;
            --white: #E6EDF3;
            --purple: #9B59B6;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--navy);
            color: var(--white);
            overflow: hidden;
            height: 100vh;
            width: 100vw;
            display: flex;
            flex-direction: column;
        }

        /* === شاشة البداية === */
        .splash-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at center, var(--deep-blue) 0%, var(--navy) 50%, #050A1A 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 10000;
            transition: opacity 0.8s ease, visibility 0.8s ease;
        }

        .splash-screen.hidden {
            opacity: 0;
            visibility: hidden;
        }

        .splash-logo {
            text-align: center;
            animation: fadeInUp 1s ease;
        }

        .splash-logo h1 {
            font-size: 3rem;
            font-weight: 700;
            color: var(--white);
            letter-spacing: 4px;
        }

        .splash-logo .e4 {
            font-size: 5rem;
            font-weight: 900;
            color: var(--accent);
            line-height: 1;
            text-shadow: 0 0 30px rgba(255, 149, 0, 0.3);
        }

        .splash-logo p {
            font-size: 1.2rem;
            color: var(--white);
            opacity: 0.7;
            margin-top: 1rem;
        }

        .splash-developer {
            position: absolute;
            bottom: 2rem;
            text-align: center;
        }

        .splash-developer p {
            font-size: 0.9rem;
            color: var(--accent);
            opacity: 0.8;
        }

        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* === الشريط العلوي === */
        .top-bar {
            background: var(--toolbar);
            padding: 12px 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            z-index: 100;
        }

        .top-bar h2 {
            font-size: 1.1rem;
            color: var(--accent);
            font-weight: 700;
        }

        .top-bar .icons {
            display: flex;
            gap: 12px;
        }

        .icon-btn {
            width: 36px;
            height: 36px;
            border-radius: 8px;
            background: var(--dark-gray);
            border: none;
            color: var(--white);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 1.1rem;
        }

        .icon-btn:hover {
            background: var(--accent);
            color: var(--navy);
        }

        .icon-btn.active {
            background: var(--accent);
            color: var(--navy);
        }

        /* === لوحة الرسم === */
        .canvas-container {
            flex: 1;
            position: relative;
            overflow: hidden;
            background: 
                linear-gradient(rgba(30, 58, 95, 0.15) 1px, transparent 1px),
                linear-gradient(90deg, rgba(30, 58, 95, 0.15) 1px, transparent 1px),
                radial-gradient(circle at center, var(--deep-blue) 0%, var(--navy) 100%);
            background-size: 20px 20px, 20px 20px, 100% 100%;
        }

        #embroideryCanvas {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            cursor: grab;
        }

        #embroideryCanvas:active {
            cursor: grabbing;
        }

        .canvas-info {
            position: absolute;
            bottom: 8px;
            left: 8px;
            background: rgba(10, 14, 39, 0.8);
            padding: 6px 12px;
            border-radius: 6px;
            font-size: 0.75rem;
            color: var(--white);
            opacity: 0.6;
            pointer-events: none;
        }

        /* === شريط المحاكاة === */
        .simulator-bar {
            background: var(--toolbar);
            padding: 8px 16px;
            border-top: 1px solid var(--border);
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .play-btn {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: var(--accent);
            border: none;
            color: var(--navy);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            font-size: 1rem;
            transition: transform 0.2s;
        }

        .play-btn:hover {
            transform: scale(1.1);
        }

        .seek-bar {
            flex: 1;
            height: 6px;
            -webkit-appearance: none;
            appearance: none;
            background: var(--border);
            border-radius: 3px;
            outline: none;
        }

        .seek-bar::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            border-radius: 50%;
            background: var(--accent);
            cursor: pointer;
            box-shadow: 0 0 10px rgba(255, 149, 0, 0.4);
        }

        .stitch-counter {
            font-size: 0.8rem;
            color: var(--white);
            opacity: 0.7;
            min-width: 60px;
            text-align: left;
        }

        /* === شريط الأدوات السفلي === */
        .bottom-toolbar {
            background: var(--toolbar);
            padding: 8px;
            border-top: 1px solid var(--border);
        }

        .tools-scroll {
            display: flex;
            gap: 8px;
            overflow-x: auto;
            padding: 4px;
            scrollbar-width: none;
        }

        .tools-scroll::-webkit-scrollbar {
            display: none;
        }

        .tool-btn {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            padding: 8px 12px;
            border-radius: 10px;
            background: var(--dark-gray);
            border: none;
            color: var(--white);
            cursor: pointer;
            transition: all 0.2s;
            min-width: 60px;
        }

        .tool-btn:hover {
            background: var(--accent);
            color: var(--navy);
        }

        .tool-btn.active {
            background: var(--accent);
            color: var(--navy);
        }

        .tool-btn .tool-icon {
            font-size: 1.4rem;
        }

        .tool-btn .tool-label {
            font-size: 0.65rem;
            white-space: nowrap;
        }

        .tool-divider {
            width: 1px;
            height: 40px;
            background: var(--border);
            margin: 0 4px;
        }

        .export-btn {
            background: linear-gradient(135deg, var(--accent), var(--orange));
            color: var(--navy);
            font-weight: 700;
            padding: 8px 16px;
            border-radius: 10px;
            border: none;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 6px;
            font-size: 0.85rem;
            transition: transform 0.2s;
        }

        .export-btn:hover {
            transform: scale(1.05);
        }

        /* === القوائم المنبثقة === */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: none;
            justify-content: center;
            align-items: center;
            z-index: 1000;
            backdrop-filter: blur(5px);
        }

        .modal-overlay.active {
            display: flex;
        }

        .modal {
            background: var(--dark-gray);
            border-radius: 16px;
            padding: 24px;
            width: 90%;
            max-width: 400px;
            border: 1px solid var(--border);
            animation: scaleIn 0.3s ease;
        }

        @keyframes scaleIn {
            from {
                transform: scale(0.9);
                opacity: 0;
            }
            to {
                transform: scale(1);
                opacity: 1;
            }
        }

        .modal h3 {
            color: var(--accent);
            margin-bottom: 16px;
            font-size: 1.2rem;
        }

        .modal input, .modal select {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid var(--border);
            background: var(--toolbar);
            color: var(--white);
            margin-bottom: 12px;
            font-size: 1rem;
        }

        .modal input:focus, .modal select:focus {
            outline: none;
            border-color: var(--accent);
        }

        .modal-buttons {
            display: flex;
            gap: 8px;
            margin-top: 16px;
        }

        .modal-btn {
            flex: 1;
            padding: 12px;
            border-radius: 8px;
            border: none;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.2s;
        }

        .modal-btn.primary {
            background: var(--accent);
            color: var(--navy);
            font-weight: 700;
        }

        .modal-btn.secondary {
            background: var(--toolbar);
            color: var(--white);
        }

        .modal-btn:hover {
            transform: translateY(-2px);
        }

        /* === لوحة الألوان === */
        .color-palette {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-bottom: 12px;
        }

        .color-option {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid transparent;
            transition: transform 0.2s;
        }

        .color-option:hover {
            transform: scale(1.2);
        }

        .color-option.selected {
            border-color: var(--white);
            box-shadow: 0 0 10px currentColor;
        }

        /* === رسائل التنبيه === */
        .toast {
            position: fixed;
            bottom: 100px;
            left: 50%;
            transform: translateX(-50%) translateY(100px);
            background: var(--toolbar);
            color: var(--white);
            padding: 12px 24px;
            border-radius: 8px;
            border: 1px solid var(--border);
            font-size: 0.9rem;
            opacity: 0;
            transition: all 0.3s ease;
            z-index: 10000;
        }

        .toast.show {
            transform: translateX(-50%) translateY(0);
            opacity: 1;
        }

        .toast.success {
            border-color: var(--accent);
            color: var(--accent);
        }

        .toast.error {
            border-color: var(--red);
            color: var(--red);
        }

        /* === تحميل الملف === */
        .file-input {
            display: none;
        }

        /* === لوحة النص === */
        .text-tool-options {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }

        .font-preview {
            font-size: 2rem;
            text-align: center;
            padding: 16px;
            background: var(--toolbar);
            border-radius: 8px;
            min-height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        /* === قائمة التصاميم === */
        .designs-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            max-height: 300px;
            overflow-y: auto;
        }

        .design-card {
            background: var(--toolbar);
            border-radius: 10px;
            padding: 12px;
            cursor: pointer;
            transition: all 0.2s;
            border: 1px solid var(--border);
        }

        .design-card:hover {
            border-color: var(--accent);
            transform: translateY(-2px);
        }

        .design-card .design-thumb {
            width: 100%;
            height: 80px;
            background: var(--dark-gray);
            border-radius: 6px;
            margin-bottom: 8px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
        }

        .design-card .design-name {
            font-size: 0.8rem;
            color: var(--white);
        }

        .design-card .design-info {
            font-size: 0.7rem;
            color: var(--white);
            opacity: 0.6;
            margin-top: 4px;
        }

        /* === تكيف مع الشاشات الصغيرة === */
        @media (max-width: 400px) {
            .tool-btn {
                min-width: 50px;
                padding: 6px 8px;
            }
            
            .tool-btn .tool-icon {
                font-size: 1.2rem;
            }
            
            .tool-btn .tool-label {
                font-size: 0.6rem;
            }
            
            .splash-logo h1 {
                font-size: 2rem;
            }
            
            .splash-logo .e4 {
                font-size: 3.5rem;
            }
        }
    </style>
</head>
<body>

    <!-- شاشة البداية -->
    <div class="splash-screen" id="splashScreen">
        <div class="splash-logo">
            <h1>MANSOUR</h1>
            <div class="e4">E4</div>
            <p>تصميم التطريز الاحترافي</p>
        </div>
        <div class="splash-developer">
            <p>المطور: علام العروشي | 780766520</p>
            <p style="font-size: 0.7rem; margin-top: 4px; opacity: 0.5;">Powered by AI + Wilcom + Dahao</p>
        </div>
    </div>

    <!-- الشريط العلوي -->
    <div class="top-bar">
        <h2>⚡ Mansour E4</h2>
        <div class="icons">
            <button class="icon-btn" onclick="showNewDesignModal()" title="تصميم جديد">📄</button>
            <button class="icon-btn" onclick="showOpenModal()" title="فتح ملف">📂</button>
            <button class="icon-btn" onclick="showSettingsModal()" title="الإعدادات">⚙️</button>
        </div>
    </div>

    <!-- لوحة الرسم -->
    <div class="canvas-container" id="canvasContainer">
        <canvas id="embroideryCanvas"></canvas>
        <div class="canvas-info" id="canvasInfo">
            التكبير: 1.0x | غرز: 0
        </div>
    </div>

    <!-- شريط المحاكاة -->
    <div class="simulator-bar">
        <button class="play-btn" id="playBtn" onclick="toggleSimulation()">▶️</button>
        <input type="range" class="seek-bar" id="seekBar" min="0" max="100" value="100" oninput="updateSimulation(this.value)">
        <span class="stitch-counter" id="stitchCounter">0/0</span>
    </div>

    <!-- شريط الأدوات السفلي -->
    <div class="bottom-toolbar">
        <div class="tools-scroll">
            <button class="tool-btn" id="toolSelect" onclick="setTool('select')">
                <span class="tool-icon">👆</span>
                <span class="tool-label">تحديد</span>
            </button>
            
            <button class="tool-btn" id="toolText" onclick="showTextTool()">
                <span class="tool-icon">📝</span>
                <span class="tool-label">نص</span>
            </button>
            
            <button class="tool-btn" id="toolSatin" onclick="setTool('satin')">
                <span class="tool-icon">〰️</span>
                <span class="tool-label">ساتان</span>
            </button>
            
            <button class="tool-btn" id="toolTatami" onclick="setTool('tatami')">
                <span class="tool-icon">▦</span>
                <span class="tool-label">تاتامي</span>
            </button>
            
            <button class="tool-btn" id="toolSequin" onclick="setTool('sequin')">
                <span class="tool-icon">🔘</span>
                <span class="tool-label">ترتر</span>
            </button>
            
            <div class="tool-divider"></div>
            
            <button class="tool-btn" onclick="showColorModal()">
                <span class="tool-icon">🎨</span>
                <span class="tool-label">ألوان</span>
            </button>
            
            <button class="tool-btn" onclick="showZoomModal()">
                <span class="tool-icon">🔍</span>
                <span class="tool-label">تكبير</span>
            </button>
            
            <div class="tool-divider"></div>
            
            <button class="export-btn" onclick="exportDesign()">
                <span>💾</span>
                <span>تصدير DST</span>
            </button>
        </div>
    </div>

    <!-- نافذة فتح ملف -->
    <div class="modal-overlay" id="openModal">
        <div class="modal">
            <h3>📂 فتح تصميم</h3>
            <div class="designs-grid" id="designsGrid">
                <!-- سيتم ملؤها بالجافاسكريبت -->
            </div>
            <div style="margin-top: 12px;">
                <input type="file" class="file-input" id="fileInput" accept=".dst,.pes,.exp" onchange="handleFileSelect(event)">
                <button class="modal-btn primary" style="width: 100%;" onclick="document.getElementById('fileInput').click()">
                    📁 استيراد من الجهاز
                </button>
            </div>
            <div class="modal-buttons">
                <button class="modal-btn secondary" onclick="closeModal('openModal')">إغلاق</button>
            </div>
        </div>
    </div>

    <!-- نافذة أداة النص -->
    <div class="modal-overlay" id="textModal">
        <div class="modal">
            <h3>📝 أداة النص</h3>
            <div class="text-tool-options">
                <input type="text" id="textInput" placeholder="أدخل النص هنا..." value="Mansour" oninput="updateTextPreview()">
                <select id="fontSelect" onchange="updateTextPreview()">
                    <option value="cairo">Cairo - عصري</option>
                    <option value="tajawal">Tajawal - أنيق</option>
                    <option value="thuluth">Thuluth - ثلث</option>
                    <option value="diwani">Diwani - ديواني</option>
                    <option value="naskh">Naskh - نسخ</option>
                </select>
                <div class="font-preview" id="fontPreview">Mansour</div>
                <div class="color-palette" id="textColorPalette">
                    <div class="color-option selected" style="background: #FF0000;" onclick="selectColor(this, '#FF0000')"></div>
                    <div class="color-option" style="background: #0000FF;" onclick="selectColor(this, '#0000FF')"></div>
                    <div class="color-option" style="background: #FFD700;" onclick="selectColor(this, '#FFD700')"></div>
                    <div class="color-option" style="background: #FF9500;" onclick="selectColor(this, '#FF9500')"></div>
                    <div class="color-option" style="background: #9B59B6;" onclick="selectColor(this, '#9B59B6')"></div>
                    <div class="color-option" style="background: #000000;" onclick="selectColor(this, '#000000')"></div>
                </div>
            </div>
            <div class="modal-buttons">
                <button class="modal-btn primary" onclick="applyText()">✓ تطبيق</button>
                <button class="modal-btn secondary" onclick="closeModal('textModal')">إلغاء</button>
            </div>
        </div>
    </div>

    <!-- نافذة الألوان -->
    <div class="modal-overlay" id="colorModal">
        <div class="modal">
            <h3>🎨 لوحة الألوان</h3>
            <p style="margin-bottom: 12px; opacity: 0.7;">اختر لون الخيط الحالي:</p>
            <div class="color-palette" id="mainColorPalette">
                <div class="color-option selected" style="background: #FF0000;" onclick="selectMainColor(this, '#FF0000')"></div>
                <div class="color-option" style="background: #0000FF;" onclick="selectMainColor(this, '#0000FF')"></div>
                <div class="color-option" style="background: #FFD700;" onclick="selectMainColor(this, '#FFD700')"></div>
                <div class="color-option" style="background: #FF9500;" onclick="selectMainColor(this, '#FF9500')"></div>
                <div class="color-option" style="background: #9B59B6;" onclick="selectMainColor(this, '#9B59B6')"></div>
                <div class="color-option" style="background: #00B4D8;" onclick="selectMainColor(this, '#00B4D8')"></div>
                <div class="color-option" style="background: #000000;" onclick="selectMainColor(this, '#000000')"></div>
                <div class="color-option" style="background: #FFFFFF;" onclick="selectMainColor(this, '#FFFFFF')"></div>
            </div>
            <div class="modal-buttons">
                <button class="modal-btn secondary" onclick="closeModal('colorModal')">إغلاق</button>
            </div>
        </div>
    </div>

    <!-- نافذة التكبير -->
    <div class="modal-overlay" id="zoomModal">
        <div class="modal">
            <h3>🔍 التكبير</h3>
            <input type="range" min="0.1" max="5" step="0.1" value="1" id="zoomSlider" oninput="updateZoom(this.value)" style="width: 100%; margin-bottom: 12px;">
            <p style="text-align: center; font-size: 1.2rem;" id="zoomValue">1.0x</p>
            <div class="modal-buttons">
                <button class="modal-btn primary" onclick="resetZoom()">↺ إعادة</button>
                <button class="modal-btn secondary" onclick="closeModal('zoomModal')">إغلاق</button>
            </div>
        </div>
    </div>

    <!-- نافذة التصدير -->
    <div class="modal-overlay" id="exportModal">
        <div class="modal">
            <h3>💾 تصدير التصميم</h3>
            <p style="margin-bottom: 12px; opacity: 0.7;">اختر صيغة التصدير:</p>
            <select id="exportFormat">
                <option value="dst">DST - Tajima/Dahao</option>
                <option value="pes">PES - Brother</option>
                <option value="exp">EXP - Melco</option>
                <option value="jef">JEF - Janome</option>
            </select>
            <input type="text" id="exportName" placeholder="اسم الملف..." value="MY_DESIGN">
            <div class="modal-buttons">
                <button class="modal-btn primary" onclick="confirmExport()">📥 تصدير</button>
                <button class="modal-btn secondary" onclick="closeModal('exportModal')">إلغاء</button>
            </div>
        </div>
    </div>

    <!-- رسائل التنبيه -->
    <div class="toast" id="toast"></div>

    <script>
        // === المتغيرات العامة ===
        let canvas, ctx;
        let stitches = [];
        let currentStitchIndex = 0;
        let isSimulating = false;
        let simulationInterval;
        let scale = 1.0;
        let translateX = 0;
        let translateY = 0;
        let currentTool = 'select';
        let currentColor = '#FF0000';
        let isDragging = false;
        let lastTouchX, lastTouchY;
        let touchStartDistance = 0;
        let initialScale = 1;

        // === تهيئة التطبيق ===
        window.onload = function() {
            // إخفاء شاشة البداية بعد 2.5 ثانية
            setTimeout(() => {
                document.getElementById('splashScreen').classList.add('hidden');
            }, 2500);

            // تهيئة Canvas
            canvas = document.getElementById('embroideryCanvas');
            ctx = canvas.getContext('2d');
            
            // تعيين حCanvas
            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);

            // إضافة أحداث اللمس
            setupTouchEvents();

            // تحميل تصميم افتراضي
            loadDemoDesign();
        };

        function resizeCanvas() {
            const container = document.getElementById('canvasContainer');
            canvas.width = container.clientWidth;
            canvas.height = container.clientHeight;
            redrawCanvas();
        }

        // === أحداث اللمس المتعدد ===
        function setupTouchEvents() {
            const container = document.getElementById('canvasContainer');

            // لمسة واحدة - السحب
            container.addEventListener('touchstart', handleTouchStart, { passive: false });
            container.addEventListener('touchmove', handleTouchMove, { passive: false });
            container.addEventListener('touchend', handleTouchEnd);

            // عجلة الماوس - التكبير
            container.addEventListener('wheel', handleWheel, { passive: false });

            // الماوس - للاختبار على الكمبيوتر
            container.addEventListener('mousedown', handleMouseDown);
            container.addEventListener('mousemove', handleMouseMove);
            container.addEventListener('mouseup', handleMouseUp);
        }

        let touches = [];

        function handleTouchStart(e) {
            e.preventDefault();
            touches = Array.from(e.touches);

            if (touches.length === 1) {
                // سحب
                isDragging = true;
                lastTouchX = touches[0].clientX;
                lastTouchY = touches[0].clientY;
            } else if (touches.length === 2) {
                // تكبير
                isDragging = false;
                const dx = touches[0].clientX - touches[1].clientX;
                const dy = touches[0].clientY - touches[1].clientY;
                touchStartDistance = Math.sqrt(dx * dx + dy * dy);
                initialScale = scale;
            }
        }

        function handleTouchMove(e) {
            e.preventDefault();
            const newTouches = Array.from(e.touches);

            if (newTouches.length === 1 && isDragging) {
                // سحب
                const dx = newTouches[0].clientX - lastTouchX;
                const dy = newTouches[0].clientY - lastTouchY;
                translateX += dx;
                translateY += dy;
                lastTouchX = newTouches[0].clientX;
                lastTouchY = newTouches[0].clientY;
                redrawCanvas();
            } else if (newTouches.length === 2) {
                // تكبير
                const dx = newTouches[0].clientX - newTouches[1].clientX;
                const dy = newTouches[0].clientY - newTouches[1].clientY;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (touchStartDistance > 0) {
                    scale = Math.max(0.1, Math.min(20, initialScale * (distance / touchStartDistance)));
                    updateCanvasInfo();
                    redrawCanvas();
                }
            }

            touches = newTouches;
        }

        function handleTouchEnd(e) {
            isDragging = false;
            touches = Array.from(e.touches);
        }

        function handleWheel(e) {
            e.preventDefault();
            const delta = e.deltaY > 0 ? 0.9 : 1.1;
            scale = Math.max(0.1, Math.min(20, scale * delta));
            updateCanvasInfo();
            redrawCanvas();
        }

        function handleMouseDown(e) {
            isDragging = true;
            lastTouchX = e.clientX;
            lastTouchY = e.clientY;
        }

        function handleMouseMove(e) {
            if (isDragging) {
                const dx = e.clientX - lastTouchX;
                const dy = e.clientY - lastTouchY;
                translateX += dx;
                translateY += dy;
                lastTouchX = e.clientX;
                lastTouchY = e.clientY;
                redrawCanvas();
            }
        }

        function handleMouseUp(e) {
            isDragging = false;
        }

        // === رسم التصميم ===
        function redrawCanvas() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            ctx.save();
            
            // تطبيق التحويلات
            ctx.translate(canvas.width / 2 + translateX, canvas.height / 2 + translateY);
            ctx.scale(scale, scale);
            
            // رسم الغرز
            let currentX = 0;
            let currentY = 0;
            let colorIndex = 0;
            
            const colors = [
                '#FF0000', '#0000FF', '#FFD700', '#FF9500', 
                '#9B59B6', '#00B4D8', '#000000'
            ];
            
            ctx.strokeStyle = colors[0];
            ctx.lineWidth = 2 / scale;
            ctx.lineCap = 'round';
            ctx.lineJoin = 'round';
            
            for (let i = 0; i < currentStitchIndex && i < stitches.length; i++) {
                const stitch = stitches[i];
                const nextX = currentX + stitch.dx;
                const nextY = currentY - stitch.dy;
                
                if (stitch.type === 'NORMAL') {
                    ctx.beginPath();
                    ctx.moveTo(currentX, currentY);
                    ctx.lineTo(nextX, nextY);
                    ctx.stroke();
                } else if (stitch.type === 'JUMP') {
                    ctx.setLineDash([5 / scale, 5 / scale]);
                    ctx.strokeStyle = '#666';
                    ctx.beginPath();
                    ctx.moveTo(currentX, currentY);
                    ctx.lineTo(nextX, nextY);
                    ctx.stroke();
                    ctx.setLineDash([]);
                    ctx.strokeStyle = colors[colorIndex % colors.length];
                } else if (stitch.type === 'STOP') {
                    colorIndex++;
                    ctx.strokeStyle = colors[colorIndex % colors.length];
                }
                
                currentX = nextX;
                currentY = nextY;
            }
            
            ctx.restore();
        }

        // === تحميل تصميم تجريبي ===
        function loadDemoDesign() {
            // إنشاء غرز تجريبية (شكل دائري)
            stitches = [];
            const radius = 100;
            const steps = 200;
            
            for (let i = 0; i < steps; i++) {
                const angle = (i / steps) * Math.PI * 2;
                const x = Math.cos(angle) * radius;
                const y = Math.sin(angle) * radius;
                const prevAngle = ((i - 1) / steps) * Math.PI * 2;
                const prevX = Math.cos(prevAngle) * radius;
                const prevY = Math.sin(prevAngle) * radius;
                
                stitches.push({
                    dx: x - prevX,
                    dy: y - prevY,
                    type: 'NORMAL'
                });
            }
            
            // إضافة غرز حشو داخلية
            for (let r = 10; r < radius - 10; r += 8) {
                for (let i = 0; i < 100; i++) {
                    const angle = (i / 100) * Math.PI * 2;
                    const x = Math.cos(angle) * r;
                    const y = Math.sin(angle) * r;
                    const prevAngle = ((i - 1) / 100) * Math.PI * 2;
                    const prevX = Math.cos(prevAngle) * r;
                    const prevY = Math.sin(prevAngle) * r;
                    
                    stitches.push({
                        dx: x - prevX,
                        dy: y - prevY,
                        type: 'NORMAL'
                    });
                }
                stitches.push({ dx: 0, dy: 0, type: 'STOP' });
            }
            
            currentStitchIndex = stitches.length;
            updateStitchCounter();
            updateCanvasInfo();
            redrawCanvas();
        }

        // === محاكاة التشغيل ===
        function toggleSimulation() {
            const btn = document.getElementById('playBtn');
            
            if (isSimulating) {
                isSimulating = false;
                clearInterval(simulationInterval);
                btn.textContent = '▶️';
            } else {
                isSimulating = true;
                btn.textContent = '⏸️';
                currentStitchIndex = 0;
                
                simulationInterval = setInterval(() => {
                    if (currentStitchIndex < stitches.length) {
                        currentStitchIndex += 5;
                        updateStitchCounter();
                        redrawCanvas();
                    } else {
                        toggleSimulation();
                    }
                }, 50);
            }
        }

        function updateSimulation(value) {
            currentStitchIndex = Math.floor((value / 100) * stitches.length);
            updateStitchCounter();
            redrawCanvas();
        }

        function updateStitchCounter() {
            document.getElementById('stitchCounter').textContent = 
                `${currentStitchIndex}/${stitches.length}`;
        }

        function updateCanvasInfo() {
            document.getElementById('canvasInfo').textContent = 
                `التكبير: ${scale.toFixed(1)}x | غرز: ${stitches.length}`;
        }

        // === أدوات التصميم ===
        function setTool(tool) {
            currentTool = tool;
            
            // إزالة التنشيط من جميع الأزرار
            document.querySelectorAll('.tool-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // تنشيط الزر المحدد
            document.getElementById(`tool${tool.charAt(0).toUpperCase() + tool.slice(1)}`)?.classList.add('active');
            
            showToast(`أداة ${tool} مفعلة`, 'success');
        }

        // === النوافذ المنبثقة ===
        function showModal(modalId) {
            document.getElementById(modalId).classList.add('active');
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
        }

        function showOpenModal() {
            // ملء قائمة التصاميم
            const grid = document.getElementById('designsGrid');
            grid.innerHTML = `
                <div class="design-card" onclick="loadDemoDesign(); closeModal('openModal'); showToast('تم تحميل التصميم', 'success');">
                    <div class="design-thumb">🌸</div>
                    <div class="design-name">تصميم وردة</div>
                    <div class="design-info">1,234 غرزة | 4 ألوان</div>
                </div>
                <div class="design-card" onclick="showToast('قريباً', 'success');">
                    <div class="design-thumb">🌙</div>
                    <div class="design-name">هلال</div>
                    <div class="design-info">856 غرزة | 2 ألوان</div>
                </div>
                <div class="design-card" onclick="showToast('قريباً', 'success');">
                    <div class="design-thumb">✨</div>
                    <div class="design-name">نجمة</div>
                    <div class="design-info">2,100 غرزة | 6 ألوان</div>
                </div>
            `;
            showModal('openModal');
        }

        function showTextTool() {
            showModal('textModal');
        }

        function showColorModal() {
            showModal('colorModal');
        }

        function showZoomModal() {
            showModal('zoomModal');
        }

        function showNewDesignModal() {
            if (confirm('هل تريد إنشاء تصميم جديد؟ سيتم فقدان التغييرات غير المحفوظة.')) {
                stitches = [];
                currentStitchIndex = 0;
                redrawCanvas();
                updateStitchCounter();
                showToast('تصميم جديد', 'success');
            }
        }

        function showSettingsModal() {
            showToast('الإعدادات - قريباً', 'success');
        }

        // === معالجة الملفات ===
        function handleFileSelect(event) {
            const file = event.target.files[0];
            if (!file) return;
            
            showToast(`جاري تحميل ${file.name}...`, 'success');
            
            const reader = new FileReader();
            reader.onload = function(e) {
                // محاكاة قراءة DST
                const array = new Uint8Array(e.target.result);
                parseDST(array);
            };
            reader.readAsArrayBuffer(file);
        }

        function parseDST(data) {
            // تخطي الترويسة (512 بايت)
            let offset = 512;
            stitches = [];
            
            while (offset + 3 <= data.length) {
                const b0 = data[offset] & 0xFF;
                const b1 = data[offset + 1] & 0xFF;
                const b2 = data[offset + 2] & 0xFF;
                
                let dx = 0, dy = 0;
                
                // فك تشفير بايت 0
                if (b0 & 0x01) dx += 1;
                if (b0 & 0x02) dx -= 1;
                if (b0 & 0x04) dx += 9;
                if (b0 & 0x08) dx -= 9;
                if (b0 & 0x10) dy += 9;
                if (b0 & 0x20) dy -= 9;
                if (b0 & 0x40) dy += 1;
                if (b0 & 0x80) dy -= 1;
                
                // فك تشفير بايت 1
                if (b1 & 0x01) dx += 3;
                if (b1 & 0x02) dx -= 3;
                if (b1 & 0x04) dx += 27;
                if (b1 & 0x08) dx -= 27;
                if (b1 & 0x10) dy += 27;
                if (b1 & 0x20) dy -= 27;
                if (b1 & 0x40) dy += 3;
                if (b1 & 0x80) dy -= 3;
                
                // فك تشفير بايت 2
                if (b2 & 0x04) dx += 81;
                if (b2 & 0x08) dx -= 81;
                if (b2 & 0x10) dy += 81;
                if (b2 & 0x20) dy -= 81;
                
                const ctrl = b2 & 0xC0;
                let type = 'NORMAL';
                if (ctrl === 0xC0) type = 'JUMP';
                else if (ctrl === 0x40) type = 'STOP';
                
                if (b0 === 0x00 && b1 === 0x00 && b2 === 0xF3) {
                    stitches.push({ dx: 0, dy: 0, type: 'END' });
                    break;
                }
                
                stitches.push({ dx, dy, type });
                offset += 3;
            }
            
            currentStitchIndex = stitches.length;
            updateStitchCounter();
            updateCanvasInfo();
            redrawCanvas();
            closeModal('openModal');
            showToast(`تم تحميل ${stitches.length} غرزة`, 'success');
        }

        // === أداة النص ===
        function updateTextPreview() {
            const text = document.getElementById('textInput').value || 'Mansour';
            const font = document.getElementById('fontSelect').value;
            const preview = document.getElementById('fontPreview');
            
            const fonts = {
                'cairo': 'Segoe UI, sans-serif',
                'tajawal': 'Georgia, serif',
                'thuluth': 'Times New Roman, serif',
                'diwani': 'Arial, sans-serif',
                'naskh': 'Courier New, monospace'
            };
            
            preview.style.fontFamily = fonts[font] || fonts['cairo'];
            preview.textContent = text;
        }

        function selectColor(element, color) {
            document.querySelectorAll('#textColorPalette .color-option').forEach(el => {
                el.classList.remove('selected');
            });
            element.classList.add('selected');
        }

        function applyText() {
            const text = document.getElementById('textInput').value;
            if (!text) {
                showToast('أدخل النص أولاً', 'error');
                return;
            }
            
            // إنشاء غرز نصية (محاكاة)
            stitches = [];
            let x = 0;
            const y = 0;
            
            for (let i = 0; i < text.length * 20; i++) {
                stitches.push({
                    dx: 5,
                    dy: Math.sin(i * 0.3) * 10,
                    type: 'NORMAL'
                });
            }
            
            stitches.push({ dx: 0, dy: 0, type: 'STOP' });
            
            // غرز ثانية
            for (let i = 0; i < text.length * 20; i++) {
                stitches.push({
                    dx: 5,
                    dy: Math.sin(i * 0.3) * 10 + 20,
                    type: 'NORMAL'
                });
            }
            
            currentStitchIndex = stitches.length;
            updateStitchCounter();
            updateCanvasInfo();
            redrawCanvas();
            closeModal('textModal');
            showToast('تم إضافة النص', 'success');
        }

        function selectMainColor(element, color) {
            currentColor = color;
            document.querySelectorAll('#mainColorPalette .color-option').forEach(el => {
                el.classList.remove('selected');
            });
            element.classList.add('selected');
            showToast(`اللون المحدد: ${color}`, 'success');
        }

        // === التكبير ===
        function updateZoom(value) {
            scale = parseFloat(value);
            document.getElementById('zoomValue').textContent = scale.toFixed(1) + 'x';
            updateCanvasInfo();
            redrawCanvas();
        }

        function resetZoom() {
            scale = 1.0;
            translateX = 0;
            translateY = 0;
            document.getElementById('zoomSlider').value = 1;
            document.getElementById('zoomValue').textContent = '1.0x';
            updateCanvasInfo();
            redrawCanvas();
            showToast('تم إعادة التعيين', 'success');
        }

        // === التصدير ===
        function exportDesign() {
            showModal('exportModal');
        }

        function confirmExport() {
            const format = document.getElementById('exportFormat').value;
            const name = document.getElementById('exportName').value || 'MY_DESIGN';
            
            if (format === 'dst') {
                exportDST(name);
            } else {
                showToast(`صيغة ${format.toUpperCase()} - قريباً`, 'success');
            }
            
            closeModal('exportModal');
        }

        function exportDST(name) {
            // إنشاء ملف DST
            const header = new Uint8Array(512);
            header.fill(0x20); // مسافات
            
            // كتابة الترويسة
            const headerText = `LA:${name.slice(0,8).padEnd(8,' ')}\rST:${stitches.length.toString().padStart(7,' ')}\rCO:  0\r+X:  0\r-X:  0\r+Y:  0\r-Y:  0\rAX:  0\rAY:  0\rMX:  0\rMY:  0\rPD:======\r`;
            const textBytes = new TextEncoder().encode(headerText);
            header.set(textBytes, 0);
            header[511] = 0x1A; // علامة النهاية
            
            // كتابة الغرز
            const stitchData = [];
            for (const stitch of stitches) {
                if (stitch.type === 'END') {
                    stitchData.push(0x00, 0x00, 0xF3);
                    break;
                }
                
                let b0 = 0, b1 = 0, b2 = 0;
                const dx = Math.round(stitch.dx);
                const dy = Math.round(stitch.dy);
                
                // ترميز X
                let absDx = Math.abs(dx);
                const signDx = dx >= 0;
                if (absDx >= 81) { b2 |= signDx ? 0x04 : 0x08; absDx -= 81; }
                if (absDx >= 27) { b1 |= signDx ? 0x04 : 0x08; absDx -= 27; }
                if (absDx >= 9) { b0 |= signDx ? 0x04 : 0x08; absDx -= 9; }
                if (absDx >= 3) { b1 |= signDx ? 0x01 : 0x02; absDx -= 3; }
                if (absDx >= 1) { b0 |= signDx ? 0x01 : 0x02; absDx -= 1; }
                
                // ترميز Y
                let absDy = Math.abs(dy);
                const signDy = dy >= 0;
                if (absDy >= 81) { b2 |= signDy ? 0x10 : 0x20; absDy -= 81; }
                if (absDy >= 27) { b1 |= signDy ? 0x10 : 0x20; absDy -= 27; }
                if (absDy >= 9) { b0 |= signDy ? 0x10 : 0x20; absDy -= 9; }
                if (absDy >= 3) { b1 |= signDy ? 0x40 : 0x80; absDy -= 3; }
                if (absDy >= 1) { b0 |= signDy ? 0x40 : 0x80; absDy -= 1; }
                
                // نوع الغرزة
                if (stitch.type === 'JUMP') b2 |= 0xC0;
                else if (stitch.type === 'STOP') b2 |= 0x40;
                
                stitchData.push(b0, b1, b2);
            }
            
            // دمج الملف
            const fileData = new Uint8Array(header.length + stitchData.length);
            fileData.set(header, 0);
            fileData.set(stitchData, header.length);
            
            // تحميل الملف
            const blob = new Blob([fileData], { type: 'application/octet-stream' });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = `${name}.DST`;
            a.click();
            URL.revokeObjectURL(url);
            
            showToast(`تم تصدير ${name}.DST`, 'success');
        }

        // === رسائل التنبيه ===
        function showToast(message, type = '') {
            const toast = document.getElementById('toast');
            toast.textContent = message;
            toast.className = `toast ${type}`;
            
            setTimeout(() => toast.classList.add('show'), 10);
            setTimeout(() => toast.classList.remove('show'), 3000);
        }

        // === إغلاق النوافذ بالنقر خارجها ===
        document.querySelectorAll('.modal-overlay').forEach(overlay => {
            overlay.addEventListener('click', function(e) {
                if (e.target === this) {
                    this.classList.remove('active');
                }
            });
        });
    </script>
</body>
</html>
