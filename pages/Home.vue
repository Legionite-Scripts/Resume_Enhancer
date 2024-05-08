<template>
  <section class="wrapper p-5">
    <h1 class="mb-4">Resume Enhancer ✨</h1>
    <p class="mb-2">Upload Resume</p>
    <div class="upload center">
      <form class="center p-1" @submit.prevent>
        <img src="@/assets/images/upload-Icon.png" alt="upload" class="mb-4" />

        <p ref="element">
          Browse and select the PDF file you want to upload from your computer.
        </p>
        <p class="mb-4">File size should not exceed <b>5mb</b>.</p>

        <input
          type="file"
          ref="resumeFile"
          accept=".pdf"
          class="mb-4"
          id="pdfInput"
          @change="uploadResume"
          required
        />

        <div class="mb-3 btns">
          <button
            class="upload-btn p-1 pr-5 pl-5 mb-2"
            type="submit"
            @click="previewResume"
          >
            Preview
          </button>

          <button
            class="upload-btn p-1 pr-5 pl-5 mb-2"
            type="submit"
            @click="downloadResume"
          >
            Download
          </button>
        </div>
        <a :href="previewResponse" target="blank">{{ previewResponse }}</a>
        <div v-show="isLoading" class="loading-container">
          <div class="loading-icon"></div>
        </div>
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
      previewResponse: null,
      isLoading: false, // New data property to  control loading animation
    };
  },
  methods: {
    async uploadResume(event) {
      console.log("Loading...");
      this.isLoading = true; // Show loading animation before fetch
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
      } finally {
        this.isLoading = false; // Hide loading animation after fetch completes
      }
      console.log(this.resumeId);
    },
    //PREVIEW RESUME
    async previewResume() {
      console.log("Resume Preview ongoing...");
      try {
        const response = await axios.get(
          `https://resume-enhancer-api.onrender.com/api/v1/resumes/${this.resumeId}`
        );
        if (response.status === 200) {
          // Handle successful response, e.g., update UI with enhanced resume details
          console.log("Preview Successful");
          console.log(response.data.data.resume);
          this.previewResponse = response.data.data.resume;
        } else {
          console.error("Failed to preview resume.");
        }
      } catch (error) {
        this.errorMessage = error.message || "Failed to preview resume.";
        this.successMessage = "";
      }
    },

    //DOWNLOAD RESUME
    async downloadResume() {
      try {
        const response = await axios.get(
          `https://resume-enhancer-api.onrender.com/api/v1/resumes/download/${this.resumeId}`,
          {
            responseType: "blob",
          }
        );
        if (response.status === 200) {
          const url = window.URL.createObjectURL(new Blob([response.data]));
          const link = document.createElement("a");
          link.href = url;
          link.setAttribute("download", "resume.pdf"); // You can set the file name here
          document.body.appendChild(link);
          link.click();
        } else {
          throw new Error("Failed to download resume.");
        }
      } catch (error) {
        this.errorMessage = error.message || "Failed to download resume.";
        this.successMessage = "";
      }
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
.btns {
  display: flex;
  flex-direction: row;
  gap: 8%;
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
.loading-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.3); /* Optional: Semi-transparent overlay */
  display: flex;
  justify-content: center;
  align-items: center;
}

.loading-icon {
  /* Add styles for your chosen loading animation here */
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 5px solid #047857;
  border-top-color: transparent; /* Simulates spinning animation */
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

@media (max-width: 768px) {
  .wrapper {
    padding: 20px !important;
  }
  .btns {
    display: flex;
    flex-direction: column;
  }
}
</style>
