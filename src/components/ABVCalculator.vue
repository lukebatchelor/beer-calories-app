<template>
  <div class="text-h5 text-center mb-8">
    Estimate the number of calories in beer from ABV
  </div>
  <v-container fill-height="100%" fluid>
    <v-row>
      <v-col class="v-col-4">
        <v-text-field
          type="number"
          min="0"
          v-model.number="abv"
          label="ABV"
          variant="outlined"
          suffix="%"
          persistent-hint
        ></v-text-field>
      </v-col>
      <v-col>
        <v-select
          label="Beer size"
          v-model.number="beerSize"
          :items="beerSizeOptions"
          variant="outlined"
          persistent-hint
        ></v-select>
      </v-col>
    </v-row>

    <v-divider></v-divider>

    <v-row>
      <v-col>
        <div class="text-h6">Estimated Calories</div>
        <div class="text-h3 text-center">
          {{ computedCalories.toFixed(2) }}kcal
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>
<script setup lang="ts">
import { ref, computed } from 'vue';

const beerSizes = {
  Pony: 140,
  'Pot/Middy': 285,
  'Stubby/Can': 375,
  Schooner: 425,
  'US Pint': 473,
  'UK Pint': 568,
  'Long Neck': 750,
  Pint: 570,
  Howler: 946.3,
  Jug: 1140,
  Growler: 1892.7,
} as const;
type BeerSize = keyof typeof beerSizes;
const beerSizeOptions = Object.entries(beerSizes).map(([name, size]) => {
  return { title: `${name} (${size}ml)`, value: name };
});

const abv = ref<number>(4.5);
const beerSize = ref<BeerSize>('Pint');
const computedCalories = computed(() => {
  const sizeInOz = beerSizes[beerSize.value] / 29.5735;
  return abv.value * 2.5 * sizeInOz;
});
</script>
