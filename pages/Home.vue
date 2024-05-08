<template>
  <section class="wrapper p-5">
    <h1 class="mb-4">Resume Enhancer ✨</h1>
    <p class="mb-2">Upload Resume</p>
    <div class="upload center">
      <form class="center p-1">
        <img src="@/assets/images/upload-Icon.png" alt="upload" class="mb-4" />

        <p ref="element">
          Browse and select the PDF file you want to upload from your computer.
        </p>
        <p class="mb-4">File size should not exceed <b>5mb</b>.</p>

        <input
          type="file"
          ref="resumeFile"
          accept=".pdf"
          class="mb-2"
          id="pdfInput"
          @change="uploadResume"
          required
        />

        <!-- <button class="upload-btn p-1 pr-5 pl-5" type="submit">
          Upload <img src="@/assets/images/plus-icon.png" alt="upload-btn" />
        </button> -->
      </form>
    </div>
  </section>
</template>

<script>
import axios from "axios";

const API_BASE_URL = "https://resume-enhancer-api.onrender.com/api/v1/resumes";

export default {
  name: "App",
  data() {
    return {
      errorMessage: "",
      successMessage: "",
      resumeId: null,
    };
  },
  methods: {
    async uploadResume(event) {
      console.log("Loading...");
      try {
        const formData = new FormData();
        formData.append("resume", event.target.files[0]);

        const response = await axios.post(
          "https://resume-enhancer-api.onrender.com/api/v1/resumes/upload",
          formData,
          {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          }
        );

        if (response.status === 201) {
          this.successMessage = "Resume uploaded successfully.";
          this.errorMessage = "";
          this.resumeId = response.data.data.resume._id; // Update this line
        } else {
          console.error("Failed to upload resume.");
        }
      } catch (error) {
        this.errorMessage = error.message || "Failed to upload resume.";
        this.successMessage = "";
      }
      console.log(this.resumeId);
    },
  },
};
</script>

<style scoped>
* {
  -webkit-user-drag: none;
}
.wrapper {
  height: 100vh;
  width: 100vw;
  background-color: #fffefe;
}
.upload {
  border: 2px dashed #d1d5dc;
  height: 30em;
}
.upload-btn {
  background-color: #047857;
  color: #ffffff;
  display: flex;
  flex-direction: row;
  align-items: center;
  border-radius: 5px;
}
input {
  cursor: pointer;
}
@media (max-width: 768px) {
  .wrapper {
    padding: 20px !important;
  }
}
</style>
