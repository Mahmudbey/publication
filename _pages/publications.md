---
layout: page
title: publications
permalink: /publications/
nav: true
nav_order: 2
---

<div class="publications">

  <div class="pub-filters" style="margin-bottom: 30px; padding: 20px; background: #f8f9fa; border-radius: 12px; border: 1px solid #e9ecef;">
    <h5 style="margin-bottom: 15px; color: #333;">Maqolalarni boshqarish</h5>
    <input type="text" id="pubSearch" placeholder="Maqola nomi, muallif yoki yilni yozing..." 
           style="width: 100%; padding: 12px; margin-bottom: 20px; border: 1px solid #ced4da; border-radius: 8px; font-size: 1rem;">
    
    <div style="display: flex; gap: 12px; flex-wrap: wrap; align-items: center;">
      <span style="font-size: 0.9rem; font-weight: bold; color: #555;">Eksport:</span>
      <button onclick="exportData('bib')" class="btn btn-sm" style="background-color: #007bff; color: white; border: none; padding: 5px 15px; border-radius: 4px;">.BIB</button>
      <button onclick="exportData('csv')" class="btn btn-sm" style="background-color: #28a745; color: white; border: none; padding: 5px 15px; border-radius: 4px;">.CSV</button>
      <button onclick="exportData('txt')" class="btn btn-sm" style="background-color: #6c757d; color: white; border: none; padding: 5px 15px; border-radius: 4px;">.TXT</button>
    </div>
    <p style="font-size: 0.75rem; margin-top: 10px; color: #888;">* Eslatma: Eksport faqat qidiruv natijasida ko'rinib turgan maqolalarni o'z ichiga oladi.</p>
  </div>

  <div id="publications-list">
    {% bibliography %}
  </div>

</div>

<script>
// Qidiruv funksiyasi
document.getElementById('pubSearch').addEventListener('keyup', function() {
    let filter = this.value.toLowerCase();
    let items = document.querySelectorAll('.bibliography > li');
    
    items.forEach(item => {
        let text = item.innerText.toLowerCase();
        item.style.display = text.includes(filter) ? "" : "none";
    });
});

// Mukammal Eksport funksiyasi
function exportData(type) {
    if (type === 'bib') {
        // .bib faylini ochiq manzildan yuklab olish
        window.open('{{ site.baseurl }}/assets/bibliography/papers.bib', '_blank');
        return;
    }

    let items = document.querySelectorAll('.bibliography > li');
    let data = [];
    
    items.forEach(item => {
        if (item.style.display !== "none") {
            let title = item.querySelector('.title')?.innerText.trim() || "Noma'lum";
            let author = item.querySelector('.author')?.innerText.trim() || "Noma'lum";
            let periodical = item.querySelector('.periodical')?.innerText.trim() || "";
            
            // Yilni ajratish
            let yearMatch = periodical.match(/\d{4}/);
            let year = yearMatch ? yearMatch[0] : "";
            
            // DOI linkini topish
            let doiLink = item.querySelector('a[href*="doi.org"]')?.href || "";
            let doi = doiLink ? doiLink.split('doi.org/')[1] : "";

            data.push({ title, author, journal: periodical.replace(year, "").replace(/, $/, "").trim(), year, doi });
        }
    });

    if (data.length === 0) {
        alert("Eksport qilish uchun maqolalar topilmadi!");
        return;
    }

    if (type === 'csv') {
        const header = "Title,Authors,Journal,Year,DOI\n";
        const rows = data.map(d => `"${d.title.replace(/"/g, '""')}","${d.author.replace(/"/g, '""')}","${d.journal.replace(/"/g, '""')}","${d.year}","${d.doi}"`).join("\n");
        downloadFile(header + rows, "publications_mahmudbey.csv", "text/csv");
    } else if (type === 'txt') {
        const content = data.map(d => 
            `TITLE: ${d.title}\nAUTHORS: ${d.author}\nJOURNAL: ${d.journal}\nYEAR: ${d.year}\nDOI: ${d.doi}\n`
        ).join("\n---\n");
        downloadFile(content, "publications_list.txt", "text/plain");
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

<style>
/* Dizaynni biroz yaxshilash */
.publications h2.year {
    margin-top: 40px;
    border-bottom: 2px solid #eee;
    padding-bottom: 10px;
    color: #444;
}
.btn:hover {
    opacity: 0.8;
    cursor: pointer;
}
</style>
