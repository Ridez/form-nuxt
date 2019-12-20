<template>
  <b-row>
    <b-col md="6">
      <b-form-group
        id="input-group-email"
      >
        <b-form-input
          id="input-email"
          v-model="form.email"
          type="email"
          required
          placeholder="Enter email"
        />
      </b-form-group>
    </b-col>
    <b-col md="6">
      <b-form-group>
        <b-form-input
          v-model="form.name"
          required
          placeholder="Enter name"
        />
      </b-form-group>
    </b-col>

    <b-col md="12">
      <b-form-group>
        <b-form-textarea 
          id="msg"
          v-model="form.msg"
          placeholder="Message..."
          rows="4" />
      </b-form-group>
    </b-col>
    <b-col md="12">
      <b-form-group id="input-group-4">
        <b-form-checkbox-group 
          id="checkboxes-4" 
          v-model="form.awesome">
          <b-form-checkbox value="awesome :)">Awesome!</b-form-checkbox>
          <b-form-checkbox value="not awesome :(">Not awesome!</b-form-checkbox>
        </b-form-checkbox-group>
      </b-form-group>
    </b-col> 
    <b-col md="12">
      <b-button
        variant="primary"
        size="sm"
        @click="$emit('update:step', 'StepTwo')">Next step</b-button>
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
    }
  }
}
</script>
