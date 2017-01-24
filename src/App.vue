<template>
    <div id="app">
        <pan></pan>
    </div>
</template>
<script>
import panel from './components/search-panel'

export default {
    name: 'app',
    components: {
        pan: panel
    }
}
</script>
<style>
html,
body {
    height: 100%;
    margin: 0;
    padding: 0;
    min-width: 600px;
}
#app{
    overflow: hidden;
}
body {
    background: url('assets/background.png') no-repeat;
}
</style>
