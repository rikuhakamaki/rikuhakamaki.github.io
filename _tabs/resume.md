---
title: Resume
icon: fas fa-file-alt
order: 5
degree_ects: 206
---
<link rel="stylesheet" href="{{ '/assets/css/resume.css' | relative_url }}">

{% assign degree_total_ects = 240 %}
{% assign degree_percent = page.degree_ects | times: 100.0 | divided_by: degree_total_ects | round: 1 %}

<article class="resume-page">
  <header class="resume-header">
    <div class="resume-header-main">
      <h2>Riku Hakamäki</h2>
      <p class="resume-subtitle">ICT Engineering Student &middot; Networks and Cybersecurity</p>
      <p class="resume-availability">Looking for a thesis opportunity</p>
    </div>
  </header>

  <section class="resume-contact-panel" aria-label="Contact and professional links">
    <div class="resume-contact-list">
      <a href="mailto:{{ site.social.email }}"><i class="fas fa-envelope" aria-hidden="true"></i><span>{{ site.social.email }}</span></a>
      <a href="https://www.linkedin.com/in/riku-hakam%C3%A4ki-036006203/" target="_blank" rel="noopener noreferrer"><i class="fab fa-linkedin" aria-hidden="true"></i><span>LinkedIn</span></a>
      <span><i class="fas fa-map-marker-alt" aria-hidden="true"></i><span>Tampere, Finland</span></span>
    </div>
    <a class="resume-download-button" href="{{ '/assets/files/RikuHakam%C3%A4ki_resume_eng_08-2026.pdf' | relative_url }}" target="_blank" rel="noopener noreferrer">
      <i class="fas fa-download" aria-hidden="true"></i>
      <span>View my resume</span>
    </a>
  </section>

  <section class="resume-section" aria-labelledby="technical-projects">
    <div class="resume-section-heading">
      <h2 id="technical-projects">Featured Technical Projects</h2>
    </div>

    <div class="resume-project-list">
      <article class="resume-project">
        <div class="resume-entry-heading">
          <h3>IPv6 Enterprise Networking Case Study</h3>
        </div>
        <p class="resume-project-meta">University and lab project &middot; <span class="is-completed">Completed</span></p>
        <p>Designed and tested a Cisco-based enterprise network using Cisco CML and physical lab equipment, covering multi-site routing, redundant switching, WAN connectivity, services, and documentation.</p>
        <p>Documentation coming soon!</p>
      </article>

      <article class="resume-project">
        <div class="resume-entry-heading">
          <h3>Windows Server and Active Directory Lab</h3>
        </div>
        <p class="resume-project-meta">2026 internship lab work &middot; <span class="is-ongoing">Ongoing</span></p>
        <p>Built and maintained a Windows infrastructure lab with directory services, policy management, update services, core network services, file shares, and client machines.</p>
        <p>Documentation coming soon!</p>
      </article>

      <a class="resume-project resume-project-link" href="{{ '/categories/cyberops/' | relative_url }}" aria-label="View CyberOps Associate Investigation Case Studies documentation">
        <div class="resume-entry-heading">
          <h3>CyberOps Associate Investigation Case Studies</h3>
        </div>
        <p class="resume-project-meta">Ongoing study and homelab &middot; <span class="is-completed">Completed</span></p>
        <p>Analyzed simulated security incidents using network traffic and packet captures to identify attacks, trace malicious activity, determine impact, and document findings.</p>
      </a>
    </div>

  </section>

  <div class="resume-two-column">
    <section class="resume-section" aria-labelledby="experience">
      <div class="resume-section-heading">
        <h2 id="experience">Professional Experience</h2>
      </div>

      <div class="resume-timeline">
        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>Trainee</h3>
            <span>05/2026&ndash;06/2026</span>
          </div>
          <p class="resume-meta">Tampere University of Applied Sciences</p>
          <ul>
            <li>Configured and maintained network devices across laboratory and development environments.</li>
            <li>Maintained virtualization environments and built a Windows Active Directory lab.</li>
            <li>Configured and performed security testing on an Aldebaran Robotics NAO robot.</li>
            <li>Developed test environments and produced supporting technical documentation.</li>
          </ul>
        </article>

        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>Trainee</h3>
            <span>05/2025&ndash;06/2025</span>
          </div>
          <p class="resume-meta">Tampere University of Applied Sciences</p>
          <ul>
            <li>Worked as part of a team designing and implementing backbone cabling for a multi-room networking laboratory.</li>
            <li>Planned, installed, terminated, tested, and documented structured network cabling and connectivity.</li>
          </ul>
        </article>

        <article class="resume-entry resume-entry-compact">
          <div class="resume-entry-heading">
            <h3>Seasonal Worker</h3>
            <span>2021, 2023&ndash;2024</span>
          </div>
          <p class="resume-meta">Froneri Finland Oy &middot; Turenki</p>
          <p>Operated production machinery across production and packaging environments, handled operational issues independently, supported production continuity, and worked effectively in a fast-paced setting.</p>
        </article>

        <article class="resume-entry resume-entry-compact">
          <div class="resume-entry-heading">
            <h3>Summer Employee</h3>
            <span>2020</span>
          </div>
          <p class="resume-meta">Suurustin Ky, K-Market Ruokasaari &middot; Riihim&auml;ki</p>
          <p>Customer service, cashier responsibilities, payment processing, parcel handling, and related Matkahuolto service-point duties.</p>
        </article>
      </div>
    </section>

    <aside class="resume-side-stack">
      <section class="resume-section" aria-labelledby="education">
        <div class="resume-section-heading">
          <h2 id="education">Education</h2>
        </div>

        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>BE, Information Technology</h3>
            <span>2023&ndash;present</span>
          </div>
          <p class="resume-meta">Tampere University of Applied Sciences</p>
          <p>Specializing in telecommunications and networks, with a growing focus on cybersecurity, network management, cloud services, and network documentation.</p>
          <p class="resume-detail-line">Expected graduation: 2027</p>
          <div class="resume-progress" aria-label="Degree progress: {{ page.degree_ects }} of {{ degree_total_ects }} ECTS, {{ degree_percent }} percent complete">
            <div class="resume-progress-meta">
              <span>Degree Progress</span>
              <span>{{ page.degree_ects }} / {{ degree_total_ects }} ECTS</span>
            </div>
            <div class="resume-progress-track" role="progressbar" aria-valuemin="0" aria-valuemax="{{ degree_total_ects }}" aria-valuenow="{{ page.degree_ects }}" aria-label="Degree progress" style="--resume-progress-percent: {{ degree_percent }}%;">
              <span></span>
            </div>
          </div>
        </article>

        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>Matriculation Examination</h3>
            <span>2018&ndash;2021</span>
          </div>
          <p class="resume-meta">Turenki High School</p>
        </article>
      </section>

      <section class="resume-section" aria-labelledby="skills">
        <div class="resume-section-heading">
          <h2 id="skills">Technical Skills</h2>
        </div>

        <div class="resume-skill-groups">
          <article class="resume-skill-group">
            <h3>Networking</h3>
            <p>Cisco &middot; Ubiquiti &middot; IPv4/IPv6 &middot; OSPF &middot; BGP &middot; VLANs &middot; EtherChannel &middot; MST &middot; HSRP &middot; GLBP</p>
          </article>

          <article class="resume-skill-group">
            <h3>Systems &amp; Infrastructure</h3>
            <p>Windows Server &middot; Active Directory &middot; Group Policy &middot; WSUS &middot; Linux &middot; Docker &middot; Portainer &middot; Raspberry Pi &middot; Virtualization</p>
          </article>

          <article class="resume-skill-group">
            <h3>Security &amp; Documentation</h3>
            <p>Wireshark &middot; Security Onion &middot; Suricata &middot; Network Security Analysis &middot; Traffic Analysis &middot; VPNs &middot; Pi-hole &middot; Unbound &middot; NetBox &middot; Network Documentation</p>
          </article>
        </div>
      </section>

      <section class="resume-section" aria-labelledby="leadership">
        <div class="resume-section-heading">
          <h2 id="leadership">Positions of Trust</h2>
        </div>

        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>Chairperson</h3>
            <span>2026</span>
          </div>
          <p class="resume-meta">SOURCE ry</p>
          <p>Leading the student association's board, operations, stakeholder cooperation, and organizational development.</p>
        </article>

        <article class="resume-entry">
          <div class="resume-entry-heading">
            <h3>External Relations Coordinator</h3>
            <span>2025</span>
          </div>
          <p class="resume-meta">SOURCE ry</p>
          <p>Managed company cooperation, excursions, recruitment events, and student overall procurement.</p>
        </article>
      </section>
    </aside>
  </div>
</article>
