<template>
    <div class="time" :class="[isToday ? 'is-today': '', isActive? 'is-active': '']" @click="clickTime">
        <a class="inner" @click.prevent.stop="update">
            <div class="date">{{ obj.month }}/{{ obj.date }}</div>
            <div class="day">({{ obj.language_shortDay }})</div>
        </a>
        <div class="sensor"
            @drop.prevent="dropDone"
            @dragenter.prevent="dropEnter"
            @dragover.prevent
        ></div>
    </div>
</template>

<script>
    export default {
        props: {
            obj: {
                require: true
            },
            today: {
                require: true
            },
            activeNode: {
                require: true
            }
        },
        computed: {
            isToday: function(){
                return (this.obj.year === this.today.year && this.obj.month === this.today.month && this.obj.date === this.today.date);
            },
            isActive: function(){
                let $obj = this.obj;

                if( this.activeNode[$obj.year +'-'+ $obj.month +'-'+ $obj.date] ){
                    return true;
                }else {
                    return false;
                }
            }
        },
        methods: {
            update: function(){
                this.$emit('sendEvent', 'updateCal', 'main', {
                    year: this.obj.year,
                    month: this.obj.month,
                    date: this.obj.date
                });

                this.$emit('sendEvent', 'updateMode', 'date');
            },
            clickTime: function(evt){
                this.$emit('sendEvent', 'clickTime', {
                    year: this.obj.year,
                    month: this.obj.month,
                    date: this.obj.date,
                    hour: 0,
                    minutes: 0
                }, 'date');
            },
            dropDone: function(){
                this.$emit('sendEvent', 'dropDone', this.obj);
            },
            dropEnter: function(){
                this.$emit('sendEvent', 'dropEnter', this.obj);
            }
        }
    }
</script>

<style scoped>

</style>
