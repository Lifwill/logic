<template>
    <div v-for="variable in this.variables" :key="variable.title">
      {{ variable.title + ' ' + variable.description}}<button @click="del(variable.title)">Delete</button>
    </div>
    <input v-model="title" />
    <input v-model="description" />
    <button @click="add">Add</button>

</template>


<script lang="ts">
import { ref, defineComponent } from 'vue'

export default defineComponent({
  name: 'VariableManager',
  data() {
    return {
      variables: [
        {
          title: 'beautiful',
          description: 'Define if the painting is considered as beautiful'
        },{
          title: 'big',
          description: 'Define if the painting is considered as big'
        },
      ],
      title: '',
      description: '',
    }
  },
  methods:{
    del(title:string){
      console.log(title);
      let index = this.variables.findIndex((el) => el.title == title );
      console.log(index);
      this.variables.splice(index, 1);
    },
    add(){
      this.variables.push({
        title: this.title,
        description: this.description,
      })
    },
  },
  setup: () => {
    const count = ref(0)
    return { count }
  },
})
</script>
