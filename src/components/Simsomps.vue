<template>
    <div>
<section v-if="simpsom && simpsom.id>0">
    <h2>Simsons #{{ simpsom?.id }}</h2>
    <h3>mi nombre es {{ simpsom?.name }}</h3>
    <img :src="`https://cdn.thesimpsonsapi.com/200${simpsom?.image}`" :alt="simpsom?.name">
    <button :disabled="simpsomId===1"
     @click="simpsomId--">Anterior</button>
    <button @click="simpsomId++">Siguiente</button>

    <ul>
        <li v-for="frase in simpsom.frases">{{ frase }}</li>
    </ul>
</section>
    </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { useSimpson } from '../composable/useSimpson';
import type {SimsompsResponse} from '../interfaces/simsompsResponse';


interface Simpsom{
    id: number;
    name:string;
    image:string;
    frases:string[];
    genero:string;

}

const simpsom= ref<Simpsom|null>(null)
const simpsomId = ref(1)
const {data} = useSimpson<SimsompsResponse>(
    ()=>`https://thesimpsonsapi.com/api/characters/${simpsomId.value}`
);

watch(data, (newSimpson)=>{
    if (!newSimpson) return ;

    simpsom.value = {
        id:newSimpson.id,
        name: newSimpson.name,
        image: newSimpson.portrait_path,
        frases: newSimpson.phrases,
        genero: newSimpson.gender

    }
})




</script>

<style lang="css" scoped>
section{
    display: flex;
    flex-direction: column;
    gap: 10px;
    align-items: center;
    border: solid;
    border-color: black;
    border-radius: 40px;

}
button{
    background-color: rgb(226, 119, 92);
    margin: 4px;
}

</style>