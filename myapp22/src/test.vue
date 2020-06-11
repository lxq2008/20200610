<template>
    <div>
        <button @click="clickFather">Click-Father</button>
        <child v-if="visibleChild" @fatherMethod="fatherMethod"></child>
        <br /><br /><br />
        <button @click="dingtime">Time</button>
        <br /><br /><br />
        countlength: <input v-model="value" type="text" />
        <button @click="countlen(value)">countlen</button>
        <br /><br /><br />
        <button @click="countlength(value)">countlength</button>
    </div>
</template>
<script>
    import child from './testChild'
    export default {
        components: {
            child
        },
        data () {
            return {
                visibleChild: false,
                value: ''
            }
        },
        methods: {
            clickFather () {
                this.visibleChild = true
            },
            fatherMethod (data) {
                this.test(data)
            },
            test (data) {
                // console.log('test--test', data)
                this.test1()
            },
            test1 () {
                alert('555555')
            },
            hello () {
                console.log('hello!!')
            },
            dingtime () {
                // setTimeout(this.hello, 3000)
                const interval = setInterval(this.hello, 1000)
                clearInterval(interval)
            },
            countlen (str) {
                const len = str.length
                let reLen = 0
                for (let i = 0; i < len; i++) {
                    // if (str.charCodeAt(i) < 27 || str.charCodeAt(i) > 126) {
                    if (str.charCodeAt(i) >= '0x4e00' && str.charCodeAt(i) <= '0x9fa5') {
                        reLen += 2
                    } else {
                        reLen++
                    }
                }
                console.log(reLen)
                return reLen
            },
            countlength (str) {
                const len = str.match(/[^ -~]/g) == null ? str.length : str.length + str.match(/[^ -~]/g).length
                console.log('len', len)
            }
        }
    }
</script>
