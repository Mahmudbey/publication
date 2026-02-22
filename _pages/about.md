---
layout: about
title: about
permalink: /
subtitle: Researcher & Senior Lecturer at National University of Uzbekistan.

profile:
  align: right
  image: prof_pic.jpg # O'zingizning rasmingizni assets/img/ papkasiga joylang
  image_circular: false
  more_info: >
    <p>Tashkent, Uzbekistan</p>

news: true 
selected_papers: true 
social: true
---

[cite_start]I am a Researcher and Senior Lecturer at the Department of Applied Mathematics and Computer Analysis, National University of Uzbekistan named after Mirzo Ulugbek[cite: 22]. [cite_start]I hold a Ph.D. in Applied Mathematics (Mathematical modelling. Numerical methods and software)[cite: 14].

[cite_start]My main research interests are the mathematical modelling of nonlinear physical processes, including analysing the qualitative features of differential equation solutions, developing self-similar solutions, and conducting research[cite: 17]. [cite_start]Furthermore, I am deeply interested in Data Analysis[cite: 18].

[cite_start]With over 6 years of experience in the Education and Research field [cite: 12][cite_start], I have extensive teaching experience in Mathematical modelling, Calculus, Database Management Systems, Numerical methods, and Programming (Python, C#, MATLAB) across multiple universities including NUUz and TSTU[cite: 19, 20, 73].

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: true # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: true
  scrollable: true # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

Write your biography here. Tell the world about yourself. Link to your favorite [subreddit](https://www.reddit.com). You can put a picture in, too. The code is already in, just name your picture `prof_pic.jpg` and put it in the `img/` folder.

Put your address / P.O. box / other info right below your picture. You can also disable any of these elements by editing `profile` property of the YAML header of your `_pages/about.md`. Edit `_bibliography/papers.bib` and Jekyll will render your [publications page](/al-folio/publications/) automatically.

Link to your social media connections, too. This theme is set up to use [Font Awesome icons](https://fontawesome.com/) and [Academicons](https://jpswalsh.github.io/academicons/), like the ones below. Add your Facebook, Twitter, LinkedIn, Google Scholar, or just disable all of them.

<div style="display: flex; justify-content: space-around; background: #f0f4f8; padding: 20px; border-radius: 10px; margin-top: 30px; border: 1px solid #d1d9e0;">
  <div style="text-align: center;">
    <h4 style="color: #005571; margin-bottom: 10px;">Scopus</h4>
    <p style="margin: 0;"><b>h-index:</b> {{ site.data.stats.scopus.h_index }}</p>
    <p style="margin: 0;"><b>Citations:</b> {{ site.data.stats.scopus.citations }}</p>
    <p style="margin: 0;"><b>Papers:</b> {{ site.data.stats.scopus.documents }}</p>
  </div>
  <div style="text-align: center; border-left: 1px solid #ccc; padding-left: 20px;">
    <h4 style="color: #A6CE39; margin-bottom: 10px;">ORCID</h4>
    <p style="margin: 0;"><b>Total Works:</b> {{ site.data.stats.orcid.works_count }}</p>
    <p style="margin: 0;"><b>iD:</b> <a href="https://orcid.org/{{ site.data.stats.orcid.id }}" target="_blank">{{ site.data.stats.orcid.id }}</a></p>
  </div>
</div>
