<template>
    <div class="headerContainer">
        <img src="../assets/duke_wordmark_white.png">
        <div class="titleContainer">
            <h1>Congressional Internship Opportunities </h1>
            <h3> *Jobs Posted Spring 2022* </h3>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TitleNav'
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
    background-color: darkblue;
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

</style>