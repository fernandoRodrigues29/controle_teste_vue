<template>
    <div>
        <h1>Selecione o item</h1>
        {{ op }}
        <SelectItems v-model="op"
         :options="options" placeholder="selecione" 
         @sholdSearch="searchCities" @select="onSelect" 
         label-key="nome" value-key="id"/>
    </div>
</template>

<script>
import  SelectItems  from "@/components/SelectItems.vue";

export default {
    name:'Principal',
    data(){
        return{
            op:'',
            options:[]

        }
    },
    components:{
        SelectItems
    },
    methods:{
        searchCities(query){
            // console.log(query);
            // fetch(`https://jsonplaceholder.typicode.com/posts?title_like=^${query}`)
            // .then(rs=>rs.json())
            // .then(data=>{
            //     // console.log('conteudo',data);
            //     this.options = data;
            // });
            
            fetch(`https://brasilapi.com.br/api/ibge/municipios/v1/SE?providers=dados-abertos-br,gov,wikipedia`)
            .then(rs=>rs.json())
            .then(data=>{
                const retornoFiltrado = data.filter((data)=>{
                    return data.nome.toLowerCase().includes(query.toLowerCase());
                    // data.nome
                });
                this.options = retornoFiltrado;
            });
        },
        onSelect(q){
            console.log('o onselect via emit:',q);
        }
    }
}
</script>

<style>

</style>