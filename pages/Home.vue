<template>
  <section class="wrapper p-5">
    <h1 class="mb-4">Resume Enhancer ✨</h1>
    <p class="mb-2">Upload Resume</p>
    <div class="upload center">
      <div class="center p-1">
        <img src="@/assets/images/upload-Icon.png" alt="upload" class="mb-4" />
        <p class="mb-1">
          Browse and chose the files you want to upload from your computer.
        </p>
        <p class="mb-1">Should not be more than <b>5mb</b></p>
        <button class="upload-btn p-1 pr-5 pl-5">
          Upload<img src="@/assets/images/plus-icon.png" alt="upload-btn" />
        </button>
      </div>
    </div>
  </section>
</template>

<script></script>

<style scoped>
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

@media (max-width: 768px) {
  .wrapper {
    padding: 20px !important;
  }
}
</style>
