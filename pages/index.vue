<template>
  <div>
    <AppHeader />
    <div class="grid grid-cols-3 gap-10">
      <form @submit.prevent="upload" class="col-span-2">
        <div class="shadow sm:rounded-md sm:overflow-hidden">
          <div class="bg-white py-6 px-4 space-y-6 sm:p-6">
            <div>
              <h3 class="text-lg leading-6 font-medium text-gray-900">Upload PDF Document</h3>
              <p class="mt-1 text-sm text-gray-500">Upload the PDF document you want to sign</p>
            </div>

            <div>
              <div>
                <input @change="handleFile" type="file" accept=".pdf"  class="mt-1 block w-full border border-gray-300 rounded-md shadow-sm py-2 px-3 focus:outline-none focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm">
              </div>
            </div>
          </div>
          <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
            <p class="text-muted text-sm text-center" v-if="uploading">Uploading... </p>
            <button v-else type="submit" class="bg-indigo-600 border border-transparent rounded-md shadow-sm py-2 px-4 inline-flex justify-center text-sm font-medium text-white hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500">
              Select
            </button>
          </div>
        </div>
      </form>
      <div class="shadow sm:rounded-md sm:overflow-hidden cursor-pointer" @click="useSample">
        <div class="bg-white py-6 px-4 space-y-6 sm:p-6 text-center">
            <h3 class="text-lg leading-6 font-medium text-gray-900">Use pre-loaded PDF</h3>
            <p class="mt-1 text-sm text-gray-500">Click here to proceed with an already existing PDF</p>
            <cld-image :public-id="sampleDocPublicId" class="mx-auto" width="200" height="200" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      sampleDocPublicId:'nuxtjs-easy-document-signing/sample/DuckDuckGo_Privacy_simplified',
      file:null,
      uploading:false,
      cloudinaryFile:null,
    };
  },
  methods:{
     async handleFile(e) {
      this.file = e.target.files[0];
    },
    async readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
    async upload() {
      this.uploading = true;
      const fileData = await this.readData(this.file);
      this.cloudinaryFile = await this.$cloudinary.upload(fileData, {
        upload_preset: "default-preset",
        folder: "nuxtjs-easy-document-signing",
      });
      this.uploading = false;
      this.$router.push(`/sign/${encodeURIComponent(this.cloudinaryFile.public_id)}` )
    },
    useSample(){
      this.$router.push(`/sign/${encodeURIComponent(this.sampleDocPublicId)}` )
    }
  }
}
</script>