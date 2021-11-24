<template>
  <div class="form-stepper">
      <div 
        v-bind:class="{ active: step === 0, checked : step > 0 }"
        class="step"
      >
        <p class="step-name step-1">1</p>
        <span class="step-line"></span>
      </div>
      <div 
        v-bind:class="{ active: step === 1, checked : step > 1 }"
        class="step"
      >
        <p class="step-name step-2">2</p>
        <span class="step-line"></span>
      </div>
      <div
        v-bind:class="{ active: step === 2, checked : step > 2 }"
        class="step"
      >
        <p class="step-name step-3">3</p>
      </div>
    </div>
</template>

<script>
export default {
  props: {
    step: {
      type: Number,
      required: true,
    },
  },
  methods: {
    
  }
}
</script>