<template>
    <div id="app">
        <Form />
    </div>
</template>

<script>
import Form from '@/components/Form'
export default {
    name: 'App',
    components: {
        Form
    }
}
</script>

<style lang="scss" scope>
    @import "@/scss/variables.scss";
    @import "@/scss/normilize.scss";

    body {
        width: 100%;
        margin: 0;
        padding: 0;
    }

    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        color: $font;
        margin: 60px 0;
        font-size: 16px;
    }

    @media screen and (max-width: 600px) {
        #app {
            font-size: 14px;
        }
    }
</style>
