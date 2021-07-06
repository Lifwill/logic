<template>
<div>
  <input v-show="index > -1" :value="this.formula.name" placeholder="Optional sub-formula name" @blur="formulaNameChanged($event.target.value)"/>
  <select @change="operatorChanged($event.target.value)">
    <option :value="LogicalOperator.and">
      and
    </option>
    <option :value="LogicalOperator.or">
      or
    </option>
  </select>
    <span v-for="(element, index) in this.formula.elements" :key="index">
      <span v-if="typeof element === 'string'"> {{element}} </span>
      <span v-else>
        <FormulaCreator :variables="this.variables" :formula="element" :index="index" @formulaChanged="formulaChanged" />
      </span>
      <span v-if="index + 1 < this.formula.elements.length">{{" "}}{{LogicalOperator[this.formula.operator]}}{{" "}}</span>
    </span>
  <select @change="addVariable($event.target.value)"  v-model="resetSelected">
    <option value=""/>
    <option v-for="(variable, index) in this.variables" :key="index" :value="variable.title" >
      {{variable.title}}
    </option>
    <option value="__FORMULA__" key="__FORMULA__">
      New Formula
      </option>
  </select>
  </div>
</template>


<script lang="ts">
import { defineComponent, PropType } from 'vue';
import { Variable} from './VariableManager.vue';
/**
 * The formula object identifies a logical section that applies an operator on the list of elements
 * The name may be included to ease the readability of a rule
 */
export interface Formula {
  operator:LogicalOperator;
  elements:Array<Formula|string>;
  name?:string;
}

export enum LogicalOperator {
  and,
  or,
}
/**
 * The formula creator is a component that allows to create simple formulas formed by 'or' and 'and' operators.
 */
export default defineComponent({
  name: 'FormulaCreator',
  data() {
    return {
      resetSelected: "",
      LogicalOperator : LogicalOperator
    }
  },
  props: {
    formula: {
      type: Object as PropType<Formula>,
      required : true,
    }, 
    variables: {
      type : Array as PropType<Array<Variable>>,
      required: true,
    }, 
    index: {
      type: Number,
      required:false,
      default : -1,
    }
  },
  methods:{
    operatorChanged(newValue: string) {
      this.$emit('formulaChanged', {...this.formula, operator:newValue});
    },
    addVariable(newValue: string) {
      this.resetSelected = "";
      let addedValue:string|Formula;
      if (newValue === '__FORMULA__') {
        addedValue = {
          name: undefined,
          operator: LogicalOperator.and,
          elements: []
        }
      } else {
        addedValue = newValue;
      }
      this.$emit('formulaChanged', {...this.formula, elements: [...this.formula.elements, addedValue]}, this.index);
    },
    formulaChanged(formula: Formula, index:number) {  
      const newElements = [...this.formula.elements];
      newElements[index] = formula;
      this.$emit('formulaChanged', {...this.formula, elements: newElements }, this.index);
    },
    formulaNameChanged(formulaName:string) {
      this.$emit('formulaChanged', {...this.formula, name: formulaName }, this.index);
    }
  },
  computed:{
    isAnd():boolean{
      return this.formula.operator == LogicalOperator.and;
    },
    isOr():boolean{
      return !this.isAnd;
    }
  },
  watch: {
  }
})
</script>
