<template>
    <div>
    <label><input type="checkbox" :checked = "!anyRemaining" @change ="checkAllTodos">check All </label>
    </div>
</template>

<script>
import {eventBus} from '../main'
export default {
    name: 'todo-check-all',
    props: {
        anyRemaining:{
            type:Boolean,
            required:true,
        }
    },
    methods: {
        allchecked(){
            eventBus.$emit('checkAllchanged', this.anyRemaining)
        }
    }
}
</script>