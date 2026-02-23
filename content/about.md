---
title: "Resume"
description: "Resume"
draft: false
---

<style>
#content p {
  font-size: 1.25rem !important;
}
#content a {
  font-size: 1.25rem !important;
}
#content li {
  font-size: 1.25rem !important;
}
#content h1 {
  font-size: 2.5rem !important;
}
#content h2 {
  font-size: 1.875rem !important;
}

/* Responsive about section layout */
.about-flex-row {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  gap: 2.5rem;
  margin: 2.5rem 0 2rem 0;
}
@media (max-width: 800px) {
  .about-flex-row {
    flex-direction: column;
    gap: 1.5rem;
  }
}
.about-photo {
  border-radius: 50%;
  width: 260px;
  height: 260px;
  object-fit: cover;
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
}
.about-text {
  max-width: 480px;
  font-size: 1.18rem;
  color: #222;
}

.timeline {
  position: relative;
  padding-left: 40px;
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
  padding: 36px 40px;
  box-shadow: 0 4px 24px 0 rgba(0,0,0,0.10), 0 1.5px 4px 0 rgba(0,0,0,0.08);
  border: none;
  min-width: 340px;
  max-width: 700px;
  margin-left: auto;
  margin-right: auto;
}

.timeline-item::before {
  content: '';
  position: absolute;
  left: -41px;
  top: 36px;
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

.timeline-item-company {
  font-size: 1.15rem;
  color: #222;
  font-weight: 500;
  margin-bottom: 2px;
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

# About Me

<div class="about-flex-row">
  <img src="/photo/photo.jpeg" alt="Rishika Bera" class="about-photo" />
  <div class="about-text">
    Hi! I'm Rishika, a graduate student in the M.S. Robotics program at Northwestern University. I completed my B.Tech in Mechanical Engineering from IIT Jodhpur, and I'm passionate about developing intelligent robotic solutions with a focus on LLMs and autonomous systems.
  </div>
</div>

## Let's Connect

- **GitHub**: [github.com/rishika2024](https://github.com/rishika2024)
- **LinkedIn**: [linkedin.com/in/rishika-bera-05b7b025a/](https://www.linkedin.com/in/rishika-bera-05b7b025a/)
- **Email**: berarishika@gmail.com

## Education

<div class="timeline">
  <div class="timeline-item">
    <div class="timeline-item-title">MS in Robotics</div>
    <div class="timeline-item-company">Northwestern University</div>
    <div class="timeline-item-meta">Expected Dec 2026 · Evanston, IL</div>
  </div>
  <div class="timeline-item">
    <div class="timeline-item-title">B.Tech in Mechanical Engineering</div>
    <div class="timeline-item-company">Indian Institute of Technology – Jodhpur</div>
    <div class="timeline-item-meta">Dec 2021 – May 2025 · Rajasthan, India</div>
  </div>
</div>

## Work Experience

<div class="timeline">
  <div class="timeline-item">
    <div class="timeline-item-title">Summer Undergraduate Research (SURF) Intern</div>
    <div class="timeline-item-company">Purdue University</div>
    <div class="timeline-item-meta">May 2024 – Jul 2024 · West Lafayette, IN</div>
    <ul>
      <li style="font-size:0.98rem; font-weight:400; text-align:left; color:#222;">Engineered PrimerCurator’s multimodal backend pipeline integrating text, image, and video using ChatGPT API</li>      
    </ul>
  </div>  
</div>


