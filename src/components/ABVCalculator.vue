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
          step="0.1"
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
    <v-row v-if="beerSize === 'Custom'" justify="center">
      <v-col class="v-col-6">
        <v-text-field
          type="number"
          min="0"
          v-model.number="customSize"
          label="Custom Size"
          variant="outlined"
          step="1"
          suffix="ml"
          persistent-hint
        ></v-text-field>
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

    <v-divider class="my-2"></v-divider>
    <v-expansion-panels>
      <v-expansion-panel title="Calculations">
        <v-expansion-panel-text>
          <pre style="overflow-x: scroll">{{ calculationText }}</pre>
          <p class="text-caption">
            Note: In between steps are shown rounded for brevity but are not
            actually rounded until the end
          </p>
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
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
  Custom: 0,
} as const;
type BeerSize = keyof typeof beerSizes;
const beerSizeOptions = Object.entries(beerSizes).map(([name, size]) => {
  return {
    title: name === 'Custom' ? 'Custom' : `${name} (${size}ml)`,
    value: name,
  };
});
const abv = ref<number>(4.5);
const customSize = ref<number>(330);
const beerSize = ref<BeerSize>('Pint');
const computedCalories = computed(() => {
  const size_ml =
    beerSize.value === 'Custom' ? customSize.value : beerSizes[beerSize.value];
  const sizeInOz = size_ml / 29.5735;
  return abv.value * 2.5 * sizeInOz;
});
const calculationText = computed(() => {
  const size_ml =
    beerSize.value === 'Custom' ? customSize.value : beerSizes[beerSize.value];
  const sizeInOz = size_ml / 29.5735;
  const abvVal = abv.value;
  return `
  1 fl oz = 29.5735ml
  
BeerSizeInOz = ${size_ml}ml / 29.5735
  = ${sizeInOz.toFixed(2)} Oz

BeerCalories = ABV x 2.5 x BeerSizeInOz
  = ${abvVal} * 2.5 * ${sizeInOz.toFixed(2)}
  = ${(abv.value * 2.5 * sizeInOz).toFixed(2)}kcal
  `.trim();
});
</script>
