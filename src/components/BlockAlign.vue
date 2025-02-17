<script setup>
const oldCode = `
/* Eski yöntem - flex kullanarak dikey hizalama */
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  min-height: 200px;
}

/* Eski yöntem - grid kullanarak dikey hizalama */
.container {
  display: grid;
  place-items: center;
  min-height: 200px;
}`;

const newCode = `
/* Yeni yöntem - align-content ile dikey hizalama */
.container {
  min-height: 200px;
  align-content: center;
}

/* Farklı hizalama seçenekleri */
.container {
  align-content: start;    /* Üstte hizala */
  align-content: end;      /* Altta hizala */
  align-content: center;   /* Ortada hizala */
  align-content: space-between; /* Eşit aralıklarla dağıt */
}`;
</script>

<template>
  <div class="feature-section">
    <h2>Blok Düzeni için Align-content</h2>
    <p class="description">
      Blok düzeninde align-content özelliği, flex veya grid kullanmadan içeriği
      dikey olarak hizalamamızı sağlar. Bu sayede daha basit ve net bir kod
      yazabiliriz.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (Block Align)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        <div class="demo-container">
          <!-- Start Alignment -->
          <div class="block-container start">
            <h4>align-content: start</h4>
            <div class="content">
              <p>İçerik üstte</p>
              <button>Buton</button>
            </div>
          </div>

          <!-- Center Alignment -->
          <div class="block-container center">
            <h4>align-content: center</h4>
            <div class="content">
              <p>İçerik ortada</p>
              <button>Buton</button>
            </div>
          </div>

          <!-- End Alignment -->
          <div class="block-container end">
            <h4>align-content: end</h4>
            <div class="content">
              <p>İçerik altta</p>
              <button>Buton</button>
            </div>
          </div>

          <!-- Space Between -->
          <div class="block-container space-between">
            <h4>align-content: space-between</h4>
            <div class="content">
              <p>İçerik 1</p>
              <p>İçerik 2</p>
              <button>Buton</button>
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
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 8px;
}

.block-container {
  background: white;
  border-radius: 8px;
  padding: 1rem;
  min-height: 300px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.block-container h4 {
  margin: 0 0 1rem 0;
  color: #374151;
  text-align: center;
}

.content {
  min-height: 200px;
  border: 2px dashed #e5e7eb;
  padding: 1rem;
  border-radius: 4px;
}

/* Align Content Demos */
.start .content {
  align-content: start;
}

.center .content {
  align-content: center;
}

.end .content {
  align-content: end;
}

.space-between .content {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

button {
  padding: 0.5rem 1rem;
  background: #3b82f6;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background 0.3s ease;
}

button:hover {
  background: #2563eb;
}

p {
  margin: 0.5rem 0;
  color: #4b5563;
}
</style>
