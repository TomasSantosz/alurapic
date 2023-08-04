
<template>
  <div>
      <h1 class="titulo">{{ titulo }}</h1>
      <input @input="filtro = $event.target.value" type="search" class="filtro" placeholder="filtre por parte do título">
      {{ filtro }}
      <ul class="lista-fotos">
        <li class="lista-fotos-item" v-for="foto of fotosComFiltro">
          <meu-painel :titulo="foto.titulo">
            <imagem-responsiva class="imagem-responsiva" :url="foto.url" :titulo=" foto.alt " />
            <meu-botao tipo="button" rotulo="REMOVER" @click.native="remove(foto)"/>
          </meu-painel>
        </li>
      </ul>

  </div>
</template>


<script>

import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/imagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';

export default {
  components:{
    'meu-painel' : Painel,
    'imagem-responsiva' : ImagemResponsiva,
    'meu-botao' : Botao
  },
  computed:{
    fotosComFiltro(){
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo))
      }else{
        return this.fotos;
      }
    }
  },
  name: 'app',
  data () {
    return {
      titulo: 'AluraPIC',
      fotos:[],
      filtro: ''
    }
  },
  methods:{
    remove(foto){
      if(confirm('Confirma operação?')){
        alert(`Foto ${foto.titulo} removida!`);
      }
    }
  },
  created(){
    this.$http.get('http://localhost:3000/v1/fotos')
      .then(res => res.json())
      .then((fotos) => this.fotos = fotos, err => console.log(err));
  }
}
</script>

<style>

  .titulo {
    text-align: center;
  }



  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }


  .filtro{
    display: block;
    width: 100%;
  }

</style>
