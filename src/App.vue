<template>
  <VariableManager :variables="[...variables]" @variablesChanged="variablesChanged"/><br />
  <FormulaCreator :formula="{...this.formula}" :variables="variables" @formulaChanged="formulaChanged"/>
  <VariableTable :variables="[...variables]" @variablesChanged="variablesChanged"/>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import VariableManager, {Variable} from './components/VariableManager.vue';
import VariableTable from './components/VariableTable.vue';
import FormulaCreator, {Formula, LogicalOperator  } from './components/FormulaCreator.vue';

export default defineComponent({
  name: 'App',
  components: {
    VariableManager,
    VariableTable,
    FormulaCreator,
  },
  data(){
    return {
      variables: [] as Array<Variable>,
      title: '',
      description: '',
      formula: {
        operator: LogicalOperator.and,
        elements: [] as Array<Formula | string>,
      },
    }
  },
  methods : {
    variablesChanged(curVariables:Variable[]) {
      this.variables = curVariables; 
    },
    formulaChanged(formula: Formula, index:number) {
      this.formula = formula;
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
