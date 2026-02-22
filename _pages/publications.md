---
layout: page
title: publications
permalink: /publications/
nav: true
nav_order: 2
---

<style>
  /* 1. ENI VA SHRIFT (KATTALASHTIRILGAN) */
  .post { max-width: 1100px !important; margin: 0 auto !important; }
  .publications { font-size: 1.15rem !important; line-height: 1.6 !important; color: #111 !important; }
  .publications .title { font-weight: bold !important; color: #000 !important; }

  /* 2. O'NG TARAF VA ORTIQCHA YILLARNI QIRQIB TASHLASH */
  .abbr, .badge, .col-sm-2 { display: none !important; }
  .col-sm-8, .col-sm-9, .col-sm-10 { max-width: 100% !important; flex: 0 0 100% !important; margin-left: 0 !important; }

  /* 3. YILLAR (QORA) VA DINAMIK RAQAMLASH */
  .year-header {
    color: #000 !important; background: #f0f0f0; padding: 12px 20px;
    border-radius: 6px; margin: 45px 0 20px 0; font-weight: 900;
    font-size: 1.6rem !important; border-left: 8px solid #000;
  }

  .publications { counter-reset: global-pub; }
  .bibliography .row {
    counter-increment: global-pub; position: relative;
    padding-left: 45px !important; margin-bottom: 30px !important; display: block !important;
  }
  .bibliography .row::before {
    content: counter(global-pub) "."; position: absolute; left: 0; top: 0; 
    font-weight: 900; color: #000; font-size: 1.3rem;
  }

  /* 4. CITE TUGMASI VA BIBTEX OYNASI */
  .custom-cite-btn {
    padding: 4px 15px; font-size: 0.95rem; border-radius: 4px;
    border: 1px solid #007bff; color: #007bff; background: transparent; cursor: pointer;
    margin-right: 8px; text-decoration: none; display: inline-block; font-weight: 600;
  }
  .custom-cite-btn:hover { background: #007bff; color: #fff; }
  
  .bibtex-box {
    background: #f8f9fa; border: 1px solid #ddd; border-left: 4px solid #007bff;
    padding: 15px; font-family: monospace; font-size: 0.9rem; margin-top: 15px;
    display: none; white-space: pre-wrap; overflow-x: auto; color: #333; border-radius: 4px;
  }
</style>

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 35px; padding: 20px; background: #fff; border: 1px solid #ddd; border-radius: 10px;">
    <input type="text" id="pubSearch" placeholder="Maqola nomi, yili yoki muallifini qidiring..." style="width: 100%; padding: 12px; border: 1px solid #ccc; border-radius: 6px; font-size: 1rem;">
    <div style="margin-top: 15px; display: flex; gap: 10px;">
        <button onclick="exportData('bib')" class="custom-cite-btn" style="border-color: #333; color: #333;">Export .BIB</button>
        <button onclick="exportData('csv')" class="custom-cite-btn" style="border-color: #333; color: #333;">Export .CSV</button>
        <button onclick="exportData('txt')" class="custom-cite-btn" style="border-color: #333; color: #333;">Export .TXT</button>
    </div>
  </div>

  <h1 style="font-weight: 900; border-bottom: 5px solid #000; padding-bottom: 10px; text-transform: uppercase;">All Publications</h1>

  {% assign publication_years = "2026,2025,2024,2023,2022,2021" | split: "," %}
  
  {% for y in publication_years %}
    {% capture entries %}{% bibliography -f papers -q @*[year={{y}}]* %}{% endcapture %}
    {% assign count = entries | split: 'class="row"' | size | minus: 1 %}
    
    {% if count > 0 %}
      <h2 class="year-header">{{ y }} ({{ count }})</h2>
      <div class="year-block">
        {{ entries }}
      </div>
    {% endif %}
  {% endfor %}

</div>

<script>
// 1. BibTeX kodini serverdan olish va Cite tugmalarini joylashtirish
fetch('{{ site.baseurl }}/assets/bibliography/papers.bib')
  .then(response => response.text())
  .then(data => {
      document.querySelectorAll('.bibliography .row').forEach(row => {
          let bibElement = row.querySelector('[id]');
          if (!bibElement) return;
          let bibKey = bibElement.id; 

          let linksDiv = bibElement.querySelector('.links');
          if (!linksDiv) {
              linksDiv = document.createElement('div');
              linksDiv.className = 'links';
              bibElement.appendChild(linksDiv);
          }

          if (!linksDiv.querySelector('.custom-cite-btn')) {
              let citeBtn = document.createElement('button');
              citeBtn.className = 'custom-cite-btn';
              citeBtn.innerText = 'Cite';
              
              let bibBox = document.createElement('div');
              bibBox.className = 'bibtex-box';

              citeBtn.onclick = function(e) {
                  e.preventDefault();
                  if (bibBox.style.display === 'none' || bibBox.style.display === '') {
                      let regex = new RegExp("@[A-Za-z]+\\{" + bibKey + ",[\\s\\S]*?\\n\\}", "g");
                      let match = data.match(regex);
                      bibBox.innerText = match ? match[0] : "BibTeX kodi topilmadi.";
                      bibBox.style.display = 'block';
                  } else {
                      bibBox.style.display = 'none';
                  }
              };

              linksDiv.prepend(citeBtn);
              bibElement.appendChild(bibBox);
          }
      });
  });

// 2. Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    document.querySelectorAll('.bibliography .row').forEach(row => {
        row.style.display = row.innerText.toLowerCase().includes(filter) ? "" : "none";
    });
});

// 3. Eksport funksiyasi (.csv, .txt, .bib)
function exportData(type) {
    if (type === 'bib') {
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
        return;
    }
    
    let csvData = "Title,Author,Journal Info\n";
    let txtData = "";
    
    document.querySelectorAll('.bibliography .row').forEach(row => {
        if (row.style.display !== "none") {
            let t = row.querySelector('.title')?.innerText.replace(/"/g, '""').trim() || "";
            let a = row.querySelector('.author')?.innerText.replace(/"/g, '""').trim() || "";
            let j = row.querySelector('.periodical')?.innerText.replace(/"/g, '""').trim() || "";
            
            csvData += `"${t}","${a}","${j}"\n`;
            txtData += `TITLE: ${t}\nAUTHORS: ${a}\nJOURNAL: ${j}\n---\n`;
        }
    });

    let content = type === 'csv' ? csvData : txtData;
    let mimeType = type === 'csv' ? 'text/csv' : 'text/plain';
    let ext = type === 'csv' ? 'csv' : 'txt';

    let a = document.createElement("a");
    a.href = URL.createObjectURL(new Blob([content], {type: mimeType}));
    a.download = `publications.${ext}`;
    a.click();
}
</script>
