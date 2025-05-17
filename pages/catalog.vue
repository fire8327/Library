<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Каталог</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col rounded-xl overflow-hidden bg-white shadow-lg" v-for="n in 10">
                <div class="relative w-full">
                    <img src="/images/about/main.jpg" alt="" class="w-full aspect-video object-cover">
                    <p class="absolute top-4 right-4 rounded-xl bg-sky-500 text-white py-1 px-4 text-sm font-medium">Жанр</p>
                </div>
                <div class="flex flex-col gap-4 p-4">
                    <p class="text-xl font-medium font-mono">Наименование</p>
                    <p>Автор</p>
                    <p>Количество страниц: 123</p>
                    <NuxtLink to="/" class="py-1 px-4 w-full transition-all duration-500 rounded-xl text-center bg-amber-500 text-white border border-amber-500 hover:text-amber-500 hover:bg-transparent">Подробнее</NuxtLink>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>

</script>