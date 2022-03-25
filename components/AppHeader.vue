<template>
  <div>
    <h2 class="h2 text-center m-5 font-bold text-xl">Easy document signing in Nuxt.Js</h2>
    <nav class="w-3/4 m-10 mx-auto bg-white border rounded-lg p-5" aria-label="Progress">
      <ol role="list" class="space-y-4 md:flex md:space-y-0 md:space-x-8">
        <li class="md:flex-1" v-for="(step,index) in steps" :key="index">
          <!-- Completed Step -->
          <div :class="getStepClasses(index)">
            <span class="text-xs text-indigo-600 font-semibold tracking-wide uppercase group-hover:text-indigo-800">Step {{index+1}}</span>
            <span class="text-sm font-medium">{{step}}</span>
          </div>
        </li>
      </ol>
    </nav>
  </div>
</template>

<script>
export default {
  props:{
    currentStep:{
      type:Number,
      default:0,
    }
  },
  data(){
    return {
      steps:[
        'Document selection',
        'Document signing',
        'Document preview'
      ]
    }
  },
  methods:{
    getStepClasses(index){
      return index <= this.currentStep 
      ? "group pl-4 py-2 flex flex-col border-l-4 border-indigo-600 md:pl-0 md:pt-4 md:pb-0 md:border-l-0 md:border-t-4"
      : "group pl-4 py-2 flex flex-col border-l-4 border-gray-2 md:pl-0 md:pt-4 md:pb-0 md:border-l-0 md:border-t-4";
    }
  }
}
</script>