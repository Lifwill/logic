<template>
  <VariableManager :variables="variables" @variablesChanged="variablesChanged"/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import VariableManager, {Variable} from './components/VariableManager.vue';

export default defineComponent({
  name: 'App',
  components: {
    VariableManager
  },
  data(){
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
  methods : {
    variablesChanged(curVariables:Variable[]) {
      this.variables = curVariables;
    }
  }
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
