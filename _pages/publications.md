---
layout: page
title: publications
permalink: /publications/
nav: true
nav_order: 2
---

<style>
  /* 1. SAHIFANING ENI VA SHRIFTI */
  .post { max-width: 1100px !important; margin: 0 auto !important; }

  .publications, .publications .title, .publications .author, .publications .periodical {
    font-size: 1.15rem !important; /* Shrift kattalashtirildi */
    line-height: 1.6 !important;
    color: #333 !important;
  }

  .publications .title { font-weight: bold !important; color: #000 !important; }

  /* 2. "CITE" (BIB) VA BOSHQA TUGMALAR DIZAYNI */
  .publications .links a.btn {
    padding: 3px 12px !important;
    font-size: 0.9rem !important;
    border-radius: 4px !important;
    text-transform: none !important; /* "Cite" yozuvi chiroyli chiqishi uchun */
    margin-right: 8px !important;
    border: 1px solid #000 !important;
    color: #000 !important;
    background: transparent !important;
    font-weight: 500 !important;
  }
  
  .publications .links a.btn:hover {
    background: #000 !important;
    color: #fff !important;
  }

  /* 3. BIBTEX BOX (CITE BOSILGANDA CHIQADIGAN JOY) */
  .publications .bibtex {
    background-color: #f9f9f9 !important;
    border: 1px solid #ddd !important;
    padding: 15px !important;
    font-family: 'Courier New', monospace !important;
    font-size: 0.95rem !important;
    margin-top: 15px !important;
    border-radius: 6px !important;
    color: #444 !important;
    overflow-x: auto;
  }

  /* 4. ORTIQCHA YILLARNI VA O'NG TARAFNI O'CHIRISH */
  .publications .abbr, .publications .badge, .publications .year, .publications div.col-sm-2 {
    display: none !important;
  }

  /* 5. CHAP TARAFDAGI YILLAR (QORA) VA UZLUKSIZ NOMERATSIYA */
  .year-header {
    color: #000 !important;
    background: #f0f0f0;
    padding: 12px 18px;
    border-radius: 5px;
    margin: 45px 0 20px 0;
    font-weight: 900;
    font-size: 1.5rem !important;
    border-left: 8px solid #000;
  }

  .publications { counter-reset: global-pub-index; }
  .publications ol.bibliography { list-style: none !important; padding-left: 0 !important; }
  .publications ol.bibliography > li {
    counter-increment: global-pub-index;
    position: relative;
    padding-left: 50px !important;
    margin-bottom: 30px !important;
    width: 100% !important;
  }
  .publications ol.bibliography > li::before {
    content: counter(global-pub-index) ".";
    position: absolute; left: 5px; top: 0; font-weight: 900; color: #000; font-size: 1.2rem;
  }

  /* Maqola tarkibini to'liq eniga yoyish */
  .publications .col-sm-8 { max-width: 100% !important; flex: 0 0 100% !important; }
</style>

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 35px; padding: 20px; background: #fff; border: 1px solid #ddd; border-radius: 10px;">
    <input type="text" id="pubSearch" placeholder="Maqolalarni qidirish..." style="width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 6px;">
    <div style="margin-top: 15px; display: flex; gap: 10px;">
        <button onclick="exportData('bib')" class="btn btn-sm btn-outline-dark">Full .BIB</button>
        <button onclick="exportData('csv')" class="btn btn-sm btn-outline-dark">Full .CSV</button>
    </div>
  </div>

  <h1 style="font-weight: 900; border-bottom: 5px solid #000; padding-bottom: 10px; text-transform: uppercase;">All Publications</h1>

  {% assign publication_years = "2026,2025,2024,2023,2022,2021" | split: "," %}
  
  {% for y in publication_years %}
    {% capture entries %}{% bibliography -f papers -q @*[year={{y}}]* --bibtex_show %}{% endcapture %}
    {% if entries contains "li" %}
      <h2 class="year-header">{{ y }}</h2>
      <div class="year-block">
        {{ entries }}
      </div>
    {% endif %}
  {% endfor %}

</div>

<script>
// "Bib" tugmasini "Cite" ga o'zgartirish va hamma yozuvlarni tekshirish
function customizeButtons() {
    let bibButtons = document.querySelectorAll('.links a.bib');
    bibButtons.forEach(btn => {
        btn.innerText = "Cite";
        btn.classList.add('btn', 'btn-sm', 'z-depth-0');
    });
}

// Sahifa yuklanganda va har safar qidiruv qilinganda tugmalarni yangilab turish
document.addEventListener("DOMContentLoaded", customizeButtons);
setInterval(customizeButtons, 500); // Dinamik elementlar uchun

// Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let items = document.querySelectorAll('.bibliography > li');
    items.forEach(item => {
        item.style.display = item.innerText.toLowerCase().includes(filter) ? "" : "none";
    });
});

function exportData(type) {
    if (type === 'bib') {
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
        return;
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
