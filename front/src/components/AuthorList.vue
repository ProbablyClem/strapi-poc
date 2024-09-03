<!-- Clickable list of authors name -->
<template>
    <div class="">
        <h2 class="text-2xl font-bold text-center mb-6">Liste des auteurs</h2>
        <div v-for="author in authors" :key="author.id" class="" @click="console.log(author)">
            <h2 class="text-xl font-semibold text-gray mb-2">{{ author.name }}</h2>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const authors = ref([]);
const fetchAuthors = async () => {
    try {
        const response = await axios.get('http://localhost:1337/api/authors',
            { headers: { Authorization: `Bearer ${localStorage.getItem('token')}` } }
        );
        authors.value = response.data.data.map(author => ({
            id: author.id,
            name: author.attributes.Name,
            description: author.attributes.description,
        }));
    } catch (error) {
        console.error("Erreur lors de la récupération des auteurs :", error);
    }
};

onMounted(() => {
    fetchAuthors();
});
</script>