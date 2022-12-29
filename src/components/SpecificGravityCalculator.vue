<template>
  <div class="text-h5 text-center mb-8">
    Estimate the number of calories in beer from Specific Gravity
  </div>
  <v-container fill-height="100%" fluid>
    <v-row>
      <v-col>
        <v-text-field
          type="number"
          min="0"
          v-model.number="originalGravity"
          label="Original Gravity (OG)"
          variant="outlined"
          suffix="SG"
          step="0.001"
          persistent-hint
        ></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
          type="number"
          min="0"
          label="Final Gravity (FG)"
          v-model.number="finalGravity"
          variant="outlined"
          suffix="SG"
          step="0.001"
          persistent-hint
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-radio-group name="foo" v-model="accuracy" inline label="Accuracy">
          <v-radio name="foo" label="Quick and dirty" value="quick"></v-radio>
          <v-radio name="foo" label="Accurate™" value="accurate"></v-radio>
        </v-radio-group>
      </v-col>
    </v-row>
    <v-divider></v-divider>

    <v-row>
      <v-col>
        <div class="text-h6 mt-4">Calculated ABV</div>
        <div class="text-h3 text-center">{{ computedAbv.toFixed(2) }}%</div>
        <div class="text-h6">Estimated Calories</div>
        <div class="text-h3 text-center">
          {{ computedCalories.totalCal.toFixed(2) }}kcal
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

type Accuracy = 'quick' | 'accurate';

const originalGravity = ref<number>(1.055);
const finalGravity = ref<number>(1.015);
const accuracy = ref<Accuracy>('quick');
const computedAbv = computed(() => {
  const og = originalGravity.value;
  const fg = finalGravity.value;
  const acc = accuracy.value;

  if (acc === 'accurate') {
    return ((76.08 * (og - fg)) / (1.775 - og)) * (fg / 0.794);
  } else {
    return (og - fg) * 131.25;
  }
});
const computedCalories = computed(() => {
  const og = originalGravity.value;
  const fg = finalGravity.value;
  const calFromAlc = (1881.22 * fg * (og - fg)) / (1.775 - og);
  const calFromCarb = 3550 * fg * (0.1808 * og + 0.8192 * fg - 1.0004);
  const totalCal = calFromAlc + calFromCarb;
  return { calFromAlc, calFromCarb, totalCal };
});
const calculationText = computed(() => {
  const og = originalGravity.value;
  const fg = finalGravity.value;
  const acc = accuracy.value;

  const calsFromAlc = (1881.22 * fg * (og - fg)) / (1.775 - og);
  const calsFromCarbs = 3550 * fg * (0.1808 * og + 0.8192 * fg - 1.0004);
  const caloriesCalc = `
CalsFromAlcohol  = 1881.22 * FG * (OG - FG)/(1.775 - OG)
    = 1881.22 * ${fg} * (${og} - ${fg})/(1.775 - ${og})
    = ${(1881.22 * fg).toFixed(3)} * (${(og - fg).toFixed(3)})/(${(
    1.775 - og
  ).toFixed(3)})
    = ${calsFromAlc.toFixed(2)}kcal

CalsFromCarbs = 3550 * FG * ((0.1808 * OG) + (0.8192 * FG) – 1.0004)
  = 3550 * ${fg} * ((0.1808 * ${og}) + (0.8192 * ${fg}) – 1.0004)
  = ${(3550 * fg).toFixed(3)} * ((${(0.1808 * og).toFixed(3)}) + (${(
    0.8192 * fg
  ).toFixed(3)}) – 1.0004)
  = ${calsFromCarbs.toFixed(3)}kcal

TotalCals = CalsFromAlcohol + CalsFromCarbs
  = ${calsFromAlc.toFixed(3)} + ${calsFromCarbs.toFixed(3)}
  = ${(calsFromAlc + calsFromCarbs).toFixed(3)}kcal  
  `.trim();

  if (acc === 'quick') {
    const abv = (og - fg) * 131.25;
    return `
    ABV = (OG - FG) * 131.25
    = (${og} - ${fg}) * 131.25
    = ${abv.toFixed(2)}%

${caloriesCalc}`.trim();
  }
  const pt1 = og - fg;
  const pt2 = 1.775 - og;
  const pt3 = fg / 0.794;
  const finalAbv = ((76.08 * pt1) / pt2) * pt3;

  return `
  ABV = (76.08 * (OG-FG) / (1.775-OG)) * (FG / 0.794)
    = (76.08 * (${og}-${fg}) / (1.775-${og})) * (${fg} / 0.794)
    = (76.08 * (${pt1.toFixed(3)}) / (${pt2.toFixed(3)})) * (${pt3.toFixed(3)})
    = ${finalAbv.toFixed(2)}%

${caloriesCalc}
`.trim();
});
</script>
