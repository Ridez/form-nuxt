<template>
  <b-row>
    <b-col md="12">
      <b-form-group label="Some random radio buttons">
        <b-form-radio-group
          id="btn-radios"
          v-model="form.radio.selected"
          :options="form.radio.options"
          buttons
          button-variant="outline-primary"
          size="sm"
          name="radio-btn-outline"
        />
      </b-form-group>
    </b-col>
    <b-col 
      sm="6" 
      class="mx-auto">
      <b-form-group label="Select option">
        <b-form-select
          v-model="form.select.selected"
          :options="form.select.options"
          class="mb-3"
          value-field="item"
          text-field="name"
          disabled-field="notEnabled"
        />
      </b-form-group>
    </b-col>
    
    <b-col md="12">
      <b-button
        variant="primary"  
        size="sm"
        @click="$emit('update:step', 'StepOne')">Prev step</b-button>
      <b-button 
        variant="primary"
        size="sm"
        @click="$emit('update:step', 'StepThree')">Next step</b-button>
    </b-col>
  </b-row>
</template>

<script>
export default {
  props: {
    step: {
      type: String,
      default: ''
    },
    form: {
      type: Object,
      default: null
    },
    files: {
      type: Object,
      default: null
    }
  }
}
</script>

<style lang="scss">
.added-files {
  &__header {
    margin-top: 1rem;
  }
  &__list {
    list-style-type: none;
    padding-left: 0px;
    text-align: left;
  }
  &__remove-btn {
    background-color: transparent;
    border: none;
    img {
      width: 12px;
    }
  }
}
</style>
