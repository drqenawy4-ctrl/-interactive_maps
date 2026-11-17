<style>
#oman-heritage-tour {
  direction: rtl;
  padding: 55px 20px;
  background: linear-gradient(145deg,#071b2b,#102f40 60%,#173d48);
  color: #fff;
  font-family: Tahoma, Arial, sans-serif;
  border-radius: 22px;
  margin: 30px auto;
  max-width: 1250px;
}
#oman-heritage-tour * { box-sizing: border-box; }
#oman-heritage-tour .tour-head {
  text-align: center;
  margin-bottom: 30px;
}
#oman-heritage-tour .tour-head h2 {
  color: #e5c27a;
  font-size: clamp(25px,4vw,38px);
  margin: 0 0 12px;
}
#oman-heritage-tour .tour-head p {
  color: #d4e0e5;
  line-height: 1.9;
  max-width: 760px;
  margin: auto;
}
#oman-heritage-tour .tour-hero {
  background: linear-gradient(120deg,#183d4b,#0a2435);
  border: 1px solid #bd9b5b;
  border-radius: 20px;
  padding: 25px;
  text-align: center;
  margin-bottom: 30px;
}
#oman-heritage-tour .tour-hero .globe {
  font-size: 65px;
  margin-bottom: 10px;
}
#oman-heritage-tour .tour-hero h3 {
  color: #f0d18e;
  font-size: 23px;
}
#oman-heritage-tour .tour-actions {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
  margin-top: 20px;
}
#oman-heritage-tour .tour-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 12px 18px;
  border-radius: 12px;
  text-decoration: none;
  font-weight: bold;
  font-size: 14px;
  transition: .2s;
  border: 1px solid transparent;
}
#oman-heritage-tour .tour-btn:hover {
  transform: translateY(-3px);
  filter: brightness(1.1);
}
#oman-heritage-tour .google-btn {
  background: #e8c779;
  color: #142b38;
}
#oman-heritage-tour .earth-btn {
  background: #147e80;
  color: white;
}
#oman-heritage-tour .street-btn {
  background: #244c60;
  color: white;
  border-color: #6b9cac;
}
#oman-heritage-tour .tour-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit,minmax(245px,1fr));
  gap: 18px;
}
#oman-heritage-tour .heritage-card {
  background: rgba(255,255,255,.07);
  border: 1px solid rgba(229,194,122,.4);
  border-radius: 17px;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  min-width: 0;
}
#oman-heritage-tour .heritage-card .fort-icon {
  font-size: 35px;
}
#oman-heritage-tour .heritage-card h3 {
  color: #f0d18e;
  margin: 0;
  font-size: 20px;
}
#oman-heritage-tour .heritage-card p {
  color: #d3e0e5;
  margin: 0;
  line-height: 1.7;
  font-size: 14px;
}
#oman-heritage-tour .heritage-card .tour-actions {
  margin-top: auto;
  flex-direction: column;
}
#oman-heritage-tour .heritage-card .tour-btn {
  width: 100%;
}
#oman-heritage-tour .tour-note {
  text-align: center;
  color: #c7d8df;
  font-size: 13px;
  line-height: 1.8;
  margin-top: 25px;
}
@media(max-width:600px) {
  #oman-heritage-tour { padding: 35px 13px; }
  #oman-heritage-tour .tour-hero { padding: 18px 12px; }
}
</style>

<section id="oman-heritage-tour">

  <header class="tour-head">
    <h2>رحلة عبر التاريخ العُماني</h2>
    <p>
      انطلق في جولة افتراضية لاستكشاف القلاع والحصون
      والمعالم التاريخية في سلطنة عُمان باستخدام
      Google Maps وGoogle Earth.
    </p>
  </header>

  <div class="tour-hero">
    <div class="globe">🌍</div>
    <h3>استكشف عُمان من الفضاء إلى المعلم التراثي</h3>
    <p>
      شاهد الموقع جغرافيًا، وانتقل إلى صور الأقمار الصناعية،
      وافتح تجربة الأرض ثلاثية الأبعاد حيث تتوفر.
    </p>
    <div class="tour-actions">
      <a class="tour-btn google-btn"
         href="https://www.google.com/maps/search/?api=1&query=Oman"
         target="_blank" rel="noopener noreferrer">
        🌐 استكشف عُمان على Google Maps
      </a>
      <a class="tour-btn earth-btn"
         href="https://earth.google.com/web/"
         target="_blank" rel="noopener noreferrer">
        🌎 افتح Google Earth ثلاثي الأبعاد
      </a>
    </div>
  </div>

  <div class="tour-grid" id="omanHeritageCards"></div>

  <p class="tour-note">
    ملاحظة: توفر العرض ثلاثي الأبعاد والتجول البانورامي
    يختلف حسب المعلم والتغطية المتاحة في Google.
    روابط الجولة تفتح في نافذة مستقلة، ولا تحتاج إلى
    مفتاح Google Maps API.
  </p>

</section>

<script>
(function () {
  "use strict";

  const heritageSites = [
    {
      name: "قلعة نزوى",
      city: "نزوى - الداخلية",
      lat: 22.933439,
      lng: 57.530614,
      description: "معلم تاريخي بارز يجسد العمارة الدفاعية العُمانية."
    },
    {
      name: "قلعة بهلاء",
      city: "بهلاء - الداخلية",
      lat: 22.973978,
      lng: 57.312870,
      description: "قلعة تاريخية مدرجة ضمن قائمة التراث العالمي."
    },
    {
      name: "قلعة جبرين",
      city: "بهلاء - الداخلية",
      lat: 22.915495,
      lng: 57.249798,
      description: "تتميز بالعمارة العُمانية والزخارف والأسقف التقليدية."
    },
    {
      name: "حصن الرستاق",
      city: "الرستاق - جنوب الباطنة",
      lat: 23.391827,
      lng: 57.426260,
      description: "أحد المعالم التاريخية المهمة في محافظة جنوب الباطنة."
    },
    {
      name: "قلعة نخل",
      city: "نخل - جنوب الباطنة",
      lat: 23.395048,
      lng: 57.829249,
      description: "حصن تاريخي يطل على مشهد جبلي مميز."
    },
    {
      name: "قلعة الحزم",
      city: "الرستاق - جنوب الباطنة",
      lat: 23.558448,
      lng: 57.483460,
      description: "نموذج مميز لفنون البناء والتحصين العُماني."
    },
    {
      name: "قلعة صحار",
      city: "صحار - شمال الباطنة",
      lat: 24.362402,
      lng: 56.749850,
      description: "ترتبط بتاريخ صحار البحري والتجاري."
    },
    {
      name: "قلعة خصب",
      city: "خصب - مسندم",
      lat: 26.198243,
      lng: 56.249772,
      description: "معلم تراثي في محافظة مسندم."
    },
    {
      name: "قلعة مرباط",
      city: "مرباط - ظفار",
      lat: 16.993176,
      lng: 54.694651,
      description: "معلم تاريخي في الساحل الشرقي لمحافظة ظفار."
    }
  ];

  const container = document.getElementById("omanHeritageCards");

  if (!container) return;

  function makeLink(url, className, label) {
    const a = document.createElement("a");
    a.className = "tour-btn " + className;
    a.href = url;
    a.target = "_blank";
    a.rel = "noopener noreferrer";
    a.textContent = label;
    return a;
  }

  heritageSites.forEach(function (site) {
    const coords = site.lat + "," + site.lng;

    const mapsUrl =
      "https://www.google.com/maps/search/?api=1&query=" +
      encodeURIComponent(coords);

    const earthUrl =
      "https://earth.google.com/web/search/" +
      encodeURIComponent(site.name + ", Oman");

    const streetUrl =
      "https://www.google.com/maps/@?api=1&map_action=pano&viewpoint=" +
      encodeURIComponent(coords);

    const directionsUrl =
      "https://www.google.com/maps/dir/?api=1&destination=" +
      encodeURIComponent(coords);

    const card = document.createElement("article");
    card.className = "heritage-card";

    const icon = document.createElement("div");
    icon.className = "fort-icon";
    icon.textContent = "🏰";

    const title = document.createElement("h3");
    title.textContent = site.name;

    const city = document.createElement("p");
    city.textContent = "📍 " + site.city;

    const description = document.createElement("p");
    description.textContent = site.description;

    const actions = document.createElement("div");
    actions.className = "tour-actions";

    actions.appendChild(
      makeLink(mapsUrl, "google-btn", "📍 الموقع على Google Maps")
    );

    actions.appendChild(
      makeLink(earthUrl, "earth-btn", "🌎 استكشاف ثلاثي الأبعاد")
    );

    actions.appendChild(
      makeLink(streetUrl, "street-btn", "🔄 تجربة Street View 360°")
    );

    actions.appendChild(
      makeLink(directionsUrl, "street-btn", "🧭 الوصول إلى المعلم")
    );

    card.appendChild(icon);
    card.appendChild(title);
    card.appendChild(city);
    card.appendChild(description);
    card.appendChild(actions);

    container.appendChild(card);
  });

})();
</script>
