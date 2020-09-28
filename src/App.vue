<template>
  <form>
    <input type="file" @change="onFileSelected" />
    <button @click="onButtonClick">Upload</button>
  </form>
</template>

<script>
import * as tus from "tus-js-client";

export default {
  name: "App",
  data() {
    return {
      selectedFile: null,
    };
  },
  methods: {
    onFileSelected(event) {
      this.selectedFile = event.target.files[0];
    },
    onButtonClick(event) {
      event.preventDefault();
      console.log("I have been clicked!");
      let upload = new tus.Upload(this.selectedFile, {
        endpoint: "https://test.api.najottalim.uz/v1/upload-video/",
        retryDelays: [0, 3000, 5000, 10000, 20000],
        metadata: {
          filename: this.selectedFile.name,
          filetype: this.selectedFile.type,
        },
        onError: (error) => {
          console.log("Failed because: " + error);
        },
        onProgress: (bytesUploaded, bytesTotal) => {
          let percantage = ((bytesUploaded / bytesTotal) * 100).toFixed(2);
          console.log(bytesUploaded, bytesTotal, percantage + "%");
        },
        onSuccess: () => {
          console.log("Download %s from %s", upload.file.name, upload.url);
        },
      });

      upload.start();
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
