<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#071827">
<meta name="description" content="الجغرافيا الذكية - مدرسة ثابت بن قيس للتعليم الأساسي - سلطنة عمان">
<title>الجغرافيا الذكية | مدرسة ثابت بن قيس</title>

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.css">

<style>
:root{
 --bg:#071827;
 --bg2:#0b2236;
 --panel:#102d46;
 --panel2:#173c56;
 --gold:#e9bd68;
 --mint:#53e1bd;
 --white:#f5f8fc;
 --muted:#a9bdcc;
 --line:#ffffff20;
 --danger:#ef7676;
 --shadow:0 15px 45px #0005;
 --radius:18px;
}
*{box-sizing:border-box;margin:0;padding:0}
html{scroll-behavior:smooth;scroll-padding-top:90px}
body{
 background:var(--bg);
 color:var(--white);
 font-family:'Cairo',sans-serif;
 line-height:1.8;
 overflow-x:hidden;
}
a{text-decoration:none;color:inherit}
button,input,textarea,select{font-family:inherit}
button{cursor:pointer}
img{max-width:100%;display:block}

header{
 position:sticky;top:0;z-index:1000;
 background:#071827f5;
 backdrop-filter:blur(15px);
 border-bottom:1px solid var(--line);
}
.nav{
 max-width:1300px;margin:auto;padding:12px 22px;
 display:flex;align-items:center;justify-content:space-between;gap:20px;
}
.logo{color:var(--gold);font-size:21px;font-weight:900}
.logo span{display:block;font-size:11px;color:var(--muted);font-weight:500}
nav{display:flex;align-items:center;flex-wrap:wrap;gap:15px}
nav a{color:#dce7ef;font-size:13px;transition:.25s}
nav a:hover{color:var(--mint)}

.hero{
 min-height:610px;display:flex;align-items:center;
 padding:80px 22px;
 background:
 linear-gradient(90deg,#071827 5%,#071827e8 48%,#07182780 100%),
 url('https://images.unsplash.com/photo-1464822759023-fed622ff2c3b?auto=format&fit=crop&w=2000&q=85') center/cover;
}
.hero-content{max-width:1250px;width:100%;margin:auto}
.hero-tag{
 display:inline-block;padding:7px 17px;
 border:1px solid #53e1bd88;border-radius:30px;
 color:var(--mint);font-size:13px;background:#53e1bd12;
}
.hero h1{font-size:clamp(38px,6vw,76px);line-height:1.4;margin:22px 0;font-weight:900}
.hero h1 em{color:var(--gold);font-style:normal}
.hero p{max-width:700px;color:#d4e2ed;font-size:17px;margin-bottom:20px}

.btn{
 display:inline-flex;justify-content:center;align-items:center;gap:8px;
 padding:11px 23px;border:0;border-radius:10px;
 background:var(--gold);color:#102033;font-weight:800;
 font-size:14px;transition:.25s;margin:5px 3px;
}
.btn:hover{transform:translateY(-3px);filter:brightness(1.08)}
.btn.secondary{background:#ffffff14;color:white;border:1px solid #ffffff35}
.btn.mint{background:var(--mint);color:#092a2a}
.btn.danger{background:var(--danger);color:white}
.btn.small{padding:7px 13px;font-size:12px}

section{max-width:1300px;margin:auto;padding:70px 22px}
.section-head{text-align:center;margin-bottom:35px}
.section-head h2{color:var(--gold);font-size:clamp(24px,4vw,34px);font-weight:900}
.section-head p{color:var(--muted);font-size:14px;margin-top:7px}

.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:20px}
.card{
 background:linear-gradient(145deg,#153952,#0c263d);
 border:1px solid var(--line);border-radius:var(--radius);
 padding:24px;box-shadow:var(--shadow);transition:.3s;
}
.card:hover{transform:translateY(-5px);border-color:var(--mint)}
.card .symbol{font-size:39px;margin-bottom:10px}
.card h3{color:var(--mint);margin-bottom:8px;font-size:19px}
.card p{color:var(--muted);font-size:14px}

.lab{
 background:#0d2941;border:1px solid var(--line);
 border-radius:var(--radius);padding:25px;box-shadow:var(--shadow);
}
input,textarea,select{
 width:100%;padding:13px;margin:8px 0;
 background:#071827;border:1px solid #ffffff35;
 color:white;border-radius:9px;outline:none;
}
input:focus,textarea:focus,select:focus{border-color:var(--mint)}
input[type=range]{accent-color:var(--mint)}
input[type=file]{background:#102d46}
textarea{min-height:110px;resize:vertical}
label{display:block;color:#d5e2ec;font-size:13px}
.result{color:var(--mint);font-size:24px;font-weight:900;margin-top:12px}
.feedback{color:var(--mint);font-weight:700;margin:12px 0}
.muted{color:var(--muted);font-size:12px}
hr{border:0;border-top:1px solid var(--line);margin:30px 0}

.map-panel{
 background:#102d46;border:1px solid var(--line);
 border-radius:var(--radius);overflow:hidden;
}
#googleMapFrame{width:100%;height:450px;border:0;display:none}
#map{width:100%;height:460px;border-radius:var(--radius)}
.map-placeholder{
 min-height:350px;display:flex;justify-content:center;
 align-items:center;text-align:center;padding:25px;
 background:radial-gradient(circle,#205574,#102d46,#071827);
}
.map-actions{display:flex;flex-wrap:wrap;justify-content:center;gap:10px;padding:18px}
.map-status{text-align:center;color:var(--muted);font-size:13px;padding:12px}

.tour-layout{
 display:grid;grid-template-columns:290px 1fr;gap:20px;align-items:start;
}
.station-list{
 display:flex;flex-direction:column;gap:9px;
 max-height:650px;overflow-y:auto;
}
.station-btn{
 width:100%;text-align:right;padding:12px;
 background:#102d46;color:white;
 border:1px solid var(--line);border-radius:10px;transition:.2s;
}
.station-btn:hover,.station-btn.active{
 background:#145345;border-color:var(--mint);color:var(--mint);
}
.tour-view{
 background:#0d2941;border:1px solid var(--line);
 border-radius:var(--radius);overflow:hidden;
}
.tour-screen{
 min-height:420px;display:flex;align-items:center;
 justify-content:center;background:#102d46;
}
.tour-screen img{width:100%;height:420px;object-fit:cover;display:none}
.scene{
 width:100%;min-height:420px;display:flex;
 flex-direction:column;align-items:center;justify-content:center;
 text-align:center;padding:30px;
 background:radial-gradient(circle,#27627a,#102d46,#071827);
}
.scene .big-icon{font-size:90px}
.scene h3{color:var(--gold);font-size:25px}
.scene p{color:#d6e5ed;max-width:550px}
.tour-info{padding:22px}
.progress{height:8px;background:#ffffff20;border-radius:20px;overflow:hidden;margin:15px 0}
.progress span{display:block;height:100%;width:0;background:var(--mint);transition:.3s}
.tour-controls{display:flex;justify-content:space-between;flex-wrap:wrap;gap:10px}
#panoramaViewer{width:100%;height:430px;display:none}

.steps{display:grid;grid-template-columns:repeat(auto-fit,minmax(160px,1fr));gap:13px}
.step{text-align:center;background:#173c56;padding:18px;border-radius:12px}
.step strong{display:block;font-size:27px;color:var(--gold)}

.project-list{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:15px;margin-top:20px}

footer{
 text-align:center;padding:40px 15px;background:#04111e;
 color:var(--muted);font-size:13px;border-top:1px solid var(--line);
}
footer strong{color:var(--gold)}

@media(max-width:800px){
 .nav{flex-direction:column;align-items:flex-start}
 nav{gap:10px}
 .hero{min-height:500px;padding:55px 18px}
 section{padding:50px 16px}
 .tour-layout{grid-template-columns:1fr}
 .station-list{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));max-height:350px}
 #map,#googleMapFrame{height:350px}
 .tour-screen,.tour-screen img,.scene{min-height:300px;height:300px}
 #panoramaViewer{height:300px}
}
</style>
</head>

<body>

<header>
 <div class="nav">
  <div class="logo">
   🌍 الجغرافيا الذكية
   <span>مدرسة ثابت بن قيس للتعليم الأساسي | سلطنة عُمان</span>
  </div>
  <nav>
   <a href="#home">الرئيسية</a>
   <a href="#explore">اكتشف</a>
   <a href="#google">خرائط Google</a>
   <a href="#atlas">الأطلس</a>
   <a href="#tour">الجولة المدرسية</a>
   <a href="#lab">المختبر</a>
   <a href="#heritage">التراث</a>
   <a href="#quiz">التحدي</a>
   <a href="#projects">المشروعات</a>
  </nav>
 </div>
</header>

<main>

<!-- الواجهة الرئيسية -->
<section class="hero" id="home">
 <div class="hero-content">
  <span class="hero-tag">منصة الجغرافيا الرقمية والابتكار</span>
  <h1>افهم المكان...<br><em>واقرأ العالم بذكاء</em></h1>
  <p>
   رحلة تعليمية تفاعلية لاستكشاف عُمان والعالم
   باستخدام الخرائط الرقمية ونظم المعلومات الجغرافية
   والاستشعار عن بُعد والتقنيات الحديثة.
  </p>
  <a class="btn" href="#explore">ابدأ رحلة الاستكشاف ←</a>
  <a class="btn secondary" href="#tour">ادخل الجولة الافتراضية</a>
 </div>
</section>

<!-- بوابة الاستكشاف -->
<section id="explore">
 <div class="section-head">
  <h2>بوابتك إلى عالم الجغرافيا</h2>
  <p>مسارات تعليمية تحول المعرفة إلى تجربة تفاعلية</p>
 </div>

 <div class="grid">
  <article class="card"><div class="symbol">🗺️</div><h3>الخرائط الذكية</h3><p>اكتشف المواقع والاتجاهات والتوزيعات المكانية عبر الخرائط الرقمية.</p></article>
  <article class="card"><div class="symbol">🛰️</div><h3>الاستشعار عن بُعد</h3><p>تعرف على دور الأقمار الصناعية في دراسة سطح الأرض والبيئة.</p></article>
  <article class="card"><div class="symbol">🌐</div><h3>نظم المعلومات الجغرافية</h3><p>اجمع البيانات المكانية وحللها وحولها إلى معلومات.</p></article>
  <article class="card"><div class="symbol">🏰</div><h3>عُمان عبر المكان والزمان</h3><p>اكتشف القلاع والحصون والمواقع التاريخية والتراث العُماني.</p></article>
  <article class="card"><div class="symbol">🌱</div><h3>الجغرافيا والاستدامة</h3><p>اكتشف علاقة الإنسان بالبيئة وطرق المحافظة على الموارد.</p></article>
  <article class="card"><div class="symbol">🔎</div><h3>الباحث الجغرافي الصغير</h3><p>استخدم مهارات الملاحظة والبحث والتحليل في دراسة المكان.</p></article>
 </div>
</section>

<!-- خرائط Google -->
<section id="google">
 <div class="section-head">
  <h2>مستكشف خرائط Google</h2>
  <p>انتقل إلى المواقع العُمانية واستكشفها من خلال الخرائط والتجول الافتراضي</p>
 </div>

 <div class="lab">
  <label for="mapLocation">اختر الموقع الجغرافي</label>
  <select id="mapLocation">
   <option value="school">مدرسة ثابت بن قيس</option>
   <option value="barka">ولاية بركاء</option>
   <option value="fort">حصن بركاء</option>
   <option value="nakhal">قلعة نخل</option>
   <option value="nizwa">قلعة نزوى</option>
   <option value="rustaq">قلعة الرستاق</option>
   <option value="bat">مواقع بات والخطم والعين</option>
   <option value="coast">سواحل بركاء</option>
  </select>

  <div class="map-panel">
   <iframe id="googleMapFrame" title="خرائط Google" loading="lazy" allowfullscreen></iframe>
   <div class="map-placeholder" id="mapPlaceholder">
    <div>
     <div style="font-size:65px">🌍</div>
     <h3 style="color:var(--gold)">مستكشف Google الجغرافي</h3>
     <p>اختر الموقع ثم افتحه في خرائط Google.</p>
     <p class="muted">يمكن إظهار الخريطة داخل الموقع بعد إضافة مفتاح Google Maps Embed API.</p>
    </div>
   </div>
  </div>

  <div class="map-actions">
   <button class="btn" onclick="openSelectedMap()">افتح في Google Maps</button>
   <button class="btn mint" onclick="openSelectedStreetView()">التجول الافتراضي Street View</button>
   <button class="btn secondary" onclick="loadGoogleMap()">تحديث الخريطة</button>
  </div>

  <div class="map-status" id="mapStatus">
   روابط Google Maps المباشرة تعمل دون مفتاح API.
  </div>
  <p class="muted">لا يمكن ضمان وجود صور Street View لكل موقع. وقد يختلف توفر التجول حسب تغطية Google.</p>
 </div>
</section>

<!-- الأطلس الجغرافي -->
<section id="atlas">
 <div class="section-head">
  <h2>أطلس عُمان التفاعلي</h2>
  <p>استكشف مجموعة من المواقع الجغرافية والتراثية في سلطنة عُمان</p>
 </div>
 <div id="map"></div>
 <p class="muted">الخريطة هنا تعتمد على OpenStreetMap، أما خرائط Google فتجدها في القسم السابق.</p>
</section>

<!-- الجولة الافتراضية -->
<section id="tour">
 <div class="section-head">
  <h2>الجولة الافتراضية داخل مدرسة ثابت بن قيس</h2>
  <p>13 محطة لاستكشاف مرافق المدرسة</p>
 </div>

 <div class="tour-layout">
  <div class="lab">
   <h3 style="color:var(--gold);margin-bottom:12px">محطات الجولة</h3>
   <div class="station-list" id="stationList"></div>
  </div>

  <div class="tour-view">
   <div class="tour-screen">
    <div class="scene" id="tourScene">
     <div class="big-icon" id="sceneIcon">🏫</div>
     <h3 id="sceneTitle">البوابة الرئيسية</h3>
     <p id="sceneDescription">مرحبًا بك في الجولة الافتراضية لمدرسة ثابت بن قيس.</p>
     <span class="muted">مشهد توضيحي قابل للاستبدال بصورة المدرسة الحقيقية</span>
    </div>
    <img id="tourImage" alt="صورة محطة الجولة">
   </div>

   <div class="tour-info">
    <div style="display:flex;justify-content:space-between;gap:10px">
     <strong id="stationCounter">المحطة 1 من 13</strong>
     <span style="color:var(--mint)" id="stationPercent">8%</span>
    </div>
    <div class="progress"><span id="tourProgress"></span></div>

    <div class="tour-controls">
     <button class="btn secondary" onclick="previousStation()">→ المحطة السابقة</button>
     <button class="btn" onclick="nextStation()">المحطة التالية ←</button>
    </div>

    <hr>

    <h3 style="color:var(--gold)">إضافة الصور الحقيقية</h3>
    <p class="muted">يمكن رفع صورة للمحطة الحالية أو مجموعة صور بالترتيب.</p>
    <div class="lab" style="margin-top:12px">
     <label>صورة المحطة الحالية</label>
     <input type="file" id="singlePhoto" accept="image/*">
     <label>مجموعة صور للمحطات</label>
     <input type="file" id="bulkPhotos" accept="image/*" multiple>
     <p class="muted" id="uploadMessage">الصور المضافة مؤقتة في المتصفح الحالي.</p>
    </div>

    <hr>

    <h3 style="color:var(--gold)">التجول البانورامي 360 درجة</h3>
    <p class="muted">أدخل رابط صورة بانورامية 360 مستضافة داخل ملفات الموقع أو على خادم يسمح بعرضها.</p>
    <input id="panoramaUrl" placeholder="images/360/gate.jpg">
    <button class="btn mint" onclick="activatePanorama()">تشغيل التجول 360°</button>
    <div id="panoramaViewer"></div>
    <button class="btn secondary small" onclick="closePanorama()">إغلاق المشهد</button>
   </div>
  </div>
 </div>
</section>

<!-- المختبر -->
<section id="lab">
 <div class="section-head">
  <h2>مختبر الجغرافيا الرقمية</h2>
  <p>أدوات تفاعلية لتطبيق المهارات الجغرافية</p>
 </div>

 <div class="grid">
  <div class="lab">
   <h3 style="color:var(--mint)">📏 مختبر مقياس الرسم</h3>
   <label>المسافة على الخريطة (سم)</label>
   <input type="number" id="mapDistance" value="5" min="0" step="0.5">
   <label>مقام مقياس الرسم</label>
   <input type="number" id="scale" value="100000" min="1">
   <button class="btn" onclick="calculateScale()">احسب المسافة الحقيقية</button>
   <div class="result" id="scaleResult">5 كم</div>
  </div>

  <div class="lab">
   <h3 style="color:var(--mint)">🌿 محاكاة الغطاء النباتي</h3>
   <label>قيمة المؤشر الافتراضي NDVI</label>
   <input type="range" id="ndvi" min="-1" max="1" step=".01" value=".45" oninput="showNDVI()">
   <div class="result" id="ndviValue">0.45</div>
   <p id="ndviText">غطاء نباتي متوسط في المحاكاة.</p>
   <p class="muted">محاكاة تعليمية وليست قراءة فعلية لبيانات الأقمار الصناعية.</p>
  </div>
 </div>

 <hr>
 <h3 style="color:var(--gold);margin-bottom:18px">مراحل عمل نظم المعلومات الجغرافية</h3>
 <div class="steps">
  <div class="step"><strong>1</strong>جمع البيانات</div>
  <div class="step"><strong>2</strong>إدخال البيانات</div>
  <div class="step"><strong>3</strong>المعالجة والتحليل</div>
  <div class="step"><strong>4</strong>إنتاج الخرائط</div>
  <div class="step"><strong>5</strong>اتخاذ القرار</div>
 </div>
</section>

<!-- التراث العماني -->
<section id="heritage">
 <div class="section-head">
  <h2>عُمان عبر المكان والزمان</h2>
  <p>رحلات تراثية وجغرافية في ربوع السلطنة</p>
 </div>

 <div class="grid">
  <article class="card">
   <div class="symbol">🏰</div>
   <h3>حصن بركاء</h3>
   <p>تعرف على العمارة الدفاعية والتراث التاريخي في جنوب الباطنة.</p>
   <a class="btn small" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=Barka+Fort+Oman">استكشف الموقع</a>
  </article>
  <article class="card">
   <div class="symbol">🏯</div>
   <h3>قلعة نخل</h3>
   <p>اكتشف علاقة الموقع بالتضاريس الجبلية والواحات.</p>
   <a class="btn small" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=Nakhal+Fort+Oman">استكشف الموقع</a>
  </article>
  <article class="card">
   <div class="symbol">⛰️</div>
   <h3>مواقع بات والخطم والعين</h3>
   <p>مواقع أثرية تعكس تاريخ الاستيطان البشري في عُمان.</p>
   <a class="btn small" target="_blank" rel="noopener" href="https://whc.unesco.org/en/list/434/">معلومات اليونسكو</a>
  </article>
  <article class="card">
   <div class="symbol">🌊</div>
   <h3>سواحل بركاء</h3>
   <p>استكشف البيئة الساحلية وأهميتها الطبيعية والاقتصادية.</p>
   <a class="btn small" target="_blank" rel="noopener" href="https://www.google.com/maps/search/?api=1&query=Barka+Beach+Oman">استكشف الموقع</a>
  </article>
 </div>
</section>

<!-- التحدي -->
<section id="quiz">
 <div class="section-head">
  <h2>التحدي الجغرافي الذكي</h2>
  <p>اختبر معلوماتك واحصل على نتيجتك فورًا</p>
 </div>
 <div class="lab">
  <div id="quizArea">
   <div class="question" id="question"></div>
   <div id="options"></div>
   <div class="feedback" id="feedback"></div>
   <button class="btn" id="nextBtn" onclick="nextQuestion()" style="display:none">السؤال التالي ←</button>
  </div>
  <div id="finalScore" style="display:none;text-align:center">
   <h2 style="color:var(--gold)">انتهى التحدي!</h2>
   <div class="result" id="scoreText"></div>
   <button class="btn" onclick="restartQuiz()">أعد المحاولة</button>
  </div>
 </div>
</section>

<!-- مشروعات الطلاب -->
<section id="projects">
 <div class="section-head">
  <h2>مبادرات الباحث الجغرافي الصغير</h2>
  <p>مساحة لتسجيل أفكار المشروعات الطلابية</p>
 </div>
 <div class="lab">
  <label>اسم المشروع</label>
  <input id="projectName" placeholder="مثال: خريطة الخدمات في بركاء">
  <label>فكرة المشروع</label>
  <textarea id="projectIdea" placeholder="اكتب فكرتك الجغرافية هنا..."></textarea>
  <button class="btn" onclick="saveProject()">احفظ الفكرة</button>
  <div id="projectMessage" class="feedback"></div>
  <div id="projectList" class="project-list"></div>
  <p class="muted">الأفكار محفوظة محليًا على الجهاز الحالي ولا تُرسل تلقائيًا إلى المدرسة.</p>
 </div>
</section>

<!-- الرؤية -->
<section>
 <div class="section-head">
  <h2>رؤيتنا الجغرافية</h2>
  <p>نحو جيل يقرأ المكان ويفهم العالم ويستخدم التقنية لصناعة المستقبل</p>
 </div>
 <div class="card" style="text-align:center">
  <div class="symbol">🇴🇲</div>
  <h3>مدرسة ثابت بن قيس للتعليم الأساسي</h3>
  <p>الجغرافيا علم يساعدنا على فهم العالم وتحليل التغيرات واتخاذ قرارات أفضل.</p>
 </div>
</section>

</main>

<footer>
 <strong>الجغرافيا الذكية</strong><br>
 مدرسة ثابت بن قيس للتعليم الأساسي – سلطنة عُمان<br>
 إعداد وتصميم: د. قناوي حسين
</footer>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pannellum@2.5.6/build/pannellum.js"></script>

<script>

// ======================================
// 1. إعداد خرائط Google
// ======================================

// ضع مفتاح Google Maps Embed API هنا.
// لا تضع مفتاحًا وهميًا.
// قيّد المفتاح بنطاق الموقع وبواجهة API المناسبة.

const GOOGLE_MAPS_API_KEY = "";

const locations = {
 school:{
  name:"مدرسة ثابت بن قيس",
  query:"مدرسة ثابت بن قيس للتعليم الأساسي بركاء سلطنة عمان",
  coords:null
 },
 barka:{
  name:"ولاية بركاء",
  query:"Barka Oman",
  coords:[23.678,57.886]
 },
 fort:{
  name:"حصن بركاء",
  query:"Barka Fort Oman",
  coords:[23.678,57.888]
 },
 nakhal:{
  name:"قلعة نخل",
  query:"Nakhal Fort Oman",
  coords:[23.394,57.832]
 },
 nizwa:{
  name:"قلعة نزوى",
  query:"Nizwa Fort Oman",
  coords:[22.933,57.533]
 },
 rustaq:{
  name:"قلعة الرستاق",
  query:"Rustaq Fort Oman",
  coords:[23.391,57.424]
 },
 bat:{
  name:"مواقع بات والخطم والعين",
  query:"Bat Al Khutm Al Ayn Oman",
  coords:[22.350,56.750]
 },
 coast:{
  name:"سواحل بركاء",
  query:"Barka Beach Oman",
  coords:[23.700,57.880]
 }
};

function getSelectedLocation(){
 return locations[document.getElementById("mapLocation").value];
}

function loadGoogleMap(){
 const place=getSelectedLocation();
 const frame=document.getElementById("googleMapFrame");
 const placeholder=document.getElementById("mapPlaceholder");
 const status=document.getElementById("mapStatus");

 if(!GOOGLE_MAPS_API_KEY){
  frame.style.display="none";
  frame.removeAttribute("src");
  placeholder.style.display="flex";
  status.textContent="لم تتم إضافة مفتاح Google Maps API. استخدم زر فتح Google Maps.";
  return;
 }

 const url="https://www.google.com/maps/embed/v1/place?key="+
 encodeURIComponent(GOOGLE_MAPS_API_KEY)+
 "&q="+encodeURIComponent(place.query);

 frame.src=url;
 frame.style.display="block";
 placeholder.style.display="none";
 status.textContent="جاري عرض خريطة Google. إذا ظهر خطأ تحقق من إعدادات المفتاح والفوترة.";
}

function openSelectedMap(){
 const place=getSelectedLocation();
 const url="https://www.google.com/maps/search/?api=1&query="+
 encodeURIComponent(place.query);
 window.open(url,"_blank","noopener");
}

function openSelectedStreetView(){
 const place=getSelectedLocation();
 let url;

 if(place.coords){
  url="https://www.google.com/maps/@?api=1&map_action=pano&viewpoint="+
   place.coords[0]+","+place.coords[1];
 }else{
  url="https://www.google.com/maps/search/?api=1&query="+
   encodeURIComponent(place.query);
  document.getElementById("mapStatus").textContent=
   "اختر موقع المدرسة في Google Maps ثم تحقق من توفر صور التجول الافتراضي.";
 }

 window.open(url,"_blank","noopener");
}

document.getElementById("mapLocation").addEventListener("change",loadGoogleMap);
loadGoogleMap();

// ======================================
// 2. أطلس عُمان - Leaflet
// ======================================

const omanMap=L.map("map").setView([23.6,57.4],6);

L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",{
 maxZoom:19,
 attribution:"&copy; OpenStreetMap contributors"
}).addTo(omanMap);

const mapPlaces=[
 {name:"مسقط",coords:[23.588,58.382],info:"العاصمة العُمانية"},
 {name:"بركاء",coords:[23.678,57.886],info:"ولاية ساحلية في جنوب الباطنة"},
 {name:"حصن بركاء",coords:[23.678,57.888],info:"معلم تراثي"},
 {name:"قلعة نخل",coords:[23.394,57.832],info:"قلعة تاريخية"},
 {name:"قلعة نزوى",coords:[22.933,57.533],info:"معلم تاريخي في الداخلية"},
 {name:"قلعة الرستاق",coords:[23.391,57.424],info:"معلم تاريخي في الرستاق"}
];

mapPlaces.forEach(place=>{
 L.marker(place.coords).addTo(omanMap)
 .bindPopup("<b>"+place.name+"</b><br>"+place.info);
});

// ======================================
// 3. الجولة الافتراضية المدرسية
// ======================================

const stations=[
 {name:"البوابة الرئيسية",icon:"🏫",desc:"بداية الجولة الافتراضية والتعرف على المدخل الرئيسي للمدرسة."},
 {name:"جوانب المدرسة",icon:"🏢",desc:"التعرف على الجوانب الخارجية للمبنى المدرسي."},
 {name:"الممرات والدرج - الجانب الأيسر",icon:"🚶",desc:"استكشاف الممرات والدرج في الجانب الأيسر."},
 {name:"الممرات والدرج - الجانب الأيمن",icon:"🚶",desc:"استكشاف الممرات والدرج في الجانب الأيمن."},
 {name:"غرفة المعلمين الأولى",icon:"👨‍🏫",desc:"التعرف على بيئة العمل التعليمية للمعلمين."},
 {name:"الإدارة المدرسية",icon:"🏢",desc:"زيارة الإدارة المدرسية والتعرف على دورها التنظيمي."},
 {name:"الفصول الدراسية",icon:"📚",desc:"استكشاف بيئة التعلم داخل الفصول الدراسية."},
 {name:"مركز مصادر التعلم",icon:"📖",desc:"التعرف على مصادر المعرفة والقراءة والبحث."},
 {name:"مختبر الحاسوب",icon:"💻",desc:"استكشاف بيئة التعلم الرقمي والتقنيات الحديثة."},
 {name:"مختبر الدراسات الاجتماعية",icon:"🌍",desc:"التعرف على أدوات تعليم الجغرافيا والتاريخ."},
 {name:"الساحة المدرسية",icon:"🌳",desc:"استكشاف الساحة والأنشطة المدرسية."},
 {name:"مختبر الجغرافيا والتقنيات الحديثة",icon:"🛰️",desc:"اكتشاف الخرائط الرقمية ونظم المعلومات الجغرافية والاستشعار عن بعد."},
 {name:"قاعة الأنشطة والابتكار",icon:"💡",desc:"مساحة الإبداع والمبادرات والمشروعات الطلابية."}
];

let currentStation=0;
let tourPhotos=Array(stations.length).fill(null);
let tourPanoUrls=Array(stations.length).fill("");
let panoViewer=null;

function renderStationList(){
 const list=document.getElementById("stationList");
 list.innerHTML="";

 stations.forEach((station,index)=>{
  const button=document.createElement("button");
  button.className="station-btn"+(index===currentStation?" active":"");
  button.textContent=(index+1)+". "+station.name;
  button.onclick=()=>{
   currentStation=index;
   renderStation();
  };
  list.appendChild(button);
 });
}

function renderStation(){
 const station=stations[currentStation];
 const image=document.getElementById("tourImage");
 const scene=document.getElementById("tourScene");

 document.getElementById("sceneIcon").textContent=station.icon;
 document.getElementById("sceneTitle").textContent=station.name;
 document.getElementById("sceneDescription").textContent=station.desc;

 if(tourPhotos[currentStation]){
  image.src=tourPhotos[currentStation];
  image.style.display="block";
  scene.style.display="none";
 }else{
  image.style.display="none";
  scene.style.display="flex";
 }

 const percent=Math.round((currentStation+1)/stations.length*100);

 document.getElementById("stationCounter").textContent=
  "المحطة "+(currentStation+1)+" من "+stations.length;
 document.getElementById("stationPercent").textContent=percent+"%";
 document.getElementById("tourProgress").style.width=percent+"%";
 document.getElementById("panoramaUrl").value=tourPanoUrls[currentStation];

 closePanorama();
 renderStationList();
}

function nextStation(){
 if(currentStation<stations.length-1){
  currentStation++;
  renderStation();
 }
}

function previousStation(){
 if(currentStation>0){
  currentStation--;
  renderStation();
 }
}

function setTourPhoto(index,file){
 if(!file || !file.type.startsWith("image/"))return;

 if(tourPhotos[index] && tourPhotos[index].startsWith("blob:")){
  URL.revokeObjectURL(tourPhotos[index]);
 }

 tourPhotos[index]=URL.createObjectURL(file);
}

document.getElementById("singlePhoto").addEventListener("change",function(){
 const file=this.files[0];
 if(!file)return;

 setTourPhoto(currentStation,file);
 renderStation();

 document.getElementById("uploadMessage").textContent=
  "تم عرض صورة المحطة الحالية.";
 this.value="";
});

document.getElementById("bulkPhotos").addEventListener("change",function(){
 const files=Array.from(this.files).filter(file=>file.type.startsWith("image/"));

 if(!files.length)return;

 files.slice(0,stations.length).forEach((file,index)=>{
  setTourPhoto(index,file);
 });

 document.getElementById("uploadMessage").textContent=
  "تم توزيع "+Math.min(files.length,stations.length)+" صورة بالترتيب.";

 currentStation=0;
 renderStation();
 this.value="";
});

function activatePanorama(){
 const url=document.getElementById("panoramaUrl").value.trim();

 if(!url){
  alert("أدخل رابط صورة بانورامية أولًا.");
  return;
 }

 if(!window.pannellum){
  alert("تعذر تحميل مكتبة العرض البانورامي. تحقق من الإنترنت.");
  return;
 }

 if(!/^https:\/\//i.test(url) && !/^[a-zA-Z0-9_./-]+$/.test(url)){
  alert("تحقق من مسار الصورة.");
  return;
 }

 tourPanoUrls[currentStation]=url;

 closePanorama();

 const viewer=document.getElementById("panoramaViewer");
 viewer.style.display="block";

 panoViewer=pannellum.viewer("panoramaViewer",{
  type:"equirectangular",
  panorama:url,
  autoLoad:true,
  compass:true,
  showZoomCtrl:true,
  showFullscreenCtrl:true
 });

 viewer.scrollIntoView({behavior:"smooth",block:"center"});
}

function closePanorama(){
 const viewer=document.getElementById("panoramaViewer");
 viewer.style.display="none";

 if(panoViewer){
  panoViewer.destroy();
  panoViewer=null;
 }
}

renderStation();

// ======================================
// 4. مختبر مقياس الرسم
// ======================================

function calculateScale(){
 const distance=Number(document.getElementById("mapDistance").value);
 const scale=Number(document.getElementById("scale").value);
 const result=document.getElementById("scaleResult");

 if(!Number.isFinite(distance)||!Number.isFinite(scale)||distance<0||scale<=0){
  result.textContent="تحقق من القيم المدخلة";
  return;
 }

 const km=distance*scale/100000;
 result.textContent=km.toLocaleString("ar",{maximumFractionDigits:3})+" كم";
}

// ======================================
// 5. محاكاة الغطاء النباتي
// ======================================

function showNDVI(){
 const value=Number(document.getElementById("ndvi").value);
 document.getElementById("ndviValue").textContent=value.toFixed(2);

 let text="";

 if(value<0)text="قيمة سالبة: قد تشير إلى الماء أو الأسطح غير النباتية.";
 else if(value<0.2)text="غطاء نباتي ضعيف في هذه المحاكاة.";
 else if(value<0.5)text="غطاء نباتي متوسط في هذه المحاكاة.";
 else text="غطاء نباتي مرتفع في هذه المحاكاة.";

 document.getElementById("ndviText").textContent=text;
}
showNDVI();

// ======================================
// 6. التحدي الجغرافي
// ======================================

const questions=[
 {
  q:"ما الأداة التي تساعد على تمثيل المواقع والظواهر على سطح الأرض؟",
  a:["الخريطة","المجهر","الساعة","المسطرة"],
  correct:0
 },
 {
  q:"ما المقصود بالاستشعار عن بُعد؟",
  a:["جمع المعلومات دون اتصال مباشر بالهدف","رسم الخرائط يدويًا فقط","قياس الزمن","حساب المسافات فقط"],
  correct:0
 },
 {
  q:"أي مما يأتي يُعد من مكونات نظم المعلومات الجغرافية؟",
  a:["البيانات المكانية","الأجهزة والبرمجيات","العنصر البشري","جميع ما سبق"],
  correct:3
 },
 {
  q:"في أي محافظة تقع ولاية بركاء؟",
  a:["الداخلية","جنوب الباطنة","ظفار","مسندم"],
  correct:1
 },
 {
  q:"ما الخطوة التي تأتي بعد جمع البيانات في سير عمل GIS المبسط؟",
  a:["إدخال البيانات","اتخاذ القرار","إلغاء البيانات","لا شيء مما سبق"],
  correct:0
 }
];

let qIndex=0;
let score=0;
let answered=false;

function renderQuestion(){
 answered=false;
 const question=questions[qIndex];

 document.getElementById("question").textContent=
  "السؤال "+(qIndex+1)+" من "+questions.length+": "+question.q;

 document.getElementById("feedback").textContent="";
 document.getElementById("nextBtn").style.display="none";

 const area=document.getElementById("options");
 area.innerHTML="";

 question.a.forEach((option,index)=>{
  const button=document.createElement("button");
  button.className="option";
  button.textContent=option;
  button.onclick=()=>chooseAnswer(index);
  area.appendChild(button);
 });
}

function chooseAnswer(index){
 if(answered)return;
 answered=true;

 const question=questions[qIndex];

 document.querySelectorAll(".option").forEach(button=>{
  button.disabled=true;
 });

 if(index===question.correct){
  score++;
  document.getElementById("feedback").textContent="✓ إجابة صحيحة، أحسنت!";
 }else{
  document.getElementById("feedback").textContent=
   "الإجابة الصحيحة: "+question.a[question.correct];
 }

 document.getElementById("nextBtn").style.display="inline-block";
 document.getElementById("nextBtn").textContent=
  qIndex===questions.length-1?"عرض النتيجة":"السؤال التالي ←";
}

function nextQuestion(){
 if(!answered)return;

 qIndex++;

 if(qIndex<questions.length){
  renderQuestion();
 }else{
  document.getElementById("quizArea").style.display="none";
  document.getElementById("finalScore").style.display="block";
  document.getElementById("scoreText").textContent=
   score+" من "+questions.length;
 }
}

function restartQuiz(){
 qIndex=0;
 score=0;
 document.getElementById("quizArea").style.display="block";
 document.getElementById("finalScore").style.display="none";
 renderQuestion();
}

renderQuestion();

// ======================================
// 7. مشروعات الطلاب
// ======================================

function saveProject(){
 const name=document.getElementById("projectName").value.trim();
 const idea=document.getElementById("projectIdea").value.trim();
 const message=document.getElementById("projectMessage");

 if(!name||!idea){
  message.textContent="يرجى كتابة اسم المشروع وفكرته.";
  return;
 }

 try{
  const projects=JSON.parse(localStorage.getItem("geoProjects")||"[]");

  projects.push({name,idea});

  localStorage.setItem("geoProjects",JSON.stringify(projects));

  document.getElementById("projectName").value="";
  document.getElementById("projectIdea").value="";
  message.textContent="تم حفظ الفكرة على هذا الجهاز.";

  renderProjects();

 }catch(error){
  message.textContent="تعذر الحفظ في هذا المتصفح.";
 }
}

function renderProjects(){
 const list=document.getElementById("projectList");
 list.innerHTML="";

 try{
  const projects=JSON.parse(localStorage.getItem("geoProjects")||"[]");

  projects.forEach(project=>{
   const card=document.createElement("article");
   card.className="card";

   const title=document.createElement("h3");
   title.textContent=project.name;

   const description=document.createElement("p");
   description.textContent=project.idea;

   card.append(title,description);
   list.appendChild(card);
  });

 }catch(error){}
}

renderProjects();

</script>
</body>
</html>
