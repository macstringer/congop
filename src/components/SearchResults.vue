<template>
  <div>
      <div class="resultsContainer">
          <button v-for="term in allColTerms" :key="term.key" class="termButton">
              {{ term }}
          </button>
      </div>
        {{ typeText }}
  </div>
</template>

<script>
export default {
    data(){
        return{
            // backgroundColor: String,
            // border: String
        }
    },
    props: {
        // isFocused: Boolean,
        typeText: String,
        allColTerms: {},
    },
    methods: {
    },
    async mounted () {
    },
}
</script>

<style>

.resultsContainer{
    position: absolute;
    width: 161px;
    display: block;
}
.termButton{
    width: 100%;
}

</style>