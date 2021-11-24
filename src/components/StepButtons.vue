<template>
  <div 
    class="container-btn py-6"
    v-bind:class="{ 
      'justify-content-end': toggleStep === 0, 
      'justify-content-between': toggleStep > 0}"  
  >
    <div
      v-show="toggleStep > 0"
      @click="togglePrevStep"
      class="btn btn-prev"
    >
      ← 上一步
    </div>
    <div
      @click="toggleNextStep"
      class="btn btn-next first-step"
    >
      {{toggleStep !== 2 ? "下一步 →" : "結帳"}}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    step: {
      type: Number,
      required: true
    }
  },
  
  data() {
    return {
      toggleStep: '',
    }
  },

  methods: {
    fetchStep() {
      this.toggleStep = this.step
    },

    togglePrevStep() {
      if(this.toggleStep > 0) {
        this.toggleStep -= 1
        this.handleStep()
      }
    },
    toggleNextStep() {
      if(this.toggleStep < 2) {
        this.toggleStep += 1
        this.handleStep()
      }else {
        this.handleStep
      }
      
    },
    handleStep() {
      this.$emit('after-stepToggle', this.toggleStep)
    }

  },

  created() {
    this.fetchStep();
  }
};
</script>