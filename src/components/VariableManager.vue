<template>
    <div v-for="variable in this.variables" :key="variable.title">
      {{ variable.title + ' ' + variable.description}}<button @click="del(variable.title)">Delete</button>
    </div>
    <input v-model="title" />
    <input v-model="description" />
    <button @click="add">Add</button>

</template>


<script lang="ts">
import { defineComponent, PropType } from 'vue'
export interface Variable {
  title:string;
  description:string;
}
export default defineComponent({
  name: 'VariableManager',
  data() {
    return {
      title: '',
      description: '',
    }
  },
  props: {
    variables: {
      type: Array as PropType<Variable[]>,
      required:true,
    },
  },
  methods:{
    del(title:string){
      console.log(title);
      let index = this.variables.findIndex((el) => el.title == title );
      console.log(index);
      this.variables.splice(index, 1);
      this.$emit('variablesChanged', this.variables);
    },
    add(){
      this.variables.push({
        title: this.title,
        description: this.description,
      });
      this.title = '';
      this.description= '';
      this.$emit('variablesChanged', this.variables);
    },
  },
})
</script>
