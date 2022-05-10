<script lang="ts" setup>

type Card = {
  value: number | string,
  type: string
}

defineProps({
    card: {
        type: Object as () => Card,
        required: true
    }
})

</script>



<template>
    
    <div class="rounded-lg red  h-[14em] w-[10em] border-2 border-dark-600 relative grid grid-cols-3 grid-rows-3">

    <div class="text-dark-600 col-start-1 row-start-1 flex items-center justify-center font-semibold text-4xl">
        {{ card.value}}
    </div>
    <div class=" col-start-2 row-start-2 flex items-center justify-center"> 
        <img style="transform: scale(1.3);" :src="`/${card.type}.svg`" :alt="card.type">
    </div>

    </div>
</template>


<style>
    .red {
        filter: invert(40%) sepia(88%) saturate(1636%) hue-rotate(340deg) brightness(100%) contrast(102%);
    }
</style>