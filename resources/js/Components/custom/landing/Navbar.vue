<script setup lang="ts">
import { onMounted, ref, onBeforeUnmount, nextTick, computed } from 'vue'
import { Link, usePage } from '@inertiajs/vue3'

onMounted(() => {
  const mobileNavShow = document.querySelector('.mobile-nav-toggle')
  const mobileNavHide = document.querySelector('.mobile-nav-hide')

  document.querySelectorAll('.mobile-nav-toggle').forEach(el => {
    el.addEventListener('click', function (event) {
      event.preventDefault()
      document.querySelector('body')?.classList.toggle('mobile-nav-active')
      this.classList.toggle('bi-list')
      this.classList.toggle('bi-x')
    })
  })
})

const village = computed(() => usePage().props.village)

const currentHash = ref('')
const currentPath = ref('')

const updateHash = () => {
  currentHash.value = window.location.hash || ''
}

onMounted(() => {
  currentPath.value = window.location.pathname
  currentHash.value = window.location.hash || ''

  window.addEventListener('hashchange', updateHash)
})

onBeforeUnmount(() => {
  window.removeEventListener('hashchange', updateHash)
})
</script>

<template>
  <header id="header" class="header d-flex align-items-center fixed-top">
    <div
      class="header-container container-fluid container-xl position-relative d-flex align-items-center justify-content-between">

      <Link href="" class="logo d-flex align-items-center me-auto me-xl-0">
      <img v-if="village.logo_aktif === 'On'" :src="village.logo ? `/storage/${village.logo}` : '/placeholder/logo-desa.webp'" alt="">
      <h1 v-if="village.logo_aktif === 'Off'" class="sitename">{{ village.nama_aplikasi }}</h1>
      </Link>

      <nav id="navmenu" class="navmenu">
        <ul>
          <li>
            <a href="/#home" :class="{ active: currentHash === '#home' }">Home</a>
          </li>
          <li>
            <a href="/#tentang" :class="{ active: currentHash === '#tentang' }">Tentang Desa</a>
          </li>
          <li>
            <a href="/#batas-wilayah" :class="{ active: currentHash === '#batas-wilayah' }">Batas Wilayah</a>
          </li>
          <li>
            <a href="/#statistik-desa" :class="{ active: currentHash === '#statistik-desa' }">Statistik Desa</a>
          </li>
          <li>
            <a href="/#bangunan"
              :class="{ active: currentHash === '#bangunan' || $page.url.startsWith('/bangunan') }">Bangunan</a>
          </li>
          <li>
            <Link :href="route('landing.schedule')"
              :class="{ active: $page.url.startsWith('/jadwal-kegiatan-desa') }">Jadwal Kegiatan</Link>
          </li>
          <li>
            <a href="/#faq"
              :class="{ active: currentHash === '#faq' || $page.url.startsWith('/faq') }">F.A.Q</a>
          </li>
        </ul>
        <i class="mobile-nav-toggle d-xl-none bi bi-list"></i>
      </nav>

      <a class="btn-getstarted" :href="route('login')">Ayo Mulai</a>

    </div>
  </header>
</template>

<style scoped></style>