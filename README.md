<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#102b3f">
  <title>منصة الجغرافيا الذكية | مدرسة ثابت بن قيس</title>

  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.css" onerror="this.onerror=null;this.href='https://unpkg.com/leaflet@1.9.4/dist/leaflet.css'">
  <style>
    :root {
      --navy: #102b3f;
      --navy-2: #173d55;
      --blue: #087e9e;
      --cyan: #39c6d6;
      --gold: #e9b949;
      --paper: #f4f7f8;
      --ink: #173042;
      --muted: #647987;
      --line: #d9e3e8;
      --white: #fff;
      --shadow: 0 12px 32px rgba(11, 39, 57, .14);
    }
    * { box-sizing: border-box; }
    body {
      margin: 0; font-family: Tahoma, Arial, sans-serif;
      background: var(--paper); color: var(--ink);
    }
    .app { min-height: 100vh; display: grid; grid-template-columns: minmax(0, 1fr) 360px; }
    #map { min-height: 100vh; width: 100%; background: #dbe8ed; }
    .sidebar {
      grid-column: 2; grid-row: 1; z-index: 500;
      background: linear-gradient(160deg, var(--navy), #0b2030);
      color: white; padding: 20px; overflow-y: auto;
      box-shadow: -8px 0 28px rgba(0,0,0,.16);
    }
    .brand { display:flex; align-items:center; gap:12px; margin-bottom:16px; }
    .brand-mark {
      width:56px; height:56px; flex:0 0 56px; border-radius:15px;
      display:grid; place-items:center; font-size:27px;
      background:linear-gradient(145deg,#fff,#d9edf1); color:var(--navy);
      border:2px solid rgba(255,255,255,.35);
    }
    .flag { display:flex; height:8px; width:100%; border-radius:8px; overflow:hidden; margin:12px 0 16px; }
    .flag span:nth-child(1){background:#df2935; flex:1}
    .flag span:nth-child(2){background:#fff; flex:1}
    .flag span:nth-child(3){background:#16834b; flex:1}
    h1 { font-size:20px; line-height:1.5; margin:0; color:#70e2ed; }
    .school { font-size:13px; color:#d7e6ec; margin-top:3px; }
    .intro { color:#c1d4de; font-size:12px; line-height:1.8; margin:0 0 14px; }
    .section-title {
      color:#ffd477; font-size:13px; font-weight:bold;
      margin:18px 0 9px; padding-bottom:7px;
      border-bottom:1px solid rgba(255,255,255,.16);
    }
    .search-wrap { position:relative; }
    .search {
      width:100%; border:1px solid #416176; border-radius:10px;
      padding:12px 13px; background:#0a2030; color:white; outline:none;
      font-size:13px;
    }
    .search:focus { border-color:var(--cyan); box-shadow:0 0 0 3px rgba(57,198,214,.14); }
    .layer {
      display:flex; align-items:center; gap:9px; padding:10px;
      margin:7px 0; border:1px solid rgba(255,255,255,.12);
      background:rgba(255,255,255,.055); border-radius:10px;
      cursor:pointer; transition:.2s;
    }
    .layer:hover { background:rgba(255,255,255,.11); }
    .layer input { width:17px; height:17px; accent-color:var(--cyan); }
    .layer label { flex:1; cursor:pointer; font-size:12px; }
    .dot { width:10px; height:10px; border-radius:50%; display:inline-block; margin-left:5px; }
    .count { font-size:11px; background:#28495d; padding:3px 8px; border-radius:20px; color:#e4f4f8; }
    .btn-grid { display:grid; grid-template-columns:1fr 1fr; gap:8px; }
    button {
      border:0; border-radius:9px; padding:11px 9px; cursor:pointer;
      font: bold 12px Tahoma, Arial, sans-serif; transition:.2s;
    }
    button:hover { filter:brightness(1.1); transform:translateY(-1px); }
    .btn-primary { background:linear-gradient(135deg,#12a9bf,#087e9e); color:white; }
    .btn-secondary { background:#29495d; color:#f1f7fa; border:1px solid #416176; }
    .btn-gold { background:var(--gold); color:#1b2d38; }
    .full { width:100%; margin-top:8px; }
    .stats {
      margin-top:17px; padding:13px; border-radius:12px;
      background:rgba(255,255,255,.08); border:1px solid rgba(255,255,255,.13);
      display:flex; justify-content:space-between; align-items:center;
    }
    .stats strong { color:#70e2ed; font-size:24px; }
    .legend { color:#b9ced8; font-size:11px; line-height:1.8; margin-top:12px; }
    .map-title {
      position:absolute; z-index:400; top:14px; left:14px;
      background:rgba(255,255,255,.94); color:var(--navy);
      padding:10px 14px; border-radius:11px; box-shadow:var(--shadow);
      font-size:12px; font-weight:bold; max-width:calc(100% - 390px);
    }
    .map-title small { display:block; color:var(--muted); font-weight:normal; margin-top:4px; }
    .leaflet-popup-content-wrapper {
      border-radius:13px; box-shadow:var(--shadow); overflow:hidden;
    }
    .leaflet-popup-content { margin:0; width:245px !important; }
    .popup { direction:rtl; text-align:right; font-family:Tahoma,Arial,sans-serif; }
    .popup img { width:100%; height:112px; object-fit:cover; display:block; background:#e7eef1; }
    .popup-body { padding:11px 13px 13px; }
    .popup h3 { margin:0 0 7px; color:#087e9e; font-size:14px; }
    .popup p { margin:0; color:#425967; font-size:12px; line-height:1.8; }
    .popup .type { display:inline-block; margin-top:9px; padding:4px 8px; border-radius:20px; background:#eaf5f7; color:#087e9e; font-size:10px; }
    .leaflet-control-attribution { font-size:9px !important; }
    .notice {
      margin-top:12px; color:#b9ced8; font-size:10px; line-height:1.7;
      border-top:1px solid rgba(255,255,255,.14); padding-top:10px;
    }
    @media (max-width: 800px) {
      .app { display:flex; flex-direction:column; }
      .sidebar { order:1; width:100%; max-height:none; }
      #map { order:2; min-height:420px; height:65vh; }
      .map-title { top:auto; bottom:calc(65vh - 52px); left:10px; max-width:calc(100% - 20px); }
    }

    .quick-nav{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin:14px 0}
    .quick-nav a{display:block;text-decoration:none;text-align:center;background:#21465d;color:#fff;border:1px solid #42677b;border-radius:10px;padding:10px 6px;font-size:12px;font-weight:bold}
    .quick-nav a:hover{background:#087e9e;color:#fff}
    .content-section{padding:48px max(18px,calc((100% - 1180px)/2));background:#f4f7f8;border-top:1px solid #d9e3e8}
    .content-section:nth-of-type(even){background:#eaf1f4}
    .content-section h2{color:#102b3f;margin:0 0 10px;font-size:26px}
    .section-lead{color:#647987;line-height:1.9;margin:0 0 22px}
    .cards-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:14px}
    .portal-card{background:#fff;border:1px solid #d9e3e8;border-radius:15px;padding:18px;box-shadow:0 8px 22px rgba(11,39,57,.07)}
    .portal-card h3{margin:7px 0;color:#087e9e;font-size:17px}
    .portal-card p{color:#647987;font-size:13px;line-height:1.8}
    .portal-icon{font-size:30px;display:inline-block}
    .portal-btn{display:inline-block;text-decoration:none;border:0;border-radius:9px;padding:10px 14px;background:#087e9e;color:#fff;font-weight:bold;font-size:12px;cursor:pointer}
    .portal-btn.gold{background:#e9b949;color:#1b2d38}
    .portal-btn.dark{background:#173d55;color:white}
    .portal-controls{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:12px;margin:15px 0}
    .portal-controls label{display:block;font-size:13px;font-weight:bold;margin-bottom:5px}
    .portal-controls select,.portal-controls input{width:100%;padding:12px;border:1px solid #cbd8de;border-radius:9px;background:#fff;color:#173042;font:14px Tahoma}
    .game-box{background:#102b3f;color:#fff;border-radius:15px;padding:20px}
    .game-box h3{color:#70e2ed;margin-top:0}
    .game-options{display:grid;gap:8px;margin-top:12px}
    .game-options button{background:#244b62;color:white;border:1px solid #557387;text-align:right;padding:12px}
    .game-options button:disabled{opacity:.8;cursor:default}
    .game-feedback{min-height:28px;color:#ffd477;font-weight:bold;margin-top:12px}
    .tour-card{overflow:hidden;padding:0}
    .tour-card .tour-head{background:linear-gradient(135deg,#173d55,#087e9e);color:#fff;padding:18px;font-size:24px}
    .tour-card .tour-body{padding:16px}
    .resource-note{background:#fff8e7;border:1px solid #ead49b;border-radius:12px;padding:12px;color:#705a25;font-size:12px;line-height:1.8;margin-top:16px}
    .site-footer{background:#0b2030;color:#d7e6ec;padding:25px;text-align:center;line-height:2}
    .action-icon{display:inline-flex;align-items:center;gap:6px}
    .map-status{position:absolute;z-index:450;right:14px;bottom:14px;max-width:min(340px,calc(100% - 28px));background:rgba(255,255,255,.95);color:#173042;border:1px solid #d9e3e8;border-radius:10px;padding:8px 11px;font-size:11px;line-height:1.7;box-shadow:var(--shadow)}
    .map-status[hidden]{display:none}
    button:focus-visible,a:focus-visible,input:focus-visible,select:focus-visible{outline:3px solid #39c6d6;outline-offset:2px}
    .map-tools-extra{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:8px}
    .map-tools-extra button{min-height:40px}
    .academy-hint{font-size:12px;color:#647987;line-height:1.8;margin-top:10px}
    @media(max-width:800px){.map-status{bottom:10px;right:10px}.map-title{max-width:calc(100% - 20px)}}
    @media(max-width:800px){.quick-nav{grid-template-columns:repeat(3,1fr)}.content-section{padding:34px 15px}.content-section h2{font-size:22px}}
    .map-fallback{padding:24px;text-align:center;color:#173042;background:#fff;border:1px solid #d9e3e8;border-radius:14px;margin:18px;font-size:14px;line-height:1.9}
    .map-fallback a{display:inline-block;margin-top:10px;padding:9px 14px;border-radius:9px;background:#087e9e;color:#fff;text-decoration:none;font-weight:bold}
    .layer input{cursor:pointer}
    .portal-btn, .mini, .quick-nav a{touch-action:manipulation}
    button:focus-visible,a:focus-visible,input:focus-visible{outline:3px solid #e9b949;outline-offset:2px}


    /* مركز المعلم: إضافة تعليمية مدمجة تحافظ على الهوية الأصلية */
    .teacher-hub{background:linear-gradient(135deg,#eef5f7,#f8fafb)}
    .teacher-grid{display:grid;grid-template-columns:minmax(260px,.9fr) minmax(300px,1.1fr);gap:18px;align-items:start}
    .teacher-panel,.teacher-output{background:#fff;border:1px solid var(--line);border-radius:16px;padding:18px;box-shadow:0 8px 24px rgba(11,39,57,.07)}
    .teacher-panel h3,.teacher-output h3{margin:0 0 12px;color:var(--navy)}
    .teacher-form{display:grid;grid-template-columns:1fr 1fr;gap:11px}
    .teacher-form .wide{grid-column:1/-1}
    .teacher-form label{display:block;font-size:12px;font-weight:bold;color:var(--ink);margin-bottom:5px}
    .teacher-form input,.teacher-form select,.teacher-form textarea{width:100%;border:1px solid #cbd9df;border-radius:9px;padding:10px;font:13px Tahoma,Arial,sans-serif;background:#fff;color:var(--ink)}
    .teacher-form textarea{min-height:105px;resize:vertical;line-height:1.8}
    .teacher-form input:focus,.teacher-form select:focus,.teacher-form textarea:focus{outline:3px solid rgba(57,198,214,.2);border-color:var(--blue)}
    .teacher-actions{display:flex;flex-wrap:wrap;gap:8px;margin-top:12px}
    .teacher-actions button{flex:1;min-width:120px}
    .teacher-output{min-height:340px}
    .teacher-placeholder{border:1px dashed #b8cbd4;border-radius:12px;padding:28px 15px;text-align:center;color:var(--muted);line-height:1.9;background:#f8fbfc}
    .plan-sheet{color:#243b49;line-height:1.9;font-size:13px}
    .plan-sheet h3{padding-bottom:8px;border-bottom:2px solid #dbe8ed;margin:0 0 12px}
    .plan-sheet h4{color:#087e9e;margin:13px 0 4px;font-size:13px}
    .plan-sheet ul{margin:4px 0;padding-right:20px}
    .plan-meta{display:grid;grid-template-columns:1fr 1fr;gap:7px;background:#f1f6f8;border-radius:10px;padding:10px;margin-bottom:12px}
    .plan-meta span{font-size:12px}
    .teacher-note{font-size:11px;color:#647987;line-height:1.8;margin-top:10px}
    .tool-strip{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-top:16px}
    .tool-chip{padding:12px;border-radius:12px;background:#fff;border:1px solid var(--line);font-size:12px;line-height:1.7}
    .tool-chip b{display:block;color:var(--navy);font-size:13px;margin-bottom:3px}
    .teacher-status{font-size:12px;color:#087e9e;margin-top:8px;min-height:20px}
    @media(max-width:800px){.teacher-grid{grid-template-columns:1fr}.tool-strip{grid-template-columns:1fr 1fr}}
    @media(max-width:520px){.teacher-form{grid-template-columns:1fr}.teacher-form .wide{grid-column:auto}.teacher-panel,.teacher-output{padding:13px}.tool-strip{grid-template-columns:1fr}}

  </style>
</head>
<body>
  <div class="app">
    <main style="position:relative; min-width:0; grid-column:1; grid-row:1;">
      <div id="map" aria-label="الخريطة التفاعلية لولاية بركاء"></div>
      <div class="map-title">
        🌍 الجغرافيا من حولنا: استكشف العالم بالتقنيات الحديثة
        <small>منصة الجغرافيا الذكية | ولاية بركاء — المدارس والمعالم والبيئة</small>
      </div>
      <div id="mapStatus" class="map-status" role="status" aria-live="polite" hidden></div>
    </main>

    <aside class="sidebar">
      <div class="brand">
        <div class="brand-mark">🗺️</div>
        <div>
          <h1>منصة الجغرافيا الذكية</h1>
          <div class="school">التقنيات الحديثة ونظم المعلومات الجغرافية</div>
          <div class="school">مدرسة ثابت بن قيس للتعليم الأساسي</div>
          <div class="school">إعداد وتصميم: د. قناوي حسين</div>
        </div>
      </div>
      <div class="flag"><span></span><span></span><span></span></div>
      <p class="intro">
        منصة تعليمية تفاعلية للتعرّف إلى التوزيع المكاني للمدارس والمدن
        والمواقع التاريخية والبيئات الطبيعية في سلطنة عُمان.
      </p>
      <div class="section-title">🚀 التنقل السريع</div>
      <div class="quick-nav">
        <a href="#map">🗺️ الخريطة</a>
        <a href="#tours">🏰 الجولات</a>
        <a href="#lab">🛰️ المختبر</a>
        <a href="#academy">🎓 الأكاديمية</a>
        <a href="#teacher-hub">🧑‍🏫 مركز المعلم</a>
        <a href="#games">🎮 الألعاب</a>
        <a href="#platforms">🌐 المنصات</a>
      </div>

      <div class="section-title">🔎 البحث عن معلم جغرافي</div>
      <div class="search-wrap">
        <input id="searchInput" class="search" type="search"
          placeholder="اكتب اسم مدرسة أو مدينة أو معلم..."
          aria-label="البحث عن معلم جغرافي">
      </div>

      <div class="section-title">🧭 طبقات المعلومات الجغرافية</div>
      <div class="layer">
        <input type="checkbox" id="layer-schools" checked>
        <label for="layer-schools"><span class="dot" style="background:#18a9d0"></span>مدارس ولاية بركاء</label>
        <span class="count" id="count-schools">0</span>
      </div>
      <div class="layer">
        <input type="checkbox" id="layer-cities" checked>
        <label for="layer-cities"><span class="dot" style="background:#e8a72e"></span>المدن والمراكز الرئيسية</label>
        <span class="count" id="count-cities">0</span>
      </div>
      <div class="layer">
        <input type="checkbox" id="layer-history" checked>
        <label for="layer-history"><span class="dot" style="background:#e45b5b"></span>المواقع التاريخية والتراثية</label>
        <span class="count" id="count-history">0</span>
      </div>
      <div class="layer">
        <input type="checkbox" id="layer-nature" checked>
        <label for="layer-nature"><span class="dot" style="background:#20a477"></span>البيئة والتضاريس</label>
        <span class="count" id="count-nature">0</span>
      </div>
      <div class="section-title">🗺️ نمط الخريطة</div>
      <div class="btn-grid">
        <button class="btn-secondary" id="darkBtn">النمط الداكن</button>
        <button class="btn-secondary" id="lightBtn">النمط الفاتح</button>
        <button class="btn-secondary" id="streetBtn">الشوارع</button>
        <button class="btn-secondary" id="resetBtn">العودة إلى بركاء</button>
      </div>

      <div class="section-title">📌 أدوات الخريطة</div>
      <button class="btn-primary full" id="showAllBtn">إظهار جميع المعالم</button>
      <button class="btn-gold full" id="clearSearchBtn">مسح البحث</button>
      <div class="map-tools-extra">
        <button class="btn-secondary" id="focusSchoolBtn" type="button">📍 مدرسة ثابت بن قيس</button>
        <button class="btn-secondary" id="fitVisibleBtn" type="button">⤢ احتواء المعالم</button>
      </div>

      <div class="stats">
        <span style="font-size:12px;color:#d7e6ec">إجمالي المعالم الظاهرة</span>
        <strong id="total-count">0</strong>
      </div>
      <div class="legend">
        اضغط على أي نقطة في الخريطة لعرض اسم المعلم ووصفه وصورة توضيحية.
        استخدم مربعات الاختيار لإظهار الطبقات أو إخفائها.
      </div>
      <div class="notice">
        تنبيه: إحداثيات بعض المدارس والمعالم في هذه النسخة تقريبية ومخصصة للاستكشاف التعليمي، وليست مرجعًا مساحيًا رسميًا. يُرجى مراجعتها ميدانيًا أو عبر مصدر رسمي قبل استخدامها في
        التقارير أو القرارات الرسمية. الصور المعروضة توضيحية وليست بالضرورة صورًا للموقع نفسه.
        <br>الخرائط: OpenStreetMap وCARTO.
      </div>
    </aside>
  </div>

  <section class="content-section" id="tours">
    <h2>🏰 جولات استكشافية في عُمان</h2>
    <p class="section-lead">استكشف مواقع عُمانية تاريخية وافتح مواقعها وموادها المرئية المتاحة. الروابط الخارجية تعرض نتائج البحث والخرائط، ولا تعني توافر جولة بانورامية رسمية لكل موقع.</p>
    <div class="cards-grid">
      <article class="portal-card tour-card"><div class="tour-head">🏰 حصن بركاء</div><div class="tour-body"><p>معلم تاريخي بارز في ولاية بركاء.</p><a class="portal-btn" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=حصن+بركاء+عمان">افتح الموقع على الخريطة ↗</a> <a class="portal-btn dark" target="_blank" rel="noopener" href="https://www.youtube.com/results?search_query=حصن+بركاء+عمان">فيديوهات الموقع ↗</a> <button class="portal-btn gold" type="button" onclick="window.explorePlace &amp;&amp; window.explorePlace('حصن بركاء')">حدده على الخريطة</button></div></article>
      <article class="portal-card tour-card"><div class="tour-head">🏛️ بيت النعمان</div><div class="tour-body"><p>معلم تراثي في بركاء يعكس العمارة العُمانية.</p><a class="portal-btn" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=بيت+النعمان+بركاء+عمان">افتح الموقع على الخريطة ↗</a> <a class="portal-btn dark" target="_blank" rel="noopener" href="https://www.youtube.com/results?search_query=بيت+النعمان+بركاء">فيديوهات الموقع ↗</a> <button class="portal-btn gold" type="button" onclick="window.explorePlace &amp;&amp; window.explorePlace('بيت النعمان')">حدده على الخريطة</button></div></article>
      <article class="portal-card tour-card"><div class="tour-head">🕌 قلعة نزوى</div><div class="tour-body"><p>من أشهر المعالم التاريخية في سلطنة عُمان.</p><a class="portal-btn" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=قلعة+نزوى+عمان">افتح الموقع على الخريطة ↗</a> <a class="portal-btn dark" target="_blank" rel="noopener" href="https://www.youtube.com/results?search_query=قلعة+نزوى+جولة">فيديوهات الموقع ↗</a> <button class="portal-btn gold" type="button" onclick="window.explorePlace &amp;&amp; window.explorePlace('قلعة نزوى')">حددها على الخريطة</button></div></article>
      <article class="portal-card tour-card"><div class="tour-head">🏯 قلعة بهلاء</div><div class="tour-body"><p>موقع عُماني مدرج على قائمة التراث العالمي.</p><a class="portal-btn" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=قلعة+بهلاء+عمان">افتح الموقع على الخريطة ↗</a> <a class="portal-btn dark" target="_blank" rel="noopener" href="https://www.youtube.com/results?search_query=قلعة+بهلاء+جولة">فيديوهات الموقع ↗</a> <button class="portal-btn gold" type="button" onclick="window.explorePlace &amp;&amp; window.explorePlace('قلعة بهلاء')">حددها على الخريطة</button></div></article>
    </div>
  </section>

  <section class="content-section" id="lab">
    <h2>🛰️ مختبر الجغرافيا الحديثة</h2>
    <p class="section-lead">أدوات خارجية موثوقة للاستكشاف الجغرافي والصور الفضائية والخرائط الرقمية.</p>
    <div class="cards-grid">
      <article class="portal-card"><span class="portal-icon">🛰️</span><h3>NASA Worldview</h3><p>استكشاف صور الأقمار الصناعية والظواهر على سطح الأرض.</p><a class="portal-btn" href="https://worldview.earthdata.nasa.gov/" target="_blank" rel="noopener">افتح المختبر ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🌍</span><h3>Copernicus Browser</h3><p>استعراض صور الأقمار الصناعية وبيانات برنامج كوبرنيكوس.</p><a class="portal-btn" href="https://browser.dataspace.copernicus.eu/" target="_blank" rel="noopener">افتح المستكشف ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🗺️</span><h3>Google Earth</h3><p>استكشاف سطح الأرض والمواقع من منظور ثلاثي الأبعاد.</p><a class="portal-btn" href="https://earth.google.com/web/" target="_blank" rel="noopener">ابدأ الاستكشاف ↗</a></article>
      <article class="portal-card"><span class="portal-icon">📡</span><h3>ArcGIS</h3><p>خرائط تفاعلية وموارد تعليمية في نظم المعلومات الجغرافية.</p><a class="portal-btn" href="https://www.arcgis.com/" target="_blank" rel="noopener">افتح ArcGIS ↗</a></article>
    </div>
  </section>

  <section class="content-section" id="academy">
    <h2>🎓 أكاديمية ثابت بن قيس التعليمية</h2>
    <p class="section-lead">بوابة للمناهج العُمانية من الصف الأول إلى الثاني عشر. اختر الصف والمادة للوصول إلى المكتبة الرسمية، مع عينات دروس تفاعلية قابلة للتوسّع.</p>
    <div class="portal-controls">
      <div><label for="gradeSelect">الصف الدراسي</label><select id="gradeSelect"></select></div>
      <div><label for="subjectSelect">المادة</label><select id="subjectSelect"></select></div>
      <div><label for="termSelect">الفصل الدراسي</label><select id="termSelect"><option>الفصل الأول</option><option>الفصل الثاني</option></select></div>
    </div>
    <div class="cards-grid" id="curriculumCards"></div>
    <div class="resource-note">ملاحظة: الروابط تقود إلى مكتبة المناهج الرسمية أو موقع وزارة التعليم. لا تدّعي المنصة أن جميع الكتب أو الدروس مخزنة داخلها؛ ويجب اعتماد النسخة المنشورة رسميًا لكل عام دراسي.</div>
    <p class="academy-hint">للوصول إلى كتاب صفك: اختر الصف والمادة والفصل، ثم افتح مكتبة المناهج الرسمية. عناوين الدروس النموذجية المعروضة هنا إرشادية، ويجب مطابقتها مع كتاب العام الدراسي المعتمد.</p>
  </section>


  <section class="content-section teacher-hub" id="teacher-hub">
    <h2>🧑‍🏫 مركز المعلم الذكي</h2>
    <p class="section-lead">مساحة عملية لإعداد مسودة تحضير الدرس، وبناء أنشطة وأوراق عمل وأسئلة تقويمية انطلاقًا من محتوى الدرس الذي تدخله. صُممت لتناسب تنظيم الدروس في المناهج العُمانية.</p>
    <div class="teacher-grid">
      <div class="teacher-panel">
        <h3>📘 بيانات الدرس</h3>
        <div class="teacher-form">
          <div><label for="thGrade">الصف الدراسي</label><select id="thGrade"></select></div>
          <div><label for="thTerm">الفصل الدراسي</label><select id="thTerm"><option>الفصل الأول</option><option>الفصل الثاني</option></select></div>
          <div><label for="thSubject">المادة</label><select id="thSubject"></select></div>
          <div><label for="thDuration">زمن الحصة</label><select id="thDuration"><option>40 دقيقة</option><option>45 دقيقة</option><option>50 دقيقة</option><option>60 دقيقة</option></select></div>
          <div class="wide"><label for="thTitle">عنوان الدرس</label><input id="thTitle" placeholder="مثال: المنجزات الحضارية في المجال الثقافي"></div>
          <div><label for="thMethod">استراتيجية التدريس</label><select id="thMethod"><option>التعلم التعاوني</option><option>التعلم بالاكتشاف</option><option>الحوار والمناقشة</option><option>التعلم القائم على المشروعات</option><option>حل المشكلات</option><option>التعلم المدمج</option></select></div>
          <div><label for="thLevel">مستوى المتعلمين</label><select id="thLevel"><option>متنوع</option><option>بحاجة إلى دعم</option><option>متوسط</option><option>متقدم</option></select></div>
          <div class="wide"><label for="thContent">محتوى الدرس أو نص من الكتاب</label><textarea id="thContent" placeholder="الصق هنا الفقرة أو الأفكار الرئيسة من الكتاب المدرسي؛ كلما كان النص أدق كانت المسودة أكثر ارتباطًا بالدرس."></textarea></div>
          <div class="wide"><label for="thPrior">المعارف السابقة (اختياري)</label><input id="thPrior" placeholder="ما الذي يعرفه الطلاب مسبقًا؟"></div>
        </div>
        <div class="teacher-actions">
          <button class="btn-primary" id="makePlan" type="button">إنشاء مسودة التحضير</button>
          <button class="btn-secondary" id="clearPlan" type="button">مسح البيانات</button>
        </div>
        <div class="teacher-status" id="teacherStatus" role="status" aria-live="polite"></div>
        <p class="teacher-note">تنبيه تقني: هذه نسخة تعمل داخل المتصفح وتُنشئ قالبًا منظمًا اعتمادًا على البيانات المدخلة. لا تدّعي أنها تقرأ صور صفحات الكتاب أو تستخدم نموذج ذكاء اصطناعي خارجيًا. تفعيل قراءة الصور والتوليد الذكي الحقيقي يتطلب خادمًا آمنًا وخدمة OCR وواجهة ذكاء اصطناعي، ولا ينبغي وضع مفاتيحها السرية داخل GitHub Pages.</p>
      </div>
      <div class="teacher-output">
        <h3>🗂️ مسودة التحضير والموارد</h3>
        <div id="planOutput" class="teacher-placeholder">أدخل بيانات الدرس ثم اضغط «إنشاء مسودة التحضير». ستظهر هنا خطة منظمة قابلة للنسخ والطباعة والحفظ.</div>
        <div class="teacher-actions" id="planActions" hidden>
          <button class="btn-primary" id="copyPlan" type="button">نسخ التحضير</button>
          <button class="btn-secondary" id="printPlan" type="button">طباعة / حفظ PDF</button>
          <button class="btn-gold" id="downloadPlan" type="button">تحميل ملف Word</button>
        </div>
      </div>
    </div>
    <div class="tool-strip">
      <div class="tool-chip"><b>📝 خطة درس منظمة</b>تمهيد، أهداف، إجراءات، تقويم، وغلق الدرس.</div>
      <div class="tool-chip"><b>🎯 نواتج تعلم</b>صياغات مقترحة قابلة للتعديل حسب محتوى الكتاب.</div>
      <div class="tool-chip"><b>📋 ورقة عمل</b>أسئلة فهم وتطبيق ومهام قصيرة مرتبطة بالنص المدخل.</div>
      <div class="tool-chip"><b>🧠 تقويم متنوع</b>أسئلة مباشرة وأسئلة تفكير أعلى مع مساحة للإجابة.</div>
      <div class="tool-chip"><b>♿ مراعاة الفروق</b>أفكار دعم وإثراء بحسب مستوى المتعلمين.</div>
      <div class="tool-chip"><b>📤 مخرجات قابلة للحفظ</b>نسخ أو طباعة أو تنزيل مستند قابل للتحرير.</div>
    </div>
  </section>

  <section class="content-section" id="games">
    <h2>🎮 مدينة الألعاب التعليمية</h2>
    <p class="section-lead">أنشطة قصيرة تعمل داخل الموقع وتمنح المتعلم تغذية راجعة فورية.</p>
    <div class="cards-grid">
      <article class="game-box"><h3>🏆 تحدي المستكشف الجغرافي</h3><p>أجب عن الأسئلة لتحصل على نتيجتك.</p><div id="gameQuestion">اضغط ابدأ لبدء التحدي.</div><div class="game-options" id="gameOptions"></div><div class="game-feedback" id="gameFeedback" role="status" aria-live="polite"></div><button class="btn-gold full" id="startGame">ابدأ اللعبة</button><button class="btn-secondary full" id="nextGame" hidden>السؤال التالي</button></article>
      <article class="game-box"><h3>🧭 رتّب مراحل إنتاج الخريطة</h3><p>اختر المراحل بالترتيب الصحيح.</p><div class="portal-controls" style="display:block"><select id="order1" aria-label="ترتيب المرحلة 1"></select><select id="order2" aria-label="ترتيب المرحلة 2"></select><select id="order3" aria-label="ترتيب المرحلة 3"></select><select id="order4" aria-label="ترتيب المرحلة 4"></select></div><button class="btn-gold full" id="checkOrder">تحقق من الإجابة</button><div class="game-feedback" id="orderFeedback" role="status" aria-live="polite"></div></article>
      <article class="game-box"><h3>📍 خمن الموقع</h3><p>أنا ولاية ساحلية في محافظة جنوب الباطنة، فمن أكون؟</p><button class="btn-gold full" id="guessBarka">بركاء</button><button class="btn-secondary full" id="guessOther">نزوى</button><div class="game-feedback" id="guessFeedback"></div></article>
    </div>
  </section>

  <section class="content-section" id="platforms">
    <h2>🌐 بوابة المنصات التعليمية</h2>
    <p class="section-lead">روابط إلى بوابات عُمانية ومصادر عالمية مساندة للطالب والمعلم.</p>
    <div class="cards-grid">
      <article class="portal-card"><span class="portal-icon">🇴🇲</span><h3>وزارة التعليم العُمانية</h3><p>الموقع الرسمي للخدمات والأخبار والموارد التعليمية.</p><a class="portal-btn" href="https://home.moe.gov.om/" target="_blank" rel="noopener">دخول الموقع الرسمي ↗</a></article>
      <article class="portal-card"><span class="portal-icon">📚</span><h3>مكتبة المناهج العُمانية</h3><p>الوصول إلى الكتب المدرسية والمناهج المنشورة رسميًا.</p><a class="portal-btn" href="https://home.moe.gov.om/ar/manaheg/index.moe" target="_blank" rel="noopener">استعرض المناهج ↗</a></article>
      <article class="portal-card"><span class="portal-icon">📱</span><h3>منصة نور التعليمية</h3><p>تطبيق تعليمي تابع لوزارة التعليم العُمانية؛ يتطلب حسابًا مدرسيًا مفعّلًا.</p><a class="portal-btn" href="https://play.google.com/store/apps/details?hl=ar&amp;id=com.app.classera.noorlms" target="_blank" rel="noopener">عرض التطبيق الرسمي ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🏫</span><h3>منصة منظرة</h3><p>منصة الوزارة التعليمية للصفوف 1–4، للدروس والأنشطة والاختبارات.</p><a class="portal-btn" href="https://eportal.moe.gov.om/egov/Login?returnurl=%2Fegov" target="_blank" rel="noopener">دخول منصة منظرة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🧾</span><h3>البوابة التعليمية العُمانية</h3><p>بوابة الخدمات والأنظمة التعليمية التابعة لوزارة التعليم.</p><a class="portal-btn" href="https://edugate.moe.gov.om/" target="_blank" rel="noopener">فتح البوابة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🧑‍🏫</span><h3>Google Classroom</h3><p>إدارة الصفوف والواجبات والمواد التعليمية.</p><a class="portal-btn" href="https://classroom.google.com/" target="_blank" rel="noopener">افتح المنصة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">💻</span><h3>Microsoft Teams</h3><p>التواصل والاجتماعات والتعاون داخل الصف.</p><a class="portal-btn" href="https://teams.microsoft.com/" target="_blank" rel="noopener">افتح المنصة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🧩</span><h3>Kahoot!</h3><p>إنشاء مسابقات واختبارات صفية تفاعلية.</p><a class="portal-btn" href="https://kahoot.com/" target="_blank" rel="noopener">افتح Kahoot ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🎨</span><h3>Canva للتعليم</h3><p>تصميم العروض والملصقات والأنشطة التعليمية.</p><a class="portal-btn" href="https://www.canva.com/education/" target="_blank" rel="noopener">افتح Canva ↗</a></article>
    </div>
  </section>

  <footer class="site-footer">منصة الجغرافيا الذكية | مدرسة ثابت بن قيس للتعليم الأساسي – سلطنة عُمان<br>إعداد وتصميم: د. قناوي حسين | © 2026</footer>

  <script>
  // تحميل Leaflet من مصدرين بالتتابع حتى لا تتوقف الخريطة إذا تعطل أحدهما.
  function loadLeafletThenStart() {
    if (window.L) { startGeographicMap(); return; }
    const sources = [
      'https://cdn.jsdelivr.net/npm/leaflet@1.9.4/dist/leaflet.js',
      'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js'
    ];
    let index = 0;
    const tryNext = () => {
      if (window.L) { startGeographicMap(); return; }
      if (index >= sources.length) {
        document.getElementById('map').innerHTML = '<div class="map-fallback"><strong>تعذر تحميل الخريطة التفاعلية.</strong><br>تحقق من اتصال الإنترنت أو افتح خريطة بركاء الخارجية.<br><a href="https://www.openstreetmap.org/#map=12/23.7042/57.8860" target="_blank" rel="noopener">فتح خريطة بركاء</a></div>';
        return;
      }
      const script = document.createElement('script');
      script.src = sources[index++];
      script.onload = () => window.L ? startGeographicMap() : tryNext();
      script.onerror = tryNext;
      document.head.appendChild(script);
    };
    tryNext();
  }

  function startGeographicMap() {
    if (window.__barkaMapStarted) return;
    window.__barkaMapStarted = true;
    // مركز الخريطة الافتراضي: ولاية بركاء
    const map = L.map('map', { zoomControl: true, tap: true }).setView([23.7042, 57.8860], 11);

    const baseLayers = {
      light: L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png', {
        attribution: '&copy; OpenStreetMap &copy; CARTO', maxZoom: 19
      }),
      dark: L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
        attribution: '&copy; OpenStreetMap &copy; CARTO', maxZoom: 19
      }),
      streets: L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap contributors', maxZoom: 19
      })
    };
    let activeBase = baseLayers.light.addTo(map);
    const mapStatus = document.getElementById('mapStatus');
    function announceMap(message, show=true) {
      mapStatus.textContent = message;
      mapStatus.hidden = !show;
    }
    let tileErrorCount = 0;
    function watchTiles(layer, type) {
      tileErrorCount = 0;
      layer.on('tileerror', () => {
        tileErrorCount++;
        if (tileErrorCount >= 4 && type !== 'streets') {
          tileErrorCount = 0;
          if (map.hasLayer(activeBase)) map.removeLayer(activeBase);
          activeBase = baseLayers.streets;
          activeBase.addTo(map);
          announceMap('تعذر تحميل بعض مربعات نمط الخريطة؛ تم التحويل تلقائيًا إلى خريطة الشوارع.');
        } else if (tileErrorCount >= 4 && type === 'streets') {
          announceMap('تعذر تحميل خلفية الخريطة. تحقق من اتصال الإنترنت؛ ويمكنك متابعة استخدام أدوات البحث والبيانات.');
        }
      });
    }
    watchTiles(activeBase, 'light');

    const colors = {
      schools: '#18a9d0',
      cities: '#e8a72e',
      history: '#e45b5b',
      nature: '#20a477'
    };

    // الصور توضيحية عامة للفئة وليست صورًا موثقة لكل موقع.
    const pictures = {
      schools: 'https://images.unsplash.com/photo-1580582932707-520aed937b7b?w=700',
      cities: 'https://images.unsplash.com/photo-1518005020951-eccb494ad742?w=700',
      history: 'https://images.unsplash.com/photo-1590736969955-71cc94901144?w=700',
      nature: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=700'
    };

    const categoryNames = {
      schools: 'مدرسة',
      cities: 'مدينة أو مركز سكاني',
      history: 'موقع تاريخي أو تراثي',
      nature: 'معلم طبيعي'
    };

    const geoData = {
      schools: [
        { coords:[23.7050,57.8820], name:'مدرسة ثابت بن قيس للتعليم الأساسي', desc:'صرح تعليمي في ولاية بركاء، ويمكن توظيف الموقع في الأنشطة التعليمية الرقمية والجغرافية.' },
        { coords:[23.6980,57.8750], name:'مدرسة بركاء للتعليم الأساسي', desc:'مؤسسة تعليمية في ولاية بركاء.' },
        { coords:[23.7120,57.8950], name:'مدرسة العلاء بن الحضرمي', desc:'مدرسة تخدم قطاع التعليم الأساسي في الولاية.' },
        { coords:[23.6850,57.8600], name:'مدرسة أم عمارة للبنات', desc:'مؤسسة تعليمية للبنات في ولاية بركاء.' },
        { coords:[23.6720,57.9100], name:'مدرسة الرميس للتعليم الأساسي', desc:'مدرسة تخدم المناطق الشرقية من ولاية بركاء.' },
        { coords:[23.6900,57.8700], name:'مدرسة أسماء بنت النعمان', desc:'مدرسة ضمن نطاق بركاء؛ الموقع المعروض تقريبي ويحتاج مراجعة ميدانية.' },
        { coords:[23.7150,57.9000], name:'مدرسة دار الفكر', desc:'مؤسسة تعليمية أُدرجت ضمن نطاق الاستكشاف؛ يرجى التحقق من موضعها الدقيق.' },
        { coords:[23.6800,57.8950], name:'مدارس التكوين الخاصة', desc:'مؤسسة تعليمية أُدرجت ضمن نطاق بركاء؛ الإحداثيات تقريبية.' },
        { coords:[23.7000,57.9150], name:'مدرسة بركات الثانوية', desc:'مؤسسة تعليمية أُدرجت ضمن نطاق الاستكشاف؛ يرجى التحقق من موضعها الدقيق.' },
        { coords:[23.6650,57.8850], name:'مدرسة زهور عُمان', desc:'مؤسسة تعليمية أُدرجت ضمن نطاق بركاء؛ الإحداثيات تقريبية.' }
      ],
      cities: [
        { coords:[23.7072,57.8861], name:'مركز ولاية بركاء', desc:'مركز عمراني في محافظة جنوب الباطنة، وتشتهر الولاية بالأنشطة الساحلية والزراعية والتجارية.' },
        { coords:[23.5880,58.3829], name:'مسقط — العاصمة', desc:'عاصمة سلطنة عُمان ومركزها الإداري والاقتصادي.' },
        { coords:[23.3959,57.4244], name:'الرستاق', desc:'مدينة تاريخية ومركز محافظة جنوب الباطنة.' },
        { coords:[24.3474,56.7094], name:'صحار', desc:'مدينة رئيسية في محافظة شمال الباطنة ومركز صناعي وتجاري مهم.' }
      ],
      history: [
        { coords:[23.7114,57.8878], name:'حصن بركاء', desc:'معلم تاريخي بارز في ولاية بركاء، يرتبط بتاريخ الولاية ودورها الساحلي.' },
        { coords:[23.6961,57.9044], name:'بيت النعمان', desc:'معلم تراثي في ولاية بركاء يعكس جانبًا من العمارة العُمانية التقليدية.' },
        { coords:[22.9333,57.5333], name:'قلعة نزوى', desc:'من أشهر القلاع التاريخية في سلطنة عُمان، وتتميز ببرجها الدائري.' },
        { coords:[22.9640,57.3040], name:'قلعة بهلاء', desc:'قلعة تاريخية في ولاية بهلاء، ضمن موقع بهلاء المدرج على قائمة التراث العالمي.' },
        { coords:[23.7100,57.8900], name:'برج الجنينة', desc:'معلم تراثي في نطاق بركاء؛ موضع العلامة تقريبي ويحتاج إلى توثيق ميداني.' },
        { coords:[23.7000,57.8800], name:'سور حي عاصم', desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.' },
        { coords:[23.6900,57.9050], name:'سور الثرامد', desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.' },
        { coords:[23.6800,57.8750], name:'سور الموالك', desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.' }
      ],
      nature: [
        { coords:[23.7810,57.7720], name:'جزر الديمانيات الطبيعية', desc:'محمية بحرية تضم جزرًا ذات قيمة بيئية، وتشتهر بالشعاب المرجانية والحياة البحرية.' },
        { coords:[23.5500,57.8500], name:'منطقة الأودية والمزارع جنوب بركاء', desc:'منطقة تتنوع فيها المظاهر الطبيعية والأنشطة الزراعية.' },
        { coords:[23.1450,57.2650], name:'الجبل الأخضر', desc:'منطقة جبلية ضمن جبال الحجر، تشتهر بتنوعها الطبيعي ومدرجاتها الزراعية.' }
      ]
    };

    const layers = {
      schools: L.layerGroup().addTo(map),
      cities: L.layerGroup().addTo(map),
      history: L.layerGroup().addTo(map),
      nature: L.layerGroup().addTo(map)
    };

    function escapeHtml(value) {
      return String(value).replace(/[&<>"']/g, ch => ({
        '&':'&amp;', '<':'&lt;', '>':'&gt;', '"':'&quot;', "'":'&#39;'
      }[ch]));
    }

    function popupContent(item, category) {
      return `
        <div class="popup">
          <img src="${pictures[category]}" alt="صورة توضيحية"
               onerror="this.style.display='none'">
          <div class="popup-body">
            <h3>${escapeHtml(item.name)}</h3>
            <p>${escapeHtml(item.desc)}</p>
            <span class="type">${categoryNames[category]}</span>
          </div>
        </div>`;
    }

    function normalizeArabic(value) {
      return String(value || '').toLowerCase()
        .replace(/[\u064B-\u065F\u0670]/g, '')
        .replace(/[إأآ]/g, 'ا').replace(/ى/g, 'ي').replace(/ة/g, 'ه')
        .replace(/ـ/g, '').trim();
    }

    let visibleMarkers = [];
    function renderFeatures() {
      const query = normalizeArabic(document.getElementById('searchInput').value);
      let totalVisible = 0;
      visibleMarkers = [];

      Object.keys(geoData).forEach(category => {
        layers[category].clearLayers();
        let count = 0;

        geoData[category].forEach(item => {
          const searchable = normalizeArabic(item.name + ' ' + item.desc);
          const matches = !query || searchable.includes(query);

          if (!matches) return;

          const marker = L.circleMarker(item.coords, {
            radius: 9, fillColor: colors[category], color: '#ffffff',
            weight: 2, opacity: 1, fillOpacity: .95
          });
          marker.bindPopup(popupContent(item, category));
          marker.addTo(layers[category]);
          visibleMarkers.push(marker);
          count++;
        });

        document.getElementById('count-' + category).textContent = count;
        const checkbox = document.getElementById('layer-' + category);
        if (checkbox.checked) {
          if (!map.hasLayer(layers[category])) layers[category].addTo(map);
          totalVisible += count;
        } else if (map.hasLayer(layers[category])) {
          map.removeLayer(layers[category]);
        }
      });

      document.getElementById('total-count').textContent = totalVisible;
      if (query && visibleMarkers.length === 0) announceMap('لم نعثر على نتيجة مطابقة. جرّب اسمًا آخر أو اضغط «مسح البحث».');
      else if (query) announceMap('نتائج البحث: ' + visibleMarkers.length + ' معلم.', true);
      else if (!mapStatus.textContent.includes('تعذر تحميل')) announceMap('', false);
    }

    function toggleLayer(category) {
      const checkbox = document.getElementById('layer-' + category);
      if (checkbox.checked) layers[category].addTo(map);
      else map.removeLayer(layers[category]);
      updateTotalCount();
    }

    function updateTotalCount() {
      let total = 0;
      Object.keys(layers).forEach(category => {
        if (document.getElementById('layer-' + category).checked) {
          total += Number(document.getElementById('count-' + category).textContent) || 0;
        }
      });
      document.getElementById('total-count').textContent = total;
    }

    function changeBasemap(type) {
      if (map.hasLayer(activeBase)) map.removeLayer(activeBase);
      activeBase = baseLayers[type];
      activeBase.addTo(map);
      watchTiles(activeBase, type);
      announceMap('تم تغيير نمط الخريطة إلى ' + ({dark:'داكن',light:'فاتح',streets:'الشوارع'}[type] || type) + '.', true);
      setTimeout(() => announceMap('', false), 3500);
    }

    function resetMapView() {
      map.setView([23.7042, 57.8860], 11);
      setTimeout(() => map.invalidateSize(), 120);
    }

    function fitVisibleFeatures() {
      const active = [];
      Object.keys(layers).forEach(category => {
        if (document.getElementById('layer-' + category).checked) {
          layers[category].eachLayer(layer => active.push(layer));
        }
      });
      if (active.length) {
        const group = L.featureGroup(active);
        map.fitBounds(group.getBounds().pad(0.12), { maxZoom: 13 });
      } else {
        resetMapView();
      }
    }
    function focusSchool() {
      const school = geoData.schools.find(item => normalizeArabic(item.name).includes(normalizeArabic('مدرسة ثابت بن قيس')));
      if (!school) { announceMap('لم يتم العثور على موقع المدرسة في بيانات الخريطة.'); return; }
      document.getElementById('searchInput').value = 'مدرسة ثابت بن قيس';
      Object.keys(layers).forEach(category => { document.getElementById('layer-' + category).checked = category === 'schools'; });
      renderFeatures();
      map.setView(school.coords, 15, { animate: true });
      layers.schools.eachLayer(layer => {
        const ll = layer.getLatLng && layer.getLatLng();
        if (ll && Math.abs(ll.lat-school.coords[0]) < 0.0001 && Math.abs(ll.lng-school.coords[1]) < 0.0001) layer.openPopup();
      });
      announceMap('موقع المدرسة المعروض تقريبي للاستكشاف التعليمي، ويحتاج إلى تأكيد ميداني قبل الاستخدام الرسمي.');
    }

    window.explorePlace = function(name) {
      document.getElementById('searchInput').value = name;
      Object.keys(layers).forEach(category => { document.getElementById('layer-' + category).checked = true; });
      renderFeatures();
      fitVisibleFeatures();
      document.getElementById('map').scrollIntoView({behavior:'smooth',block:'center'});
      const match = Object.keys(geoData).flatMap(category => geoData[category].map(item => ({item,category})))
        .find(entry => normalizeArabic(entry.item.name).includes(normalizeArabic(name)));
      if (match) {
        setTimeout(() => {
          layers[match.category].eachLayer(layer => {
            const ll = layer.getLatLng && layer.getLatLng();
            if (ll && Math.abs(ll.lat-match.item.coords[0]) < 0.0001 && Math.abs(ll.lng-match.item.coords[1]) < 0.0001) layer.openPopup();
          });
        }, 350);
      }
    };

    document.getElementById('searchInput').addEventListener('input', renderFeatures);
    Object.keys(layers).forEach(category => {
      document.getElementById('layer-' + category)
        .addEventListener('change', () => toggleLayer(category));
    });

    document.getElementById('darkBtn').addEventListener('click', () => changeBasemap('dark'));
    document.getElementById('lightBtn').addEventListener('click', () => changeBasemap('light'));
    document.getElementById('streetBtn').addEventListener('click', () => changeBasemap('streets'));
    document.getElementById('resetBtn').addEventListener('click', resetMapView);
    document.getElementById('showAllBtn').addEventListener('click', () => {
      document.getElementById('searchInput').value = '';
      Object.keys(layers).forEach(category => {
        document.getElementById('layer-' + category).checked = true;
      });
      renderFeatures();
      fitVisibleFeatures();
    });
    document.getElementById('clearSearchBtn').addEventListener('click', () => {
      document.getElementById('searchInput').value = '';
      renderFeatures();
      document.getElementById('searchInput').focus();
    });
    document.getElementById('focusSchoolBtn').addEventListener('click', focusSchool);
    document.getElementById('fitVisibleBtn').addEventListener('click', fitVisibleFeatures);
    document.getElementById('searchInput').addEventListener('keydown', event => {
      if (event.key === 'Escape') {
        event.currentTarget.value = '';
        renderFeatures();
      }
      if (event.key === 'Enter') fitVisibleFeatures();
    });

    renderFeatures();
    // إصلاح أبعاد الخريطة عند فتح الصفحة أو تغيير حجم شاشة الهاتف.
    setTimeout(() => map.invalidateSize(), 250);
    window.addEventListener('resize', () => setTimeout(() => map.invalidateSize(), 120));
    document.querySelectorAll('.quick-nav a[href="#map"]').forEach(link => link.addEventListener('click', () => setTimeout(() => map.invalidateSize(), 250)));
    }
  loadLeafletThenStart();
  </script>
  <script>
  // جميع أدوات الأكاديمية والألعاب مرتبطة بأحداث فعلية، وليست أيقونات شكلية.
  (() => {
    const grades = Array.from({length:12},(_,i)=>i+1);
    const subjects = ['اللغة العربية','التربية الإسلامية','الرياضيات','العلوم','الدراسات الاجتماعية','اللغة الإنجليزية','تقنية المعلومات','الفنون','الرياضة'];
    const gradeSelect=document.getElementById('gradeSelect');
    const subjectSelect=document.getElementById('subjectSelect');
    grades.forEach(g=>gradeSelect.add(new Option('الصف '+g,g)));
    subjects.forEach(s=>subjectSelect.add(new Option(s,s)));
    const officialBooks='https://home.moe.gov.om/ar/manaheg/index.moe';
    const sampleLessons={
      '7|الدراسات الاجتماعية':['الخريطة وأهمية الخريطة','مراحل إنتاج الخريطة'],
      '8|الدراسات الاجتماعية':['الطقس والمناخ','عُمان والعصر العباسي','قيام الدولة العباسية واتساعها','المنجزات الحضارية في المجال الثقافي','عُمان ومقاومة الاحتلال البرتغالي','المنجزات الحضارية العُمانية']
    };
    function renderCurriculum(){
      const g=gradeSelect.value,s=subjectSelect.value,t=document.getElementById('termSelect').value;
      const items=sampleLessons[g+'|'+s]||[];
      const cards=document.getElementById('curriculumCards');
      cards.innerHTML='';
      const main=document.createElement('article');main.className='portal-card';
      main.innerHTML=`<span class="portal-icon">📘</span><h3>الصف ${g} – ${s}</h3><p>${t}. انتقل إلى المكتبة الرسمية لاختيار الكتاب المعتمد.</p><a class="portal-btn" href="${officialBooks}" target="_blank" rel="noopener">فتح كتب الصفوف ↗</a>`;
      cards.appendChild(main);
      if(items.length){items.forEach((lesson,i)=>{const card=document.createElement('article');card.className='portal-card';card.innerHTML=`<span class="portal-icon">📝</span><h3>${lesson}</h3><p>نموذج عنوان درس من موضوعات الدراسات الاجتماعية.</p><button class="portal-btn dark" type="button" data-lesson="${i}">عرض فكرة الدرس</button><p class="lesson-msg" hidden>يمكن إضافة شرح الدرس والأنشطة وأسئلة التقويم بعد مطابقة المحتوى مع الكتاب الرسمي.</p>`;card.querySelector('button').addEventListener('click',()=>{const m=card.querySelector('.lesson-msg');m.hidden=!m.hidden});cards.appendChild(card)});}
      else {const card=document.createElement('article');card.className='portal-card';card.innerHTML='<span class="portal-icon">📚</span><h3>محتوى الصف قيد الإثراء</h3><p>اختر الكتاب من مكتبة وزارة التعليم. ستضاف الدروس التفاعلية بعد توثيق محتواها.</p>';cards.appendChild(card)}
    }
    [gradeSelect,subjectSelect,document.getElementById('termSelect')].forEach(el=>el.addEventListener('change',renderCurriculum));renderCurriculum();

    const questions=[
      {q:'في أي محافظة تقع ولاية بركاء؟',a:['جنوب الباطنة','ظفار','مسندم','الداخلية'],c:0},
      {q:'ما الأداة التي تمثل سطح الأرض أو جزءًا منه؟',a:['الخريطة','الساعة','المجهر','المسطرة'],c:0},
      {q:'ما التقنية التي تدرس الأرض من مسافة؟',a:['الاستشعار عن بُعد','الكتابة','الطباعة','النسخ'],c:0},
      {q:'أي موقع من الآتي يقع في بركاء؟',a:['حصن بركاء','قلعة نزوى','قلعة بهلاء','حصن جبرين'],c:0},
      {q:'ما فائدة نظم المعلومات الجغرافية؟',a:['تحليل البيانات المكانية','تشغيل الموسيقى','تحرير النصوص فقط','قياس الزمن'],c:0},
      {q:'ما العنصر الذي يوضح مدلولات الرموز على الخريطة؟',a:['مفتاح الخريطة','الإطار','العنوان فقط','مقياس الحرارة'],c:0}
    ];
    let qi=0,score=0,locked=false;
    const qBox=document.getElementById('gameQuestion'),opts=document.getElementById('gameOptions'),feedback=document.getElementById('gameFeedback'),next=document.getElementById('nextGame'),start=document.getElementById('startGame');
    function showQuestion(){locked=false;const q=questions[qi];qBox.textContent=`السؤال ${qi+1} من ${questions.length}: ${q.q}`;feedback.textContent='';opts.innerHTML='';next.hidden=true;q.a.forEach((a,i)=>{const b=document.createElement('button');b.textContent=a;b.addEventListener('click',()=>{if(locked)return;locked=true;[...opts.children].forEach(x=>x.disabled=true);if(i===q.c){score++;feedback.textContent='إجابة صحيحة!';}else feedback.textContent='الإجابة الصحيحة: '+q.a[q.c];next.hidden=false});opts.appendChild(b)})}
    start.addEventListener('click',()=>{qi=0;score=0;start.hidden=true;showQuestion()});
    next.addEventListener('click',()=>{qi++;if(qi<questions.length)showQuestion();else{qBox.textContent=`انتهى التحدي. نتيجتك ${score} من ${questions.length}.`;opts.innerHTML='';feedback.textContent=score===questions.length?'أداء ممتاز!':'أحسنت، حاول مرة أخرى لتحسين نتيجتك.';next.hidden=true;start.hidden=false;start.textContent='أعد اللعب'}});

    const stages=['تحديد الهدف وجمع البيانات','إدخال البيانات ومعالجتها','إخراج الخريطة وتصميمها','مراجعة الخريطة وإنتاجها النهائي'];
    function shuffleStages(){
      const shuffled=[...stages];
      for(let i=shuffled.length-1;i>0;i--){const j=Math.floor(Math.random()*(i+1));[shuffled[i],shuffled[j]]=[shuffled[j],shuffled[i]];}
      [1,2,3,4].forEach(n=>{const el=document.getElementById('order'+n);el.innerHTML='';el.add(new Option('اختر المرحلة',''));shuffled.forEach(x=>el.add(new Option(x,x)));});
      document.getElementById('orderFeedback').textContent='';
    }
    shuffleStages();
    document.getElementById('checkOrder').addEventListener('click',()=>{const values=[1,2,3,4].map(n=>document.getElementById('order'+n).value);const out=document.getElementById('orderFeedback');if(values.some(v=>!v)||new Set(values).size!==4){out.textContent='اختر المراحل الأربع دون تكرار.';return}out.textContent=values.every((v,i)=>v===stages[i])?'ممتاز! الترتيب صحيح.':'ليس الترتيب الصحيح بعد. حاول مجددًا.';});
    document.getElementById('orderFeedback').insertAdjacentHTML('afterend','<button class="btn-secondary full" id="reshuffleOrder" type="button">إعادة ترتيب الخيارات</button>');
    document.getElementById('reshuffleOrder').addEventListener('click',shuffleStages);
    document.getElementById('guessBarka').addEventListener('click',()=>document.getElementById('guessFeedback').textContent='إجابة صحيحة! بركاء ولاية في محافظة جنوب الباطنة.');
    document.getElementById('guessOther').addEventListener('click',()=>document.getElementById('guessFeedback').textContent='ليست الإجابة الصحيحة. الإجابة هي بركاء.');
  })();
  </script>

  <script>
  // أدوات مركز المعلم: بناء مسودة منظمة محليًا دون ادعاء اتصال بخدمة ذكاء اصطناعي.
  (() => {
    const $ = id => document.getElementById(id);
    const grade = $('thGrade'), subject = $('thSubject');
    if (!grade || !subject) return;
    for (let i=1;i<=12;i++) grade.add(new Option('الصف '+i, 'الصف '+i));
    ['الدراسات الاجتماعية','اللغة العربية','التربية الإسلامية','الرياضيات','العلوم','اللغة الإنجليزية','تقنية المعلومات','الفنون','المهارات الحياتية','الرياضة المدرسية'].forEach(x=>subject.add(new Option(x,x)));
    const esc = value => String(value ?? '').replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
    let currentPlanText = '';
    function makePlan(){
      const title=$('thTitle').value.trim(), content=$('thContent').value.trim();
      if(!title){$('teacherStatus').textContent='أدخل عنوان الدرس أولًا.';$('thTitle').focus();return;}
      if(!content){$('teacherStatus').textContent='أدخل فقرة أو أفكار الدرس حتى ترتبط المسودة بالمحتوى.';$('thContent').focus();return;}
      const sentences=content.split(/[.!؟؛\n]+/).map(x=>x.trim()).filter(x=>x.length>12);
      const ideas=sentences.slice(0,5);
      const g=grade.value, s=subject.value, term=$('thTerm').value, duration=$('thDuration').value, method=$('thMethod').value, level=$('thLevel').value, prior=$('thPrior').value.trim()||'استدعاء خبرات الطلاب المرتبطة بموضوع الدرس.';
      const objectives=[`أن يوضح المتعلم المفاهيم الرئيسة المرتبطة بدرس «${title}».`,`أن يستخلص المتعلم أبرز الأفكار والمعلومات من محتوى الدرس.`,`أن يوظف المتعلم ما تعلمه في مثال أو موقف جديد.`];
      const safeIdeas=ideas.length?ideas:['الفكرة الرئيسة الأولى من محتوى الدرس','الفكرة الرئيسة الثانية من محتوى الدرس','تطبيقات الدرس وأهميته'];
      const questions=safeIdeas.slice(0,4).map((x,i)=>i%2===0?`اشرح الفكرة الآتية بأسلوبك: ${x}`:`استخرج معلومة أو دليلًا من النص يدعم الفكرة: ${x}`);
      const plan={title,g,s,term,duration,method,level,prior,objectives,ideas:safeIdeas,questions};
      const html=`<article class="plan-sheet" id="generatedPlan"><h3>خطة تحضير درس: ${esc(title)}</h3><div class="plan-meta"><span><b>الصف:</b> ${esc(g)}</span><span><b>المادة:</b> ${esc(s)}</span><span><b>الفصل:</b> ${esc(term)}</span><span><b>الزمن:</b> ${esc(duration)}</span><span><b>الاستراتيجية:</b> ${esc(method)}</span><span><b>مستوى المتعلمين:</b> ${esc(level)}</span></div><h4>أولًا: المعارف السابقة والتمهيد</h4><p>${esc(prior)} ثم عرض سؤال محفّز يربط موضوع الدرس بخبرة المتعلمين أو واقعهم.</p><h4>ثانيًا: نواتج التعلم المقترحة</h4><ul>${objectives.map(x=>`<li>${esc(x)}</li>`).join('')}</ul><h4>ثالثًا: سير التعلم وإجراءاته</h4><ol><li><b>التهيئة (5 دقائق):</b> سؤال تمهيدي وصورة أو موقف مرتبط بالدرس.</li><li><b>بناء المعرفة:</b> قراءة المحتوى وتحليل الأفكار الرئيسة وفق استراتيجية ${esc(method)}.</li><li><b>التطبيق:</b> تنفيذ نشاط فردي أو جماعي لتوظيف المفاهيم.</li><li><b>الغلق:</b> تلخيص التعلم والتحقق من تحقق النواتج.</li></ol><h4>رابعًا: الأفكار الرئيسة المستخلصة من النص</h4><ul>${safeIdeas.map(x=>`<li>${esc(x)}</li>`).join('')}</ul><h4>خامسًا: أسئلة التقويم وورقة العمل</h4><ol>${questions.map(x=>`<li>${esc(x)}<br>........................................................................</li>`).join('')}<li>اكتب تطبيقًا من واقع الحياة لما تعلمته في الدرس.<br>........................................................................</li></ol><h4>سادسًا: مراعاة الفروق الفردية</h4><ul><li><b>الدعم:</b> تقديم كلمات مفتاحية وخريطة مفاهيم أو مثال محلول.</li><li><b>الإثراء:</b> تكليف المتعلم المتقدم بمقارنة أو تفسير أو استنتاج إضافي.</li></ul><h4>سابعًا: التقويم الختامي والواجب</h4><p>بطاقة خروج تتضمن: معلومة تعلمتها، وسؤالًا ما زال لديك، وتطبيقًا واحدًا. يحدد المعلم واجبًا قصيرًا من الكتاب المدرسي بعد مطابقته بالدرس.</p><p><b>ملاحظة للمعلم:</b> هذه مسودة مقترحة تحتاج مراجعة وتكييفًا مع أهداف الدرس ودليل المعلم والكتاب المعتمد.</p></article>`;
      $('planOutput').className='teacher-output-content'; $('planOutput').innerHTML=html; $('planActions').hidden=false; $('teacherStatus').textContent='تم إعداد المسودة. راجعها وعدّلها قبل اعتمادها.';
      currentPlanText=`خطة تحضير درس: ${title}\nالصف: ${g} | المادة: ${s} | ${term} | الزمن: ${duration}\nاستراتيجية التدريس: ${method}\nمستوى المتعلمين: ${level}\n\nالمعارف السابقة: ${prior}\n\nنواتج التعلم:\n- ${objectives.join('\n- ')}\n\nسير الدرس:\n1. التهيئة: سؤال تمهيدي.\n2. بناء المعرفة: تحليل النص باستخدام ${method}.\n3. التطبيق: نشاط فردي أو جماعي.\n4. الغلق: تلخيص وبطاقة خروج.\n\nالأفكار الرئيسة:\n- ${safeIdeas.join('\n- ')}\n\nأسئلة التقويم:\n- ${questions.join('\n- ')}\n- اكتب تطبيقًا من واقع الحياة.\n\nمراعاة الفروق: دعم بكلمات مفتاحية ومثال، وإثراء بمقارنة أو استنتاج.\n\nملاحظة: مسودة مقترحة تحتاج مراجعة المعلم ومطابقتها بالكتاب ودليل المعلم.`;
    }
    $('makePlan').addEventListener('click',makePlan);
    $('clearPlan').addEventListener('click',()=>{['thTitle','thContent','thPrior'].forEach(id=>$(id).value='');$('planOutput').className='teacher-placeholder';$('planOutput').textContent='أدخل بيانات الدرس ثم اضغط «إنشاء مسودة التحضير». ستظهر هنا خطة منظمة قابلة للنسخ والطباعة والحفظ.';$('planActions').hidden=true;$('teacherStatus').textContent='تم مسح بيانات التحضير.';currentPlanText='';});
    $('copyPlan').addEventListener('click',async()=>{try{await navigator.clipboard.writeText(currentPlanText);$('teacherStatus').textContent='تم نسخ التحضير.';}catch(e){$('teacherStatus').textContent='تعذر النسخ التلقائي؛ استخدم الطباعة أو التنزيل.';}});
    $('printPlan').addEventListener('click',()=>{const content=$('generatedPlan');if(!content)return;const w=window.open('','_blank');if(!w){$('teacherStatus').textContent='اسمح بفتح نافذة الطباعة من إعدادات المتصفح.';return;}w.document.write('<!doctype html><html lang="ar" dir="rtl"><meta charset="utf-8"><title>تحضير الدرس</title><style>body{font-family:Tahoma,Arial;padding:28px;line-height:1.9;color:#173042}h3{border-bottom:2px solid #ddd;padding-bottom:10px}h4{color:#087e9e;margin-bottom:4px}.plan-meta{display:grid;grid-template-columns:1fr 1fr;background:#f1f6f8;padding:10px}</style>'+content.outerHTML+'</html>');w.document.close();w.focus();w.print();});
    $('downloadPlan').addEventListener('click',()=>{if(!currentPlanText)return;const blob=new Blob(['\ufeff',currentPlanText],{type:'application/msword;charset=utf-8'});const url=URL.createObjectURL(blob);const a=document.createElement('a');a.href=url;a.download='تحضير_درس_مدرسة_ثابت_بن_قيس.doc';document.body.appendChild(a);a.click();a.remove();URL.revokeObjectURL(url);$('teacherStatus').textContent='تم تجهيز ملف التحضير للتنزيل.';});
  })();
  </script>

</body>
</html>
