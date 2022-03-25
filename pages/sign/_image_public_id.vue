<template>
  <div>
    <AppHeader :currentStep="1"/>

    <div class="text-center my-10 ">
          <h3 class="text-lg leading-6 font-medium text-gray-900">Sign PDF Document</h3>
          <p class="mt-1 text-sm text-gray-500">Feel free to draw your signature anywhere on the document below</p>
    </div>

    <section class="sign-container">
      <div class="wrapper border">
        <img :src="imageUrl" height="800" width="600"/>
        <canvas id="signature-pad" height="800" width="600"></canvas>
      </div>
    </section>

    <div class="my-10 text-center">
      <p class="text-muted text-sm text-center" v-if="uploading">Generating Signature & Uploading... </p>
      <span v-else>
        <button 
          type="button" 
          @click="save"
          class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Save
        </button>
        
        <button 
          type="button" 
          @click="clear"
          class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-indigo-700 bg-indigo-100 hover:bg-indigo-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          Clear
        </button>
      </span>
    </div>

  </div>
</template>

<script>

import SignaturePad from "signature_pad";

export default {
   asyncData ({ params }) {
    return {
       imagePublicId: params.image_public_id
    }
  },
  data(){
    return {
      signaturePad:null,
      uploading:false,
    }
  },
  mounted(){
    this.initializePad();
  },
  computed:{
    imageUrl(){
      return this.$cloudinary.image.url(this.imagePublicId);
    }
  },
  methods:{
    initializePad(){
      this.signaturePad = new SignaturePad(document.getElementById('signature-pad'));
    },
    async save(){
        this.uploading = true;
        
        var data = this.signaturePad.toDataURL('image/png');

        const signatureCloudinaryInstance = await this.$cloudinary.upload(data, {
          folder: 'nuxtjs-easy-document-signing',
          upload_preset: "default-preset",
        });

        this.$router.push(`/preview/${encodeURIComponent(this.imagePublicId)}/${encodeURIComponent(signatureCloudinaryInstance.public_id)}` )
    },
    clear(){
        this.signaturePad.clear();
    }
  }
}
</script>

<style scoped>
.sign-container{
  width:100vw;
  height:100vh;
}
.sign-container .wrapper{
  height:800px;
  width:600px;
  margin-left:auto;
  margin-right:auto;
  position:relative;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

.sign-container .wrapper img{
  top:0;
  left:0;
  position:absolute;
  height:800px;
  width:600px;
}

.sign-container .wrapper #signature-pad{
  position:absolute;
  top:0;
  left:0;
  height:800px;
  width:600px;
}
</style>