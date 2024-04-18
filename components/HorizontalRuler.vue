<template>
    <div class='range-slider'>
        <input type="number" min="0" max="180" step="1" v-model="sliderMin">
        <input type="range" min="0" max="180" step="1" v-model="sliderMin">
        <input type="number" min="0" max="180" step="1" v-model="sliderMax">
        <input type="range" min="0" max="180" step="1" v-model="sliderMax">
    </div>
</template>

<script>
export default {

}
</script>

<style lang="css" scoped>
.range-slider {
    width: 300px;
    /* margin: auto; */
    text-align: center;
    position: relative;
    height: 6em;
    margin-bottom: 50px;
}

.range-slider input[type=range] {
    position: absolute;
    left: 0;
    bottom: 0;
}


input[type=range] {
    -webkit-appearance: none;
    width: 100%;
}

input[type=range]:focus {
    outline: none;
}



input[type=range]::-webkit-slider-runnable-track {
    width: 100%;
    height: 5px;
    cursor: pointer;
    animate: 0.2s;
    background: #eee;
    border-radius: none;
    box-shadow: none;
    border: 0;
}

input[type=range]::-webkit-slider-thumb {
    z-index: 2;
    position: relative;
    box-shadow: none;
    border: none;
    height: 16px;
    width: 16px;
    border-radius: 25px;
    background: #444;
    cursor: pointer;
    -webkit-appearance: none;
    margin-top: -5px;
}
</style>