<template>
    <div class="vue-time-input">
        <button @click="minus">-</button>
        <input v-model="timeVal"/>
        <button @click="add">+</button>
    </div>
</template>

<script>
export default {
    props: {
        value: {
            type: [Number, String],
            default: '00:00:00,000'
        },
        max: {
            type: [Number, String],
            default: '00:00:01,000'
        },
        min: {
            type: [Number, String],
            default: '00:00:00,000'
        }
    },
    data () {
        return {
            timeVal: '',
            showBtn: false
        }
    },
    watch: {
        value: {
            immediate: true,
            handler (newVal) {
                this.timeVal = newVal
            }
        }
    },
    methods: {
        onFocus () {
            this.showBtn = true
        },
        onBlur () {
            this.showBtn = false
        },
        add () {
            let time = this.time_to_sec(this.timeVal) + 0.1
            let max = this.time_to_sec(this.max) - 0.1
            if (time > max) {
                this.timeVal = this.sec_to_time(max)
                return
            }
            this.timeVal = this.sec_to_time(time)
            this.$emit('input', this.timeVal)
        },
        minus () {
            let time = this.time_to_sec(this.timeVal) - 0.1
            let min = this.time_to_sec(this.min) + 0.1
            if (time < min) {
                this.timeVal = this.sec_to_time(min)
                return
            }
            this.timeVal = this.sec_to_time(time)
            this.$emit('input', this.timeVal)
        },
        time_to_sec (time) {
            let ms = Number(time.split(',')[1]) / 1000
            time = time.split(',')[0]
            let s = "";
            let hour = time.split(":")[0];
            let min = time.split(":")[1];
            let sec = time.split(":")[2];
            s = Number(hour * 3600) + Number(min * 60) + Math.floor(Number(sec)) + ms;
            return s;
        },
        sec_to_time (time) {
            if (time == '' || time == null || time == undefined || time <= 0) return '00:00:00,000';
            const min = Math.floor(time % 3600);
            let ms = Number(Number(time * 1000).toFixed(0)) % 1000
            return [
            (Math.floor(time / 3600)).toString().padStart(2, '0'),
            (Math.floor(min / 60)).toString().padStart(2, '0'),
            (Math.floor(time % 60)).toString().padStart(2, '0')
            ].join(':') + `,${ms.toString().padStart(3, '0')}`
        }
    }
}
</script>

<style scoped>
.vue-time-input{

}
</style>