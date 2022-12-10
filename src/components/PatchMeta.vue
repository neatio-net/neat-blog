<template>
  <span />
</template>

<script lang="ts">
import { onMounted, defineComponent } from 'vue'

const defaultMetas = {
  siteName: 'Neat Blog',
  title: 'Minimal Vue3 + Markdown Neatio Blog',
  description: 'Stay connected with latest Neatio news',
  previewUrl: 'https://neatio.net',
  shareUrl: 'https://neatio.net'
}

const patchMeta = ({
  siteName = defaultMetas.siteName,
  title = defaultMetas.title,
  description = defaultMetas.description,
  previewUrl = defaultMetas.previewUrl,
  shareUrl = defaultMetas.shareUrl
}) => {
  // Patch title
  const [titleEl] = document.getElementsByTagName('title')
  titleEl.textContent = `${siteName} | ${title} - ${description}`

  // Patch META
  Array.from(document.getElementsByClassName('APP_META')).forEach(el => { el.remove() })
  const APP_META = `<meta class="APP_META" property="og:title" content="${title}">
      <meta class="APP_META" property="og:description" content="${description}">
      <meta class="APP_META" property="og:image" content="${previewUrl}">
      <meta class="APP_META" property="og:url" content="${shareUrl}">
      <meta class="APP_META" name="twitter:card" content="summary_large_image">
      <meta class="APP_META" property="og:site_name" content="${siteName}">`
  const [headEl] = document.getElementsByTagName('head')
  if (!headEl) return
  headEl.innerHTML = `${headEl.innerHTML}${APP_META}`
}

export default defineComponent({
  props: {
    siteName: {
      type: String,
      default: defaultMetas.siteName
    },
    title: {
      type: String,
      default: defaultMetas.title
    },
    description: {
      type: String,
      default: defaultMetas.description
    },
    previewUrl: {
      type: String,
      default: defaultMetas.previewUrl
    },
    shareUrl: {
      type: String,
      default: defaultMetas.shareUrl
    }
  },
  setup (props) {
    onMounted(() => {
      patchMeta(props)
    })
  }
})
</script>
