<template>
  <div>
    <AppHeader :currentStep="2"/>

    <div class="text-center my-10 ">
          <h3 class="text-lg leading-6 font-medium text-gray-900">Signed PDF Preview</h3>
          <p class="mt-1 text-sm text-gray-500">Here is your signed PDF document</p>
    </div>

    <div class="text-center">
        <cld-image class="mx-auto" :public-id="imagePublicId" height="800" width="600">
            <cld-transformation
                :overlay="signaturePublicId.replace(/\//g, ':')"
            />
        </cld-image>
    </div>

    <div class="my-10 text-center">
      <a 
        :href="pdfLink"
        target="_blank"
        class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md shadow-sm text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
      >
        View PDF
      </a>
      <nuxt-link
        to="/"
        class="inline-flex items-center px-4 py-2 border border-transparent text-sm font-medium rounded-md text-indigo-700 bg-indigo-100 hover:bg-indigo-200 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
        Start again
      </nuxt-link>
    </div>

  </div>
</template>

<script>

export default {
   asyncData ({ params }) {
        return {
            imagePublicId: params.image_public_id,
            signaturePublicId: params.signature_public_id
        }
  },
  computed:{
    pdfLink(){
      return this.$cloudinary.image.url(
        this.imagePublicId,
        {
          transformation:[
            {overlay:this.signaturePublicId.replace(/\//g, ':')},
          ]
        }
        ).replace("/f_auto,q_auto","");
    }
  }
}
</script>