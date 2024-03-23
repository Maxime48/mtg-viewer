<script setup>
import {onMounted, ref} from 'vue';
import {useRoute} from 'vue-router';
import {fetchCard} from '../services/cardService';
import CardProperty from '../components/CardProperty.vue';

const route = useRoute();
const card = ref({});
const loadingCard = ref(true);

async function loadCard(uuid) {
  loadingCard.value = true;
  card.value = await fetchCard(uuid);
  loadingCard.value = false;
}

onMounted(() => {
  loadCard(route.params.uuid);
});

</script>

<template>
  <div v-if="loadingCard" class="loading">Loading...</div>
  <div v-else>
    <div class="card">
      <h1>{{ card.name }}</h1>
      <CardProperty :value="card.manaCost" name="coût en mana"/>
      <pre class="card-property-text">{{ card.text }}</pre>
      <CardProperty :value="card.type" name="Type"/>
      <CardProperty :value="card.rarity" name="Rareté"/>
      <CardProperty :value="card.setCode" name="Édition"/>
    </div>
  </div>
  <div>
    <router-link :to="{ name: 'all-cards' }">Retourner à la liste complète</router-link>
  </div>
</template>
