<template>
    <div id="app">
        <Notes></Notes>
    </div>
</template>

<script>
    import Notes from './components/Notes'

    export default {
        name: 'app',
        data() {
            return {
                title: 'Notes'
            }
        },
        methods: {

        },
        components: {
            Notes
        }
    }
</script>
<style>

</style>

