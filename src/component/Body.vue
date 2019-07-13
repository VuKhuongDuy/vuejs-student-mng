<template>
    <div id="body">
        <Left @click_student="event_click_student_from_left" v-bind:load_data="load_data"></Left>
        <Right @load_data="on_event_load_data" v-bind:student_choose="student_choose"></Right>
    </div>    
</template>

<script>
import Left from './body/Left.vue'
import Right from './body/Right.vue'
export default {
    data: function(){
        return {
            student_choose: [],
            load_data: false
        }
    },
    methods: {
        event_click_student_from_left: function(value){
            this.student_choose = value;
        },
        emit_event_click_student_to_right: function(){
            this.$emit('receive_student_choose',this.student_choose);
        },
        on_event_load_data: function(){
            this.load_data=!this.load_data
        }
    },
    components: {
        Left,
        Right
    }
}
</script>

<style>
    #body{
        width: 100%;
        height: 864px;
    }
</style>

