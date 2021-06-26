<template>
  <VariableManager :variables="[...variables]" @variablesChanged="variablesChanged"/>
  <VariableTable :variables="[...variables]" @variablesChanged="variablesChanged"/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import VariableManager, {Variable} from './components/VariableManager.vue';
import VariableTable from './components/VariableTable.vue';

export default defineComponent({
  name: 'App',
  components: {
    VariableManager,
    VariableTable,
  },
  data(){
    return {
      variables: [] as Array<Variable>,
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
