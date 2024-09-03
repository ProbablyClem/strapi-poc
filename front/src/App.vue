<template>
  <div class="container mx-auto p-4">
    <h1 class="text-4xl font-bold text-center mb-6 text-red">{{ homePageContent.header }}</h1>
    <div class="flex flex-row justify-center">
      <AuthorList class="absolute-left" />
      <BookList class="" />
    </div>

  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import AuthorList from './components/AuthorList.vue';
import BookList from './components/BookList.vue';
localStorage.setItem('token', '9a13f751db33899f95df49c4e3075b9a48bd5db55dc952a4f2119937f4420796b5bfb8c843cff76fcb472e0c52e5248c7845c00fef970c3d0851f384f8a3a07360f844f713b2a3633824deb480275f1e8e43b5afe330bb43c849280e4e268e185d27f5e5bb8094896b5b4deb7fbb7b5aed2a7894939e514263138fe4653aa4fa')

const homePageContent = ref({})
const fetchHomePageContent = async () => {
  try {
    const response = await axios.get('http://localhost:1337/api/home-page',
      { headers: { Authorization: `Bearer ${localStorage.getItem('token')}` } }
    );
    homePageContent.value = response.data.data.attributes;
  } catch (error) {
    console.error("Erreur lors de la récupération du contenu de la page d'accueil :", error);
  }
};

onMounted(() => {
  fetchHomePageContent();
});
</script>

<style scoped>
.absolute-left {
  position: absolute;
  left: 0;
}

.justify-center {
  justify-content: center;
}

/* Styles globaux ou spécifiques */
</style>
