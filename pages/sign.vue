<template>
  <section class="sign-container">
    <div class="wrapper">
      <img :src="imageUrl" height="800" width="600"/>
      <canvas id="signature-pad" height="800" width="600"></canvas>
    </div>
    <button @click="save">Save</button>
    <button @click="clear">Clear</button>
  </section>
</template>

<script>

import SignaturePad from "signature_pad";

export default {
  name: 'IndexPage',
  data(){
    return {
      signaturePad:null,
      pdf:{
        publicId: 'nuxtjs-easy-document-signing/sample/DuckDuckGo_Privacy_simplified'
      }
    }
  },
  mounted(){
    this.initializePad();
  },
  computed:{
    imageUrl(){
      return this.$cloudinary.image
                .url(this.pdf.publicId);
    }
  },
  methods:{
    initializePad(){
      this.signaturePad = new SignaturePad(document.getElementById('signature-pad'));
    },
    async save(){
        var data = this.signaturePad.toDataURL('image/png');

        const instance = await this.$cloudinary.upload(data, {
          folder: 'nuxtjs-easy-document-signing',
          upload_preset: "default-preset",
        });

        console.log(instance);
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
  background: rgba(0,0,0,0.1);
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