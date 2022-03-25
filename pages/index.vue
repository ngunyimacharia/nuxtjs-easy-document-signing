<template>
  <section class="sign-container">
    <div class="wrapper">
      <img :src="imageUrl" height="800" width="600"/>
      <canvas id="signature-pad" height="800" width="600"></canvas>
    </div>
    <button id="save">Save</button>
    <button id="clear">Clear</button>
  </section>
</template>

<script>

import SignaturePad from "signature_pad";

export default {
  name: 'IndexPage',
  data(){
    return {
      pdf:{
        publicId: 'nuxtjs-easy-document-signing/sample/DuckDuckGo_Privacy_simplified'
      }
    }
  },
  mounted(){
    var signaturePad = new SignaturePad(document.getElementById('signature-pad'));
    var saveButton = document.getElementById('save');
    var cancelButton = document.getElementById('clear');

    saveButton.addEventListener('click', function (event) {
      var data = signaturePad.toDataURL('image/png');

    // Send data to server instead...
      window.open(data);
    });

    cancelButton.addEventListener('click', function (event) {
      signaturePad.clear();
    });
  },
  computed:{
    imageUrl(){
      return this.$cloudinary.image
                .url(this.pdf.publicId);
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