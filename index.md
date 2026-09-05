---
layout: home
title: Home
permalink: /
degree_ects: 206
degree_total_ects: 240
---

<link rel="stylesheet" href="{{ '/assets/css/home.css' | relative_url }}">

{% assign degree_percent = page.degree_ects | times: 100.0 | divided_by: page.degree_total_ects | round: 1 %}
<div class="portfolio-home">
  <header class="home-hero" aria-labelledby="home-title">
    <div class="home-hero-image">
      <img src="{{ site.avatar | relative_url }}" alt="Portrait of Riku Hakamäki" class="normal home-profile-picture">
    </div>

    <div class="home-hero-content">
      <h1 id="home-title">Riku Hakam&auml;ki</h1>
      <p class="home-title">ICT Engineering Student &middot; Networks &amp; Cybersecurity</p>
      <p class="home-summary">Final-stage ICT engineering student at Tampere University of Applied Sciences, specializing in Telecommunication and Networks. I&rsquo;m particularly interested in network infrastructure, cybersecurity, cloud technologies, and understanding how real systems are designed, operated, and defended.</p>
      <a class="home-text-link home-hero-link" href="{{ '/resume/' | relative_url }}">View Resume</a>
    </div>

    <div class="home-degree" aria-label="Degree progress: {{ page.degree_ects }} of {{ page.degree_total_ects }} ECTS">
      <div class="home-degree-meta">
        <span>Degree Progress</span>
        <strong>{{ page.degree_ects }} / {{ page.degree_total_ects }} ECTS</strong>
      </div>
      <div class="home-degree-track" role="progressbar" aria-valuemin="0" aria-valuemax="{{ page.degree_total_ects }}" aria-valuenow="{{ page.degree_ects }}" aria-label="Degree progress" style="--home-degree-progress: {{ degree_percent }}%;">
        <span></span>
      </div>
    </div>
  </header>

  <section class="home-section" aria-labelledby="featured-projects">
    <div class="home-section-heading">
      <h2 id="featured-projects">Featured Projects</h2>
    </div>

    <div class="home-project-grid">
      <article class="home-project-card">
        <h3>IPv6 Enterprise Networking Case Study</h3>
        <p>Designed and tested a Cisco-based enterprise network case study covering multi-site IPv6 routing, redundant switching, WAN connectivity, service deployment, and network documentation.</p>
        <p class="home-project-note">Documentation coming soon</p>
      </article>

      <article class="home-project-card">
        <h3>Windows Server and Active Directory Lab</h3>
        <p>Built and maintained a Windows infrastructure lab including Active Directory, Group Policy, WSUS, DHCP, DNS, file shares and client machines.</p>
        <p class="home-project-note">Documentation coming soon</p>
      </article>

      <article class="home-project-card">
        <h3>CyberOps Security Investigation Case Studies</h3>
        <p>Analyzed simulated security incidents using network traffic and packet captures to identify attacks, trace malicious activity, determine impact, and document findings.</p>
        <a class="home-text-link home-project-link" href="{{ '/categories/cyberops-associate/' | relative_url }}">View documentation</a>
      </article>

      <article class="home-project-card">
        <h3>Raspberry Pi DNS Infrastructure</h3>
        <p>Built and maintained a self-hosted DNS stack using Pi-hole and Unbound for network-wide filtering, recursive DNS resolution, and greater control over local network traffic.</p>
        <p class="home-project-note">Documentation coming soon</p>
      </article>
    </div>
  </section>

  <section class="home-section" aria-labelledby="recent-highlights">
    <div class="home-section-heading">
      <h2 id="recent-highlights">Recent Highlights</h2>
    </div>

    <div class="home-timeline">
      <article class="home-timeline-item">
        <div class="home-timeline-date">2026</div>
        <div class="home-timeline-card">
          <h3>Chairperson &mdash; SOURCE ry</h3>
          <p>Student association leadership focused on coordination, communication, responsibility, and keeping larger activities moving.</p>
        </div>
      </article>

      <article class="home-timeline-item">
        <div class="home-timeline-date">05/2026&ndash;06/2026</div>
        <div class="home-timeline-card">
          <h3>Trainee &mdash; TAMK Network and IT Lab Environments</h3>
          <p>Worked with network devices, virtualization, Windows infrastructure, security testing, and technical documentation.</p>
        </div>
      </article>

      <article class="home-timeline-item">
        <div class="home-timeline-date">05/2025&ndash;06/2025</div>
        <div class="home-timeline-card">
          <h3>Trainee &mdash; TAMK Network Backbone Cabling</h3>
          <p>Helped design, install, terminate, test, and document laboratory network backbone cabling.</p>
        </div>
      </article>
    </div>
  </section>

  {% assign recent_posts = site.posts | where_exp: 'post', 'post.hidden != true' %}
  {% if recent_posts.size > 0 %}
    <section class="home-section" aria-labelledby="recent-posts">
      <div class="home-section-heading">
        <h2 id="recent-posts">Latest from the Blog</h2>
      </div>

      <div class="home-recent-list">
        {% for post in recent_posts limit: 1 %}
          <article class="home-recent-card">
            <div>
              <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
              {% if post.description %}
                <p>{{ post.description }}</p>
              {% endif %}
            </div>
            <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: "%b %-d, %Y" }}</time>
          </article>
        {% endfor %}
      </div>
    </section>
  {% endif %}

  <section class="home-final-cta" aria-labelledby="thesis-opportunity">
    <div class="home-final-cta-copy">
      <h2 id="thesis-opportunity">Get in touch</h2>
      <p>Interested in thesis opportunities related to networks, infrastructure, cybersecurity and cloud technologies.</p>
    </div>
    <a class="home-text-link" href="https://www.linkedin.com/in/riku-hakam%C3%A4ki-036006203/" target="_blank" rel="noopener noreferrer">LinkedIn</a>
  </section>
</div>
