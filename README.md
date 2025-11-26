<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>日照中心員工工作手冊 - 入職指南</title>
    <!-- 載入 Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* 使用 Inter 字體作為主要字體，並加入適合中文的字體 */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&family=Noto+Sans+TC:wght@100..900&display=swap');
        body {
            font-family: 'Inter', 'Noto Sans TC', sans-serif;
            background-color: #e5f3f7; /* 輕微的、帶有活力的淺藍綠色背景 */
        }
        /* 自定義滾動條樣式 */
        .custom-scroll::-webkit-scrollbar {
            width: 8px;
        }
        .custom-scroll::-webkit-scrollbar-thumb {
            background-color: #a7f3d0; /* 淺綠色 (Teal-200) */
            border-radius: 4px;
        }
        .custom-scroll::-webkit-scrollbar-track {
            background-color: #f0fdfa; /* 淺白綠色 (Teal-50) */
        }
        /* 隱藏移動版側邊欄 */
        .sidebar-mobile-hidden {
            transform: translateX(-100%);
            opacity: 0;
            pointer-events: none;
        }
        /* 讓內容區的列表更美觀 */
        .content-list ul {
            list-style: disc;
            padding-left: 1.5rem;
            margin-top: 0.5rem;
        }
        .content-list li {
            margin-bottom: 0.5rem;
        }
    </style>
</head>
<body class="min-h-screen antialiased text-gray-800">

    <div id="app" class="flex flex-col lg:flex-row min-h-screen">
        <!-- 側邊欄/導航 -->
        <aside id="sidebar" class="fixed inset-y-0 left-0 z-30 w-64 bg-white border-r border-gray-200 transition-all duration-300 shadow-2xl lg:shadow-xl lg:static lg:block lg:translate-x-0 sidebar-mobile-hidden">
            <div class="p-6 h-full flex flex-col custom-scroll overflow-y-auto">
                <h1 class="text-2xl font-bold text-teal-600 mb-6 border-b-4 border-teal-100 pb-3">
                    <span class="text-3xl mr-2">🌞</span>日照工作手冊
                </h1>
                <nav id="nav-list" class="space-y-3 flex-grow"></nav>
                <div class="mt-8 pt-4 border-t text-sm text-gray-500">
                    <p>版本：v1.0.0 (活力版)</p>
                    <p>更新日期：2025/11</p>
                </div>
            </div>
        </aside>

        <!-- 主內容區域 -->
        <main class="flex-grow p-4 lg:p-8 transition-all duration-300">
            <!-- 手機版 Header & 漢堡選單 -->
            <header class="flex items-center justify-between lg:hidden mb-4 p-2 bg-white rounded-2xl shadow-lg sticky top-0 z-20">
                <h2 class="text-xl font-bold text-teal-600">日照手冊</h2>
                <button id="menu-toggle" class="p-2 text-gray-600 focus:outline-none rounded-lg hover:bg-gray-100 transition-colors">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16m-7 6h7"></path></svg>
                </button>
            </header>

            <!-- 內容區 -->
            <div id="content-container" class="bg-white p-6 lg:p-10 rounded-3xl shadow-2xl min-h-[80vh]">
                <!-- 內容將由 JS 注入 -->
            </div>
        </main>
    </div>

    <!-- 側邊欄覆蓋層 (僅限移動設備) -->
    <div id="sidebar-backdrop" class="fixed inset-0 bg-gray-900 bg-opacity-50 z-20 hidden lg:hidden"></div>

    <script>
        // --- 數據結構：手冊的章節內容 ---
        const handbookContent = [
            {
                id: 'vision',
                title: '一、中心簡介與核心價值 (Welcome and Vision)',
                icon: '🏠',
                subsections: [
                    { title: '歡迎詞', content: '中心主任或創辦人的歡迎訊息。讓員工感受到被重視。' },
                    { title: '中心願景與使命', content: '說明中心的服務理念、對長輩的承諾以及在社區中的角色。重點強調**人本精神**和**服務品質**。' },
                    { title: '組織架構與部門職責', content: '介紹主要管理層級、各部門（照護、行政、社工、護理）及其職責，了解「找誰」和「負責什麼」。' },
                    { title: '核心價值與服務準則', content: '例如：**尊重、專業、愛心、同理心**等。這是所有行為的指導原則。' }
                ],
                color: 'sky' // 藍天色
            },
            {
                id: 'hr',
                title: '二、人事與行政規範 (HR and Administrative Policies)',
                icon: '📝',
                subsections: [
                    { title: '聘用條款', content: '工資、福利、保險（勞健保、團保等）、試用期規定。依據勞動法規詳列。' },
                    { title: '排班與休假制度', content: '例行工時、休息時間、請假流程（特休、病假、事假）、輪班規範。確保公平性和透明度。' },
                    { title: '教育訓練與考核', content: '新人訓練內容、在職訓練、年度考核與晉升機制。鼓勵專業成長。' },
                    { title: '服裝儀容規範', content: '制服穿著、名牌配戴、個人衛生要求。展現專業形象。' },
                    { title: '保密協議與個資保護', content: '說明長輩資料、中心營運資料的保密要求。遵守《個人資料保護法》。' }
                ],
                color: 'indigo' // 專業紫
            },
            {
                id: 'care',
                title: '三、專業服務與照護流程 (Professional Care Procedures)',
                icon: '❤️',
                isCritical: true,
                subsections: [
                    { title: 'A. 日常照護標準作業程序 (SOP)', content: '<div class="content-list"><ul><li>**報到與離所程序：** 長輩上下車、交接班注意事項。</li><li>**身體清潔與衛生：** 協助如廁、更換尿布、洗手、口腔清潔等步驟。</li><li>**膳食服務：** 餵食技巧、餐飲紀錄、特殊飲食處理。</li><li>**用藥管理：** 代為服藥或提醒用藥的正確流程、紀錄方式。</li><li>**活動帶領：** 帶領長輩進行認知、體能、社交等活動的技巧與安全須知。</li></ul></div><p class="mt-4 p-3 bg-red-100 text-red-800 rounded-lg border border-red-300 font-bold shadow-md">🚨 【行動提醒】本處需要填寫貴中心最詳盡的 SOP 步驟圖，以確保照護品質一致性。</p>' },
                    { title: 'B. 緊急事件處理流程', content: '<div class="content-list"><ul><li>**跌倒處理流程：** 立即反應、評估、通知家屬、紀錄。</li><li>**疾病突發處理：** (例如：中風、噎食) 急救步驟與送醫流程。</li><li>**失蹤協尋流程：** 立即通報、內部尋找、聯繫家屬與警政單位。</li><li>**消防與防災：** 熟悉逃生路線、滅火器材位置、疏散步驟。</li></ul></div>' },
                    { title: 'C. 文件與紀錄', content: '紀錄表單填寫規範：個案記錄、身體狀況觀察表、活動參與記錄。交接班事項：紀錄重點、口頭交接內容。' }
                ],
                color: 'red' // 緊急紅
            },
            {
                id: 'ethics',
                title: '四、倫理與溝通 (Ethics and Communication)',
                icon: '💬',
                subsections: [
                    { title: '專業倫理規範', content: '避免與長輩或家屬建立不當關係、尊重長輩自主權。確保服務的公正性。' },
                    { title: '投訴與意見回饋', content: '長輩、家屬、員工提出意見或投訴的正式管道和處理流程。維護服務品質。' },
                    { title: '員工溝通技巧', content: '與長輩溝通的原則（耐心、清晰）、與家屬溝通的界線與重點。**同理心**是關鍵。' },
                    { title: '衝突管理', content: '處理長輩間、長輩與員工間衝突的原則與方法。保持冷靜與客觀。' }
                ],
                color: 'green' // 和諧綠
            },
            {
                id: 'safety',
                title: '五、安全與環境管理 (Safety and Environment)',
                icon: '⚠️',
                subsections: [
                    { title: '環境設施與設備', content: '輔具使用（輪椅、助行器）、復健器材的操作與保養。確保設備安全。' },
                    { title: '感控與衛生', content: '個人防護設備（口罩、手套）使用時機、環境消毒程序。預防交叉感染。' },
                    { title: '事故預防', content: '危險區域標示、防跌措施、約束工具的使用原則與限制。**約束需極度謹慎且符合法規。**' }
                ],
                color: 'amber' // 警示黃 (取代舊的 orange)
            }
        ];

        // --- DOM 元素引用 ---
        const navList = document.getElementById('nav-list');
        const contentContainer = document.getElementById('content-container');
        const sidebar = document.getElementById('sidebar');
        const menuToggle = document.getElementById('menu-toggle');
        const sidebarBackdrop = document.getElementById('sidebar-backdrop');
        let activeSectionId = handbookContent[0].id; // 預設顯示第一個章節

        // --- 輔助函數：生成 Tailwind Class ---
        function getButtonClasses(color, isActive) {
            // 基礎樣式：更圓潤、有彈性、帶有陰影
            const base = 'w-full text-left py-3 px-4 rounded-3xl font-semibold transition-all duration-300 transform flex items-center space-x-3 shadow-md hover:shadow-lg hover:scale-[1.01] active:scale-[0.98]';
            
            // 懸停和非活躍狀態
            const hover = `hover:bg-${color}-100 hover:text-${color}-800`;
            const inactive = 'text-gray-600 bg-white border border-gray-100';

            // 活躍狀態：更明顯的顏色和邊框
            const active = `bg-${color}-200 text-${color}-900 shadow-xl ring-4 ring-${color}-300 scale-[1.02]`;
            
            return `${base} ${isActive ? active : inactive} ${hover}`;
        }

        // --- 渲染導航列表 ---
        function renderNav() {
            navList.innerHTML = '';
            handbookContent.forEach(section => {
                const isActive = section.id === activeSectionId;
                const button = document.createElement('button');
                button.className = getButtonClasses(section.color, isActive);
                button.setAttribute('data-id', section.id);
                button.innerHTML = `<span class="text-2xl">${section.icon}</span><span>${section.title}</span>`;
                
                button.addEventListener('click', () => {
                    setActiveSection(section.id);
                    // 在手機版點擊後關閉側邊欄
                    if (window.innerWidth < 1024) {
                        toggleSidebar(false);
                    }
                });

                navList.appendChild(button);
            });
        }

        // --- 渲染內容區塊 ---
        function renderContent() {
            const section = handbookContent.find(s => s.id === activeSectionId);

            if (!section) {
                contentContainer.innerHTML = `<div class="text-center py-20 text-gray-500">找不到內容。</div>`;
                return;
            }

            // 調整標題樣式，更突出
            let html = `<h2 class="text-5xl font-extrabold mb-8 text-${section.color}-700 border-b-8 border-${section.color}-300/50 pb-4">${section.title}</h2>`;

            section.subsections.forEach(sub => {
                html += `
                    <div class="mb-8 p-6 bg-white rounded-2xl shadow-lg border-l-4 border-${section.color}-400 transition-all duration-300 hover:shadow-xl hover:scale-[1.005]">
                        <h3 class="text-2xl font-bold mb-4 text-${section.color}-600">${sub.title}</h3>
                        <div class="text-gray-700 leading-relaxed">${sub.content}</div>
                    </div>
                `;
            });

            // 結尾備註
            if (section.isCritical) {
                html += `
                    <div class="mt-10 p-6 bg-${section.color}-50 text-${section.color}-800 border-l-8 border-${section.color}-500 rounded-lg shadow-xl">
                        <p class="text-2xl font-bold mb-2">💡 核心提醒：</p>
                        <p>第三章節是服務品質的核心。所有同仁應定期複習並確保遵循每一項 S.O.P.，以提供最高標準的專業照護。</p>
                    </div>
                `;
            }

            contentContainer.innerHTML = html;
        }

        // --- 設置活躍章節並重新渲染 ---
        function setActiveSection(id) {
            if (activeSectionId !== id) {
                activeSectionId = id;
                renderNav();
                renderContent();
                contentContainer.scrollTop = 0; // 滾動到頂部
            }
        }

        // --- 側邊欄開關 (行動版) ---
        function toggleSidebar(isVisible) {
            const isCurrentlyVisible = !sidebar.classList.contains('sidebar-mobile-hidden');

            if (typeof isVisible === 'boolean') {
                if (isVisible === isCurrentlyVisible) return;
            } else {
                isVisible = !isCurrentlyVisible;
            }

            if (isVisible) {
                sidebar.classList.remove('sidebar-mobile-hidden');
                sidebarBackdrop.classList.remove('hidden');
                document.body.style.overflow = 'hidden'; // 防止背景滾動
            } else {
                sidebar.classList.add('sidebar-mobile-hidden');
                sidebarBackdrop.classList.add('hidden');
                document.body.style.overflow = '';
            }
        }

        // --- 事件監聽器 ---
        menuToggle.addEventListener('click', () => toggleSidebar());
        sidebarBackdrop.addEventListener('click', () => toggleSidebar(false));

        // --- 初始化應用程式 ---
        function initApp() {
            renderNav();
            renderContent();
        }

        // 頁面載入完成後執行初始化
        window.onload = initApp;

        // 窗口大小改變時，調整行動版側邊欄狀態
        window.onresize = () => {
            if (window.innerWidth >= 1024) {
                // 在桌面版強制移除行動版樣式
                sidebar.classList.remove('sidebar-mobile-hidden');
                sidebarBackdrop.classList.add('hidden');
                document.body.style.overflow = '';
            } else {
                // 如果是行動版且側邊欄應該隱藏，則確保隱藏
                if (sidebar.classList.contains('sidebar-mobile-hidden')) {
                     sidebarBackdrop.classList.add('hidden');
                     document.body.style.overflow = '';
                }
            }
        };

    </script>
</body>
</html>
