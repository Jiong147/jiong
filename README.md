<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>平行四边形不稳定性探究工具</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --primary-color: #4361ee;
            --primary-light: #4895ef;
            --secondary-color: #3a0ca3;
            --success-color: #4cc9f0;
            --danger-color: #f72585;
            --warning-color: #f8961e;
            --light-color: #f8f9fa;
            --dark-color: #212529;
            --gray-color: #6c757d;
            --border-radius: 12px;
            --box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
            --transition: all 0.3s ease;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'Microsoft YaHei', sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #e4e8f0 100%);
            color: var(--dark-color);
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1400px;
            margin: 0 auto;
        }
        
        /* 头部样式优化 */
        .app-header {
            text-align: center;
            margin-bottom: 30px;
            padding: 25px;
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            position: relative;
            overflow: hidden;
        }
        
        .header-gradient {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 5px;
            background: linear-gradient(90deg, var(--primary-color), var(--success-color), var(--danger-color));
        }
        
        .header-content {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
            flex-wrap: wrap;
        }
        
        .logo {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, var(--primary-color), var(--secondary-color));
            color: white;
            border-radius: 50%;
            font-size: 32px;
            box-shadow: 0 6px 12px rgba(67, 97, 238, 0.3);
        }
        
        .title-section h1 {
            font-size: 2.3rem;
            color: var(--primary-color);
            margin-bottom: 8px;
            background: linear-gradient(90deg, var(--primary-color), var(--secondary-color));
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        
        .title-section p {
            font-size: 1.1rem;
            color: var(--gray-color);
            max-width: 800px;
            line-height: 1.6;
        }
        
        /* 主体内容布局优化 */
        .main-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 25px;
            margin-bottom: 25px;
        }
        
        @media (max-width: 1100px) {
            .main-content {
                grid-template-columns: 1fr;
            }
        }
        
        /* 卡片样式优化 */
        .shape-card {
            background-color: white;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
            overflow: hidden;
            transition: var(--transition);
            height: fit-content;
        }
        
        .shape-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 12px 28px rgba(0, 0, 0, 0.15);
        }
        
        .card-header {
            padding: 18px 25px;
            background-color: var(--light-color);
            border-bottom: 1px solid rgba(0, 0, 0, 0.05);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .card-title {
            display: flex;
            align-items: center;
            gap: 12px;
            font-size: 1.3rem;
            font-weight: 600;
            color: var(--dark-color);
        }
        
        .card-icon {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 40px;
            height: 40px;
            background: linear-gradient(135deg, var(--primary-color), var(--primary-light));
            color: white;
            border-radius: 10px;
        }
        
        .status-badge {
            padding: 6px 15px;
            border-radius: 50px;
            font-size: 0.9rem;
            font-weight: 600;
        }
        
        .status-stable {
            background-color: rgba(76, 201, 240, 0.15);
            color: var(--success-color);
        }
        
        .status-unstable {
            background-color: rgba(248, 37, 133, 0.15);
            color: var(--danger-color);
        }
        
        /* Canvas区域样式优化 */
        .canvas-container {
            padding: 20px;
            position: relative;
        }
        
        .canvas-wrapper {
            width: 100%;
            height: 320px;
            border-radius: 8px;
            overflow: hidden;
            border: 1px solid #eaeaea;
            background-color: #fdfdfd;
            position: relative;
        }
        
        canvas {
            display: block;
            cursor: pointer;
            width: 100%;
            height: 100%;
        }
        
        .canvas-hint {
            position: absolute;
            bottom: 15px;
            left: 0;
            width: 100%;
            text-align: center;
            color: var(--gray-color);
            font-size: 0.9rem;
            padding: 0 15px;
            z-index: 5;
        }
        
        .canvas-hint i {
            margin-right: 6px;
            color: var(--primary-color);
        }
        
        /* 属性面板样式优化 */
        .properties-panel {
            padding: 20px;
        }
        
        .property-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .property-card {
            padding: 18px;
            border-radius: 10px;
            background-color: var(--light-color);
            border: 1px solid rgba(0, 0, 0, 0.05);
            transition: var(--transition);
            cursor: default;
            text-align: center;
        }
        
        .property-card:hover {
            border-color: var(--primary-light);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }
        
        .property-title {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            margin-bottom: 10px;
            font-size: 0.95rem;
            color: var(--gray-color);
        }
        
        .property-icon {
            color: var(--primary-color);
        }
        
        .property-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--primary-color);
            margin-bottom: 5px;
        }
        
        .property-unit {
            font-size: 0.85rem;
            color: var(--gray-color);
        }
        
        /* 控制面板样式优化 */
        .control-panel-card {
            grid-column: 1 / -1;
            padding: 25px;
            margin-top: 10px;
        }
        
        .control-title {
            font-size: 1.3rem;
            margin-bottom: 20px;
            color: var(--dark-color);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .control-buttons {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
        }
        
        .btn {
            padding: 14px 20px;
            border-radius: 10px;
            border: none;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            transition: var(--transition);
        }
        
        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: var(--secondary-color);
        }
        
        .btn-secondary {
            background-color: var(--light-color);
            color: var(--dark-color);
            border: 1px solid #dee2e6;
        }
        
        .btn-secondary:hover {
            background-color: #e9ecef;
        }
        
        .btn-success {
            background-color: var(--success-color);
            color: white;
        }
        
        .btn-success:hover {
            background-color: #3ab0d6;
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #e11570;
        }
        
        .btn-warning {
            background-color: var(--warning-color);
            color: white;
        }
        
        .btn-warning:hover {
            background-color: #e6891e;
        }
        
        /* 尺寸设置面板 */
        .size-control-panel {
            background-color: rgba(67, 97, 238, 0.05);
            border-radius: 10px;
            padding: 20px;
            margin-top: 20px;
        }
        
        .size-control-title {
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: var(--dark-color);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .size-control-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }
        
        @media (max-width: 768px) {
            .size-control-grid {
                grid-template-columns: 1fr;
            }
        }
        
        .size-input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .size-label {
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 600;
            color: var(--gray-color);
            font-size: 0.95rem;
        }
        
        .size-label i {
            color: var(--primary-color);
        }
        
        .size-input-container {
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .size-input {
            flex: 1;
            padding: 10px 15px;
            border-radius: 8px;
            border: 1px solid #ced4da;
            font-size: 1rem;
            transition: var(--transition);
        }
        
        .size-input:focus {
            outline: none;
            border-color: var(--primary-color);
            box-shadow: 0 0 0 3px rgba(67, 97, 238, 0.1);
        }
        
        .size-unit {
            color: var(--gray-color);
            font-size: 0.9rem;
            min-width: 40px;
        }
        
        /* 角度调节器样式 */
        .angle-control-container {
            margin-top: 25px;
        }
        
        .angle-control-title {
            font-size: 1.1rem;
            margin-bottom: 15px;
            color: var(--dark-color);
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .angle-slider-container {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 15px;
        }
        
        .angle-slider {
            flex: 1;
            height: 10px;
            -webkit-appearance: none;
            appearance: none;
            background: linear-gradient(90deg, var(--danger-color), var(--warning-color), var(--success-color));
            border-radius: 5px;
            outline: none;
        }
        
        .angle-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: var(--primary-color);
            cursor: pointer;
            border: 3px solid white;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
        }
        
        .angle-slider::-moz-range-thumb {
            width: 24px;
            height: 24px;
            border-radius: 50%;
            background: var(--primary-color);
            cursor: pointer;
            border: 3px solid white;
            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
        }
        
        .angle-value {
            font-size: 1.2rem;
            font-weight: 700;
            color: var(--primary-color);
            min-width: 50px;
            text-align: center;
        }
        
        .angle-buttons {
            display: flex;
            gap: 10px;
            margin-top: 15px;
        }
        
        .angle-btn {
            padding: 10px 15px;
            border-radius: 8px;
            border: none;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            transition: var(--transition);
            flex: 1;
        }
        
        .angle-btn-small {
            padding: 8px 12px;
            font-size: 0.9rem;
        }
        
        /* 信息面板样式优化 */
        .info-panel-card {
            grid-column: 1 / -1;
            margin-top: 20px;
        }
        
        .info-tabs {
            display: flex;
            border-bottom: 1px solid #eaeaea;
            margin-bottom: 0;
            overflow-x: auto;
            scrollbar-width: none;
        }
        
        .info-tabs::-webkit-scrollbar {
            display: none;
        }
        
        .tab-btn {
            padding: 15px 25px;
            background: none;
            border: none;
            font-size: 1.05rem;
            font-weight: 600;
            color: var(--gray-color);
            cursor: pointer;
            position: relative;
            transition: var(--transition);
            white-space: nowrap;
            flex-shrink: 0;
        }
        
        .tab-btn:hover {
            color: var(--primary-color);
            background-color: rgba(67, 97, 238, 0.05);
        }
        
        .tab-btn.active {
            color: var(--primary-color);
        }
        
        .tab-btn.active::after {
            content: '';
            position: absolute;
            bottom: -1px;
            left: 0;
            width: 100%;
            height: 3px;
            background-color: var(--primary-color);
            border-radius: 3px 3px 0 0;
        }
        
        .tab-content {
            padding: 25px;
        }
        
        .tab-pane {
            display: none;
            animation: fadeIn 0.5s ease-out;
        }
        
        .tab-pane.active {
            display: block;
        }
        
        .info-content h3 {
            font-size: 1.3rem;
            margin-bottom: 15px;
            color: var(--dark-color);
        }
        
        .info-content p {
            line-height: 1.7;
            margin-bottom: 15px;
            color: var(--gray-color);
        }
        
        .info-content ul {
            padding-left: 20px;
            margin-bottom: 20px;
        }
        
        .info-content li {
            margin-bottom: 10px;
            line-height: 1.6;
        }
        
        .comparison-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
        }
        
        .comparison-table th {
            background-color: var(--primary-color);
            color: white;
            padding: 16px 15px;
            text-align: left;
            font-weight: 600;
        }
        
        .comparison-table td {
            padding: 14px 15px;
            border-bottom: 1px solid #eaeaea;
        }
        
        .comparison-table tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        
        .comparison-table tr:last-child td {
            border-bottom: none;
        }
        
        /* 底部样式 */
        footer {
            text-align: center;
            padding: 20px;
            color: var(--gray-color);
            font-size: 0.9rem;
            margin-top: 30px;
        }
        
        .footer-links {
            display: flex;
            justify-content: center;
            gap: 25px;
            margin-top: 15px;
        }
        
        .footer-link {
            color: var(--primary-color);
            text-decoration: none;
            transition: var(--transition);
        }
        
        .footer-link:hover {
            color: var(--secondary-color);
            text-decoration: underline;
        }
        
        /* 响应式设计优化 */
        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                text-align: center;
            }
            
            .title-section h1 {
                font-size: 1.8rem;
            }
            
            .property-grid {
                grid-template-columns: 1fr;
            }
            
            .control-buttons {
                grid-template-columns: 1fr;
            }
            
            .info-tabs {
                flex-wrap: nowrap;
                overflow-x: auto;
            }
            
            .tab-btn {
                padding: 12px 15px;
                font-size: 0.95rem;
            }
            
            .canvas-wrapper {
                height: 280px;
            }
        }
        
        /* 动画效果 */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .fade-in {
            animation: fadeIn 0.5s ease-out;
        }
        
        /* 操作提示 */
        .operation-tip {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            background-color: rgba(67, 97, 238, 0.1);
            border-radius: 8px;
            padding: 12px 20px;
            margin-top: 15px;
            color: var(--primary-color);
            font-weight: 500;
        }
        
        .operation-tip i {
            font-size: 1.2rem;
        }
        
        /* 自适应比例指示器 */
        .scale-indicator {
            position: absolute;
            top: 10px;
            right: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 0.8rem;
            z-index: 10;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- 头部 -->
        <header class="app-header fade-in">
            <div class="header-gradient"></div>
            <div class="header-content">
                <div class="logo">
                    <i class="fas fa-shapes"></i>
                </div>
                <div class="title-section">
                    <h1>平行四边形不稳定性探究工具</h1>
                    <p>通过交互式操作理解平行四边形的不稳定性。拖动平行四边形顶点，观察形状变化，并与稳定结构进行对比。</p>
                </div>
            </div>
        </header>
        
        <!-- 主内容区 -->
        <div class="main-content">
            <!-- 长方形卡片 -->
            <div class="shape-card fade-in">
                <div class="card-header">
                    <div class="card-title">
                        <div class="card-icon">
                            <i class="fas fa-square"></i>
                        </div>
                        <span>长方形（稳定结构）</span>
                    </div>
                    <div class="status-badge status-stable">
                        <i class="fas fa-lock"></i> 稳定结构
                    </div>
                </div>
                <div class="canvas-container">
                    <div class="canvas-wrapper" id="rectCanvasWrapper">
                        <canvas id="rectangleCanvas"></canvas>
                        <div class="scale-indicator" id="rectScaleIndicator">比例: 100%</div>
                        <div class="canvas-hint">
                            <i class="fas fa-info-circle"></i> 长方形角度固定为90°，形状不可改变
                        </div>
                    </div>
                </div>
                <div class="properties-panel">
                    <div class="property-grid">
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-ruler property-icon"></i>
                                <span>长度</span>
                            </div>
                            <div class="property-value" id="rectLength">300</div>
                            <div class="property-unit">像素</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-ruler-vertical property-icon"></i>
                                <span>宽度</span>
                            </div>
                            <div class="property-value" id="rectWidth">180</div>
                            <div class="property-unit">像素</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-angle-right property-icon"></i>
                                <span>内角</span>
                            </div>
                            <div class="property-value">90</div>
                            <div class="property-unit">度</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-expand-alt property-icon"></i>
                                <span>面积</span>
                            </div>
                            <div class="property-value" id="rectArea">54000</div>
                            <div class="property-unit">平方像素</div>
                        </div>
                    </div>
                    <div class="operation-tip">
                        <i class="fas fa-lock"></i> 此形状为稳定结构，无法通过拖动改变
                    </div>
                </div>
            </div>
            
            <!-- 平行四边形卡片 -->
            <div class="shape-card fade-in">
                <div class="card-header">
                    <div class="card-title">
                        <div class="card-icon">
                            <i class="fas fa-vector-square"></i>
                        </div>
                        <span>平行四边形（不稳定结构）</span>
                    </div>
                    <div class="status-badge status-unstable">
                        <i class="fas fa-unlock"></i> 不稳定结构
                    </div>
                </div>
                <div class="canvas-container">
                    <div class="canvas-wrapper" id="paraCanvasWrapper">
                        <canvas id="parallelogramCanvas"></canvas>
                        <div class="scale-indicator" id="paraScaleIndicator">比例: 100%</div>
                        <div class="canvas-hint">
                            <i class="fas fa-mouse-pointer"></i> 拖动任意顶点改变形状，观察角度和面积变化
                        </div>
                    </div>
                </div>
                <div class="properties-panel">
                    <div class="property-grid">
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-ruler property-icon"></i>
                                <span>边长 a</span>
                            </div>
                            <div class="property-value" id="paraSideA">300</div>
                            <div class="property-unit">像素</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-ruler-vertical property-icon"></i>
                                <span>边长 b</span>
                            </div>
                            <div class="property-value" id="paraSideB">180</div>
                            <div class="property-unit">像素</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-angle-right property-icon"></i>
                                <span>内角 α</span>
                            </div>
                            <div class="property-value" id="paraAngle">90</div>
                            <div class="property-unit">度</div>
                        </div>
                        <div class="property-card">
                            <div class="property-title">
                                <i class="fas fa-expand-alt property-icon"></i>
                                <span>面积</span>
                            </div>
                            <div class="property-value" id="paraArea">54000</div>
                            <div class="property-unit">平方像素</div>
                        </div>
                    </div>
                    <div class="operation-tip">
                        <i class="fas fa-hand-point-up"></i> 点击并拖动任意顶点，或使用下方角度调节器
                    </div>
                </div>
            </div>
            
            <!-- 控制面板 -->
            <div class="shape-card control-panel-card fade-in">
                <h2 class="control-title">
                    <i class="fas fa-sliders-h"></i> 控制面板
                </h2>
                <div class="control-buttons">
                    <button class="btn btn-primary" id="resetBtn">
                        <i class="fas fa-redo"></i> 重置为长方形
                    </button>
                    <button class="btn btn-success" id="randomBtn">
                        <i class="fas fa-random"></i> 随机形状
                    </button>
                    <button class="btn btn-secondary" id="toggleGridBtn">
                        <i class="fas fa-th"></i> 切换网格
                    </button>
                    <button class="btn btn-warning" id="stepGuideBtn">
                        <i class="fas fa-graduation-cap"></i> 分步指导
                    </button>
                    <button class="btn btn-danger" id="showExtremeBtn">
                        <i class="fas fa-arrows-alt-h"></i> 极限情况
                    </button>
                </div>
                
                <!-- 尺寸设置面板 -->
                <div class="size-control-panel">
                    <h3 class="size-control-title">
                        <i class="fas fa-ruler-combined"></i> 尺寸设置
                    </h3>
                    <div class="size-control-grid">
                        <div class="size-input-group">
                            <div class="size-label">
                                <i class="fas fa-square"></i>
                                <span>长方形长度</span>
                            </div>
                            <div class="size-input-container">
                                <input type="number" min="50" max="500" value="300" class="size-input" id="rectLengthInput">
                                <span class="size-unit">px</span>
                            </div>
                        </div>
                        
                        <div class="size-input-group">
                            <div class="size-label">
                                <i class="fas fa-square"></i>
                                <span>长方形宽度</span>
                            </div>
                            <div class="size-input-container">
                                <input type="number" min="50" max="500" value="180" class="size-input" id="rectWidthInput">
                                <span class="size-unit">px</span>
                            </div>
                        </div>
                        
                        <div class="size-input-group">
                            <div class="size-label">
                                <i class="fas fa-vector-square"></i>
                                <span>平行四边形底边</span>
                            </div>
                            <div class="size-input-container">
                                <input type="number" min="50" max="500" value="300" class="size-input" id="paraBaseInput">
                                <span class="size-unit">px</span>
                            </div>
                        </div>
                        
                        <div class="size-input-group">
                            <div class="size-label">
                                <i class="fas fa-vector-square"></i>
                                <span>平行四边形高</span>
                            </div>
                            <div class="size-input-container">
                                <input type="number" min="50" max="500" value="180" class="size-input" id="paraHeightInput">
                                <span class="size-unit">px</span>
                            </div>
                        </div>
                    </div>
                    
                    <div class="angle-buttons" style="margin-top: 20px;">
                        <button class="angle-btn btn-primary" id="applySizeBtn">
                            <i class="fas fa-check-circle"></i> 应用尺寸设置
                        </button>
                    </div>
                </div>
                
                <!-- 角度调节器 -->
                <div class="angle-control-container">
                    <h3 class="angle-control-title">
                        <i class="fas fa-adjust"></i> 角度调节器
                    </h3>
                    <div class="angle-slider-container">
                        <span style="color: var(--gray-color); font-size: 0.9rem;">10°</span>
                        <input type="range" min="10" max="170" value="90" class="angle-slider" id="angleSlider">
                        <span style="color: var(--gray-color); font-size: 0.9rem;">170°</span>
                        <div class="angle-value" id="angleValue">90°</div>
                    </div>
                    <div class="angle-buttons">
                        <button class="angle-btn btn-secondary angle-btn-small" id="decreaseAngleBtn1">
                            <i class="fas fa-minus"></i> 1°
                        </button>
                        <button class="angle-btn btn-secondary angle-btn-small" id="decreaseAngleBtn5">
                            <i class="fas fa-minus"></i> 5°
                        </button>
                        <button class="angle-btn btn-secondary angle-btn-small" id="increaseAngleBtn1">
                            <i class="fas fa-plus"></i> 1°
                        </button>
                        <button class="angle-btn btn-secondary angle-btn-small" id="increaseAngleBtn5">
                            <i class="fas fa-plus"></i> 5°
                        </button>
                    </div>
                </div>
            </div>
            
            <!-- 信息面板 -->
            <div class="shape-card info-panel-card fade-in">
                <div class="info-tabs">
                    <button class="tab-btn active" data-tab="instruction">
                        <i class="fas fa-book-open"></i> 使用指南
                    </button>
                    <button class="tab-btn" data-tab="concept">
                        <i class="fas fa-lightbulb"></i> 概念理解
                    </button>
                    <button class="tab-btn" data-tab="comparison">
                        <i class="fas fa-balance-scale"></i> 对比分析
                    </button>
                    <button class="tab-btn" data-tab="application">
                        <i class="fas fa-cogs"></i> 实际应用
                    </button>
                </div>
                
                <div class="tab-content">
                    <div class="tab-pane active" id="instruction-tab">
                        <div class="info-content">
                            <h3>如何使用本工具</h3>
                            <p>本工具旨在帮助您直观理解平行四边形的不稳定性。以下是操作指南：</p>
                            
                            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-top: 20px;">
                                <div style="background-color: rgba(67, 97, 238, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--primary-color); margin-bottom: 10px;">
                                        <i class="fas fa-ruler"></i> 尺寸设置
                                    </h4>
                                    <p>使用控制面板中的尺寸设置功能，自定义长方形和平行四边形的尺寸。</p>
                                </div>
                                
                                <div style="background-color: rgba(67, 97, 238, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--primary-color); margin-bottom: 10px;">
                                        <i class="fas fa-sliders-h"></i> 角度调节器
                                    </h4>
                                    <p>使用角度调节器精确控制平行四边形内角，每次可以调整1°或5°。</p>
                                </div>
                                
                                <div style="background-color: rgba(67, 97, 238, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--primary-color); margin-bottom: 10px;">
                                        <i class="fas fa-expand"></i> 自适应显示
                                    </h4>
                                    <p>工具会自动调整图形大小，确保在各种角度下都能完整显示在画布中。</p>
                                </div>
                            </div>
                            
                            <h3 style="margin-top: 25px;">交互提示</h3>
                            <ul>
                                <li><strong>自定义尺寸</strong>：通过控制面板中的尺寸设置功能，可以调整长方形和平行四边形的大小</li>
                                <li><strong>顶点拖动</strong>：直接点击平行四边形顶点并拖动，感受形状变化</li>
                                <li><strong>精确控制</strong>：使用角度调节器可以精确调整角度，每次1°或5°</li>
                                <li><strong>自适应显示</strong>：当角度变化导致图形过大时，工具会自动缩放以确保图形完整显示</li>
                            </ul>
                        </div>
                    </div>
                    
                    <div class="tab-pane" id="concept-tab">
                        <div class="info-content">
                            <h3>平行四边形不稳定性概念</h3>
                            <p>平行四边形的不稳定性是指：在保持四条边长不变的情况下，平行四边形的形状可以发生改变，其内角大小可以变化。</p>
                            
                            <div style="background-color: rgba(76, 201, 240, 0.1); padding: 20px; border-radius: 10px; margin: 20px 0;">
                                <h4 style="color: var(--success-color); margin-bottom: 10px;">
                                    <i class="fas fa-calculator"></i> 数学原理
                                </h4>
                                <p>平行四边形的面积公式为：<strong>面积 = 底边 × 高 = a × b × sin(θ)</strong>，其中θ为两邻边的夹角。</p>
                                <p>当边长a和b固定时，面积随角度θ的变化而变化：</p>
                                <ul>
                                    <li>当θ = 90°时，平行四边形变为长方形，面积最大</li>
                                    <li>当θ接近0°或180°时，平行四边形变得非常扁平，面积接近0</li>
                                    <li>角度在0°到180°之间变化时，面积在0到a×b之间变化</li>
                                </ul>
                            </div>
                            
                            <p>这种特性与三角形稳定性形成鲜明对比。三角形一旦边长确定，其形状和角度就完全固定，而平行四边形在边长确定的情况下，可以有无数种形状。</p>
                        </div>
                    </div>
                    
                    <div class="tab-pane" id="comparison-tab">
                        <div class="info-content">
                            <h3>稳定性对比</h3>
                            <p>下表展示了平行四边形与长方形在稳定性方面的对比：</p>
                            
                            <table class="comparison-table">
                                <thead>
                                    <tr>
                                        <th>特性</th>
                                        <th>平行四边形</th>
                                        <th>长方形</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr>
                                        <td><strong>稳定性</strong></td>
                                        <td class="status-unstable" style="border-radius: 0; background: transparent;">不稳定</td>
                                        <td class="status-stable" style="border-radius: 0; background: transparent;">稳定</td>
                                    </tr>
                                    <tr>
                                        <td><strong>角度变化</strong></td>
                                        <td>内角可以变化（0° < α < 180°）</td>
                                        <td>内角固定为90°</td>
                                    </tr>
                                    <tr>
                                        <td><strong>形状确定性</strong></td>
                                        <td>边长固定时，形状不唯一</td>
                                        <td>边长固定时，形状唯一</td>
                                    </tr>
                                    <tr>
                                        <td><strong>面积变化</strong></td>
                                        <td>面积随角度变化而变化</td>
                                        <td>面积固定（长×宽）</td>
                                    </tr>
                                    <tr>
                                        <td><strong>实际应用</strong></td>
                                        <td>伸缩门、折叠椅、施工升降机</td>
                                        <td>建筑框架、门窗、桌子</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                    
                    <div class="tab-pane" id="application-tab">
                        <div class="info-content">
                            <h3>平行四边形不稳定性的实际应用</h3>
                            <p>平行四边形的不稳定性在工程和日常生活中有广泛应用：</p>
                            
                            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-top: 20px;">
                                <div style="background-color: rgba(247, 37, 133, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--danger-color); margin-bottom: 10px;">
                                        <i class="fas fa-compress-alt"></i> 伸缩结构
                                    </h4>
                                    <p>伸缩门、伸缩护栏等利用平行四边形不稳定性实现长度变化。</p>
                                </div>
                                
                                <div style="background-color: rgba(247, 37, 133, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--danger-color); margin-bottom: 10px;">
                                        <i class="fas fa-chair"></i> 折叠家具
                                    </h4>
                                    <p>折叠椅、折叠桌通过平行四边形结构实现折叠功能。</p>
                                </div>
                                
                                <div style="background-color: rgba(247, 37, 133, 0.05); padding: 20px; border-radius: 10px;">
                                    <h4 style="color: var(--danger-color); margin-bottom: 10px;">
                                        <i class="fas fa-tools"></i> 工程机械
                                    </h4>
                                    <p>施工升降机、汽车千斤顶利用平行四边形原理实现升降功能。</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- 底部 -->
        <footer>
            <p>平行四边形不稳定性探究工具 &copy; 2023 | 数学可视化教学工具</p>
            <div class="footer-links">
                <a href="#" class="footer-link" id="helpLink">
                    <i class="fas fa-question-circle"></i> 帮助
                </a>
                <a href="#" class="footer-link" id="feedbackLink">
                    <i class="fas fa-envelope"></i> 反馈
                </a>
                <a href="#" class="footer-link" id="fullscreenLink">
                    <i class="fas fa-expand"></i> 全屏
                </a>
            </div>
        </footer>
    </div>

    <script>
        // 初始化Canvas元素
        const rectCanvas = document.getElementById('rectangleCanvas');
        const paraCanvas = document.getElementById('parallelogramCanvas');
        
        // 设置Canvas尺寸
        function resizeCanvases() {
            const rectWrapper = document.getElementById('rectCanvasWrapper');
            const paraWrapper = document.getElementById('paraCanvasWrapper');
            
            rectCanvas.width = rectWrapper.clientWidth;
            rectCanvas.height = rectWrapper.clientHeight;
            
            paraCanvas.width = paraWrapper.clientWidth;
            paraCanvas.height = paraWrapper.clientHeight;
            
            // 绘制初始图形
            drawRectangle();
            drawParallelogram();
        }
        
        // 初始化时设置尺寸
        window.addEventListener('load', resizeCanvases);
        window.addEventListener('resize', resizeCanvases);
        
        // 获取Canvas上下文
        const rectCtx = rectCanvas.getContext('2d');
        const paraCtx = paraCanvas.getContext('2d');
        
        // 长方形参数（用户可设置）
        let rectLength = 300; // 长方形长度
        let rectWidth = 180;  // 长方形宽度
        
        // 平行四边形参数（用户可设置）
        let paraBase = 300;   // 平行四边形底边
        let paraHeight = 180; // 平行四边形高
        let paraAngle = 90;   // 平行四边形内角
        
        // 存储平行四边形的四个顶点：A(0), B(1), C(2), D(3)
        let paraPoints = [];
        
        // 缩放比例
        let rectScale = 1.0;
        let paraScale = 1.0;
        
        // 被拖动的顶点索引
        let draggedPointIndex = -1;
        let showGrid = false;
        
        // 初始化平行四边形点（长方形状态）
        function initParallelogramPoints() {
            // 计算平行四边形实际尺寸（考虑缩放）
            const actualBase = paraBase * paraScale;
            const actualHeight = paraHeight * paraScale;
            
            // 计算中心点
            const centerX = paraCanvas.width / 2;
            const centerY = paraCanvas.height / 2;
            
            // 根据角度计算平行四边形的偏移
            const angleRad = paraAngle * Math.PI / 180;
            const offsetX = actualHeight * Math.cos(angleRad);
            
            paraPoints = [
                {x: centerX - actualBase/2, y: centerY - actualHeight/2}, // A: 左上
                {x: centerX + actualBase/2, y: centerY - actualHeight/2}, // B: 右上
                {x: centerX + actualBase/2 + offsetX, y: centerY + actualHeight/2}, // C: 右下
                {x: centerX - actualBase/2 + offsetX, y: centerY + actualHeight/2}  // D: 左下
            ];
        }
        
        // 计算平行四边形边界
        function getParallelogramBounds() {
            let minX = Infinity, maxX = -Infinity;
            let minY = Infinity, maxY = -Infinity;
            
            for (const point of paraPoints) {
                minX = Math.min(minX, point.x);
                maxX = Math.max(maxX, point.x);
                minY = Math.min(minY, point.y);
                maxY = Math.max(maxY, point.y);
            }
            
            return {
                minX, maxX, minY, maxY,
                width: maxX - minX,
                height: maxY - minY
            };
        }
        
        // 计算并更新平行四边形缩放比例
        function updateParallelogramScale() {
            // 先计算未缩放时的边界
            const tempScale = paraScale;
            paraScale = 1.0;
            initParallelogramPoints();
            const bounds = getParallelogramBounds();
            
            // 计算需要的缩放比例
            const canvasWidth = paraCanvas.width;
            const canvasHeight = paraCanvas.height;
            
            // 留出边距
            const margin = 40;
            const availableWidth = canvasWidth - 2 * margin;
            const availableHeight = canvasHeight - 2 * margin;
            
            // 计算缩放比例
            const scaleX = availableWidth / bounds.width;
            const scaleY = availableHeight / bounds.height;
            paraScale = Math.min(scaleX, scaleY, 1.0); // 最大不超过1.0
            
            // 限制最小缩放比例
            paraScale = Math.max(paraScale, 0.1);
            
            // 更新比例指示器
            document.getElementById('paraScaleIndicator').textContent = `比例: ${Math.round(paraScale * 100)}%`;
            
            // 重新计算顶点位置
            initParallelogramPoints();
        }
        
        // 计算并更新长方形缩放比例
        function updateRectangleScale() {
            // 计算需要的缩放比例
            const canvasWidth = rectCanvas.width;
            const canvasHeight = rectCanvas.height;
            
            // 留出边距
            const margin = 40;
            const availableWidth = canvasWidth - 2 * margin;
            const availableHeight = canvasHeight - 2 * margin;
            
            // 计算缩放比例
            const scaleX = availableWidth / rectLength;
            const scaleY = availableHeight / rectWidth;
            rectScale = Math.min(scaleX, scaleY, 1.0); // 最大不超过1.0
            
            // 限制最小缩放比例
            rectScale = Math.max(rectScale, 0.1);
            
            // 更新比例指示器
            document.getElementById('rectScaleIndicator').textContent = `比例: ${Math.round(rectScale * 100)}%`;
        }
        
        // 绘制网格
        function drawGrid(ctx, width, height) {
            ctx.strokeStyle = 'rgba(0, 0, 0, 0.05)';
            ctx.lineWidth = 1;
            
            // 垂直线
            for (let x = 0; x < width; x += 20) {
                ctx.beginPath();
                ctx.moveTo(x, 0);
                ctx.lineTo(x, height);
                ctx.stroke();
            }
            
            // 水平线
            for (let y = 0; y < height; y += 20) {
                ctx.beginPath();
                ctx.moveTo(0, y);
                ctx.lineTo(width, y);
                ctx.stroke();
            }
        }
        
        // 绘制长方形
        function drawRectangle() {
            rectCtx.clearRect(0, 0, rectCanvas.width, rectCanvas.height);
            
            // 绘制背景网格
            if (showGrid) {
                drawGrid(rectCtx, rectCanvas.width, rectCanvas.height);
            }
            
            // 更新缩放比例
            updateRectangleScale();
            
            // 计算实际尺寸
            const actualLength = rectLength * rectScale;
            const actualWidth = rectWidth * rectScale;
            
            // 计算长方形位置（居中）
            const rectX = (rectCanvas.width - actualLength) / 2;
            const rectY = (rectCanvas.height - actualWidth) / 2;
            
            // 绘制长方形
            rectCtx.fillStyle = 'rgba(67, 97, 238, 0.08)';
            rectCtx.strokeStyle = '#4361ee';
            rectCtx.lineWidth = 3;
            rectCtx.lineJoin = 'round';
            
            rectCtx.beginPath();
            rectCtx.rect(rectX, rectY, actualLength, actualWidth);
            rectCtx.fill();
            rectCtx.stroke();
            
            // 绘制对角线
            rectCtx.strokeStyle = 'rgba(67, 97, 238, 0.3)';
            rectCtx.lineWidth = 1;
            rectCtx.setLineDash([5, 3]);
            rectCtx.beginPath();
            rectCtx.moveTo(rectX, rectY);
            rectCtx.lineTo(rectX + actualLength, rectY + actualWidth);
            rectCtx.moveTo(rectX + actualLength, rectY);
            rectCtx.lineTo(rectX, rectY + actualWidth);
            rectCtx.stroke();
            rectCtx.setLineDash([]);
            
            // 绘制顶点
            const rectPoints = [
                {x: rectX, y: rectY},
                {x: rectX + actualLength, y: rectY},
                {x: rectX + actualLength, y: rectY + actualWidth},
                {x: rectX, y: rectY + actualWidth}
            ];
            
            rectPoints.forEach((point, index) => {
                rectCtx.fillStyle = '#4361ee';
                rectCtx.beginPath();
                rectCtx.arc(point.x, point.y, 6, 0, Math.PI * 2);
                rectCtx.fill();
                
                // 顶点标签
                rectCtx.fillStyle = '#fff';
                rectCtx.font = 'bold 10px Arial';
                rectCtx.fillText(index + 1, point.x - 4, point.y + 4);
            });
            
            // 更新属性显示
            document.getElementById('rectLength').textContent = rectLength;
            document.getElementById('rectWidth').textContent = rectWidth;
            document.getElementById('rectArea').textContent = rectLength * rectWidth;
        }
        
        // 计算两点距离
        function distance(x1, y1, x2, y2) {
            return Math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2);
        }
        
        // 计算平行四边形实际内角（考虑缩放）
        function calculateActualAngle() {
            // 计算向量AB和AD
            const vectorAB = {
                x: paraPoints[1].x - paraPoints[0].x,
                y: paraPoints[1].y - paraPoints[0].y
            };
            
            const vectorAD = {
                x: paraPoints[3].x - paraPoints[0].x,
                y: paraPoints[3].y - paraPoints[0].y
            };
            
            // 计算点积
            const dotProduct = vectorAB.x * vectorAD.x + vectorAB.y * vectorAD.y;
            
            // 计算模长
            const magnitudeAB = Math.sqrt(vectorAB.x ** 2 + vectorAB.y ** 2);
            const magnitudeAD = Math.sqrt(vectorAD.x ** 2 + vectorAD.y ** 2);
            
            // 计算夹角（弧度）并转换为角度
            let angle = Math.acos(dotProduct / (magnitudeAB * magnitudeAD));
            angle = angle * 180 / Math.PI;
            
            // 确保角度在0-180度之间
            return Math.max(1, Math.min(179, Math.round(angle)));
        }
        
        // 计算平行四边形面积
        function calculateParallelogramArea() {
            // 使用向量叉积计算面积
            const vectorAB = {
                x: paraPoints[1].x - paraPoints[0].x,
                y: paraPoints[1].y - paraPoints[0].y
            };
            
            const vectorAD = {
                x: paraPoints[3].x - paraPoints[0].x,
                y: paraPoints[3].y - paraPoints[0].y
            };
            
            // 叉积的绝对值除以2
            const area = Math.abs(vectorAB.x * vectorAD.y - vectorAB.y * vectorAD.x);
            // 需要除以缩放比例的平方来得到实际面积
            const actualArea = area / (paraScale * paraScale);
            return Math.round(actualArea);
        }
        
        // 计算平行四边形边长
        function calculateParallelogramSides() {
            // 计算边长a (AB边)
            const sideA = distance(paraPoints[0].x, paraPoints[0].y, paraPoints[1].x, paraPoints[1].y);
            // 计算边长b (AD边)
            const sideB = distance(paraPoints[0].x, paraPoints[0].y, paraPoints[3].x, paraPoints[3].y);
            
            // 需要除以缩放比例来得到实际边长
            return {
                sideA: Math.round(sideA / paraScale),
                sideB: Math.round(sideB / paraScale)
            };
        }
        
        // 更新平行四边形属性显示
        function updateParallelogramProperties() {
            // 计算边长
            const sides = calculateParallelogramSides();
            
            // 计算角度
            const angle = calculateActualAngle();
            
            // 计算面积
            const area = calculateParallelogramArea();
            
            // 更新属性显示
            document.getElementById('paraSideA').textContent = sides.sideA;
            document.getElementById('paraSideB').textContent = sides.sideB;
            document.getElementById('paraAngle').textContent = angle;
            document.getElementById('paraArea').textContent = area;
            
            // 更新角度调节器
            document.getElementById('angleSlider').value = angle;
            document.getElementById('angleValue').textContent = `${angle}°`;
            
            // 更新当前角度变量
            paraAngle = angle;
        }
        
        // 绘制平行四边形
        function drawParallelogram() {
            paraCtx.clearRect(0, 0, paraCanvas.width, paraCanvas.height);
            
            // 绘制背景网格
            if (showGrid) {
                drawGrid(paraCtx, paraCanvas.width, paraCanvas.height);
            }
            
            // 更新平行四边形顶点
            initParallelogramPoints();
            
            // 绘制平行四边形
            paraCtx.fillStyle = 'rgba(247, 37, 133, 0.08)';
            paraCtx.strokeStyle = '#f72585';
            paraCtx.lineWidth = 3;
            paraCtx.lineJoin = 'round';
            
            paraCtx.beginPath();
            paraCtx.moveTo(paraPoints[0].x, paraPoints[0].y);
            paraCtx.lineTo(paraPoints[1].x, paraPoints[1].y);
            paraCtx.lineTo(paraPoints[2].x, paraPoints[2].y);
            paraCtx.lineTo(paraPoints[3].x, paraPoints[3].y);
            paraCtx.closePath();
            paraCtx.fill();
            paraCtx.stroke();
            
            // 绘制对角线
            paraCtx.strokeStyle = 'rgba(247, 37, 133, 0.3)';
            paraCtx.lineWidth = 1;
            paraCtx.setLineDash([5, 3]);
            paraCtx.beginPath();
            paraCtx.moveTo(paraPoints[0].x, paraPoints[0].y);
            paraCtx.lineTo(paraPoints[2].x, paraPoints[2].y);
            paraCtx.moveTo(paraPoints[1].x, paraPoints[1].y);
            paraCtx.lineTo(paraPoints[3].x, paraPoints[3].y);
            paraCtx.stroke();
            paraCtx.setLineDash([]);
            
            // 绘制顶点
            paraPoints.forEach((point, index) => {
                paraCtx.fillStyle = '#f72585';
                paraCtx.beginPath();
                paraCtx.arc(point.x, point.y, 8, 0, Math.PI * 2);
                paraCtx.fill();
                
                // 顶点标签
                paraCtx.fillStyle = '#fff';
                paraCtx.font = 'bold 12px Arial';
                paraCtx.fillText(index + 1, point.x - 5, point.y + 4);
            });
            
            // 计算并更新属性
            updateParallelogramProperties();
        }
        
        // 检查点是否在顶点上
        function getPointAt(x, y) {
            for (let i = 0; i < paraPoints.length; i++) {
                if (distance(x, y, paraPoints[i].x, paraPoints[i].y) < 20) {
                    return i;
                }
            }
            return -1;
        }
        
        // 鼠标事件处理 - 优化拖动逻辑
        function setupMouseEvents() {
            // 鼠标按下事件
            paraCanvas.addEventListener('mousedown', (e) => {
                const rect = paraCanvas.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                
                draggedPointIndex = getPointAt(x, y);
                if (draggedPointIndex !== -1) {
                    paraCanvas.style.cursor = 'grabbing';
                }
            });
            
            // 鼠标移动事件
            paraCanvas.addEventListener('mousemove', (e) => {
                const rect = paraCanvas.getBoundingClientRect();
                const x = e.clientX - rect.left;
                const y = e.clientY - rect.top;
                
                // 检查鼠标是否在顶点上
                if (getPointAt(x, y) !== -1) {
                    paraCanvas.style.cursor = 'grab';
                } else {
                    paraCanvas.style.cursor = 'default';
                }
                
                // 如果正在拖动顶点
                if (draggedPointIndex !== -1) {
                    // 限制拖动范围在Canvas内
                    const newX = Math.max(20, Math.min(paraCanvas.width - 20, x));
                    const newY = Math.max(20, Math.min(paraCanvas.height - 20, y));
                    
                    // 获取拖动的顶点索引
                    const idx = draggedPointIndex;
                    
                    // 优化拖动逻辑：拖动一个顶点时，对边顶点自行移动
                    // 保持平行四边形的性质：对边平行且相等
                    
                    // 根据拖动的顶点索引，确定对应的对边顶点
                    let oppositeIdx;
                    let adjacent1Idx, adjacent2Idx;
                    
                    if (idx === 0) { // 拖动A点
                        oppositeIdx = 2; // C点是对边顶点
                        adjacent1Idx = 1; // B点
                        adjacent2Idx = 3; // D点
                    } else if (idx === 1) { // 拖动B点
                        oppositeIdx = 3; // D点是对边顶点
                        adjacent1Idx = 0; // A点
                        adjacent2Idx = 2; // C点
                    } else if (idx === 2) { // 拖动C点
                        oppositeIdx = 0; // A点是对边顶点
                        adjacent1Idx = 1; // B点
                        adjacent2Idx = 3; // D点
                    } else { // 拖动D点
                        oppositeIdx = 1; // B点是对边顶点
                        adjacent1Idx = 0; // A点
                        adjacent2Idx = 2; // C点
                    }
                    
                    // 更新拖动的顶点
                    paraPoints[idx].x = newX;
                    paraPoints[idx].y = newY;
                    
                    // 计算新的对边顶点位置
                    // 平行四边形的性质：对角线互相平分
                    // 所以对边顶点 = 相邻顶点1 + (相邻顶点2 - 拖动的顶点)
                    paraPoints[oppositeIdx].x = paraPoints[adjacent1Idx].x + (paraPoints[adjacent2Idx].x - newX);
                    paraPoints[oppositeIdx].y = paraPoints[adjacent1Idx].y + (paraPoints[adjacent2Idx].y - newY);
                    
                    // 重新绘制
                    drawParallelogram();
                    
                    // 更新缩放比例以确保平行四边形完整显示
                    updateParallelogramScale();
                }
            });
            
            // 鼠标释放事件
            paraCanvas.addEventListener('mouseup', () => {
                draggedPointIndex = -1;
                paraCanvas.style.cursor = 'default';
            });
            
            // 鼠标离开Canvas
            paraCanvas.addEventListener('mouseleave', () => {
                draggedPointIndex = -1;
                paraCanvas.style.cursor = 'default';
            });
        }
        
        // 重置平行四边形为长方形
        function resetParallelogram() {
            paraAngle = 90;
            updateParallelogramScale();
            drawParallelogram();
            
            // 显示提示
            showNotification('已重置平行四边形为长方形', 'success');
        }
        
        // 生成随机平行四边形
        function randomizeParallelogram() {
            // 随机生成一个角度（30°到150°之间）
            paraAngle = Math.floor(Math.random() * 120) + 30;
            updateParallelogramScale();
            drawParallelogram();
            
            // 显示提示
            showNotification('已生成随机平行四边形形状', 'success');
        }
        
        // 切换网格显示
        function toggleGrid() {
            showGrid = !showGrid;
            drawRectangle();
            drawParallelogram();
            
            // 更新按钮文本
            const gridBtn = document.getElementById('toggleGridBtn');
            if (showGrid) {
                gridBtn.innerHTML = '<i class="fas fa-th-large"></i> 隐藏网格';
                showNotification('已显示背景网格', 'info');
            } else {
                gridBtn.innerHTML = '<i class="fas fa-th"></i> 显示网格';
                showNotification('已隐藏背景网格', 'info');
            }
        }
        
        // 显示极限情况
        function showExtremeCases() {
            // 生成接近10°的极限情况（避免完全扁平）
            paraAngle = 10;
            updateParallelogramScale();
            drawParallelogram();
            
            // 显示提示
            showNotification('已显示接近10°的极限情况（非常扁平）', 'warning');
        }
        
        // 调整平行四边形角度（精确到1°）
        function adjustParallelogramAngle(angle) {
            paraAngle = Math.max(10, Math.min(170, angle));
            updateParallelogramScale();
            drawParallelogram();
        }
        
        // 应用尺寸设置
        function applySizeSettings() {
            // 获取输入值
            const newRectLength = parseInt(document.getElementById('rectLengthInput').value);
            const newRectWidth = parseInt(document.getElementById('rectWidthInput').value);
            const newParaBase = parseInt(document.getElementById('paraBaseInput').value);
            const newParaHeight = parseInt(document.getElementById('paraHeightInput').value);
            
            // 验证输入值
            if (newRectLength < 50 || newRectLength > 500 || 
                newRectWidth < 50 || newRectWidth > 500 ||
                newParaBase < 50 || newParaBase > 500 ||
                newParaHeight < 50 || newParaHeight > 500) {
                showNotification('尺寸必须在50px到500px之间', 'danger');
                return;
            }
            
            // 更新尺寸
            rectLength = newRectLength;
            rectWidth = newRectWidth;
            paraBase = newParaBase;
            paraHeight = newParaHeight;
            
            // 重新绘制图形
            drawRectangle();
            updateParallelogramScale();
            drawParallelogram();
            
            // 显示提示
            showNotification('已应用新的尺寸设置', 'success');
        }
        
        // 显示通知
        function showNotification(message, type = 'info') {
            // 创建通知元素
            const notification = document.createElement('div');
            notification.style.cssText = `
                position: fixed;
                top: 20px;
                right: 20px;
                padding: 15px 25px;
                border-radius: 10px;
                background-color: ${type === 'success' ? '#4cc9f0' : type === 'warning' ? '#f8961e' : type === 'danger' ? '#f72585' : '#4361ee'};
                color: white;
                font-weight: 600;
                box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
                z-index: 1000;
                animation: fadeIn 0.3s ease-out;
                max-width: 300px;
            `;
            notification.textContent = message;
            
            // 添加到页面
            document.body.appendChild(notification);
            
            // 3秒后移除
            setTimeout(() => {
                notification.style.opacity = '0';
                notification.style.transform = 'translateX(20px)';
                setTimeout(() => {
                    document.body.removeChild(notification);
                }, 300);
            }, 3000);
        }
        
        // 标签页切换
        function setupTabs() {
            const tabButtons = document.querySelectorAll('.tab-btn');
            const tabPanes = document.querySelectorAll('.tab-pane');
            
            tabButtons.forEach(button => {
                button.addEventListener('click', () => {
                    // 移除所有活动状态
                    tabButtons.forEach(btn => btn.classList.remove('active'));
                    tabPanes.forEach(pane => pane.classList.remove('active'));
                    
                    // 添加当前活动状态
                    button.classList.add('active');
                    const tabId = button.getAttribute('data-tab');
                    document.getElementById(`${tabId}-tab`).classList.add('active');
                });
            });
        }
        
        // 分步指导
        function showStepGuide() {
            // 切换到使用指南标签
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
            document.querySelectorAll('.tab-pane').forEach(pane => pane.classList.remove('active'));
            
            document.querySelector('.tab-btn[data-tab="instruction"]').classList.add('active');
            document.getElementById('instruction-tab').classList.add('active');
            
            // 显示通知
            showNotification('请查看"使用指南"标签页了解详细操作步骤', 'info');
        }
        
        // 初始化
        function init() {
            // 初始化平行四边形点
            updateParallelogramScale();
            
            // 绘制初始图形
            drawRectangle();
            drawParallelogram();
            
            // 设置事件监听
            setupMouseEvents();
            setupTabs();
            
            // 按钮事件监听
            document.getElementById('resetBtn').addEventListener('click', resetParallelogram);
            document.getElementById('randomBtn').addEventListener('click', randomizeParallelogram);
            document.getElementById('toggleGridBtn').addEventListener('click', toggleGrid);
            document.getElementById('showExtremeBtn').addEventListener('click', showExtremeCases);
            document.getElementById('stepGuideBtn').addEventListener('click', showStepGuide);
            
            // 应用尺寸设置按钮
            document.getElementById('applySizeBtn').addEventListener('click', applySizeSettings);
            
            // 尺寸输入框事件监听（回车键应用）
            const sizeInputs = document.querySelectorAll('.size-input');
            sizeInputs.forEach(input => {
                input.addEventListener('keyup', (e) => {
                    if (e.key === 'Enter') {
                        applySizeSettings();
                    }
                });
            });
            
            // 角度调节器 - 滑块
            const angleSlider = document.getElementById('angleSlider');
            angleSlider.addEventListener('input', () => {
                const angle = parseInt(angleSlider.value);
                adjustParallelogramAngle(angle);
                document.getElementById('angleValue').textContent = `${angle}°`;
            });
            
            // 角度调节按钮 - 每次调整1°或5°
            document.getElementById('decreaseAngleBtn1').addEventListener('click', () => {
                const currentAngle = parseInt(document.getElementById('paraAngle').textContent);
                adjustParallelogramAngle(currentAngle - 1);
                showNotification(`角度减小为 ${currentAngle - 1}°`, 'info');
            });
            
            document.getElementById('decreaseAngleBtn5').addEventListener('click', () => {
                const currentAngle = parseInt(document.getElementById('paraAngle').textContent);
                adjustParallelogramAngle(currentAngle - 5);
                showNotification(`角度减小为 ${currentAngle - 5}°`, 'info');
            });
            
            document.getElementById('increaseAngleBtn1').addEventListener('click', () => {
                const currentAngle = parseInt(document.getElementById('paraAngle').textContent);
                adjustParallelogramAngle(currentAngle + 1);
                showNotification(`角度增大为 ${currentAngle + 1}°`, 'info');
            });
            
            document.getElementById('increaseAngleBtn5').addEventListener('click', () => {
                const currentAngle = parseInt(document.getElementById('paraAngle').textContent);
                adjustParallelogramAngle(currentAngle + 5);
                showNotification(`角度增大为 ${currentAngle + 5}°`, 'info');
            });
            
            // 底部链接
            document.getElementById('helpLink').addEventListener('click', (e) => {
                e.preventDefault();
                showStepGuide();
            });
            
            document.getElementById('feedbackLink').addEventListener('click', (e) => {
                e.preventDefault();
                showNotification('反馈功能尚未实现，请通过其他方式联系我们', 'info');
            });
            
            document.getElementById('fullscreenLink').addEventListener('click', (e) => {
                e.preventDefault();
                if (!document.fullscreenElement) {
                    document.documentElement.requestFullscreen().catch(err => {
                        showNotification('全屏模式失败：' + err.message, 'danger');
                    });
                } else {
                    document.exitFullscreen();
                }
            });
            
            // 显示欢迎消息
            setTimeout(() => {
                showNotification('欢迎使用平行四边形不稳定性探究工具！请拖动右侧平行四边形的顶点开始探索。', 'success');
            }, 1000);
        }
        
        // 页面加载完成后初始化
        window.addEventListener('load', init);
    </script>
</body>
</html>
