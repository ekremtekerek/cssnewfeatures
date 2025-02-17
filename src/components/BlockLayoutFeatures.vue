<script setup>
const oldCode = `
/* Eski yöntem - karmaşık düzen ayarları */
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.content {
  margin: auto;
  width: 100%;
  max-width: 800px;
}

.sidebar {
  margin-left: auto;
  width: 300px;
}`

const newCode = `
/* Yeni blok düzeni özellikleri */
.container {
  align-content: center;
  margin-block: auto;
}

.content {
  margin-inline: auto;
  max-inline-size: 800px;
}

.sidebar {
  margin-inline-start: auto;
  inline-size: 300px;
}

/* Otomatik marjin davranışları */
.element {
  margin-block: auto;
  margin-inline: auto;
  block-size: fit-content;
}`
</script>

<template>
  <div class="feature-section">
    <h2>Yeni Blok Düzeni Özellikleri</h2>
    <p class="description">
      Yeni blok düzeni özellikleri ile daha mantıksal ve esnek düzenler oluşturabilirsiniz.
      Otomatik marjin davranışları ve gelişmiş içerik hizalama özellikleri ile 
      daha az kod yazarak daha güçlü düzenler elde edebilirsiniz.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (Blok Düzeni)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        <div class="demo-container">
          <!-- İçerik Hizalama Demo -->
          <div class="demo-section">
            <h4>İçerik Hizalama</h4>
            <div class="alignment-demo">
              <div class="block center-block">
                <p>Merkeze Hizalı Blok</p>
              </div>
              <div class="block start-block">
                <p>Başlangıca Hizalı Blok</p>
              </div>
              <div class="block end-block">
                <p>Sona Hizalı Blok</p>
              </div>
            </div>
          </div>

          <!-- Otomatik Marjin Demo -->
          <div class="demo-section">
            <h4>Otomatik Marjin Davranışları</h4>
            <div class="margin-demo">
              <div class="auto-margin-block">
                <p>Otomatik Marjinli Blok</p>
              </div>
              <div class="space-between-blocks">
                <div class="block">Blok 1</div>
                <div class="block">Blok 2</div>
              </div>
            </div>
          </div>

          <!-- Blok Akış Demo -->
          <div class="demo-section">
            <h4>Blok Akış Kontrolü</h4>
            <div class="flow-demo">
              <div class="flow-container">
                <header class="flow-header">Başlık</header>
                <main class="flow-content">
                  <p>Ana İçerik</p>
                  <p>Daha fazla içerik...</p>
                </main>
                <footer class="flow-footer">Alt Bilgi</footer>
              </div>
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
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
  padding: 2rem;
  background: #f8f9fa;
  border-radius: 8px;
}

.demo-section {
  background: white;
  padding: 1.5rem;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);

  h4 {
    margin: 0 0 1rem 0;
    color: #374151;
  }
}

/* Alignment Demo */
.alignment-demo {
  .block {
    padding: 1rem;
    margin-bottom: 1rem;
    background: #f3f4f6;
    border-radius: 4px;
    text-align: center;
  }

  .center-block {
    margin-inline: auto;
    max-inline-size: 200px;
  }

  .start-block {
    margin-inline-start: 0;
    margin-inline-end: auto;
    max-inline-size: 200px;
  }

  .end-block {
    margin-inline-start: auto;
    margin-inline-end: 0;
    max-inline-size: 200px;
  }
}

/* Margin Demo */
.margin-demo {
  .auto-margin-block {
    margin-block: auto;
    padding: 1rem;
    background: #f3f4f6;
    border-radius: 4px;
    text-align: center;
  }

  .space-between-blocks {
    display: flex;
    justify-content: space-between;
    margin-top: 1rem;

    .block {
      padding: 1rem;
      background: #f3f4f6;
      border-radius: 4px;
      text-align: center;
    }
  }
}

/* Flow Demo */
.flow-demo {
  .flow-container {
    border: 1px solid #e5e7eb;
    border-radius: 4px;
    overflow: hidden;
  }

  .flow-header {
    padding: 1rem;
    background: #f3f4f6;
    text-align: center;
    font-weight: bold;
  }

  .flow-content {
    padding: 1rem;
    background: white;

    p {
      margin: 0.5rem 0;
    }
  }

  .flow-footer {
    padding: 1rem;
    background: #f3f4f6;
    text-align: center;
  }
}

/* Block Flow Control */
.flow-container {
  display: flow-root;
  min-block-size: 300px;
}

.flow-content {
  flow: inherit;
}
</style> 