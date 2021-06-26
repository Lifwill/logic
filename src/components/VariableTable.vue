<template>
  <div>
      <div v-for="variable in this.variables" :key="variable.title" style="display:inline-block;width:100px;">
        {{ variable.title}}
      </div>
      
  </div>
  <div v-for="line in logicalLineNumber" :key="line">
    <div v-for="variable in this.variables" :key="variable.title + line" style="display:inline-block;width:100px;">
     {{ line }} {{ this.logicalTable[line -1][variable.title]}}
    </div>
  </div>
</template>


<script lang="ts">
import { defineComponent, PropType } from 'vue'
import {Variable} from './VariableManager.vue'
export default defineComponent({
  name: 'VariableManager',
  data() {
    return {
      logicalTable: [] as Array<{ [key: string]: boolean; }>,
    }
  },
  props: {
    variables: {
      type: Array as PropType<Variable[]>,
      required:true,
    },
  },
  methods:{
  },
  computed:{
    logicalLineNumber():number {
      const variableQuantity = this.variables.length;
      if (variableQuantity === 0) return 0;
      return  Math.pow(2, variableQuantity);
    }
  },
  watch: {
    // if the variable list change, the logicalTable should be adapted accordingly
    variables(newVariables, oldVariables){
      console.log("it changed", newVariables, oldVariables);
      // add a new variable, line and adapt values
      if (newVariables.length > oldVariables.length) {
        const newTitle:string = newVariables[newVariables.length - 1].title;
        if (oldVariables.length === 0){
          // if empty a first line should be added
          this.logicalTable = [{}];
        }
        // duplicate the table and add the newTitle Value once 
        let newLogicalTable = [...this.logicalTable, ...this.logicalTable];
        this.logicalTable = newLogicalTable.map((logicalLine, index) => {
          return {...logicalLine, [newTitle] : index < Math.pow(2,newVariables.length-1)};
        });
      // remove the the lines and values
      } else if (newVariables.length < oldVariables.length) {
        // first find the value that have been removed
        const newTitles:Array<String> = newVariables.map((newVariable:Variable) => newVariable.title);
        const removedVariable = oldVariables.find((oldVariable : Variable) => !newTitles.includes(oldVariable.title));
        const removedTitle = removedVariable.title;
        // Then remove the duplicated lines
        let newLogicalTable = this.logicalTable.filter(logicalLine => logicalLine[removedTitle]);
        this.logicalTable = newLogicalTable.map(({[removedTitle] : removedTitleValue, ...keptTitles}) => keptTitles);
      }
      console.log('logicaltable', [...this.logicalTable], this.logicalLineNumber, this.variables);
    }
  }
})
</script>
