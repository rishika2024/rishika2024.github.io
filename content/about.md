---
title: "About"
description: "About"
draft: false
---

<style>
#content p {
  font-size: 1.15rem !important;
}
#content a {
  font-size: 1.15rem !important;
}
#content li {
  font-size: 1.15rem !important;
}
#content h1 {
  font-size: 2.5rem !important;
}
#content h2 {
  font-size: 2rem !important;
}

.about-main-row {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  gap: 3.5rem;
  margin: 2.5rem 0 2rem 0;
  padding-left: 3vw;
  padding-right: 3vw;
}
@media (max-width: 900px) {
  .about-main-row {
    flex-direction: column;
    align-items: center;
    gap: 2rem;
    padding-left: 1vw;
    padding-right: 1vw;
  }
}
.about-left {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-width: 260px;
  max-width: 320px;
}
.about-photo {
  border-radius: 50%;
  width: 210px;
  height: 210px;
  object-fit: cover;
  aspect-ratio: 1/1;
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
}
.about-name {
  font-size: 1.45rem;
  font-weight: 600;
  margin-top: 1.2rem;
  margin-bottom: 0.2rem;
  text-align: center;
}
.about-degree {
  font-size: 1.05rem;
  color: #888;
  margin-bottom: 1.1rem;
  text-align: center;
}
.about-icons {
  display: flex;
  gap: 0.7rem;
  margin-bottom: 1.2rem;
}
.about-icons a {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 38px;
  height: 38px;
  border-radius: 8px;
  background: #fff;
  color: #222;
  font-size: 1.15rem;
  font-weight: 400;
  box-shadow: 0 1px 4px rgba(0,0,0,0.08);
  transition: background 0.2s, color 0.2s;
  text-decoration: none;
}
.about-icons a:hover {
  background: #0066cc;
  color: #fff;
}

.about-right {
  flex: 1;
  min-width: 260px;
}
.about-summary {
  margin-bottom: 1.5rem;
}
.about-skills {
  margin-top: 2.2rem;
}
.skills-list {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  margin-bottom: 1.2rem;
}
.skill-pill {
  background: #111;
  color: #fff;
  border-radius: 999px;
  padding: 0.25em 0.7em;
  font-size: 0.85rem;
  font-weight: 500;
  display: inline-block;
}
.about-download {
  margin-top: 0.7rem;
  font-size: 1.08rem;
}
.about-download a {
  color: #111;
  text-decoration: underline;
  font-weight: 500;
}
.about-download a:hover {
  color: #0066cc;
}

/* Timeline */
.timeline {
  position: relative;
  padding-left: 40px;
  max-width: 700px;
  margin-left: auto;
  margin-right: auto;
}
.timeline::before {
  content: '';
  position: absolute;
  left: 8px;
  top: 0;
  bottom: 0;
  width: 2px;
  background-color: #0066cc;
}
.timeline-item {
  position: relative;
  margin-bottom: 40px;
  background: white;
  border-radius: 16px;
  padding: 24px 28px;
  box-shadow: 0 4px 24px 0 rgba(0,0,0,0.10), 0 1.5px 4px 0 rgba(0,0,0,0.08);
  border: none;
  text-align: left !important;
}
.timeline-item::before {
  content: '';
  position: absolute;
  left: -39px;
  top: 24px;
  width: 14px;
  height: 14px;
  background-color: white;
  border: 3px solid #0066cc;
  border-radius: 50%;
  z-index: 1;
}
.timeline-item-title {
  font-size: 1.35rem;
  font-weight: 700;
  color: #444;
  margin-bottom: 2px;
  letter-spacing: 0.01em;
}
#content .timeline,
#content .timeline *,
#content .timeline-item,
#content .timeline-item div,
#content .timeline-item span,
#content .timeline-item p,
#content .timeline-item li {
  text-align: left !important;
}
.timeline-item-header {
  display: table;
  width: 100%;
  border-collapse: separate;
  border-spacing: 10px 0;
}
.timeline-item-header img {
  display: table-cell;
  vertical-align: top;
  padding-top: 3px;
  width: 22px;
  height: 22px;
}
.timeline-item-header-text {
  display: table-cell;
  vertical-align: top;
  text-align: left !important;
}
.timeline-item-meta {
  font-size: 1rem;
  color: #888;
  margin-bottom: 18px;
}
.timeline-item ul {
  margin-left: 20px;
  margin-top: 12px;
}
.timeline-item li {
  font-size: 1.08rem;
  color: #222;
  margin-bottom: 10px;
  line-height: 1.6;
  font-weight: 500;
}
.timeline-item li strong {
  font-weight: 700;
  color: #111;
}
</style>

<div class="about-main-row">
  <div class="about-left">
    <img src="/photo/photo.jpeg" alt="Rishika Bera" class="about-photo" />
    <div class="about-name">Rishika Bera</div>
    <div class="about-degree">M.S. in Robotics @ Northwestern</div>
    <div class="about-icons">
      <a href="mailto:berarishika@gmail.com" title="Email" target="_blank" rel="noopener">✉️</a>
      <a href="https://www.linkedin.com/in/rishika-bera-05b7b025a/" title="LinkedIn" target="_blank" rel="noopener"><svg width="20" height="20" fill="currentColor" viewBox="0 0 24 24" style="vertical-align:middle;"><path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-10h3v10zm-1.5-11.268c-.966 0-1.75-.784-1.75-1.75s.784-1.75 1.75-1.75 1.75.784 1.75 1.75-.784 1.75-1.75 1.75zm13.5 11.268h-3v-5.604c0-1.337-.025-3.063-1.868-3.063-1.868 0-2.154 1.459-2.154 2.968v5.699h-3v-10h2.881v1.367h.041c.401-.761 1.379-1.563 2.838-1.563 3.034 0 3.595 1.997 3.595 4.59v5.606z"/></svg></a>
      <a href="https://github.com/rishika2024" title="GitHub" target="_blank" rel="noopener"><svg width="20" height="20" fill="currentColor" viewBox="0 0 24 24" style="vertical-align:middle;"><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.387.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.416-4.292-2.713-.135-.344-.72-1.416-1.23-1.707-.418-.24-1.01-.823-.015-.84.94-.016 1.611.866 1.838 1.223 1.07 1.835 2.809 1.305 3.495.998.108-.775.418-1.305.762-1.604-2.665-.3-5.466-1.334-5.466-5.93 0-1.31.47-2.38 1.236-3.22-.124-.303-.535-1.523.117-3.176 0 0 1.008-.322 3.3 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.553 3.297-1.23 3.297-1.23.653 1.653.242 2.873.118 3.176.77.84 1.235 1.91 1.235 3.22 0 4.61-2.803 5.625-5.475 5.921.43.372.823 1.102.823 2.222 0 1.606-.014 2.896-.014 3.286 0 .319.218.694.825.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg></a>
    </div>
  </div>
  <div class="about-right">
    <h1>About</h1>
    <div class="about-summary" style="text-align:justify;">
      Hi! I'm Rishika, a graduate student in the M.S. Robotics program at Northwestern University. I completed my B.Tech in Mechanical Engineering from IIT Jodhpur, and I'm passionate about developing intelligent robotic solutions with a focus on LLMs and autonomous systems.
    </div>
    <div class="about-skills">
      <h2>Skills</h2>
      <div class="skills-list">
        <span class="skill-pill">ROS 2</span>
        <span class="skill-pill">SLAM</span>
        <span class="skill-pill">UAV</span>
        <span class="skill-pill">CAD</span>        
      </div>
      <div class="about-download">
        <a href="/resume/Rishika_Resume.pdf" download>Download</a> my resume as a PDF.
      </div>
    </div>
  </div>
</div>

## Education

<div class="timeline">
  <div class="timeline-item">
    <div class="timeline-item-header">
      <img src="/about/nw.png" alt="Northwestern University Logo" style="height:22px; width:22px; object-fit:contain;" />
      <div class="timeline-item-header-text">
        <div style="font-size:1.2rem; font-weight:700; color:#111;">MS in Robotics</div>
        <div style="font-size:0.95rem; font-weight:500; color:#444; margin-top:0.15rem;">Northwestern University</div>
        <div style="font-size:0.88rem; color:#888; margin-top:0.1rem;">Expected Dec 2026 · Evanston, IL</div>
      </div>
    </div>
  </div>
  <div class="timeline-item">
    <div class="timeline-item-header">
      <img src="/about/jodhpur.png" alt="IIT Jodhpur Logo" style="height:22px; width:22px; object-fit:contain;" />
      <div class="timeline-item-header-text">
        <div style="font-size:1.2rem; font-weight:700; color:#111;">B.Tech in Mechanical Engineering</div>
        <div style="font-size:0.95rem; font-weight:500; color:#444; margin-top:0.15rem;">Indian Institute of Technology – Jodhpur</div>
        <div style="font-size:0.88rem; color:#888; margin-top:0.1rem;">Dec 2021 – May 2025 · Rajasthan, India</div>
      </div>
    </div>
  </div>
</div>

## Work Experience

<div class="timeline">
  <div class="timeline-item">
    <div class="timeline-item-header">
      <img src="/about/purdue.png" alt="Purdue University Logo" style="height:22px; width:22px; object-fit:contain;" />
      <div class="timeline-item-header-text">
        <div style="font-size:1.2rem; font-weight:700; color:#111;">Summer Undergraduate Research (SURF) Intern</div>
        <div style="font-size:0.95rem; font-weight:500; color:#444; margin-top:0.15rem;">Purdue University</div>
        <div style="font-size:0.88rem; color:#888; margin-top:0.1rem;">May 2024 – Jul 2024 · West Lafayette, IN</div>
      </div>
    </div>
    <ul style="margin-left:20px; margin-top:10px;">
      <li style="font-size:0.98rem; font-weight:400; color:#222;">Engineered PrimerCurator's multimodal backend pipeline integrating text, image, and video using ChatGPT API</li>
    </ul>
  </div>  
</div>