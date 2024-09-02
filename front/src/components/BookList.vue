<template>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-8">
        <div v-for="book in books" :key="book.id"
            class="rounded-lg overflow-hidden shadow-lg transform transition duration-300 hover:scale-105 bg-white border border-gray-200">
            <!-- Image de couverture du livre -->
            <div class="h-64 w-full overflow-hidden">
                <img :src="book.coverUrl" alt="Couverture du livre" class="w-full h-full object-cover object-center" />
            </div>

            <!-- Informations sur le livre -->
            <div class="p-4">
                <h2 class="text-xl font-semibold text-gray-800 mb-2">{{ book.name }}</h2>
                <p class="text-gray-600 mb-4">{{ book.description }}</p>

                <!-- Auteur -->
                <p class="text-gray-700 font-semibold mb-1">Auteur : {{ book.author.Name }}</p>

                <!-- Genres -->
                <div class="flex flex-wrap gap-2 mt-2">
                    <span v-for="genre in book.genres" :key="genre.id"
                        class="text-sm font-medium bg-indigo-100 text-indigo-700 px-2 py-1 rounded">
                        {{ genre.Name }}
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const books = ref([]);

const fetchBooks = async () => {
    try {
        const response = await axios.get('http://localhost:1337/api/books?populate=author,genres,cover',
            { headers: { Authorization: `Bearer ${localStorage.getItem('token')}` } }
        );
        books.value = response.data.data.map(book => ({
            id: book.id,
            name: book.attributes.name,
            description: book.attributes.description,
            coverUrl: `http://localhost:1337${book.attributes.cover.data.attributes.url}`,
            author: book.attributes.author.data.attributes,
            genres: book.attributes.genres.data.map(genre => genre.attributes),
        }));
    } catch (error) {
        console.error("Erreur lors de la récupération des livres :", error);
    }
};

onMounted(() => {
    fetchBooks();
});
</script>

<style scoped>
/* Aucun style personnalisé requis pour l'instant */
</style>
