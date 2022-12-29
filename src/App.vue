<template>
  <v-app :theme="theme">
    <v-app-bar title="Beer Calorie Estimator">
      <v-btn :icon="themeIcon" @click="toggleTheme"></v-btn>
    </v-app-bar>
    <v-main class="mt-8">
      <SpecificGravityCalculator
        v-if="activePage === 'from-gravity'"
      ></SpecificGravityCalculator>
      <ABVCalculator v-if="activePage === 'from-abv'"></ABVCalculator>
    </v-main>
    <v-bottom-navigation v-model="activePage" mandatory="force">
      <v-btn value="from-abv">
        <v-icon icon="mdi-beer"></v-icon>
        From ABV
      </v-btn>

      <v-btn value="from-gravity">
        <v-icon icon="mdi-test-tube"></v-icon>
        From Gravity
      </v-btn>
    </v-bottom-navigation>
  </v-app>
</template>

<style scoped></style>

<script setup lang="ts">
import { ref, computed, onMounted } from 'vue';
import SpecificGravityCalculator from '@/components/SpecificGravityCalculator.vue';
import ABVCalculator from '@/components/ABVCalculator.vue';

type Page = 'from-gravity' | 'from-abv';

const theme = ref('dark');
const activePage = ref<Page>('from-abv');
const themeIcon = computed(() => {
  return theme.value === 'light' ? 'mdi-weather-sunny' : 'mdi-weather-night';
});

function toggleTheme() {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
}
</script>
