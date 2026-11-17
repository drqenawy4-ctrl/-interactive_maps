<section class="content-section" id="lab">
    <h2>🛰️ مختبر الجغرافيا الحديثة</h2>
    <p class="section-lead">أدوات خارجية للاستكشاف الجغرافي والصور الفضائية والخرائط الرقمية.</p>
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
  </section>

  <section class="content-section" id="games">
    <h2>🎮 مدينة الألعاب التعليمية</h2>
    <p class="section-lead">أنشطة قصيرة تعمل داخل الموقع وتمنح المتعلم تغذية راجعة فورية.</p>
    <div class="cards-grid">
      <article class="game-box">
        <h3>🏆 تحدي المستكشف الجغرافي</h3>
        <p>أجب عن الأسئلة لتحصل على نتيجتك.</p>
        <div id="gameQuestion">اضغط ابدأ لبدء التحدي.</div>
        <div class="game-options" id="gameOptions"></div>
        <div class="game-feedback" id="gameFeedback"></div>
        <button class="btn-gold full" id="startGame">ابدأ اللعبة</button>
        <button class="btn-secondary full" id="nextGame" hidden>السؤال التالي</button>
      </article>
      <article class="game-box">
        <h3>🧭 رتّب مراحل إنتاج الخريطة</h3>
        <p>اختر المراحل بالترتيب الصحيح.</p>
        <div class="portal-controls" style="display:block">
          <select id="order1"></select>
          <select id="order2"></select>
          <select id="order3"></select>
          <select id="order4"></select>
        </div>
        <button class="btn-gold full" id="checkOrder">تحقق من الإجابة</button>
        <div class="game-feedback" id="orderFeedback"></div>
      </article>
      <article class="game-box">
        <h3>📍 خمن الموقع</h3>
        <p>أنا ولاية ساحلية في محافظة جنوب الباطنة، فمن أكون؟</p>
        <button class="btn-gold full" id="guessBarka">بركاء</button>
        <button class="btn-secondary full" id="guessOther">نزوى</button>
        <div class="game-feedback" id="guessFeedback"></div>
      </article>
    </div>
  </section>

  <section class="content-section" id="platforms">
    <h2>🌐 بوابة المنصات التعليمية</h2>
    <p class="section-lead">روابط إلى بوابات عُمانية ومصادر عالمية مساندة للطالب والمعلم.</p>
    <div class="cards-grid">
      <article class="portal-card"><span class="portal-icon">🇴🇲</span><h3>وزارة التعليم العُمانية</h3><p>الموقع الرسمي للخدمات والأخبار والموارد التعليمية.</p><a class="portal-btn" href="https://home.moe.gov.om/" target="_blank" rel="noopener">دخول الموقع الرسمي ↗</a></article>
      <article class="portal-card"><span class="portal-icon">📚</span><h3>مكتبة المناهج العُمانية</h3><p>الوصول إلى الكتب المدرسية والمناهج المنشورة رسميًا.</p><a class="portal-btn" href="https://home.moe.gov.om/ar/manaheg/index.moe" target="_blank" rel="noopener">استعرض المناهج ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🧑‍🏫</span><h3>Google Classroom</h3><p>إدارة الصفوف والواجبات والمواد التعليمية.</p><a class="portal-btn" href="https://classroom.google.com/" target="_blank" rel="noopener">افتح المنصة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">💻</span><h3>Microsoft Teams</h3><p>التواصل والاجتماعات والتعاون داخل الصف.</p><a class="portal-btn" href="https://teams.microsoft.com/" target="_blank" rel="noopener">افتح المنصة ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🧩</span><h3>Kahoot!</h3><p>إنشاء مسابقات واختبارات صفية تفاعلية.</p><a class="portal-btn" href="https://kahoot.com/" target="_blank" rel="noopener">افتح Kahoot ↗</a></article>
      <article class="portal-card"><span class="portal-icon">🎨</span><h3>Canva للتعليم</h3><p>تصميم العروض والملصقات والأنشطة التعليمية.</p><a class="portal-btn" href="https://www.canva.com/education/" target="_blank" rel="noopener">افتح Canva ↗</a></article>
    </div>
  </section>

  <footer class="site-footer">
    منصة الجغرافيا الذكية | مدرسة ثابت بن قيس للتعليم الأساسي – سلطنة عُمان<br>
    إعداد وتصميم: د. قناوي حسين | © 2026
  </footer>

  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
  <script>
    // مركز الخريطة الافتراضي: ولاية بركاء
    const map = L.map('map', { zoomControl:true }).setView([23.7042,57.8860],11);

    const baseLayers = {
      light:L.tileLayer('https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png',{
        attribution:'&copy; OpenStreetMap &copy; CARTO',maxZoom:19
      }),
      dark:L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png',{
        attribution:'&copy; OpenStreetMap &copy; CARTO',maxZoom:19
      }),
      streets:L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
        attribution:'&copy; OpenStreetMap contributors',maxZoom:19
      })
    };
    let activeBase=baseLayers.light.addTo(map);

    const colors={
      schools:'#18a9d0',cities:'#e8a72e',
      history:'#e45b5b',nature:'#20a477'
    };

    const pictures={
      schools:'https://images.unsplash.com/photo-1580582932707-520aed937b7b?w=700',
      cities:'https://images.unsplash.com/photo-1518005020951-eccb494ad742?w=700',
      history:'https://images.unsplash.com/photo-1590736969955-71cc94901144?w=700',
      nature:'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?w=700'
    };

    const categoryNames={
      schools:'مدرسة',cities:'مدينة أو مركز سكاني',
      history:'موقع تاريخي أو تراثي',nature:'معلم طبيعي'
    };

    const geoData={
      schools:[
        {coords:[23.7050,57.8820],name:'مدرسة ثابت بن قيس للتعليم الأساسي',desc:'صرح تعليمي في ولاية بركاء، ويمكن توظيف الموقع في الأنشطة التعليمية الرقمية والجغرافية.'},
        {coords:[23.6980,57.8750],name:'مدرسة بركاء للتعليم الأساسي',desc:'مؤسسة تعليمية في ولاية بركاء.'},
        {coords:[23.7120,57.8950],name:'مدرسة العلاء بن الحضرمي',desc:'مدرسة تخدم قطاع التعليم الأساسي في الولاية.'},
        {coords:[23.6850,57.8600],name:'مدرسة أم عمارة للبنات',desc:'مؤسسة تعليمية للبنات في ولاية بركاء.'},
        {coords:[23.6720,57.9100],name:'مدرسة الرميس للتعليم الأساسي',desc:'مدرسة تخدم المناطق الشرقية من ولاية بركاء.'},
        {coords:[23.6900,57.8700],name:'مدرسة أسماء بنت النعمان',desc:'مدرسة ضمن نطاق بركاء؛ الموقع المعروض تقريبي ويحتاج مراجعة ميدانية.'},
        {coords:[23.7150,57.9000],name:'مدرسة دار الفكر',desc:'مؤسسة تعليمية أُدرجت ضمن نطاق الاستكشاف؛ يرجى التحقق من موضعها الدقيق.'},
        {coords:[23.6800,57.8950],name:'مدارس التكوين الخاصة',desc:'مؤسسة تعليمية أُدرجت ضمن نطاق بركاء؛ الإحداثيات تقريبية.'},
        {coords:[23.7000,57.9150],name:'مدرسة بركات الثانوية',desc:'مؤسسة تعليمية أُدرجت ضمن نطاق الاستكشاف؛ يرجى التحقق من موضعها الدقيق.'},
        {coords:[23.6650,57.8850],name:'مدرسة زهور عُمان',desc:'مؤسسة تعليمية أُدرجت ضمن نطاق بركاء؛ الإحداثيات تقريبية.'}
      ],
      cities:[
        {coords:[23.7072,57.8861],name:'مركز ولاية بركاء',desc:'مركز عمراني في محافظة جنوب الباطنة، وتشتهر الولاية بالأنشطة الساحلية والزراعية والتجارية.'},
        {coords:[23.5880,58.3829],name:'مسقط — العاصمة',desc:'عاصمة سلطنة عُمان ومركزها الإداري والاقتصادي.'},
        {coords:[23.3959,57.4244],name:'الرستاق',desc:'مدينة تاريخية ومركز محافظة جنوب الباطنة.'},
        {coords:[24.3474,56.7094],name:'صحار',desc:'مدينة رئيسية في محافظة شمال الباطنة ومركز صناعي وتجاري مهم.'}
      ],
      history:[
        {coords:[23.7114,57.8878],name:'حصن بركاء',desc:'معلم تاريخي بارز في ولاية بركاء، يرتبط بتاريخ الولاية ودورها الساحلي.'},
        {coords:[23.6961,57.9044],name:'بيت النعمان',desc:'معلم تراثي في ولاية بركاء يعكس جانبًا من العمارة العُمانية التقليدية.'},
        {coords:[22.9333,57.5333],name:'قلعة نزوى',desc:'من أشهر القلاع التاريخية في سلطنة عُمان، وتتميز ببرجها الدائري.'},
        {coords:[23.7100,57.8900],name:'برج الجنينة',desc:'معلم تراثي في نطاق بركاء؛ موضع العلامة تقريبي ويحتاج إلى توثيق ميداني.'},
        {coords:[23.7000,57.8800],name:'سور حي عاصم',desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.'},
        {coords:[23.6900,57.9050],name:'سور الثرامد',desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.'},
        {coords:[23.6800,57.8750],name:'سور الموالك',desc:'موقع تراثي محلي مدرج للاستكشاف؛ الإحداثيات تقريبية.'}
      ],
      nature:[
        {coords:[23.7810,57.7720],name:'جزر الديمانيات الطبيعية',desc:'محمية بحرية تضم جزرًا ذات قيمة بيئية، وتشتهر بالشعاب المرجانية والحياة البحرية.'},
        {coords:[23.5500,57.8500],name:'منطقة الأودية والمزارع جنوب بركاء',desc:'منطقة تتنوع فيها المظاهر الطبيعية والأنشطة الزراعية.'},
        {coords:[23.1450,57.2650],name:'الجبل الأخضر',desc:'منطقة جبلية ضمن جبال الحجر، تشتهر بتنوعها الطبيعي ومدرجاتها الزراعية.'}
      ]
    };
</script>
