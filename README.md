<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>الخريطة الجغرافية التفاعلية - سلطنة عمان</title>
    <!-- مكتبة Leaflet للخرائط التفاعلية -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <style>
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
        body { display: flex; height: 100vh; background-color: #121212; color: #fff; overflow: hidden; }
        
        /* لوحة التحكم الجانبية */
        #sidebar {
            width: 320px;
            background: #1e1e1e;
            border-left: 1px solid #333;
            display: flex;
            flex-direction: column;
            padding: 20px;
            z-index: 1000;
            box-shadow: -5px 0 15px rgba(0,0,0,0.5);
        }
        #sidebar h2 { font-size: 1.2rem; margin-bottom: 5px; color: #4dabf7; }
        #sidebar p { font-size: 0.85rem; color: #aaa; margin-bottom: 20px; }
        
        .section-title { font-size: 0.95rem; font-weight: bold; margin: 15px 0 10px; color: #ffd43b; border-bottom: 1px solid #333; padding-bottom: 5px; }
        
        .layer-toggle {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
            background: #2a2a2a;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.2s;
        }
        .layer-toggle:hover { background: #333; }
        .layer-toggle input { margin-left: 10px; width: 18px; height: 18px; accent-color: #4dabf7; cursor: pointer; }
        .layer-toggle label { cursor: pointer; font-size: 0.9rem; flex-grow: 1; }

        /* منطقة الخريطة */
        #map { flex-grow: 1; height: 100%; }

        /* أزرار تفاعلية إضافية */
        .btn-action {
            background: #228be6;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 10px;
            transition: background 0.2s;
        }
        .btn-action:hover { background: #1c7ed6; }

        /* تخصيص النافذة المنبثقة للخرائط */
        .leaflet-popup-content-wrapper { background: #222; color: #fff; border-radius: 8px; }
        .leaflet-popup-tip { background: #222; }
        .popup-title { font-weight: bold; color: #4dabf7; font-size: 1rem; margin-bottom: 5px; }
        .popup-desc { font-size: 0.85rem; color: #ccc; }
    </style>
</head>
<body>

    <!-- لوحة التحكم الجانبية -->
    <div id="sidebar">
        <h2>خريطة سلطنة عمان الجغرافية</h2>
        <p>تطبيق تفاعلي لدراسة البيانات المكانية والظواهر الجغرافية.</p>

        <div class="section-title">طبقات الخريطة</div>
        
        <div class="layer-toggle">
            <input type="checkbox" id="layer-cities" checked onchange="toggleLayer('cities')">
            <label for="layer-cities">المدن الرئيسية</label>
        </div>

        <div class="layer-toggle">
            <input type="checkbox" id="layer-history" checked onchange="toggleLayer('history')">
            <label for="layer-history">المواقع التاريخية والتراثية</label>
        </div>

        <div class="layer-toggle">
            <input type="checkbox" id="layer-nature" checked onchange="toggleLayer('nature')">
            <label for="layer-nature">المظاهر التضاريسية والبيئية</label>
        </div>

        <div class="section-title">أدوات تفاعلية</div>
        <button class="btn-action" onclick="resetMapView()">إعادة ضبط الخريطة</button>
    </div>

    <!-- عنصر الخريطة -->
    <div id="map"></div>

    <!-- جلب سكربت مكت
