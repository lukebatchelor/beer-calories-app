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
          {{ computedCalories.toFixed(2) }}kcal
        </div>
      </v-col>
    </v-row>
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
  return 180.88;
});
</script>
