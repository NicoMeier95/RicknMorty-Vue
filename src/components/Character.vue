<template>
    <div class="col-sm-12 col-md-6 col-lg-4"  v-bind:key="character.id">
        <div class="card mb-3">
            <img class="card-img-top" v-bind:src="character.image" v-bind:alt="character.name">
            <div class="card-body">
              <h4 class="card-title">{{character.name}}</h4>
              <button class="btn btn-success btn-sm rounded" @click="showMore(character.id)" data-bs-toggle="modal" data-bs-target="#exampleModal">Ver más</button>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props:['character'],
    methods: {
        showMore(id){
            this.$emit('showModal', id)
        }
    },
}
</script>
