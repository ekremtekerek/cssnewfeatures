<script setup>
const oldCode = `
/* theme.css */
.button {
  background: blue;
  color: white;
}

/* widget.css */
.button {  /* Tema stillerini ezer! */
  background: green;
  color: black;
}

/* Çakışmayı önlemek için BEM veya benzeri yaklaşımlar */
.widget__button {
  background: green;
  color: black;
}`

const newCode = `
/* theme.css */
@scope {
  .button {
    background: blue;
    color: white;
  }
}

/* widget.css */
@scope (.widget) {
  .button {  /* Sadece .widget içindeki butonları etkiler */
    background: green;
    color: black;
  }
}

/* Daha spesifik kapsam */
@scope (.widget) to (.button) {
  :scope {  /* Sadece .widget içindeki ilk seviye butonları etkiler */
    background: green;
    color: black;
  }
}`
</script>

<template>
  <div class="feature-section">
    <h2>CSS Scope</h2>
    <p class="description">
      CSS Scope, stillerin belirli bir kapsam içinde kalmasını sağlar. Bu sayede, farklı bileşenlerin 
      stilleri birbirini etkilemez ve stil çakışmalarını önleyebiliriz. BEM gibi metodolojilere olan 
      ihtiyacı azaltır.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (CSS Scope)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        <div class="demo-container">
          <!-- Global Tema Butonları -->
          <div class="theme-section">
            <h4>Global Tema</h4>
            <div class="button-group">
              <button class="button">Tema Butonu 1</button>
              <button class="button">Tema Butonu 2</button>
            </div>
          </div>

          <!-- Widget Butonları -->
          <div class="widget">
            <h4>Widget Bileşeni</h4>
            <div class="content">
              <button class="button">Widget Butonu 1</button>
              <div class="nested">
                <button class="button">İç İçe Widget Butonu</button>
              </div>
            </div>
          </div>

          <!-- Alt Widget -->
          <div class="sub-widget">
            <h4>Alt Widget</h4>
            <div class="content">
              <button class="button">Alt Widget Butonu</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.feature-section {
  margin-bottom: 3rem;
}

.description {
  margin: 1rem 0;
  line-height: 1.6;
}

pre {
  background: #f8f9fa;
  padding: 1rem;
  border-radius: 4px;
  overflow-x: auto;
  margin: 1rem 0;
}

code {
  font-family: monospace;
  white-space: pre;
}

.demo {
  margin-top: 2rem;
}

.demo-container {
  background: #f8f9fa;
  padding: 2rem;
  border-radius: 8px;
  display: grid;
  gap: 2rem;
}

h4 {
  margin-top: 0;
  margin-bottom: 1rem;
  color: #333;
}

/* Global tema stilleri */
@scope {
  .button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    background: #3b82f6;
    color: white;
    cursor: pointer;
    transition: background 0.3s ease;
  }

  .button:hover {
    background: #2563eb;
  }
}

/* Widget kapsamı */
@scope (.widget) {
  .button {
    background: #10b981;
    color: white;
  }

  .button:hover {
    background: #059669;
  }
}

/* Alt widget kapsamı */
@scope (.sub-widget) {
  .button {
    background: #f59e0b;
    color: white;
  }

  .button:hover {
    background: #d97706;
  }
}

.button-group {
  display: flex;
  gap: 1rem;
}

.widget, .sub-widget {
  background: white;
  padding: 1.5rem;
  border-radius: 6px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.1);
}

.content {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.nested {
  margin-left: 1rem;
  padding-left: 1rem;
  border-left: 2px solid #e5e7eb;
}

/* Demo Styles */
.demo {
  margin-top: 2rem;
}

.controls {
  margin-bottom: 2rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
}

.scope-control {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.scope-control input[type="checkbox"] {
  width: 1.2rem;
  height: 1.2rem;
  margin-right: 0.5rem;
  cursor: pointer;
}

.scope-control label {
  display: flex;
  align-items: center;
  font-size: 1rem;
  color: #1a1a1a;
  cursor: pointer;
  user-select: none;
  padding: 0.5rem;
  background: white;
  border-radius: 4px;
  box-shadow: 0 1px 2px rgba(0,0,0,0.1);
  transition: all 0.3s ease;
}

.scope-control label:hover {
  background: #f0f0f0;
}
</style> 