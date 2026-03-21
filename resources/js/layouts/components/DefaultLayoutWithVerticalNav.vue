<script setup>
import navItems from '@/navigation/vertical'
import { themeConfig } from '@themeConfig'

// Components
import Footer from '@/layouts/components/Footer.vue'
import UserProfile from '@/layouts/components/UserProfile.vue'
import NavBarI18n from '@core/components/I18n.vue'

// @layouts plugin
import { useConfigStore } from '@/@core/stores/config'
import { VerticalNavLayout } from '@layouts'

const configStore = useConfigStore()
const selectedItem = ref([configStore.theme])

// Update icon if theme is changed from other sources
watch(() => configStore.theme, () => {
  selectedItem.value = [configStore.theme]
}, { deep: true })
</script>

<template>
  <VerticalNavLayout :nav-items="navItems">
    <!-- 👉 navbar -->
    <template #navbar="{ toggleVerticalOverlayNavActive }">
      <div class="d-flex h-100 align-center">
        <IconBtn id="vertical-nav-toggle-btn" class="ms-n3 d-lg-none" @click="toggleVerticalOverlayNavActive(true)">
          <VIcon size="26" icon="tabler-menu-2" />
        </IconBtn>

        <div id="navbar-breadcrumbs" class="d-flex align-center ml-2"></div>

        <!-- <NavbarThemeSwitcher /> -->

        <VSpacer />

        <NavBarI18n v-if="themeConfig.app.i18n.enable && themeConfig.app.i18n.langConfig?.length"
          :languages="themeConfig.app.i18n.langConfig" />

        <div class="d-flex overflow-hidden ">
          <div class="d-flex flex-column flex-grow-1 overflow-hidden mr-2 w-fit">
            <span class="text-subtitle-2 font-weight-medium text-truncate text-[#6E6B7B]">John Doe</span>
            <span class="text-caption  text-[#B9B9C3]">Admin</span>
          </div>
          <UserProfile />
        </div>
      </div>
    </template>

    <!-- 👉 Sidebar Footer -->
    <template #after-vertical-nav-items>
      <div class="mt-auto px-4 pb-4">
        <VCard variant="flat" :style="`${configStore.theme === 'light' ? 'background: #F4F5F8' : ''}`"
          class="rounded-lg mb-2 pt-2 px-3 pb-3">
          <div class="d-flex ga-5 flex-column">
            <div class="d-flex align-center w-100">
              <div class="d-flex align-center w-100 ga-5">
                <UserProfile />
                <div class="d-flex flex-column flex-grow-1 overflow-hidden mr-2">
                  <span class="text-subtitle-2 font-weight-medium text-truncate">John Doe</span>
                  <span class="text-caption text-medium-emphasis">Admin</span>
                </div>
              </div>
              <VBtn icon="tabler-logout" variant="text" size="small" color="medium-emphasis" class="ml-auto" />
            </div>
            <div class="d-flex flex-column ga-1 text-truncate w-100">
              <span class="text-title-small text-[#1F1E23]">Email</span>
              <span class="text-title-small text-[#727273]">johndoe@gmail.com</span>
            </div>
          </div>
        </VCard>

        <div class="d-flex rounded-pill align-center w-100"
          :style="`padding: 4px; background: ${configStore.theme === 'light' ? '#F4F5F8' : ''};`">
          <div
            class="flex-grow-1 text-center py-2 rounded-pill font-weight-medium text-title-small text-body-1 cursor-pointer d-flex align-center justify-center"
            :class="configStore.theme === 'light' ? 'bg-white text-high-emphasis ' : 'bg-[#F4F5F8] text-medium-emphasis'"
            @click="configStore.theme = 'light'" :style="`${configStore.theme === 'light' ? 'background: white' : ''}`"
            style="transition: background-color 0.2s ease, box-shadow 0.2s ease;">
            <VIcon size="22" class="mr-2">tabler-sun-high</VIcon> Light
          </div>
          <div
            class="flex-grow-1 text-center py-2 rounded-pill font-weight-medium text-title-small text-body-1 cursor-pointer d-flex align-center justify-center"
            :class="configStore.theme === 'dark' ? 'bg-white  text-high-emphasis ' : 'bg-[#F4F5F8] text-medium-emphasis'"
            @click="configStore.theme = 'dark'" style="transition: background-color 0.2s ease, box-shadow 0.2s ease;">
            <VIcon size="22" class="mr-2">tabler-moon</VIcon> Dark
          </div>
        </div>
      </div>
    </template>

    <!-- 👉 Pages -->
    <slot />

    <!-- 👉 Footer -->
    <template #footer>
      <Footer />
    </template>

    <!-- 👉 Customizer -->
    <!-- <TheCustomizer /> -->
  </VerticalNavLayout>
</template>
