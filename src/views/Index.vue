<template>
  <div
    class="w-100 text-center justify-content-center align-middle"
    v-show="!show"
  >
    <div id="lae-logo-container" class="w-100">
      <!-- 插入 SVG -->
      <img :src="laeLogoUrl" id="lae-logo" class="h-auto" />

      <div class="mt-3"></div>
      <div class="spinner-border" role="status">
        <span class="visually-hidden">Loading...</span>
      </div>
    </div>
  </div>

  <div class="mb-2" v-show="show">
    <h4>欢迎使用 莱云</h4>

    <!-- <span v-if="item.attributes">
      {{ item.attributes.title }}
    </span>

    <span v-if="item.attributes">
      {{ new Date(item.attributes.createdAt).toLocaleString() }}
    </span> -->

    <div v-show="base_url">
      <h4 class="mt-3">公告</h4>
      <div class="list-group mt-3" v-for="item in pinned">
        <span v-if="item.attributes">
          <a
            class="list-group-item list-group-item-action shadow-sm rounded"
            target="_blank"
            :href="base_url + '/d/' + item.attributes.slug"
          >
            <div class="d-flex w-100 justify-content-between">
              <h5 class="mb-1 text-success">
                {{ item.attributes.title }}
              </h5>
              <small>
                {{ new Date(item.attributes.lastPostedAt).toLocaleString() }}
              </small>
            </div>
            <!-- <p class="mb-1">
            <span></span>
          </p> -->
            <!-- <small class="text-muted">
        </small> -->
          </a>
        </span>
      </div>
    </div>

    <!-- 贡献代码 -->
    <h3>GitHub</h3>
    <p>如果您想要为莱云 控制面板 贡献代码，或者自定义它。</p>
    <a target="_blank" href="https://github.com/iVampireSP/lae-ui"
      >https://github.com/iVampireSP/lae-ui</a
    >
  </div>
</template>

<script setup>
  import { ref, onMounted } from 'vue'
  import http from '../api/http'
  //   import store from '../plugins/store'

  const pinned = ref([])
  const base_url = ref('')
  const show = ref(false)

  let color = 'dark'
  if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
    color = 'white'
  }

  let laeLogoUrl = '/assets/lae-' + color + '.png'

  onMounted(() => {
    let laeLogoContainer = document.getElementById('lae-logo-container')
    let laeLogo = document.getElementById('lae-logo')
    // 上下居中
    laeLogoContainer.style.marginTop =
      (laeLogoContainer.clientHeight - laeLogo.clientHeight) / 2 + 100 + 'px'

    // 如果屏幕比较大，就把 SVG 缩小一点
    if (window.innerWidth > 768) {
      laeLogo.style.width = '20%'
    } else {
      laeLogo.style.width = '50%'
    }
  })

  http.get('forum/pinned').then((res) => {
    pinned.value = res.data

    base_url.value = pinned.value.base_url

    setTimeout(() => {
      show.value = true
    }, 300)
  })
</script>
