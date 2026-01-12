<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>挑码助手</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: "PingFang SC", "Microsoft YaHei", sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4efe9 100%);
            color: #333;
            line-height: 1.6;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        header {
            background: linear-gradient(135deg, #ff6b6b 0%, #ffa726 100%);
            color: white;
            text-align: center;
            padding: 25px 20px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        h1 {
            font-size: 2.2rem;
            margin-bottom: 10px;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.2);
        }
        
        .subtitle {
            font-size: 1.1rem;
            opacity: 0.9;
        }
        
        .main-content {
            padding: 20px;
        }
        
        .panel {
            background: #f9f9f9;
            border-radius: 10px;
            padding: 20px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
            margin-bottom: 20px;
        }
        
        .panel-title {
            font-size: 1.3rem;
            color: #2c3e50;
            margin-bottom: 15px;
            padding-bottom: 10px;
            border-bottom: 2px solid #ffa726;
        }
        
        /* 尾号筛选区样式 */
        .tail-filter {
            margin-bottom: 20px;
        }
        
        .tail-buttons {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .tail-btn {
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            background: #dfe6e9;
            color: #333;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }
        
        .tail-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .tail-btn.active {
            background: linear-gradient(135deg, #fdcb6e, #e17055);
            color: white;
            transform: scale(1.05);
        }
        
        /* 尾号结果显示区 */
        .tail-result {
            background: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            margin-top: 15px;
        }
        
        .tail-result-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: #2c3e50;
            text-align: center;
        }
        
        .tail-numbers {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 8px;
        }
        
        /* 数字球样式 - 调整大小 */
        .tail-number {
            width: 100%;
            aspect-ratio: 1;
            border-radius: 50%;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: white;
            cursor: pointer;
            position: relative;
            overflow: hidden;
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.2);
            border: 2px solid rgba(255, 255, 255, 0.3);
        }
        
        /* 颜色加深 */
        .tail-number.red {
            background: linear-gradient(135deg, #e53935, #c62828);
        }
        
        .tail-number.green {
            background: linear-gradient(135deg, #00897b, #00695c);
        }
        
        .tail-number.blue {
            background: linear-gradient(135deg, #1565c0, #0d47a1);
        }
        
        /* 调整字体大小 */
        .tail-number .number {
            font-size: 1.2rem; /* 调整为1.2rem */
            font-weight: 900;
            z-index: 2;
            position: relative;
            color: white;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
        }
        
        .tail-number .zodiac {
            font-size: 0.9rem; /* 调整为0.9rem */
            font-weight: 600;
            z-index: 2;
            position: relative;
            color: white;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
            margin-top: 2px;
        }
        
        /* 选中状态 */
        .tail-number.selected {
            opacity: 0.8;
            transform: scale(0.9);
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.3);
        }
        
        .tail-number.killed {
            opacity: 0.5;
            transform: scale(0.85);
            background: #757575 !important;
        }
        
        /* 列表区 - 移动到显示号码下方 */
        .number-lists {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-top: 15px;
        }
        
        .list-box {
            background: white;
            border-radius: 10px;
            padding: 15px;
            box-shadow: 0 3px 10px rgba(0, 0, 0, 0.08);
        }
        
        .list-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 10px;
            text-align: center;
            padding-bottom: 5px;
            border-bottom: 1px solid #eee;
        }
        
        .selected-list .list-title {
            color: #00897b;
        }
        
        .killed-list .list-title {
            color: #e53935;
        }
        
        .list-items {
            display: flex;
            flex-wrap: wrap;
            gap: 5px;
            min-height: 100px;
        }
        
        .list-number {
            width: 35px; /* 调整为35px */
            height: 35px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 14px; /* 调整为14px */
            font-weight: bold;
            color: white;
            cursor: pointer;
        }
        
        .selected-list .list-number {
            background: #00897b;
        }
        
        .killed-list .list-number {
            background: #757575;
        }
        
        .empty-message {
            color: #999;
            font-style: italic;
            text-align: center;
            width: 100%;
            margin-top: 20px;
        }
        
        /* 其他分类筛选区 */
        .category-section {
            margin-bottom: 20px;
        }
        
        .section-title {
            font-size: 1.1rem;
            font-weight: 600;
            margin-bottom: 10px;
            color: #2c3e50;
        }
        
        .category-buttons {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 8px;
        }
        
        .category-buttons.head-buttons {
            grid-template-columns: repeat(5, 1fr);
        }
        
        .category-buttons.property-buttons {
            grid-template-columns: repeat(7, 1fr);
        }
        
        .category-btn {
            padding: 10px 5px;
            border: none;
            border-radius: 6px;
            font-size: 14px;
            background: #dfe6e9;
            color: #333;
            cursor: pointer;
            transition: all 0.2s ease;
            text-align: center;
            min-width: 0;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
        }
        
        .category-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .category-btn:active {
            transform: scale(0.98);
        }
        
        .category-btn.red {
            background: linear-gradient(135deg, #e53935, #c62828);
            color: white;
        }
        
        .category-btn.green {
            background: linear-gradient(135deg, #00897b, #00695c);
            color: white;
        }
        
        .category-btn.blue {
            background: linear-gradient(135deg, #1565c0, #0d47a1);
            color: white;
        }
        
        .category-btn.active {
            background: linear-gradient(135deg, #fdcb6e, #e17055);
            color: white;
            transform: scale(1.05);
        }
        
        /* 控制按钮 */
        .control-buttons {
            display: flex;
            gap: 10px;
            margin: 20px 0;
        }
        
        .control-btn {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
        }
        
        .clear-btn {
            background: linear-gradient(135deg, #e53935, #c62828);
            color: white;
        }
        
        .copy-btn {
            background: linear-gradient(135deg, #1565c0, #0d47a1);
            color: white;
        }
        
        .share-btn {
            background: linear-gradient(135deg, #00897b, #00695c);
            color: white;
        }
        
        .control-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
        }
        
        .control-btn:active {
            transform: translateY(0);
        }
        
        footer {
            text-align: center;
            padding: 20px;
            background: #2c3e50;
            color: #ecf0f1;
            margin-top: 20px;
        }
        
        @media (max-width: 768px) {
            .tail-buttons {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .tail-numbers {
                grid-template-columns: repeat(4, 1fr);
            }
            
            .category-buttons {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .category-buttons.head-buttons {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .category-buttons.property-buttons {
                grid-template-columns: repeat(4, 1fr);
            }
            
            .number-lists {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>挑码助手</h1>
            <p class="subtitle">智能选号，轻松挑码</p>
        </header>
        
        <div class="main-content">
            <!-- 尾号筛选区 -->
            <div class="panel">
                <div class="panel-title">尾号筛选</div>
                
                <div class="tail-filter">
                    <div class="tail-buttons" id="tailButtons">
                        <!-- 尾号按钮将通过JavaScript动态生成 -->
                    </div>
                    
                    <div class="tail-result" id="tailResult" style="display: none;">
                        <div class="tail-result-title">尾号 <span id="selectedTail">X</span> 相关数字</div>
                        <div class="tail-numbers" id="tailNumbers">
                            <!-- 尾号相关数字将通过JavaScript动态生成 -->
                        </div>
                        
                        <!-- 已选和已杀号码列表 - 移动到显示号码下方 -->
                        <div class="number-lists">
                            <div class="list-box selected-list">
                                <div class="list-title">已选号码</div>
                                <div class="list-items" id="selectedList">
                                    <div class="empty-message">暂无已选号码</div>
                                </div>
                            </div>
                            
                            <div class="list-box killed-list">
                                <div class="list-title">已杀号码</div>
                                <div class="list-items" id="killedList">
                                    <div class="empty-message">暂无已杀号码</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            
            <!-- 其他分类筛选区 -->
            <div class="panel">
                <div class="panel-title">其他筛选</div>
                
                <div class="category-section">
                    <div class="section-title">十二生肖</div>
                    <div class="category-buttons" id="zodiacButtons">
                        <!-- 生肖按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">头号</div>
                    <div class="category-buttons head-buttons" id="headButtons">
                        <!-- 头号按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
                
                <div class="category-section">
                    <div class="section-title">波色与属性</div>
                    <div class="category-buttons property-buttons" id="propertyButtons">
                        <!-- 属性按钮将通过JavaScript动态生成 -->
                    </div>
                </div>
            </div>
            
            <!-- 控制按钮 -->
            <div class="control-buttons">
                <button class="control-btn clear-btn" id="clearBtn">清空选择</button>
                <button class="control-btn copy-btn" id="copyBtn">复制结果</button>
                <button class="control-btn share-btn" id="shareBtn">分享结果</button>
            </div>
        </div>
        
        <footer>
            <p>挑码助手 &copy; 2023 - 专业选号工具</p>
        </footer>
    </div>

    <script>
        // 数字数据
        const numbersData = Array.from({length: 49}, (_, i) => {
            const num = i + 1;
            let color = 'blue';
            if ([1, 2, 7, 8, 12, 13, 18, 19, 23, 24, 29, 30, 34, 35, 40, 45, 46].includes(num)) color = 'red';
            if ([5, 6, 11, 16, 17, 21, 22, 27, 28, 32, 33, 38, 39, 43, 44, 49].includes(num)) color = 'green';
            
            const zodiacs = {
                '蛇': [1, 13, 25, 37, 49],
                '龙': [2, 14, 26, 38],
                '兔': [3, 15, 27, 39],
                '虎': [4, 16, 28, 40],
                '牛': [5, 17, 29, 41],
                '鼠': [6, 18, 30, 42],
                '猪': [7, 19, 31, 43],
                '狗': [8, 20, 32, 44],
                '鸡': [9, 21, 33, 45],
                '猴': [10, 22, 34, 46],
                '羊': [11, 23, 35, 47],
                '马': [12, 24, 36, 48]
            };
            
            let zodiac = '';
            for (const [zodiacName, numbers] of Object.entries(zodiacs)) {
                if (numbers.includes(num)) {
                    zodiac = zodiacName;
                    break;
                }
            }
            
            return {num, color, zodiac};
        });

        // 分类数据
        const categories = {
            zodiac: ['鼠', '牛', '虎', '兔', '龙', '蛇', '马', '羊', '猴', '鸡', '狗', '猪'],
            tail: ['0尾', '1尾', '2尾', '3尾', '4尾', '5尾', '6尾', '7尾', '8尾', '9尾'],
            head: ['0头', '1头', '2头', '3头', '4头'],
            property: ['红波', '绿波', '蓝波', '大', '小', '单', '双']
        };

        // 状态管理
        let selectedNumbers = [];
        let killedNumbers = [];
        let selectedTails = []; // 存储选中的尾号
        
        // 初始化函数
        function init() {
            renderTailButtons();
            renderCategoryButtons();
            setupEventListeners();
        }
        
        // 渲染尾号按钮
        function renderTailButtons() {
            const tailButtons = document.getElementById('tailButtons');
            tailButtons.innerHTML = '';
            
            categories.tail.forEach(tail => {
                const button = document.createElement('button');
                button.className = 'tail-btn';
                button.textContent = tail;
                button.dataset.tail = tail.replace('尾', '');
                tailButtons.appendChild(button);
            });
        }
        
        // 渲染分类按钮
        function renderCategoryButtons() {
            // 生肖按钮
            const zodiacButtons = document.getElementById('zodiacButtons');
            categories.zodiac.forEach(zodiac => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                button.textContent = zodiac;
                button.dataset.category = 'zodiac';
                button.dataset.value = zodiac;
                zodiacButtons.appendChild(button);
            });
            
            // 头号按钮
            const headButtons = document.getElementById('headButtons');
            categories.head.forEach(head => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                button.textContent = head;
                button.dataset.category = 'head';
                button.dataset.value = head;
                headButtons.appendChild(button);
            });
            
            // 属性按钮
            const propertyButtons = document.getElementById('propertyButtons');
            categories.property.forEach(property => {
                const button = document.createElement('button');
                button.className = 'category-btn';
                
                if (property === '红波') button.classList.add('red');
                else if (property === '绿波') button.classList.add('green');
                else if (property === '蓝波') button.classList.add('blue');
                
                button.textContent = property;
                button.dataset.category = 'property';
                button.dataset.value = property;
                propertyButtons.appendChild(button);
            });
        }
        
        // 设置事件监听器
        function setupEventListeners() {
            // 尾号按钮点击事件 - 支持多选
            document.getElementById('tailButtons').addEventListener('click', function(e) {
                const tailBtn = e.target.closest('.tail-btn');
                if (!tailBtn) return;
                
                const tailNum = parseInt(tailBtn.dataset.tail);
                
                // 切换按钮状态
                if (tailBtn.classList.contains('active')) {
                    tailBtn.classList.remove('active');
                    selectedTails = selectedTails.filter(t => t !== tailNum);
                } else {
                    tailBtn.classList.add('active');
                    selectedTails.push(tailNum);
                }
                
                // 显示尾号结果区域
                const tailResult = document.getElementById('tailResult');
                tailResult.style.display = selectedTails.length > 0 ? 'block' : 'none';
                
                // 更新标题
                document.getElementById('selectedTail').textContent = selectedTails.join(', ');
                
                // 渲染相关数字
                renderTailNumbers();
            });
            
            // 尾号数字点击事件 - 支持双击
            document.getElementById('tailNumbers').addEventListener('click', function(e) {
                const tailNumber = e.target.closest('.tail-number');
                if (!tailNumber) return;
                
                const number = parseInt(tailNumber.dataset.number);
                const timerId = `number-${number}`;
                
                if (clickTimers[timerId]) {
                    clearTimeout(clickTimers[timerId]);
                    handleDoubleClick(number, tailNumber);
                    delete clickTimers[timerId];
                } else {
                    clickTimers[timerId] = setTimeout(() => {
                        handleSingleClick(number, tailNumber);
                        delete clickTimers[timerId];
                    }, 300);
                }
            });
            
            // 分类按钮点击事件
            document.querySelectorAll('.category-btn').forEach(button => {
                button.addEventListener('click', function() {
                    const numbers = getNumbersByCategory(button.dataset.category, button.dataset.value);
                    
                    button.classList.remove('killed');
                    
                    if (button.classList.contains('active')) {
                        numbers.forEach(num => {
                            selectedNumbers = selectedNumbers.filter(n => n !== num);
                            killedNumbers = killedNumbers.filter(n => n !== num);
                        });
                        button.classList.remove('active');
                    } else {
                        numbers.forEach(num => {
                            if (!selectedNumbers.includes(num)) selectedNumbers.push(num);
                            killedNumbers = killedNumbers.filter(n => n !== num);
                        });
                        button.classList.add('active');
                    }
                    
                    updateLists();
                });
            });
            
            // 控制按钮事件
            document.getElementById('clearBtn').addEventListener('click', clearAll);
            document.getElementById('copyBtn').addEventListener('click', copyResults);
            document.getElementById('shareBtn').addEventListener('click', shareResults);
            
            // 列表项点击事件（从列表中移除）
            document.getElementById('selectedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (listNumber) removeFromList(listNumber, 'selected');
            });
            
            document.getElementById('killedList').addEventListener('click', function(e) {
                const listNumber = e.target.closest('.list-number');
                if (listNumber) removeFromList(listNumber, 'killed');
            });
        }
        
        // 处理数字单击
        function handleSingleClick(number, numberBall) {
            if (killedNumbers.includes(number)) {
                killedNumbers = killedNumbers.filter(n => n !== number);
                numberBall.classList.remove('killed');
            }
            
            const index = selectedNumbers.indexOf(number);
            if (index === -1) {
                selectedNumbers.push(number);
                numberBall.classList.add('selected');
            } else {
                selectedNumbers.splice(index, 1);
                numberBall.classList.remove('selected');
            }
            
            updateLists();
        }
        
        // 处理数字双击
        function handleDoubleClick(number, numberBall) {
            if (selectedNumbers.includes(number)) {
                selectedNumbers = selectedNumbers.filter(n => n !== number);
                numberBall.classList.remove('selected');
            }
            
            const index = killedNumbers.indexOf(number);
            if (index === -1) {
                killedNumbers.push(number);
                numberBall.classList.add('killed');
            } else {
                killedNumbers.splice(index, 1);
                numberBall.classList.remove('killed');
            }
            
            updateLists();
        }
        
        // 渲染尾号相关数字
        function renderTailNumbers() {
            const tailNumbers = document.getElementById('tailNumbers');
            tailNumbers.innerHTML = '';
            
            // 获取所有选中尾号对应的数字
            let numbers = [];
            selectedTails.forEach(tailNum => {
                numbers = numbers.concat(numbersData.filter(data => data.num % 10 === tailNum));
            });
            
            // 去重
            const uniqueNumbers = [...new Set(numbers.map(n => n.num))].map(num => {
                return numbersData.find(data => data.num === num);
            });
            
            // 按数字排序
            uniqueNumbers.sort((a, b) => a.num - b.num);
            
            uniqueNumbers.forEach(data => {
                const numberBall = document.createElement('div');
                numberBall.className = `tail-number ${data.color}`;
                numberBall.dataset.number = data.num;
                
                const numberElement = document.createElement('div');
                numberElement.className = 'number';
                numberElement.textContent = data.num;
                numberBall.appendChild(numberElement);
                
                const zodiacLabel = document.createElement('div');
                zodiacLabel.className = 'zodiac';
                zodiacLabel.textContent = data.zodiac;
                numberBall.appendChild(zodiacLabel);
                
                // 设置选中状态
                if (selectedNumbers.includes(data.num)) {
                    numberBall.classList.add('selected');
                } else if (killedNumbers.includes(data.num)) {
                    numberBall.classList.add('killed');
                }
                
                tailNumbers.appendChild(numberBall);
            });
        }
        
        // 根据分类获取数字
        function getNumbersByCategory(category, value) {
            switch(category) {
                case 'zodiac':
                    return numbersData
                        .filter(data => data.zodiac === value)
                        .map(data => data.num);
                case 'head':
                    const headNum = parseInt(value);
                    return numbersData
                        .filter(data => Math.floor(data.num / 10) === headNum)
                        .map(data => data.num);
                case 'property':
                    switch(value) {
                        case '红波': return numbersData.filter(d => d.color === 'red').map(d => d.num);
                        case '绿波': return numbersData.filter(d => d.color === 'green').map(d => d.num);
                        case '蓝波': return numbersData.filter(d => d.color === 'blue').map(d => d.num);
                        case '大': return numbersData.filter(d => d.num >= 25).map(d => d.num);
                        case '小': return numbersData.filter(d => d.num < 25).map(d => d.num);
                        case '单': return numbersData.filter(d => d.num % 2 === 1).map(d => d.num);
                        case '双': return numbersData.filter(d => d.num % 2 === 0).map(d => d.num);
                        default: return [];
                    }
                default: return [];
            }
        }
        
        // 更新列表显示
        function updateLists() {
            const selectedList = document.getElementById('selectedList');
            const killedList = document.getElementById('killedList');
            
            // 更新已选列表
            selectedList.innerHTML = '';
            if (selectedNumbers.length === 0) {
                selectedList.innerHTML = '<div class="empty-message">暂无已选号码</div>';
            } else {
                selectedNumbers.sort((a, b) => a - b).forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    
                    // 保持数字原来的颜色
                    const originalData = numbersData.find(d => d.num === num);
                    if (originalData) {
                        listNumber.style.backgroundColor = 
                            originalData.color === 'red' ? '#e53935' : 
                            originalData.color === 'green' ? '#00897b' : '#1565c0';
                    }
                    
                    selectedList.appendChild(listNumber);
                });
            }
            
            // 更新已杀列表
            killedList.innerHTML = '';
            if (killedNumbers.length === 0) {
                killedList.innerHTML = '<div class="empty-message">暂无已杀号码</div>';
            } else {
                killedNumbers.sort((a, b) => a - b).forEach(num => {
                    const listNumber = document.createElement('div');
                    listNumber.className = 'list-number';
                    listNumber.textContent = num;
                    listNumber.dataset.number = num;
                    killedList.appendChild(listNumber);
                });
            }
        }
        
        // 从列表中移除数字
        function removeFromList(listNumber, listType) {
            const num = parseInt(listNumber.dataset.number);
            
            if (listType === 'selected') {
                selectedNumbers = selectedNumbers.filter(n => n !== num);
            } else if (listType === 'killed') {
                killedNumbers = killedNumbers.filter(n => n !== num);
            }
            
            updateLists();
        }
        
        // 清空所有
        function clearAll() {
            selectedNumbers = [];
            killedNumbers = [];
            selectedTails = [];
            
            // 重置按钮状态
            document.querySelectorAll('.tail-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            document.querySelectorAll('.category-btn').forEach(btn => {
                btn.classList.remove('active', 'killed');
            });
            
            // 隐藏尾号结果区域
            document.getElementById('tailResult').style.display = 'none';
            
            updateLists();
        }
        
        // 复制结果
        function copyResults() {
            const text = `已选: ${selectedNumbers.sort((a,b)=>a-b).join(', ') || '无'}\n已杀: ${killedNumbers.sort((a,b)=>a-b).join(', ') || '无'}`;
            
            navigator.clipboard.writeText(text).then(() => {
                alert('结果已复制到剪贴板');
            }).catch(err => {
                console.error('复制失败:', err);
                alert('复制失败，请手动复制');
            });
        }
        
        // 分享结果
        function shareResults() {
            const text = `已选: ${selectedNumbers.sort((a,b)=>a-b).join(', ') || '无'}\n已杀: ${killedNumbers.sort((a,b)=>a-b).join(', ') || '无'}`;
            
            if (navigator.share) {
                navigator.share({
                    title: '挑码助手结果',
                    text: text
                }).catch(err => {
                    console.error('分享失败:', err);
                    alert('分享失败，请手动复制结果');
                });
            } else {
                copyResults();
            }
        }
        
        // 初始化应用
        const clickTimers = {};
        document.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
