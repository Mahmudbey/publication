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
    font-size: 1.15rem !important;
    line-height: 1.6 !important;
    color: #333 !important;
  }

  .publications .title { font-weight: bold !important; color: #000 !important; }

  /* 2. CITE TUGMASI VA BOSHQA LINKLAR */
  .publications .links a, .custom-cite-btn {
    display: inline-block;
    padding: 4px 12px !important;
    font-size: 0.9rem !important;
    border-radius: 4px !important;
    margin-right: 8px !important;
    border: 1px solid #007bff !important;
    color: #007bff !important;
    background: transparent !important;
    font-weight: 500 !important;
    text-decoration: none;
    cursor: pointer;
  }
  
  .publications .links a:hover, .custom-cite-btn:hover {
    background: #007bff !important;
    color: #fff !important;
  }

  /* 3. BIBTEX BOX (CITE BOSILGANDA CHIQADIGAN OYNA) */
  .bibtex-box {
    background-color: #f8f9fa;
    border: 1px solid #ddd;
    border-left: 4px solid #007bff;
    padding: 15px;
    font-family: 'Courier New', monospace;
    font-size: 0.95rem;
    margin-top: 15px;
    border-radius: 4px;
    color: #333;
    white-space: pre-wrap;
    overflow-x: auto;
  }

  /* 4. O'NG TARAFNI VA ORTIQCHA YILLARNI YASHIRISH */
  .publications .abbr, .publications .badge, .publications .year, .publications div.col-sm-2 {
    display: none !important;
  }

  /* 5. YILLAR (QORA) VA DINAMIK RAQAMLASH */
  .year-header {
    color: #000 !important;
    background: #eeeeee;
    padding: 12px 20px;
    border-radius: 5px;
    margin: 45px 0 20px 0;
    font-weight: 900;
    font-size: 1.6rem !important;
    border-left: 8px solid #000;
  }

  /* 6. UZLUKSIZ NOMERATSIYA */
  .publications { counter-reset: global-pub-index; }
  .publications ol.bibliography { list-style: none !important; padding-left: 0 !important; }
  .publications ol.bibliography > li {
    counter-increment: global-pub-index;
    position: relative;
    padding-left: 50px !important;
    margin-bottom: 35px !important;
    width: 100% !important;
  }
  .publications ol.bibliography > li::before {
    content: counter(global-pub-index) ".";
    position: absolute; left: 5px; top: 0; 
    font-weight: 900; color: #000; font-size: 1.2rem;
  }

  .publications .col-sm-8 { max-width: 100% !important; flex: 0 0 100% !important; }
</style>

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 35px; padding: 20px; background: #fff; border: 1px solid #ddd; border-radius: 10px;">
    <input type="text" id="pubSearch" placeholder="Maqolalarni qidirish..." style="width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 6px; font-size: 1rem;">
    <div style="margin-top: 15px; display: flex; gap: 10px;">
        <button onclick="exportData('bib')" class="custom-cite-btn" style="border-color: #333; color: #333;">Full .BIB</button>
        <button onclick="exportData('csv')" class="custom-cite-btn" style="border-color: #333; color: #333;">Full .CSV</button>
    </div>
  </div>

  <h1 style="font-weight: 900; border-bottom: 5px solid #000; padding-bottom: 10px; text-transform: uppercase;">All Publications</h1>

  {% assign publication_years = "2026,2025,2024,2023,2022,2021" | split: "," %}
  
  {% for y in publication_years %}
    {% capture entries %}{% bibliography -f papers -q @*[year={{y}}]* %}{% endcapture %}
    
    {% assign count = entries | split: "<li" | size | minus: 1 %}
    
    {% if count > 0 %}
      <h2 class="year-header">{{ y }} ({{ count }})</h2>
      <div class="year-block">
        {{ entries }}
      </div>
    {% endif %}
  {% endfor %}

</div>

<script>
// 1. BibTeX ma'lumotlarini serverdan fonda yuklab olish
let globalBibData = "";
fetch('{{ site.baseurl }}/assets/bibliography/papers.bib')
    .then(response => response.text())
    .then(data => {
        globalBibData = data;
        injectCiteButtons(); // Ma'lumot kelgach tugmalarni qo'yish
    });

// 2. Kafolatlangan "Cite" tugmalarini qo'shish logikasi
function injectCiteButtons() {
    document.querySelectorAll('ol.bibliography > li').forEach(li => {
        let id = li.id; // Maqolaning BibTeX kaliti (Masalan: Mukhamadiyev2026)
        if (!id) return;

        // Tugmalar joylashadigan qutini topish yoki yaratish
        let linksDiv = li.querySelector('.links');
        if (!linksDiv) {
            linksDiv = document.createElement('div');
            linksDiv.className = 'links';
            li.appendChild(linksDiv);
        }

        // Agar "Cite" tugmasi hali qo'yilmagan bo'lsa, qo'shamiz
        if (!linksDiv.querySelector('.custom-cite-btn')) {
            let citeBtn = document.createElement('button');
            citeBtn.innerText = 'Cite';
            citeBtn.className = 'custom-cite-btn';
            
            // BibTeX chiqadigan yashirin quti yaratamiz
            let bibBox = document.createElement('div');
            bibBox.className = 'bibtex-box';
            bibBox.style.display = 'none';

            // Tugma bosilganda nima bo'lishi:
            citeBtn.onclick = function(e) {
                e.preventDefault();
                if (bibBox.style.display === 'none') {
                    // .bib faylidan aynan shu maqolani qirqib olish
                    let blocks = globalBibData.split(/\n\s*@/);
                    let foundBib = "";
                    for(let b of blocks) {
                        if(b.includes('{' + id + ',')) {
                            foundBib = (b.startsWith('@') ? '' : '@') + b.trim();
                            break;
                        }
                    }
                    bibBox.innerText = foundBib || "BibTeX ma'lumoti topilmadi. .bib faylini tekshiring.";
                    bibBox.style.display = 'block';
                } else {
                    bibBox.style.display = 'none'; // Qayta bosa yopiladi
                }
            };

            linksDiv.prepend(citeBtn); // Tugmani eng oldiga qo'shish
            li.appendChild(bibBox); // Qutini maqola tagiga qo'shish
        }
    });
}

// 3. Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let items = document.querySelectorAll('.bibliography > li');
    items.forEach(item => {
        item.style.display = item.innerText.toLowerCase().includes(filter) ? "" : "none";
    });
});

// 4. Global Eksport
function exportData(type) {
    if (type === 'bib') {
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
    } else if (type === 'csv') {
        let data = "Title,Author,Journal Info\n";
        document.querySelectorAll('.bibliography > li').forEach(li => {
            if (li.style.display !== "none") {
                let t = li.querySelector('.title')?.innerText.trim() || "";
                let a = li.querySelector('.author')?.innerText.trim() || "";
                let j = li.querySelector('.periodical')?.innerText.trim() || "";
                data += `"${t}","${a}","${j}"\n`;
            }
        });
        let a = document.createElement("a");
        a.href = URL.createObjectURL(new Blob([data], {type: "text/csv"}));
        a.download = "publications.csv";
        a.click();
    }
}
</script>
