<template>
    <div class="todo-item">
        <div class="todo-item-left">
        <input type="checkbox" v-model ="completed" @change="doneEdit">
        <div v-if ="!editing" @dblclick= "editTodo" :class="[{completed: completed}, todo-item-label]">{{title}}</div>
        <input v-else class="todo-item-edit" type="text" v-model="title" 
        @blur="doneEdit" @keyup.enter="doneEdit" @keyup.esc="cancelEdit" v-focus>
    </div>
    <div>
        <button @click="pluralize">plural</button>
        <span class="remove-item" @click ="removeTodox(index)">
            &times;
        </span>
    </div>
    </div>
</template>
<script>
import {eventBus} from '../main'
export default {
    name: 'todo-item',
    props: {
     todo:{
         type: Object,
         required: true,
     },
     index: {
             type:Number,
             required: true,
         },
         checkAll:{
             type:Boolean,
             required: true,
         }
     } ,
     data(){
         return {
             'id': this.todo.id,
             'title':this.todo.title,
             'completed': this.todo.completed,
             'editing': this.todo.editing,
             'beforeEditCache': ' ',
         }
     },
     created(){
         eventBus.$on('pluralize', this.handlePluralize)
     },
     watch: {
         checkAll(){
            if(this.checkAll){
             this.completed = true
              }
              else{
                  this.completed = this.todo.completed
              }
            // this.completed = this.checkAll ? true : this.todo.completed
         } 
     },
      directives: {
  focus: {
    // directive definition
    inserted: function (el) {
      el.focus()
    }
  }
},
     methods: {
         removeTodox(index){
             eventBus.$emit('removedTodo', index)
         },
         editTodo(){
          this.beforeEditCache = this.title
          this.editing = true;
      },
      doneEdit(){
          if(this.title.trim().length == 0){
             this.title = this.beforeEditCache
          }
          this.editing = false
          eventBus.$emit('finishedEdit', {
              'index': this.index,
              'todo': {
                  id: this.id,
                  'title:': this.title,
                  'completed': this.completed,
                  'editing': this.editing,
              }
          }) 
      },
      cancelEdit(){
          this.title = this.beforeEditCache
          this.editing = false
      },
      pluralize(){
          event.$emit('pluralize')
      },
      handlePluralize(){
          this.title = this.title+'s'
      }
    }
}
</script>