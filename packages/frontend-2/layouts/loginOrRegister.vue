<template>
  <main class="h-screen overflow-hidden flex flex-col">
    <!-- Header -->
    <div class="relative px-4 py-3 bg-white/80 backdrop-blur-sm z-10">
      <div class="flex justify-between items-center max-w-screen-2xl mx-auto">
        <LogoBlock />
        <FormButton size="sm" text @click="toggleTheme">
          <Icon class="w-4 h-4" />
        </FormButton>
      </div>
    </div>

    <!-- Content -->
    <div class="flex-1 overflow-auto">
      <slot />
    </div>
  </main>
</template>

<script setup lang="ts">
import { SunIcon, MoonIcon } from '@heroicons/vue/24/solid'
import LogoBlock from '~/components/header/LogoBlock.vue'
import { useTheme } from '~~/lib/core/composables/theme'

const { isDarkTheme, toggleTheme } = useTheme()
const Icon = computed(() => (isDarkTheme.value ? SunIcon : MoonIcon))
</script>