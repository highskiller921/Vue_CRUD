<template>
    <div class="memo">
        <div class="act">
         <button class="btn btn-primary" @click="add()">+Add</button>
        </div>
        <ul>
            <li v-for="(d,idx) in state.data" :key="idx" @click="edit(idx)">{{ d }}</li>
        </ul>
    </div>
</template>

<script>
import {reactive} from "vue";
import axios from "axios";
export default {
    
    setup() {
        const state=reactive({
            data:[]
        
        })
         
        const add=()=>{
            const content=prompt("enter content!");
            //state.data.push("Add Memo contents");
             axios.post("/api/memos", {content}).then((res)=>{
                state.data=res.data;
             });
        };

        const edit=(idx)=>{
            const content=prompt("re-enter content!", state.data[idx]);
            console.log(content);
            axios.put("/api/memos/" + idx, {content}).then((res)=>{
                state.data=res.data;
            })
        }
        axios.get("/api/memos").then ((res)=>{
            state.data=res.data;
        });


        return {state, add, edit}
    },
}
</script>
 <style lang="scss" scoped>
   .memo {
       
       .act {
           padding: 10px 10px 5px 5px;
           text-align: right;
       }
       
       ul{
       border-top:1px solid #eee;
       list-style: none;
       margin: 0;
       padding:0;

       li {
           padding:15px;
           margin:5px;
           border:1px solid #eee;
       }
       }


   }
 </style>