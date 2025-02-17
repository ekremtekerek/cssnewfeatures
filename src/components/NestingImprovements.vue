<script setup>
const oldCode = `
/* Eski iç içe yerleştirme yöntemi */
.card {
  padding: 1rem;
}

.card & .title {
  font-size: 1.5rem;
}

.card & .content {
  margin-top: 1rem;
}

@media (min-width: 768px) {
  .card & .title {
    font-size: 2rem;
  }
}`

const newCode = `
/* Yeni iç içe yerleştirme yöntemi */
.card {
  padding: 1rem;
  
  .title {
    font-size: 1.5rem;
  }
  
  .content {
    margin-top: 1rem;
  }
  
  @media (min-width: 768px) {
    .title {
      font-size: 2rem;
    }
  }
  
  @container (min-width: 400px) {
    padding: 2rem;
  }
}`
</script>

<template>
  <div class="feature-section">
    <h2>CSS İç İçe Yerleştirme İyileştirmeleri</h2>
    <p class="description">
      CSS İç İçe Yerleştirme özelliği artık daha ergonomik ve kullanımı kolay. 
      & işareti kullanmadan doğrudan iç içe yazabilir, medya sorguları ve kapsayıcı 
      sorgularını kolayca ekleyebilirsiniz.
    </p>

    <div class="example">
      <h3>Eski Yöntem</h3>
      <pre><code>{{ oldCode }}</code></pre>

      <h3>Yeni Yöntem (İyileştirilmiş İç İçe Yerleştirme)</h3>
      <pre><code>{{ newCode }}</code></pre>

      <div class="demo">
        <h3>Canlı Demo</h3>
        <div class="demo-container">
          <!-- Temel Kart -->
          <div class="demo-section">
            <h4>Temel Kart</h4>
            <div class="card">
              <div class="title">Kart Başlığı</div>
              <div class="content">
                <p>Bu bir örnek kart içeriğidir. İç içe yerleştirme sayesinde stil tanımları daha organize.</p>
                <button>Detaylar</button>
              </div>
            </div>
          </div>

          <!-- Medya Sorgusu Demo -->
          <div class="demo-section">
            <h4>Medya Sorgusu ile Responsive Kart</h4>
            <div class="card responsive">
              <div class="title">Responsive Başlık</div>
              <div class="content">
                <p>Ekran boyutuna göre başlık boyutu ve boşluklar değişir.</p>
                <button>Detaylar</button>
              </div>
            </div>
          </div>

          <!-- Container Query Demo -->
          <div class="demo-section">
            <h4>Container Query ile Uyarlanabilir Kart</h4>
            <div class="container-demo">
              <div class="card container-responsive">
                <div class="title">Kapsayıcıya Duyarlı</div>
                <div class="content">
                  <p>Kapsayıcı boyutuna göre stil değişir.</p>
                  <button>Detaylar</button>
                </div>
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
  h4 {
    margin: 0 0 1rem 0;
    color: #374151;
  }
}

.card {
  background: white;
  border-radius: 8px;
  padding: 1rem;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);

  .title {
    font-size: 1.5rem;
    color: #1f2937;
    margin-bottom: 1rem;
  }

  .content {
    color: #4b5563;

    p {
      margin-bottom: 1rem;
    }

    button {
      padding: 0.5rem 1rem;
      background: #3b82f6;
      color: white;
      border: none;
      border-radius: 4px;
      cursor: pointer;
      transition: background 0.3s ease;

      &:hover {
        background: #2563eb;
      }
    }
  }
}

.responsive {
  @media (min-width: 768px) {
    padding: 2rem;

    .title {
      font-size: 2rem;
    }
  }
}

.container-demo {
  container-type: inline-size;
  resize: horizontal;
  overflow: auto;
  padding: 1rem;
  background: #e5e7eb;
  border-radius: 4px;
  min-width: 200px;
  max-width: 100%;
}

.container-responsive {
  @container (min-width: 400px) {
    padding: 2rem;

    .title {
      font-size: 2rem;
    }

    .content {
      font-size: 1.1rem;
    }
  }
}
</style> 