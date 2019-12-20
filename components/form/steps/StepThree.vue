<template>
  <b-row>
    <b-col 
      md="8" 
      sm="12"
      class="mx-auto">
      <h6>Add some files</h6>
      <b-form-group style="padding-bottom: 1rem;"> 
        <b-form-file
          :file-name-formatter="formatNames"
          placeholder="Choose file..."
          drop-placeholder="Drop file here..."
          multiple=""
          no-drop=""
          @change="loadFile($event.target.files)"
        />
        <div 
          v-if="files.attachments.length" 
          class="added-files">
          <h6 class="added-files__header">Added files:</h6>
          <ul class="added-files__list">
            <li 
              v-for="(attachment, index) in files.attachments" 
              :key="index" 
              class="added-files__list-item">
              {{ attachment.filename }}
              <button 
                title="Remove file" 
                class="added-files__remove-btn" 
                @click="removeFile(files.attachments, index)"><img 
                  src="/images/close_black.svg"></button>
            </li>
          </ul>
        </div>
        <div
          v-if="files.sizeError"
          class="resp-info">
          Maximum file size: 5MB!
        </div>
      </b-form-group>
    </b-col>
    <b-col 
      md="12">
      <b-button 
        variant="primary"
        size="sm"
        @click="$emit('update:step', 'StepTwo')">Prev step</b-button>
      <b-button 
        type="submit"
        size="sm" 
        variant="primary">Submit</b-button>
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
  },
  methods: {
    getBase64(file, index) {
      var reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = () => {
        this.files.fileContent = reader.result.substr(
          reader.result.indexOf(',') + 1
        )
        const calcSize = this.files.filesSize + file.size
        if (calcSize < 5000000) {
          this.files.sizeError = false
          this.files.filesSize += file.size
          this.files.attachments.push({
            filename: file.name,
            content: this.files.fileContent,
            filesize: file.size
          })
        } else {
          this.files.sizeError = true
        }
      }
      reader.onerror = function(error) {
        console.log('Error: ', error)
      }
    },
    loadFile(files) {
      Object.keys(files).forEach((key, index) => {
        //check if file already added
        if (
          !this.files.attachments.some(
            file => file.filename === files[key].name
          )
        ) {
          this.getBase64(files[key], index)
        }
      })
    },
    removeFile(target, index) {
      this.files.filesSize -= target[index].filesize
      this.$delete(target, index)
      if (this.files.filesSize < 5000000) {
        this.files.sizeError = false
      }
    },
    formatNames() {
      const files = this.files.attachments

      if (files.length === 0) {
        return 'Choose file...'
      } else {
        return 'Choose another file...'
      }
    }
  }
}
</script>
