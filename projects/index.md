---
layout: default
title: Projects
---

<div id = "content">
  <header class="post-header">
    <h1>Projects</h1>
  </header>
<!--   To see a complete list of my projects, see my <a href="https://www.linkedin.com/in/rnikhil275/">Linkedin</a> profile or my <a href=""> Github profile</a>. Below, is a list of some of my selected projects.
 -->
 
  site still in maintenance
  <div class="projects">
    {% for project in site.data.projects %}
      <div class="project">
        <div class="project-name">
          <a href = "{{ project.url }}">{{ project.name }}</a>
          <span class="star-button">
            {% if project.github %}
              {% if project.github.user %}
                <a class="github-button" href="https://github.com/{{ project.github.user }}/{{ project.github.repo }}" data-size="large" data-show-count="true" data-icon="octicon-star" aria-label="Star {{ project.github.user }}/{{ project.github.repo }} on GitHub">Star</a>
              {% else %}
                <a class="github-button" href="https://github.com/whoami-nr/{{ project.github.repo }}" data-size="large" data-show-count="true" data-icon="octicon-star" aria-label="Star whoami-nr/{{ project.github.repo }} on GitHub">Star</a>
              {% endif %}
            {% endif %}
          </span>
        </div>
        <ul class="project-description">
          {% for line in project.description %}
            <li>{{ line }}</li>
          {% endfor %}
        </ul>
        <span class="project-tech">
          {% if project.tech %}
            <span>Technologies involved</span>: {{ project.tech }}
          {% else %}
            <span class="project-padding">.</span>
          {% endif %}
        </span>
        {% if project.source_url %}
          <span class="project-source"><a href = "{{ project.source_url }}">View Source</a></span>
        {% endif %}
      </div>
    {% endfor %}
  </div>
</div>

<script async defer src="https://buttons.github.io/buttons.js"></script>
