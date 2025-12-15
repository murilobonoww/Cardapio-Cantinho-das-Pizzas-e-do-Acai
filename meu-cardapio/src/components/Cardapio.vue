<script setup>
import { ref } from 'vue'
import axios from 'axios';
import { onMounted } from 'vue';

defineProps({
  msg: String,
})

const cardapio = ref({});

const getMenu = async () => {
  try {
    const res = await axios.get('https://back-cantinho-das-pizzas.onrender.com/cardapio');
    console.log(res.data);
    cardapio.value = res.data;
  } catch (error) {
    console.error('Erro ao consultar o cardápio:', error);
    throw error;
  }
}

onMounted(() => {
  getMenu();
})

const scrollToCategoria = (categoria) => {
  const el = document.getElementById(`secao-${categoria}`)
  if (el) {
    el.scrollIntoView({
      behavior: 'smooth',
      block: 'start'
    })
  }
}


</script>

<template>
  <div class="w-screen min-h-screen h-full justify-center items-center text-center p-10 bg-[#000000]">
    <img id="pizza_landing_1" src="/pizza.png">
    <img id="pizza_group_1" src="/pizza_group_1.png">
    <img id="pizza_group_2" src="/pizza_group_2.png">
    <img id="pizza_group_3" src="/pizza_group_3.png">
    <h1 id="title" class="text-[#ff3635]">Cantinho das Pizzas e do Açaí</h1>
    <h2 id="subTitle">Seja bem-vindo!</h2>
    <h2 id="lastTitle" class="text-[#f8b400]">Confira nosso cardápio</h2>



    <div flex flex-col text-center items-center w-full>
      <!-- carregamento -->
      <div class="text-2xl" v-if="Object.keys(cardapio).length === 0">
        Carregando...
      </div>

      <div id="categorias_nav" v-else class="flex gap-[clamp(0px,2vw,40px)] text-center justify-center mt-10 mb-10">
        <div v-for="categoria in Object.keys(cardapio)" :key="categoria">
          <h2
            class="categorias cursor-pointer hover:underline decoration-solid decoration-2 decoration-white hover:scale-110 transition-all 0.3s"
            @click="scrollToCategoria(categoria)">
            {{ categoria }}
          </h2>

        </div>
      </div>

      <!-- container do card do menu -->
      <div class="text-center flex justify-center itens-center">
        <!-- card do menu -->
        <div class="bg-[rgba(23,23,23,0.3)] rounded-2xl w-150" id="menu">
          <div v-for="categoria in Object.keys(cardapio)" :key="categoria">
            <h2 :id="`secao-${categoria}`" class="menu_categoria">{{ categoria }}</h2>
            <ul id="menu_elements" class="mb-15">
              <li class="border-b border-solid border-[rgba(137,137,137,0.5)] hover:bg-[rgba(83,83,83,0.3)]"
                v-for="item in cardapio[categoria]" :key="item.sabor ?? item.nome">
                <!-- Pizzas e esfihas -->


                <template v-if="categoria === 'pizzas'">
                  {{ item.sabor }} - R$ {{ item.preco_25 }} | R$ {{ item.preco_35 }}
                </template>

                <template v-if="categoria === 'esfihas'">
                  {{ item.sabor }} - R$ {{ item.preco }}
                </template>

                <!-- Outras categorias -->
                <template v-else>
                  {{ item.nome }} - R$ {{ item.preco }}
                </template>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
