<template>
    <div id="app">
        <WheaterApp></WheaterApp>
    </div>
</template>
<script>
import WheaterApp from './components/WeatherApp.vue'

export default {
    name: 'App',
    components: {
        WheaterApp
    }
}
</script>
<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: "comfortaa", sans-serif;
    background-color: #0d9094;
}

#app {
    transition: 0.4s;
    background-image: var(--bg-image-cold);
    background-repeat: no-repeat;
    background-size: contain;
    background-position: center;
}
</style>