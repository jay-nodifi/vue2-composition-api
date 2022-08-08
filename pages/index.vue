<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
        <v-card-text class="white--text">
          <h1>Todo List({{totalTodos.length}})</h1>
          <p><i>Using Composition API</i></p>
          

          <v-form>
            <p v-if="isTyping" class="text--info">Typing...</p>
            <v-text-field v-model="form.title" placeholder="Enter Title" :messages="'test'">
            </v-text-field>

            <v-textarea v-model="form.description" placeholder="Enter Description" :messages="'test'">
            </v-textarea>
            <v-btn @click="addItem">Add Item</v-btn>
          </v-form>

          <v-list>
            <v-list-item v-for="(item, index) in totalTodos" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{item.title}}</v-list-item-title>
                <v-list-item-subtitle>{{item.description}}</v-list-item-subtitle>
                <v-list-item-subtitle>
                  <v-btn depressed outlined color="primary" @click="changeStatus(index, 'completed')">
                    Mark as Completed
                  </v-btn>
                </v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
      </v-card>
    </v-col>
    <v-col cols="12" sm="8" md="6">
      <v-card class="logo py-4 d-flex justify-center">
       <v-card-text class="white--text">
          <h1>Completed List({{totalCompleted.length}})</h1>
          <p><i>Using Composition API</i></p>
          
          <v-list>
            <v-list-item v-for="(item, index) in totalCompleted" :key="index">
              <v-list-item-content>
                <v-list-item-title>{{item.title}}</v-list-item-title>
                <v-list-item-subtitle>{{item.description}}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import { computed, onUnmounted, ref, watch } from 'vue'

const existingItems = [
  {title: 'Todo 1', description: 'Description 1', completed: false},
  {title: 'Todo 2', description: 'Description 2', completed: false},
  {title: 'Todo 3', description: 'Description 3', completed: false},
]

export default {
  name: 'IndexPage',
  setup() {
   /* 
    [EXAMPLE]: Reactive data
    [Further Read]: https://medium.com/@bsalwiczek/ref-vs-reactive-in-vue-3-whats-the-right-choice-7c6f7265ce39
   */

   const form = ref({title: '', description: '', completed: false})
   const items = ref(existingItems)
   const isTyping = ref(false)
   let timeout = null

  /* 
    [EXAMPLE]: computed properties
  */
   
   const totalTodos = computed(() => {
    return items.value.filter(item => !item.completed)
   })

   const totalCompleted = computed(() => {
    return items.value.filter(item => item.completed)
   })

    /* 
      [EXAMPLE]: methods
    */
   const addItem = () => {
     items.value.push(form)
   }
   const changeStatus = (index, status) => {
     totalTodos.value[index].completed = status
   }

    /* 
      [EXAMPLE]: watch with { deep:true }
    */

   const watchTyping = watch(form.value, ()=> {
    clearInterval(timeout)
    isTyping.value = !!(form.value.description || form.value.title)

      timeout = setInterval(() => {
        
        isTyping.value = false
      }, 800);
    }, {deep: true})

    /* 
      [EXAMPLE]: setting watch off
    */
   onUnmounted(() => {
    watchTyping()
   })

    return { form, items, totalTodos, totalCompleted, isTyping, addItem, changeStatus }
  },
}
</script>
