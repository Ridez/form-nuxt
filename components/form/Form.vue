<template>
  <div class="form-wrap">
    <b-card class="form-card">
      <b-form
        @submit="onSubmit">
        <b-row>
          <b-col 
            md="12" 
            class="title-col">
            <h2 class="form-title">Awesome form!</h2>
          </b-col>
        </b-row>
        <transition 
          name="slide" 
          mode="out-in">
          <component 
            :step.sync="step"
            :form="form"
            :files="files"
            :is="step"/>
        </transition>

        <b-row class="response-info">
          <b-col 
            lg="6" 
            md="12" 
            class="mx-auto" 
            style="margin-top: 1rem;"> 
            <div
              v-show="isSubmitted"
              class="resp-info">
              Your message was sent successfully!
            </div>
            <div v-show="submitting">
              <b-spinner 
                variant="primary" 
                label="Spinning"/>
            </div>
            <div
              v-if="error"
              class="resp-info">
              An error occured, please try again!
            </div>
          </b-col>
        </b-row>
      </b-form>
    </b-card>

    <b-card 
      class="mt-3" 
      header="Form Data Result">
      <strong style="margin-bottom: 20px;">Current step: {{ step }}</strong>
      <pre>{{ form }}</pre>
      attachments:
      <pre>{{ attachmentsNames }}</pre>
    </b-card>
  </div>
</template>

<script>
import StepOne from '~/components/form/steps/StepOne.vue'
import StepTwo from '~/components/form/steps/StepTwo.vue'
import StepThree from '~/components/form/steps/StepThree.vue'

export default {
  components: {
    StepOne,
    StepTwo,
    StepThree
  },
  data() {
    return {
      submitting: false,
      isSubmitted: false,
      error: false,
      step: 'StepOne',
      form: {
        email: '',
        name: '',
        msg: '',
        awesome: [],
        radio: {
          selected: 'radio1',
          options: [
            { text: 'Radio 1', value: 'radio1' },
            { text: 'Radio 3', value: 'radio2' },
            { text: 'Radio 3 (disabled)', value: 'radio3', disabled: true },
            { text: 'Radio 4', value: 'radio4' }
          ]
        },
        select: {
          selected: 'A',
          options: [
            { item: 'A', name: 'Option A' },
            { item: 'B', name: 'Option B' },
            { item: 'C', name: 'Option C', notEnabled: true },
            { item: 'D', name: 'Option D' }
          ]
        }
      },
      files: {
        attachments: [],
        filesSize: 0,
        sizeError: false,
        fileName: '',
        fileContent: ''
      }
    }
  },
  computed: {
    attachmentsNames() {
      return this.files.attachments.map(file => file.filename)
    }
  },
  methods: {
    async onSubmit(e) {
      e.preventDefault()

      this.submitting = true
      this.error = false

      try {
        await this.$axios.$post(
          'https://quizzical-engelbart-5cbd43.netlify.com/.netlify/functions/api/form',
          {
            form: this.form,
            files: this.files.attachments
          }
        )
        this.submitting = false
        this.isSubmitted = true
      } catch (e) {
        this.submitting = false
        this.error = true
        console.log(e)
      }
    }
  }
}
</script>

<style lang="scss">
.form-wrap {
  overflow: hidden;
  width: 80%;
  margin-top: 5rem;
  .card {
    background-color: #f6f6f6;
    .title-col {
      margin-top: 1rem;
      margin-bottom: 2rem;
    }
    &.mt-3 {
      .card-body {
        display: flex;
        flex-direction: column;
        align-items: center;
        pre {
          text-align: left;
        }
      }
    }
  }
}
.slide-enter-active {
  transition: all 0.5s ease;
}
.slide-leave-active {
  transition: all 0.5s ease;
}

.slide-enter {
  //transform: translateX(20%);
  opacity: 0;
}
.slide-leave-to {
  //transform: translateX(-20%);
  opacity: 0;
}
</style>
