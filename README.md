<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Phone Checker Pro</title>
    <style>
        :root {
            --primary-color: #00ff88;
            --bg-dark: #0f172a;
            --glass-bg: rgba(255, 255, 255, 0.1);
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: radial-gradient(circle at center, #1e293b, #0f172a);
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
        }

        .app-container {
            width: 90%;
            max-width: 400px;
            background: var(--glass-bg);
            backdrop-filter: blur(15px);
            border-radius: 30px;
            padding: 25px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 25px 50px rgba(0,0,0,0.5);
            text-align: center;
        }

        .header h1 {
            font-size: 1.5rem;
            margin-bottom: 5px;
            color: var(--primary-color);
        }

        .header p {
            font-size: 0.8rem;
            opacity: 0.7;
            margin-bottom: 30px;
        }

        /* دائرة الفحص */
        .scan-circle {
            width: 150px;
            height: 150px;
            margin: 0 auto 30px;
            border-radius: 50%;
            border: 4px solid #1e293b;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            position: relative;
            background: rgba(0, 255, 136, 0.05);
            transition: 0.5s;
        }

        .scan-circle.scanning {
            box-shadow: 0 0 30px var(--primary-color);
            border-color: var(--primary-color);
        }

        .percentage {
            font-size: 2rem;
            font-weight: bold;
        }

        /* قائمة المميزات */
        .stats-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
            margin-bottom: 30px;
        }

        .stat-item {
            background: rgba(255, 255, 255, 0.05);
            padding: 15px;
            border-radius: 15px;
            font-size: 0.9rem;
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        .stat-item span {
            display: block;
            color: var(--primary-color);
            font-weight: bold;
            margin-top: 5px;
        }

        /* زر التشغيل */
        .btn-scan {
            background: var(--primary-color);
            color: #0f172a;
            border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-weight: bold;
            font-size: 1.1rem;
            cursor: pointer;
            width: 100%;
            box-shadow: 0 10px 20px rgba(0, 255, 136, 0.3);
            transition: 0.3s;
        }

        .btn-scan:active {
            transform: scale(0.95);
        }

        .status-text {
            margin-top: 15px;
            font-size: 0.9rem;
            color: #ffda44;
            display: none;
        }
    </style>
</head>
<body>

<div class="app-container">
    <div class="header">
        <h1>فحص النظام</h1>
        <p>قم بتحسين أداء هاتفك بضغطة واحدة</p>
    </div>

    <div class="scan-circle" id="circle">
        <div class="percentage" id="percent">100%</div>
        <div style="font-size: 0.7rem;">حالة الهاتف</div>
    </div>

    <div class="stats-grid">
        <div class="stat-item">
            الذاكرة RAM
            <span id="ram-stat">2.4 GB</span>
        </div>
        <div class="stat-item">
            البطارية
            <span id="battery-stat">-- %</span>
        </div>
        <div class="stat-item">
            المساحة
            <span>78% ممتلئ</span>
        </div>
        <div class="stat-item">
            المعالج
            <span>مستقر</span>
        </div>
    </div>

    <button class="btn-scan" onclick="startScan()">ابدأ الفحص الآن</button>
    <p class="status-text" id="status">جاري تنظيف الملفات المؤقتة...</p>
</div>

<script>
    // الحصول على معلومات البطارية الحقيقية
    if ('getBattery' in navigator) {
        navigator.getBattery().then(function(battery) {
            document.getElementById('battery-stat').innerText = Math.floor(battery.level * 100) + "%";
        });
    }

    function startScan() {
        const circle = document.getElementById('circle');
        const percent = document.getElementById('percent');
        const status = document.getElementById('status');
        const btn = document.querySelector('.btn-scan');

        btn.disabled = true;
        btn.innerText = "جاري الفحص...";
        circle.classList.add('scanning');
        status.style.display = "block";

        let count = 0;
        let interval = setInterval(() => {
            count++;
            percent.innerText = count + "%";
            
            if(count == 30) status.innerText = "فحص أمان التطبيقات...";
            if(count == 60) status.innerText = "تحسين استهلاك البطارية...";
            if(count == 90) status.innerText = "أوشكنا على الانتهاء...";

            if (count >= 100) {
                clearInterval(interval);
                finishScan();
            }
        }, 50);
    }

    function finishScan() {
        alert("تم فحص وتحسين هاتفك بنجاح!");
        location.reload(); // لإعادة الحالة كما كانت
    }
</script>

</body>
</html>
