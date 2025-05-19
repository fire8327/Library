<template>
    <div class="flex max-lg:flex-col gap-6 w-full">
        <div class="flex flex-col gap-6 w-full lg:w-1/2 bg-white p-6 shadow-md rounded-xl border border-gray-200 h-fit">
            <img src="/images/about/main.jpg" alt="" class="w-full rounded-lg shadow-lg">
            <div class="flex items-center gap-2">
                <div class="w-3 h-3 rounded-full animate-pulse bg-green-500"></div>
                <p class="font-medium">Доступно 3 шт.</p>
            </div>
            <button class="w-full bg-amber-500 hover:opacity-60 text-white py-1.5 px-4 rounded-xl transition-all duration-500 font-medium">
                Забронировать
            </button>
        </div>
        <div class="flex flex-col gap-6 w-full lg:w-1/2 bg-white p-6 shadow-md rounded-xl border border-gray-200 h-fit">
            <p class="text-3xl font-semibold font-mono">Мастер и Маргарита</p>
            <p class="text-xl text-gray-500">Михаил Булгаков</p>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
                <div>
                    <p class="text-sm text-gray-500">Год издания</p>
                    <p class="font-medium">1967</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Язык</p>
                    <p class="font-medium">Русский</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Страниц</p>
                    <p class="font-medium">384</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Жанр</p>
                    <p class="font-medium">Роман, Классика</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Возраст</p>
                    <p class="font-medium">16+</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Вес</p>
                    <p class="font-medium">450 г</p>
                </div>
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-xl font-semibold font-mono">Описание</p>
                <div class="w-full h-px bg-gray-300"></div>
                <p class="text-gray-500">Философский роман о добре, зле и любви, действие которого разворачивается в Москве 1930-х годов и в древнем Иерусалиме. Произведение сочетает в себе сатиру, фантастику и глубокие размышления о природе человека.</p>   
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-xl font-semibold font-mono">Детали</p>
                <div class="w-full h-px bg-gray-300"></div>
                <p><span class="font-medium">ISBN:</span> 978-5-699-12345-6</p>
                <p><span class="font-medium">Издательство:</span> АСТ</p>
            </div>
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Ещё книги</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink to="/" class="flex flex-col rounded-xl overflow-hidden bg-white shadow-lg transition-all duration-500 border hover:border-amber-500 p-4 gap-4" v-for="n in 4">
                <img src="/images/about/main.jpg" alt="" class="w-full aspect-video object-cover rounded-lg">
                <span class="text-xl font-medium font-mono">Наименование</span>
                <span>Автор</span>
            </NuxtLink>
        </div>
    </div>
</template>

<script setup>

</script>