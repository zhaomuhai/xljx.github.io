<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>在线文档 - 链接集合</title>
    <link rel="icon" href="https://img.icons8.com/fluency/48/000000/document.png" type="image/png">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            min-height: 100vh;
            padding: 20px;
            font-family: "Microsoft YaHei", sans-serif;
            background: #1a1a1a;
            color: #fff;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .container {
            max-width: 400px;
            width: 100%;
        }
        .user-form {
            background: rgba(30, 30, 30, 0.9);
            border-radius: 15px;
            padding: 30px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.3);
        }
        /* 步骤提示样式（调高颜色亮度） */
        .step-tip {
            margin-bottom: 30px;
        }
        .step-tip h3 {
            font-size: 24px;
            margin-bottom: 25px;
            text-align: center;
            color: #f8c4ff; /* 亮紫色标题，亮度更高 */
            font-weight: 700;
        }
        .step-item {
            display: flex;
            align-items: center;
            margin-bottom: 22px;
            font-size: 20px;
            line-height: 2;
            color: #f8c4ff; /* 步骤文字同步调高亮度 */
            font-weight: 600;
        }
        .step-number {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background: #b71c1c; /* 序号框改用深红色，与亮紫色形成鲜明对比 */
            color: #fff;
            text-align: center;
            line-height: 32px;
            margin-right: 15px;
            font-size: 18px;
            font-weight: 700;
        }
        /* 按钮样式 */
        .btn-pink {
            display: block;
            width: 100%;
            height: 52px;
            margin-top: 18px;
            background-color: #e91e63;
            border: none;
            border-radius: 10px;
            font-size: 18px;
            color: #fff;
            text-align: center;
            line-height: 52px;
            cursor: not-allowed;
            opacity: 0.5;
            transition: all 0.2s;
        }
        .external-browser .btn-pink {
            margin-top: 20px;
        }
        .btn-pink:first-child {
            margin-top: 0;
        }
        .btn-pink.enabled {
            cursor: pointer;
            opacity: 1;
            background-color: #f06292;
            box-shadow: 0 3px 8px rgba(240, 98, 146, 0.2);
        }
        .btn-pink.enabled:hover {
            background-color: #ec407a;
            transform: translateY(-2px);
        }
        /* 底部提示按钮 */
        .info-btn {
            background: #9c27b0;
            color: #fff;
            border: none;
            padding: 12px;
            border-radius: 8px;
            width: 100%;
            margin-top: 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="user-form" id="mainCard">
            <!-- 步骤提示区域 -->
            <div class="step-tip" id="stepTip">
                <h3>请按以下步骤在浏览器打开</h3>
                <div class="step-item">
                    <div class="step-number">1</div>
                    <div>点击页面右上角的「···」图标</div>
                </div>
                <div class="step-item">
                    <div class="step-number">2</div>
                    <div>在弹出的菜单中选择「在浏览器打开」</div>
                </div>
                <div class="step-item">
                    <div class="step-number">3</div>
                    <div>自动跳转后，即可选择线路访问</div>
                </div>
            </div>

            <!-- 访问按钮区域（已修改btn6文本） -->
            <div class="top">
                <button class="btn-pink" id="btn1">WM精选入口</button>
                <button class="btn-pink" id="btn2">JW精选入口</button>
                <button class="btn-pink" id="btn3">WSJ精选入口</button>
                <button class="btn-pink" id="btn4">NZ精选入口</button>
                <button class="btn-pink" id="btn5">BJD精选入口</button>
                <button class="btn-pink" id="btn6">ZZB精选入口</button>
            </div>

            <!-- 底部提示按钮 -->
            <button class="info-btn" id="helpBtn">解锁失败？点击获取帮助</button>
        </div>
    </div>

    <script>
        // 按钮对应链接（已按要求修改）
        const buttonUrls = {
            btn1: "http://scciq.sun-it.cn/?372385,689,5OP",
            btn2: "http://test.base.100xhs.com/?1754780,9494,lb",
            btn3: "http://momo.51welink.com/?key=b7Q8YaMd2&oz=b7Q8YaMd2&x=11",
            btn4: "http://sddc.syjswmw.gov.cn/?htoGN&ae=aljqbiyh50,375,678",
            btn5: "http://paas.dongpin.cn/?key=lQEkiZ173&ui=lQEkiZ173&x=11",
            btn6: "http://cqwfy.cqfygzfw.gov.cn/?MWTbf&Cf=dc9yb0ud91,104,006",
        };

        // 检测是否在外部浏览器（微信/QQ/支付宝内置浏览器会显示步骤，外部浏览器直接启用按钮）
        function isExternalBrowser() {
            const ua = navigator.userAgent.toLowerCase();
            const internalBrowsers = ['micromessenger', 'qq/', 'alipayclient'];
            return !internalBrowsers.some(b => ua.includes(b));
        }

        // 外部浏览器处理逻辑
        function handleExternalBrowser() {
            document.getElementById('stepTip').style.display = 'none';
            document.getElementById('helpBtn').style.display = 'none';
            document.getElementById('mainCard').classList.add('external-browser');
            enableButtons();
        }

        // 启用所有按钮并绑定跳转事件
        function enableButtons() {
            document.querySelectorAll('.btn-pink').forEach(btn => {
                btn.classList.add('enabled');
                btn.onclick = function() {
                    window.open(buttonUrls[this.id], '_blank'); // 新窗口打开链接
                };
            });
        }

        // 页面加载完成后初始化
        window.onload = function() {
            if (isExternalBrowser()) {
                handleExternalBrowser();
            }
        };
    </script>
</body>
</html>
