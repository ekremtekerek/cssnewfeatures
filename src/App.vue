<template>
  <div class="app">
    <header>
      <h1>HTML/CSS Yenilikleri 2024</h1>
      <p>Google I/O'da tanıtılan yeni CSS özellikleri ve kullanım örnekleri</p>
    </header>

    <nav>
      <div v-for="section in sections" :key="section.id" class="nav-item">
        <button
          :class="{ active: activeSection === section.id }"
          @click="activeSection = section.id"
        >
          {{ section.title }}
          <div class="browser-support">
            <span
              v-for="(support, browser) in section.browsers"
              :key="browser"
              :class="['browser-icon', browser, { supported: support }]"
              :title="
                support ? browser + ' destekliyor' : browser + ' desteklemiyor'
              "
            >
              <i :class="getBrowserIcon(browser)"></i>
            </span>
          </div>
        </button>
      </div>
    </nav>

    <main>
      <ContainerQueries v-if="activeSection === 'container'" />
      <CssLayers v-if="activeSection === 'layers'" />
      <CssNesting v-if="activeSection === 'nesting'" />
      <HasSelector v-if="activeSection === 'has'" />
      <CssScope v-if="activeSection === 'scope'" />
      <CustomMedia v-if="activeSection === 'custom-media'" />
      <ScrollAnimations v-if="activeSection === 'scroll-animations'" />
      <ViewTransitions v-if="activeSection === 'view-transitions'" />
      <ViewTransitionTypes v-if="activeSection === 'transition-types'" />
      <BlockAlign v-if="activeSection === 'block-align'" />
      <NestingImprovements v-if="activeSection === 'nesting-improvements'" />
      <MultiPageTransitions v-if="activeSection === 'multi-page'" />
      <AdvancedSelectors v-if="activeSection === 'advanced-selectors'" />
      <BlockLayoutFeatures v-if="activeSection === 'block-layout'" />
      <PopoverApi v-if="activeSection === 'popover'" />
      <AnchorPositioning v-if="activeSection === 'anchor'" />
    </main>
  </div>
</template>

<script setup>
import { ref } from "vue";
import ContainerQueries from "./components/ContainerQueries.vue";
import CssLayers from "./components/CssLayers.vue";
import CssNesting from "./components/CssNesting.vue";
import HasSelector from "./components/HasSelector.vue";
import CssScope from "./components/CssScope.vue";
import CustomMedia from "./components/CustomMedia.vue";
import ScrollAnimations from "./components/ScrollAnimations.vue";
import ViewTransitions from "./components/ViewTransitions.vue";
import ViewTransitionTypes from "./components/ViewTransitionTypes.vue";
import BlockAlign from "./components/BlockAlign.vue";
import NestingImprovements from "./components/NestingImprovements.vue";
import MultiPageTransitions from "./components/MultiPageTransitions.vue";
import AdvancedSelectors from "./components/AdvancedSelectors.vue";
import BlockLayoutFeatures from "./components/BlockLayoutFeatures.vue";
import PopoverApi from "./components/PopoverApi.vue";
import AnchorPositioning from "./components/AnchorPositioning.vue";

const activeSection = ref("container");

const sections = [
  {
    id: "popover",
    title: "Popover API",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },
  {
    id: "anchor",
    title: "Anchor Positioning",
    browsers: {
      chrome: true,
      firefox: false,
      safari: false,
      edge: true,
      opera: true,
    },
  },
  {
    id: "container",
    title: "Container Queries",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },
  {
    id: "layers",
    title: "CSS Layers",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },
  {
    id: "scope",
    title: "CSS Scope",
    browsers: {
      chrome: true,
      firefox: false,
      safari: false,
      edge: true,
      opera: true,
    },
  },
  {
    id: "nesting",
    title: "CSS Nesting",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },
  {
    id: "custom-media",
    title: "Custom Media Queries",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },

  {
    id: "scroll-animations",
    title: "Scroll Animations",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },

  {
    id: "block-align",
    title: "Block Align",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },

  {
    id: "multi-page",
    title: "Multi-Page Transitions",
    browsers: {
      chrome: true,
      firefox: false,
      safari: false,
      edge: true,
      opera: true,
    },
  },
  {
    id: "advanced-selectors",
    title: "Advanced Selectors",
    browsers: {
      chrome: true,
      firefox: true,
      safari: true,
      edge: true,
      opera: true,
    },
  },

  {
    id: "view-transitions",
    title: "View Transitions",
    browsers: {
      chrome: true,
      firefox: false,
      safari: false,
      edge: true,
      opera: true,
    },
  },
  {
    id: "transition-types",
    title: "View Transition Types",
    browsers: {
      chrome: true,
      firefox: false,
      safari: false,
      edge: true,
      opera: true,
    },
  },
];

const getBrowserIcon = (browser) => {
  const icons = {
    chrome: "fab fa-chrome",
    firefox: "fab fa-firefox-browser",
    safari: "fab fa-safari",
    edge: "fab fa-edge",
    opera: "fab fa-opera",
  };
  return icons[browser];
};
</script>

<style scoped>
.app {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  font-size: 2.5rem;
  color: #2c3e50;
  margin-bottom: 0.5rem;
}

nav {
  display: flex;
  gap: 1rem;
  flex-wrap: wrap;
  justify-content: center;
  margin-bottom: 2rem;
}

.nav-item {
  position: relative;
}

button {
  padding: 0.75rem 1rem;
  border: none;
  border-radius: 8px;
  background: #e5e7eb;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  align-items: center;
  min-width: 200px;
}

button.active {
  background: #b4b4b5;
  color: #000000;
}

.browser-support {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.5rem;
  background: rgba(255, 255, 255, 0.1);
  padding: 0.5rem;
  border-radius: 4px;
}

.browser-icon {
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.3;
  transition: all 0.3s ease;
  position: relative;
}

.browser-icon i {
  font-size: 20px;
}

.browser-icon.supported {
  opacity: 1;
}

.browser-icon::after {
  content: attr(title);
  position: absolute;
  bottom: 100%;
  left: 50%;
  transform: translateX(-50%);
  background: #1f2937;
  color: white;
  padding: 0.25rem 0.5rem;
  border-radius: 4px;
  font-size: 0.75rem;
  white-space: nowrap;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.browser-icon:hover::after {
  opacity: 1;
  visibility: visible;
  transform: translateX(-50%) translateY(-4px);
}

button.active .browser-support {
  background: rgba(0, 0, 0, 0.1);
}

/* Eski SVG ikonlarını kaldırıyorum */
.icon-chrome::before,
.icon-firefox::before,
.icon-safari::before,
.icon-edge::before,
.icon-opera::before {
  display: none;
}

/* Tarayıcı ikonları için özel renkler */
.chrome i {
  color: #4285f4;
}
.firefox i {
  color: #ff9500;
}
.safari i {
  color: #000000;
}
.edge i {
  color: #0078d7;
}
.opera i {
  color: #ff1b2d;
}

main {
  background: white;
  border-radius: 8px;
  padding: 2rem;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

@media (max-width: 768px) {
  button {
    min-width: 150px;
  }

  .browser-support {
    flex-wrap: wrap;
    justify-content: center;
  }
}
</style>
