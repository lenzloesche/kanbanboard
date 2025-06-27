<script>
import { columnsTitleJS } from '@/data/columns';
import Button from './button.vue';
import OrderButton from './orderButton.vue';

export default {   
    data(){
        return {
            searchTerm:"",
            columnTitle: columnsTitleJS,
            sortingOrder:{
                number:0,
                ascending:true,
            },
    }},
    components: {
        Button,
        OrderButton,
    },
    props: {
        handleTitleClick: Function,
        tasks: {
            type: Array,
            required: true
        }
    },
    methods: {
        filterSearch(){
            if (this.searchTerm==="")return [...this.tasks];
            return [...this.tasks].filter((task) => task.title.toLowerCase().includes(this.searchTerm.toLowerCase())||task.body.toLowerCase().includes(this.searchTerm.toLowerCase()));
        },

        sortTasks(howToSort){
            if (howToSort===0){
                if (this.sortingOrder.ascending===true){
                    return this.filterSearch().sort((a,b) => a.title.localeCompare(b.title, 'de'));
                }else{
                    return this.filterSearch().sort((a,b) => b.title.localeCompare(a.title, 'de'));
                }                
            } else if (howToSort===1){
                if (this.sortingOrder.ascending===true){
                    return this.filterSearch().sort((a,b) => a.body.localeCompare(b.body, 'de'));
                } else {
                    return this.filterSearch().sort((a,b) => b.body.localeCompare(a.body, 'de'));
                }
            } else if (howToSort===2){
                if (this.sortingOrder.ascending===true){
                    return this.filterSearch().sort((a,b) => a.column - b.column);
                } else {
                return this.filterSearch().sort((a,b) => b.column - a.column);
                }
            } else {                
                return this.filterSearch();
            }
            
        },
        changeSortingOrder(whichOne){
            if (this.sortingOrder.number===whichOne) 
            {
                this.sortingOrder.ascending = !this.sortingOrder.ascending;
            } else{
            this.sortingOrder.number=whichOne;
            this.sortingOrder.ascending=true;
            }
        },

        
    }
}
</script>
<template>
    <div class="list-view">
        <p class="filter">Filter: <input type="text" class="input-search" v-model="searchTerm" /></p>
        <table>
            <thead>
                <tr class="table-head">
                    <td>
                        <h2 class="head-line">Name
                            <OrderButton :changeSortingOrder="changeSortingOrder" :orderNumberToChange=0 :sortingOrder="sortingOrder"></OrderButton>                            
                        </h2>
                    </td>
                    <td>
                        <h2 class="head-line">Body
                            <OrderButton :changeSortingOrder="changeSortingOrder" :orderNumberToChange=1 :sortingOrder="sortingOrder"></OrderButton>  
                        </h2>
                    </td>
                    <td>
                        <h2 class="head-line">Status
                            <OrderButton :changeSortingOrder="changeSortingOrder" :orderNumberToChange=2 :sortingOrder="sortingOrder"></OrderButton>
                        </h2>
                    </td>
                </tr>
            </thead>
           <tbody>
            <tr v-for="task in sortTasks(sortingOrder.number)" :key="task.id" class="list-background">
                <td class="td"><div class="text-overflow text-body" @click ="handleTitleClick(task)">{{ task.title.length>0 ? task.title : "-Kein Titel-" }}</div></td>
                <td class="td"><div class="text-overflow text-body" @click ="handleTitleClick(task)">{{ task.body.length>0 ? task.body : "-Kein Body-" }}</div></td>
                <td class="td"><div class="text-overflow text-body" @click ="handleTitleClick(task)">{{ columnTitle[task.column] }}</div></td>
            </tr>
            <tr class="table-head">
                    <td><h2 class="head-line">-</h2></td>
                    <td><h2 class="head-line">-</h2></td>
                    <td><h2 class="head-line">-</h2></td>
                </tr>
           </tbody>
            
        </table>
    </div>
</template>

<style>
.filter {
    font-size: large;
    margin-bottom: 4px;;
}
.list-background:nth-child(even) {
  background-color: #f3f4f6;
}
.dark .list-background{
  background-color: #000000;
}
.dark .list-background:nth-child(even) {
  background-color: #424242;
}
.head-line {
    color: white;
}
.table-head {
    background-color: rgb(0, 57, 82);
}
.list-view {
    padding-left:20px;
}
table {
    min-width: 100%;
}
.td {
    min-width: 200px;
}
.text-overflow {    
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
.text-body {
    color:black;
 }
 .dark .text-body {
    color:white;
 }
</style>