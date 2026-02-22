---
layout: page
title: publications
permalink: /publications/
nav: true
nav_order: 2
---

<style>
  /* 1. SAHIFANING UMUMIY ENI VA SHRIFTI */
  .post { max-width: 1100px !important; margin: 0 auto !important; }

  .publications, .publications .title, .publications .author, .publications .periodical {
    font-size: 1.1rem !important;
    line-height: 1.5 !important;
    color: #333 !important;
  }

  .publications .title { font-weight: bold !important; color: #000 !important; }

  /* 2. "CITE" VA BOSHQA TUGMALAR DIZAYNI */
  .publications .links a.btn {
    padding: 2px 10px !important;
    font-size: 0.85rem !important;
    border-radius: 4px !important;
    text-transform: uppercase !important;
    margin-right: 5px !important;
    border: 1px solid #007bff !important;
    color: #007bff !important;
    background: transparent !important;
  }
  
  .publications .links a.btn:hover {
    background: #007bff !important;
    color: #fff !important;
  }

  /* 3. BIBTEX BOX (CITE BOSILGANDA CHIQADIGAN JOY) */
  .publications .bibtex {
    background-color: #f8f9fa !important;
    border: 1px solid #ddd !important;
    padding: 15px !important;
    font-family: monospace !important;
    font-size: 0.9rem !important;
    margin-top: 10px !important;
    border-radius: 5px !important;
    display: none; /* Default yashiriq */
  }

  /* 4. O'NG TARAFNI VA ORTIQCHA YILLARNI O'CHIRISH */
  .publications .abbr, .publications .badge, .publications .year, .publications div.col-sm-2 {
    display: none !important;
  }

  /* 5. CHAP TARAFDAGI YILLAR VA NOMERATSIYA */
  .year-header {
    color: #000 !important;
    background: #eeeeee;
    padding: 10px 15px;
    border-radius: 4px;
    margin: 40px 0 20px 0;
    font-weight: 900;
    border-left: 6px solid #000;
  }

  .publications { counter-reset: main-counter; }
  .publications ol.bibliography { list-style: none !important; padding-left: 0 !important; }
  .publications ol.bibliography > li {
    counter-increment: main-counter;
    position: relative;
    padding-left: 45px !important;
    margin-bottom: 25px !important;
  }
  .publications ol.bibliography > li::before {
    content: counter(main-counter) ".";
    position: absolute; left: 0; font-weight: 900; color: #000;
  }

  /* Maqola tarkibini to'liq eniga yoyish */
  .publications .col-sm-8 { max-width: 100% !important; flex: 0 0 100% !important; }
</style>

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 30px; padding: 20px; border: 1px solid #ddd; border-radius: 8px;">
    <input type="text" id="pubSearch" placeholder="Qidiruv..." style="width: 100%; padding: 12px; border-radius: 5px; border: 1px solid #ccc;">
    <div style="margin-top: 15px; display: flex; gap: 8px;">
        <button onclick="exportData('bib')" class="btn btn-sm btn-outline-dark">Full .BIB</button>
        <button onclick="exportData('csv')" class="btn btn-sm btn-outline-dark">Full .CSV</button>
    </div>
  </div>

  <h1 style="font-weight: 900; border-bottom: 4px solid #000; padding-bottom: 10px;">All Publications</h1>

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
// "Cite" tugmasi nomini o'zgartirish va funksiyasini ulash
document.addEventListener("DOMContentLoaded", function() {
    // al-folio da odatda "Bib" deb chiqadi, biz uni "Cite" ga o'zgartiramiz
    let bibButtons = document.querySelectorAll('.links a.bib');
    bibButtons.forEach(btn => {
        btn.innerText = "Cite";
        btn.classList.add('btn'); // Bootstrap stili uchun
    });
});

// Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let items = document.querySelectorAll('.bibliography > li');
    items.forEach(item => {
        item.style.display = item.innerText.toLowerCase().includes(filter) ? "" : "none";
    });
});

// Ma'lumotlarni yig'ish (Eksport uchun)
function exportData(type) {
    if (type === 'bib') {
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
        return;
    }
    // ... CSV/TXT eksporti kodlari shu yerda qoladi
}

function downloadFile(content, fileName, contentType) {
    let a = document.createElement("a");
    let file = new Blob([content], {type: contentType});
    a.href = URL.createObjectURL(file);
    a.download = fileName;
    a.click();
}
</script>
