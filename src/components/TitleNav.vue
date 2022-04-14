<template>
    <div class="headerContainer">
        <img src="../assets/duke_wordmark_white.png">
        <div class="titleContainer">
            <h1>Congressional Internship Opportunities </h1>
            <button class="instBut" @click="dispInstructions = !dispInstructions"> Click Here to Toggle Instructions! </button>
            <div class="instructions" v-if="dispInstructions">
                <h3>
                    Instructions:
                </h3>
                <p> To search for internships, use the drop-down menus at the top of the screen to search and filter by chamber (House/Senate), state and district, party, name, paid/unpaid status, and other categories. </p>
                <p> Internship information is being collected on a rolling basis and does not currently include all congressional offices. </p>
                <p> This information is current as of April 2022 but is subject to change; please email or call offices directly to confirm the details of their internship programs. </p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TitleNav',
    data(){
        return{
            dispInstructions: false
        }
    }

}
</script>

<style>
h1 {
    font-weight:bolder;
}
img {
    width: 300px;
    padding-left: 100px;
}

.headerContainer{
    display: flex;
    color: white;
    background-color: #003087;
    width: 255%;
    align-items: center;
    /* vertical-align: middle; */
}
.titleContainer{
    padding: 30px;
    display: block;
    justify-content: left;
    font-family: Georgia, 'Times New Roman', Times, serif;
    font-weight: bold;
}

.instBut{
    background-color: #003087;
    color: white;
    font-weight: bold;
    padding: 5px;

    border: 2px solid white;

    padding-left: 10px;
    padding-right: 10px;
}
.instBut:active{
    border: 2px solid white;
    -webkit-transform: translateY(2px);
    transform: translateY(2px);
}

.instructions{
    width: 75vw;

    position: absolute;
    top: 20%;
    right: 20%;

    background-color: black;
    color: white;

    padding-left: 10%;
    padding-right: 10%;
    padding-top: 3%;
    padding-bottom: 3%;

    z-index: 5;

    border: 20px solid white;
}

</style>