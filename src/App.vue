<template>
  <div id="app">
    <div class="dropbox" @dragover="$event.preventDefault()" @drop="handleDrop">
      <h2 v-show="!this.file" class="help">Drop image to upload</h2>
      <img class="image" :src="preview" alt="">
    </div>
    <button
      :disabled="!this.file"
      @click="handleClick"
      :class="[btnClass, { disabled: !file }]"
    >
      Upload
    </button>
    <span class="error" v-show="error">File Not supported</span>
  </div>
</template>

<script>
const fileTypes = ['image/png', 'image/jpg', 'image/jpeg', 'image/gif', 'image/bmp'];

function addImage(_file) {
  this.preview = _file.target.result;
}

export default {
  name: "App",
  data() {
    return {
      preview: "",
      file: null,
      error: false,
      btn: `upload ${this.file === null ? "" : "disabled"}`,
      btnClass: 'upload'
    }
  },
  methods: {
    handleDrop(event) {
      event.preventDefault();
      const file = event.dataTransfer.files[0];
      if (
        fileTypes.includes(file.type) &&
        !(this.file && this.file.size == file.size)
      ) {
        this.error = false;
        this.file = file;
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = addImage.bind(this);
      } else {
        this.error = true;
        this.file = null;
        this.preview = '';
      }
    },
    handleClick() {
      console.log(this.preview);
    }
  }
};
</script>

<style scope>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 100px;
}
.dropbox {
  border-radius: 20px;
  width: 400px;
  height: 300px;
  background: #a3aaad;
  display: flex;
  justify-content: center;
  border: 2px dashed grey;
}
.image {
  max-height: 100%;
  max-width: 100%; 
  object-fit: contain;
}
.error {
  color: red;
}
.upload {
  margin-top: 10px;
  border-radius: 10px;
  padding: 10px;
  width: 400px;
  border: none;
  background: #087fbf;
  color: #FFF;
}

.disabled {
  opacity: 0.1;
}
</style>
