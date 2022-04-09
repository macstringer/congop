<template>
  <div class="tagName" :style="{'background-color': backgroundColor, 'border': border}">
      {{ tagName }}
  </div>
</template>

<script>
export default {
    data(){
        return{
            backgroundColor: String,
            border: String
        }
    },
    props: {
        tagName: String
    },
    methods: {
        // randomColor(id) {
        //     const r = () => Math.floor(256 * Math.random());
        //     return this.colorCache[id] || (this.colorCache[id] = `rgb(${r()}, ${r()}, ${r()})`);
        // }
    },
    async mounted () {
        var h = 360 * Math.random();
        var s = (25 + 70 * Math.random());
        var l = (85 + 10 * Math.random());
        this.backgroundColor = "hsl(" + h + "," + s + "%," + l + "%)"

        var newL = l * 0.25;

        this.border = "2px solid hsl(" + h + "," + s + "%," + newL + "%)"

        // this.backgroundColor = "hsl(" + 360 * Math.random() + ',' +
        //      (25 + 70 * Math.random()) + '%,' + 
        //      (85 + 10 * Math.random()) + '%)'

    },
}
</script>

<style>

.tagName {
    padding: 10%;
    color: black;
    width: fit-content;

    display: inline-block;
    border-radius: 0px 10px 10px 0px;;
}

</style>