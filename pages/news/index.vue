<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">📰 Библиотечные новости</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink to="/news/new-1" class="flex flex-col gap-4 border border-gray-200 bg-white rounded-xl p-4 transition-all duration-500 hover:shadow-md hover:border-amber-300" v-for="n in 6">
                <div class="flex items-center gap-2 text-sm text-gray-400">
                    <span>15 октября 2023</span>
                    <span>•</span>
                    <span>3 мин чтения</span>
                </div>
                <p class="text-xl font-semibold font-mono">Открытие нового детского зала</p>
                <p class="text-gray-600 line-clamp-2">В центральном филиале появилось современное пространство для юных читателей с интерактивными книгами и зоной творчества.</p>
                <div class="flex items-center gap-2 flex-wrap">
                    <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm">События</p>
                    <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm">Дети</p>
                </div>
            </NuxtLink>
        </div>
    </div>
    <div class="flex justify-center gap-2">
        <button
            class="w-10 h-10 flex items-center justify-center border border-gray-300 rounded-lg hover:bg-gray-100">
            &lt;
        </button>
        <button class="w-10 h-10 flex items-center justify-center bg-amber-500 text-white rounded-lg">
            1
        </button>
        <button
            class="w-10 h-10 flex items-center justify-center border border-gray-300 rounded-lg hover:bg-gray-100">
            2
        </button>
        <button
            class="w-10 h-10 flex items-center justify-center border border-gray-300 rounded-lg hover:bg-gray-100">
            &gt;
        </button>
    </div>
</template>

<script setup>

</script>