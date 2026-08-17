---
title: About
icon: fas fa-info-circle
order: 4
profile_role: ICT Engineering Student - Networks and Cybersecurity
profile_location: Tampere, Finland
---
<link rel="stylesheet" href="{{ '/assets/css/about.css' | relative_url }}">

{% assign profile_name = site.social.name | default: site.title %}
{% assign profile_role = page.profile_role %}
{% assign profile_location = page.profile_location %}
{% assign profile_avatar = '/assets/img/avatar2.jpg' %}
{% assign linkedin_url = '' %}
{% for link in site.social.links %}
  {% if link contains 'linkedin.com' %}
    {% assign linkedin_url = link %}
  {% endif %}
{% endfor %}

<article class="about-page">
  <header class="about-hero" aria-labelledby="about-profile-name">
    <div class="about-profile-image" role="img" aria-label="Portrait of {{ profile_name | escape }}" style="background-image: url('{{ profile_avatar | relative_url }}');"></div>

    <div class="about-intro">
      <div class="about-name-line">
        <h2 id="about-profile-name">{{ profile_name }}</h2>
        <span class="about-status-badge">
          <span class="about-status-dot" aria-hidden="true"></span>
          <span>Looking for a thesis opportunity!</span>
        </span>
      </div>
      <p class="about-subtitle">{{ profile_role }}</p>
      <p class="about-status">{{ profile_location }}</p>

      <p>I am an Information Technology engineering student at Tampere University of Applied Sciences, specializing in telecommunications and computer networks. My professional direction sits around practical infrastructure: networks, systems, documentation, and the security visibility needed to understand what those systems are doing.</p>
    </div>
  </header>

  <section class="about-section" aria-labelledby="about-focus">
    <div class="about-section-heading">
      <h2 id="about-focus">Technical Focus</h2>
    </div>

    <div class="about-focus-grid">
      <article class="about-focus-card">
        <h3>Networks and Telecommunications</h3>
        <p>Routing, switching, redundancy, IPv4/IPv6 design, network services, and practical lab infrastructure.</p>
      </article>

      <article class="about-focus-card">
        <h3>Systems and Identity</h3>
        <p>Windows Server, Active Directory, Group Policy, virtualization, Linux, and lab environments.</p>
      </article>

      <article class="about-focus-card">
        <h3>Security Visibility</h3>
        <p>Traffic analysis, segmentation, monitoring, DNS filtering, logging, and defensive architecture.</p>
      </article>
    </div>
  </section>

  <section class="about-section about-prose" aria-label="Background and work context">
    <p>I learn best by building things until the details become concrete. My current studies and projects have centered on Cisco-based network design, IPv6 addressing, OSPF, BGP, VLANs, EtherChannel, MST, HSRP, GLBP, DNS, DHCP, RADIUS, NTP, and documentation with tools such as NetBox.</p>

    <p>A major practical learning point has been a large IPv6 enterprise networking case study built with Cisco CML and physical lab equipment. It pulled together multi-site design, redundant switching, dynamic routing, WAN connectivity, service deployment, troubleshooting, and documentation. That kind of full-system work is where networking starts to feel properly alive to me.</p>

    <p>I am also building experience on the systems side. Through internship and lab work, I have worked with Windows Server, Active Directory, Group Policy, WSUS, DHCP, DNS, file shares, client machines, virtualization, and Linux-based services.</p>

    <p>Cybersecurity fits into this as a practical layer rather than a separate universe. I am interested in how architecture, access control, segmentation, logs, packet captures, and monitoring make systems easier to understand and defend. Tools like Wireshark, Security Onion, Suricata, Pi-hole, Unbound, and VPNs show up in that learning path.</p>

    <p>Outside the purely technical track, I have work experience in retail, logistics, customer service, and industrial production. Those roles taught me routine, responsibility, communication, and how much good work depends on noticing small details before they become bigger problems.</p>

    <p>I have also been active in SOURCE ry, first through external relations and later as chairperson. That work has involved company cooperation, excursions, recruitment events, student overall ordering, board work, stakeholder cooperation, and keeping shared projects moving.</p>
  </section>

  <section class="about-section about-prose" aria-labelledby="about-interests">
    <div class="about-section-heading">
      <h2 id="about-interests">Outside The Classroom</h2>
    </div>

    <p>Outside coursework, I keep learning through personal projects and a homelab. That includes Raspberry Pi systems, Docker, Portainer, UniFi networking, VLANs, VPNs, Pi-hole, Unbound, and small infrastructure experiments where troubleshooting is part of the point.</p>

    <p>I am also interested in projects that combine hardware, software, and patient iteration. The existing portfolio mentions work with Arduino and an Aldebaran NAO robot; both fit the same pattern: test, observe, adjust, document, and keep going.</p>
  </section>

  <section class="about-section about-gallery" aria-label="Personal photos">
    <div class="about-photo-grid">
      <figure class="about-photo-card">
        <img src="{{ '/assets/img/about/about-1.jpg' | relative_url }}" alt="Silhouette of a photographer against a colorful sunset sky" class="about-photo-image">
        <figcaption>I also enjoy photography. A moment behind the camera, captured by my girlfriend.</figcaption>
      </figure>

      <figure class="about-photo-card">
        <img src="{{ '/assets/img/about/about-2.jpg' | relative_url }}" alt="Tabby cat stretching on a couch" class="about-photo-image">
        <figcaption>My yawning, furry friend, Mini.</figcaption>
      </figure>
    </div>
  </section>

  <section class="about-section about-contact" aria-labelledby="about-contact-title">
    <div class="about-contact-card">
      <div class="about-contact-copy">
        <h2 id="about-contact-title">Connect With Me!</h2>
        <p>For thesis opportunities, networking, infrastructure, or cybersecurity-related work, the configured portfolio links below are the best ways to reach me.</p>
      </div>

      <div class="about-contact-links" aria-label="Contact links">
        {% if site.social.email %}
          <a href="mailto:{{ site.social.email }}">
            <i class="fas fa-envelope" aria-hidden="true"></i>
            <span>Email</span>
          </a>
        {% endif %}

        {% if linkedin_url != '' %}
          <a href="{{ linkedin_url }}" target="_blank" rel="noopener noreferrer">
            <i class="fab fa-linkedin" aria-hidden="true"></i>
            <span>LinkedIn</span>
          </a>
        {% endif %}

      </div>
    </div>
  </section>
</article>
