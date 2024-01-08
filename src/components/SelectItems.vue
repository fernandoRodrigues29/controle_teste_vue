<template>
  <div class="conteiner">
    <div class="inputBox">
        <input 
        @input="onInput($event.target.value)"
        :value="keyword" 
        class="campoPesquisa" 
        placeholder="pesquisa">
        <div class="lupaBox">
            <span>
                <svg
                            data-v-8dab3068=""
                            aria-hidden="true"
                            focusable="false"
                            data-prefix="fal"
                            data-icon="search"
                            role="img"
                            xmlns="http://www.w3.org/2000/svg"
                            viewBox="0 0 512 512"
                        ><path
                            data-v-8dab3068=""
                            fill="currentColor"
                            d="M508.5 481.6l-129-129c-2.3-2.3-5.3-3.5-8.5-3.5h-10.3C395 312 416 262.5 416 208 416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c54.5 0 104-21 141.1-55.2V371c0 3.2 1.3 6.2 3.5 8.5l129 129c4.7 4.7 12.3 4.7 17 0l9.9-9.9c4.7-4.7 4.7-12.3 0-17zM208 384c-97.3 0-176-78.7-176-176S110.7 32 208 32s176 78.7 176 176-78.7 176-176 176z"
                            class=""
                        /></svg>
            </span>
        </div>
        <div class="botaoSair">
            <span @click="onClear">
                <img :src="`${publicPath}fechar.svg`" alt="">
            </span>
        </div>
    </div>
    <div class="listaBox" v-if="listaMutaveis.length">
        <ul class="lista">
            <li class="item-lista" v-for="(op,index) in listaMutaveis" 
            :key="index" 
            @click="onSelect(op)">
                {{op.nome}}
            </li>
        </ul>
    </div>

  </div>
</template>

<script>
export default {
 name:"SelectItems",
props:{
    value:{
        type:String,
        default:''
    },
    options:{
        type: Array,
        default: ()=> []
    },
    labelKey:{
        type:String,
        default:'label'
    },
    valueKey:{
        type:String,
        default:'id'
    },
    valorMinimoParaPesquisa:{
        type:Number,
        default:3
    }
},
data(){
    return{
        keyword:'',
        listaOriginal: [],
        listaMutaveis: [],
        part:'',
        partes:[],
        publicPath: process.env.BASE_URL
    }
},
watch:{
    value(old,nuve){
        console.log('value ',this.value);
        this.keyword = this.value;
    },
    //observar se o options mudou
    options(){
        this.cloneOptions();
    }
},
created(){
    this.keyword = this.value
    //clonando o options vindo do props
    if(this.options){
        this.cloneOptions();
    }
},
methods:{
    onInput(v1){
        this.$emit('input',v1);
        if(v1.length >= this.valorMinimoParaPesquisa){
            if(!this.listaOriginal.length){
                this.$emit('sholdSearch',v1);
            }else{
                console.log('pesquisar na lista já baixada');
            }
        }else{
            console.log('resetar');
        }

    },
    onSelect(opt){
        this.$emit('select',opt);
        this.keyword = opt.title;
        this.$emit('input',opt.nome);
        this.onClearList();
    },
    onClear(){
        this.$emit('select',null);
        this.resetOptions();
        this.resetKeyWord();
        // this.keyword='';
        // this.$emit('input',this.keyword);
    },
    onClearList(){
        this.options = [];
    },
    cloneOptions(){
        this.listaOriginal = JSON.parse(JSON.stringify(this.options));
        this.listaMutaveis = JSON.parse(JSON.stringify(this.options));
    },
    resetOptions(){
        this.listaOriginal = [];
        this.listaMutaveis = [];
    },
    resetKeyWord(){
        this.keyword='';
        this.$emit('input',this.keyword);
    }

}
}
</script>

<style>
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
ul li{
    list-style: none;
}
    .conteiner{
        margin: 0 auto;
        width: 45vw;
        display: flex;
        justify-content: center;
        flex-direction: column;
        font-family: Arial, Helvetica, sans-serif;
    }
    .card{
        padding: 10px;
    }
    .inputBox{
        position: relative;
    }

    
    .campoPesquisa{
        display: block;
        width: 100%;
        padding: 8px 30px;
        font-size: 1rem;
        line-height: 1.5;
        color: #495057;
        background: #fff;
        border:1px solid #ced4da;
        border-radius: .25rem;
        transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
     }
     .lista{
        position: relative;
        margin-top: 2vh;
        padding: 10px;
        box-shadow: 1px 2px 3px #ccc;
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        max-height: 13rem;
        overflow: scroll;
     }
     .lista li{
        padding: 8px;
     }
     span svg{
        max-width: 1rem;
     }
     .item-lista{
        width: 400px;
        margin-bottom: 2px;
        text-overflow:ellipsis;
        border: 1px solid #c4c4c4;
        cursor: pointer ;
     }
     .lupaBox{
        position: absolute;
        top: 22%;
        background: transparent;
        width: 30px;
     }
     .botaoSair{
        position: absolute;
        top: 22%;
        right: 10px;
        background: transparent;
        width: 30px;
        cursor: pointer;
     }
     .botaoSair img{
        width: 24px;
     }
</style>