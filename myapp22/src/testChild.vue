<template>
    <div>
        <button @click="childMethod()">Click-Child</button>
    </div>
</template>
<script>
    export default {
        methods: {
            childMethod () {
                const data = { a: '666', b: 'test' }
                this.$emit('fatherMethod', data)
            }
        }
    }
</script>
