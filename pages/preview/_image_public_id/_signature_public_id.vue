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

  </div>
</template>

<script>

export default {
   asyncData ({ params }) {
        return {
            imagePublicId: params.image_public_id,
            signaturePublicId: params.signature_public_id
        }
  }
}
</script>
