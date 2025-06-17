---
layout: default
title: "OPAL - Oppositional Positions in ALabama"
---

# OPAL - Oppositional Positions in ALabama

<div class="quick-start-hero">
  <h2>Extract Alabama Court Data & News with Ease</h2>
  <p>A powerful web scraping tool designed specifically for Alabama public records and news sources</p>
  <a href="getting-started/quickstart-tutorial/" class="md-button">Start Scraping in 5 Minutes →</a>
</div>

## What is OPAL?

OPAL is a specialized web scraping framework that makes it easy to extract structured data from Alabama news sites and court records. Built with researchers, journalists, and civic technologists in mind.

<div class="stats-grid">
  <div class="stat-card">
    <p class="stat-number">3</p>
    <p class="stat-label">Data Sources</p>
  </div>
  <div class="stat-card">
    <p class="stat-number">5min</p>
    <p class="stat-label">Setup Time</p>
  </div>
  <div class="stat-card">
    <p class="stat-number">JSON</p>
    <p class="stat-label">Export Format</p>
  </div>
  <div class="stat-card">
    <p class="stat-number">100%</p>
    <p class="stat-label">Open Source</p>
  </div>
</div>

## Available Parsers

<div class="feature-grid">
  <div class="feature-card" onclick="window.location.href='user-guide/parsers/parserAppealsAL/'">
    <h4>⚖️ Alabama Appeals Court</h4>
    <p>Extract appellate court decisions with full metadata including case numbers, parties, and opinions</p>
    <code>opal parse appeals-al</code>
  </div>
  <div class="feature-card" onclick="window.location.href='user-guide/parsers/parser1819/'">
    <h4>📰 1819 News</h4>
    <p>Scrape articles from Alabama's independent news source with author, date, and content extraction</p>
    <code>opal parse parser-1819</code>
  </div>
  <div class="feature-card" onclick="window.location.href='user-guide/parsers/daily-news/'">
    <h4>📊 Alabama Daily News</h4>
    <p>Parse daily news articles with structured data extraction and category classification</p>
    <code>opal parse daily-news</code>
  </div>
</div>

## Quick Start

<div class="terminal-window">
  <div class="terminal-header">
    <div class="terminal-dot red"></div>
    <div class="terminal-dot yellow"></div>
    <div class="terminal-dot green"></div>
  </div>
  <div class="terminal-body">
    <div>
      <span class="terminal-prompt">$</span>
      <span class="terminal-command">pip install -e .</span>
    </div>
    <div class="terminal-output">Successfully installed opal-scraper</div>
    <div style="margin-top: 0.5rem;">
      <span class="terminal-prompt">$</span>
      <span class="terminal-command">opal parse appeals-al --date 2024-01-15</span>
    </div>
    <div class="terminal-output">
      Initializing Alabama Appeals Court parser...<br>
      Found 12 cases<br>
      Successfully scraped to appeals_al_2024-01-15.json
    </div>
  </div>
</div>

## Choose Your Path

<div class="tab-container">
  <div class="tab-buttons">
    <button class="tab-button active" onclick="showTab('enduser')">👤 End User</button>
    <button class="tab-button" onclick="showTab('developer')">👨‍💻 Developer</button>
  </div>
  
  <div class="tab-content">
    <div id="enduser" class="tab-panel active">
      <h3>I want to use OPAL to scrape data</h3>
      <div class="progress-tracker">
        <div class="progress-step">
          <span>1</span>
          <span class="progress-step-label"><a href="getting-started/prerequisites-checker/">Check Prerequisites</a></span>
        </div>
        <div class="progress-step">
          <span>2</span>
          <span class="progress-step-label"><a href="getting-started/complete-setup-guide/">Install OPAL</a></span>
        </div>
        <div class="progress-step">
          <span>3</span>
          <span class="progress-step-label"><a href="getting-started/quickstart-tutorial/">Run First Scrape</a></span>
        </div>
        <div class="progress-step">
          <span>4</span>
          <span class="progress-step-label"><a href="user-guide/output-examples/">Understand Output</a></span>
        </div>
      </div>
    </div>
    
    <div id="developer" class="tab-panel">
      <h3>I want to extend OPAL or contribute</h3>
      <ul>
        <li>📐 <a href="developer/architecture/">Understand the Architecture</a></li>
        <li>🔧 <a href="developer/creating-parsers/">Create Custom Parsers</a></li>
        <li>🧪 <a href="developer/testing/">Testing Guidelines</a></li>
        <li>🤝 <a href="about/contributing/">Contribution Guide</a></li>
      </ul>
    </div>
  </div>
</div>

## Built by Alabama Forward

This project was created by **Gabriel Cabán Cubero**, Data Director at [Alabama Forward](https://www.alabamaforward.org/), to support civic engagement and transparency in Alabama.

<script>
function showTab(tabName) {
  // Hide all panels
  document.querySelectorAll('.tab-panel').forEach(panel => {
    panel.classList.remove('active');
  });
  
  // Remove active from all buttons
  document.querySelectorAll('.tab-button').forEach(button => {
    button.classList.remove('active');
  });
  
  // Show selected panel
  document.getElementById(tabName).classList.add('active');
  
  // Mark button as active
  event.target.classList.add('active');
}
</script>
