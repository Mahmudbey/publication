---
layout: page
title: publications
permalink: /publications/
nav: true
nav_order: 2
---

<style>
  /* 1. SAHIFANING ENI (WIDTH) - KATTALASHTIRILDI */
  /* al-folio dagi asosiy konteyner kengligini 1100px gacha kengaytiramiz */
  .post {
    max-width: 1100px !important; 
    margin: 0 auto !important;
  }

  /* 2. SHRIFT O'LCHAMI - KATTALASHTIRILDI */
  .publications, 
  .publications .title, 
  .publications .author, 
  .publications .periodical, 
  .publications .links a {
    font-size: 1.15rem !important; /* 1.0rem dan 1.15rem ga oshirildi */
    line-height: 1.6 !important;
  }
  
  .publications .title {
    font-weight: 600 !important; /* Sarlavhani biroz qalinroq qilamiz */
  }

  /* 3. O'NG TARAFDAGI YIL VA BADGE-LARNI BUTUNLAY O'CHIRISH */
  /* al-folio bibliografiyasida o'ng tomonda chiqadigan barcha elementlarni yashiramiz */
  .publications .abbr,
  .publications .badge,
  .publications .year,
  .publications .periodical + br + .year,
  .publications div.col-sm-2 {
    display: none !important;
    visibility: hidden !important;
    width: 0 !important;
    padding: 0 !important;
    margin: 0 !important;
  }

  /* 4. CHAP TARAFDAGI YILLAR (QORA VA ANIY) */
  .year-header {
    color: #000000 !important;
    background-color: #f8f9fa !important;
    padding: 15px !important;
    display: block !important;
    border-radius: 6px !important;
    margin: 45px 0 20px 0 !important;
    font-size: 1.5rem !important;
    font-weight: 800 !important;
    border-left: 8px solid #000 !important;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.05);
  }

  /* 5. UZLUKSIZ NOMERATSIYA (Global Counter) */
  .publications {
    counter-reset: main-pub-counter;
  }
  .publications ol.bibliography {
    list-style: none !important;
    padding-left: 0 !important;
  }
  .publications ol.bibliography > li {
    counter-increment: main-pub-counter;
    position: relative;
    padding-left: 50px !important;
    margin-bottom: 25px !important;
    display: block !important;
    width: 100% !important; /* Maqola butun eni bo'ylab yoyilsin */
  }
  .publications ol.bibliography > li::before {
    content: counter(main-pub-counter) ".";
    position: absolute;
    left: 10px;
    top: 0;
    font-weight: 900;
    font-size: 1.2rem;
    color: #000;
  }

  /* Maqola tarkibi o'ngga surilib qolmasligi uchun */
  .publications .col-sm-8, .publications .col-sm-9 {
    max-width: 100% !important;
    flex: 0 0 100% !important;
  }
</style>

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 35px; padding: 25px; background: #fff; border: 1px solid #ddd; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05);">
    <input type="text" id="pubSearch" placeholder="Maqolalarni qidirish (sarlavha, yil, muallif)..." 
           style="width: 100%; padding: 14px; border: 1px solid #ccc; border-radius: 8px; font-size: 1.1rem;">
    <div style="margin-top: 20px; display: flex; gap: 12px;">
      <button onclick="exportData('bib')" class="btn btn-sm" style="background: #000; color: #fff; border: none; padding: 8px 20px;">Export .BIB</button>
      <button onclick="exportData('csv')" class="btn btn-sm" style="background: #444; color: #fff; border: none; padding: 8px 20px;">Export .CSV</button>
      <button onclick="exportData('txt')" class="btn btn-sm" style="background: #777; color: #fff; border: none; padding: 8px 20px;">Export .TXT</button>
    </div>
  </div>

  <h1 style="font-weight: 900; text-transform: uppercase; border-bottom: 5px solid #000; padding-bottom: 10px;">All Publications</h1>

  {% assign year_list = "2026,2025,2024,2023,2022,2021" | split: "," %}
  
  {% for y in year_list %}
    {% capture bib_content %}{% bibliography -f papers -q @*[year={{y}}]* %}{% endcapture %}
    {% if bib_content contains "li" %}
      <h2 class="year-header">{{ y }}</h2>
      <div class="year-block">
        {{ bib_content }}
      </div>
    {% endif %}
  {% endfor %}

</div>

<script>
// Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let items = document.querySelectorAll('.bibliography > li');
    items.forEach(item => {
        item.style.display = item.innerText.toLowerCase().includes(filter) ? "" : "none";
    });
});

// Eksport funksiyasi
function exportData(type) {
    if (type === 'bib') {
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
        return;
    }
    let items = document.querySelectorAll('.bibliography > li');
    let data = [];
    items.forEach(item => {
        if (item.style.display !== "none") {
            data.push({
                t: item.querySelector('.title')?.innerText.trim() || "",
                a: item.querySelector('.author')?.innerText.trim() || "",
                j: item.querySelector('.periodical')?.innerText.trim() || ""
            });
        }
    });
    if (type === 'csv') {
        let csv = "Title,Author,Journal\n" + data.map(d => `"${d.t}","${d.a}","${d.j}"`).join("\n");
        downloadFile(csv, "publications.csv", "text/csv");
    } else if (type === 'txt') {
        let txt = data.map(d => `${d.t}\n${d.a}\n${d.j}\n`).join("\n---\n");
        downloadFile(txt, "publications_list.txt", "text/plain");
    }
}

function downloadFile(content, fileName, contentType) {
    let a = document.createElement("a");
    let file = new Blob([content], {type: contentType});
    a.href = URL.createObjectURL(file);
    a.download = fileName;
    a.click();
}
</script>
