<template>
  <div id="app">
    
    <div class="container-fluid bg-light p-3 text-center">
        <h2 class="text-success">Rick&Morty<span class="text-black fs-5">Personajes</span></h2>
        <form class="container">
          <div class="row g-3 my-5  align-items-center">
            <div class="col-auto">
              <input v-model="search" type="text" class="form-control" placeholder="Buscar personaje" v-on:keyup.enter="searchData">
            </div>
            <div class="col-auto">
              <button class="btn btn-success btn-large" v-on:click="searchData">Buscar</button>
            </div>
          </div>
        </form>
        
    </div>
    
    <div class="container">
      <div class="row">
        <Character 
          v-for="character of characters" 
          v-bind:key="character.id"
          v-bind:character="character"
          @showModal="showModal"
        />
      </div>
      <nav aria-label="Search results pages" role="navigation">
          <ul class="pagination justify-content-center">
            <li class="page-item"><a class="page-link" v-on:click="changePage(page - 1)">Previous</a></li>
            <li class="page-item page-link active">
              <a aria-current="page">{{page}}</a>
            </li>
            <li class="page-item"><a class="page-item page-link" v-on:click="changePage(page + 1)">Next</a></li>
          </ul>
      </nav>
    </div>

    <!--MODAL-->
  <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true"
    :class="{ 'active':modal}"
    v-if="modal"
  > 
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">Acerca de: {{currentCharacter.name}}</h5>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <p class="text-decoration-underline">Género:</p>
          <p class="ms-3">{{currentCharacter.gender}}</p>
          <p class="text-decoration-underline">Estatus:</p>
          <p class="ms-3">{{currentCharacter.status}}</p>
          <p class="text-decoration-underline">Especie:</p>
          <p class="ms-3">{{currentCharacter.species}}</p>
          <p class="text-decoration-underline">Tipo:</p>
          <p class="ms-3">{{currentCharacter.type}}</p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        </div>
      </div>
    </div>
  </div>
  
  </div>
</template>

<script>
import axios from 'axios'
import Character from './components/Character'

export default {
  name: 'App',
  components: {
    Character
  },
  data: function(){
    return{
      characters : [],
      page: 1,
      pages: 1,
      search: '',
      modal: false,
      currentCharacter: {}
    }
  },
  created() {
    this.fetch();
  },
  methods: {
    fetch(){
      const params = {
        page: this.page,
        name: this.search
      }


      let result = axios.get(`https://rickandmortyapi.com/api/character/`,{params})
        .then((res) => {
          this.characters= res.data.results;
          this.pages = res.data.info.pages;
        })
        .catch((err)=>{
          console.log(err)
        })
        return result;
    },
    changePage(page){
      this.page = (page<=0 || page>this.pages) ? this.page : page
      this.fetch
    },
    searchData(){
      this.page=1,
      this.fetch();
    },
    showModal(id){
      this.fetchOne(id)
    },
    async fetchOne(id){
      let result = await axios.get(`https://rickandmortyapi.com/api/character/${id}/`);
      this.currentCharacter = result.data;
      this.modal=true;

      
    }

  },
}
</script>

<style>

</style>
