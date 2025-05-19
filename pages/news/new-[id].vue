<template>
    <div class="flex flex-col gap-6 bg-white overflow-hidden rounded-xl p-4 h-fit">
        <div class="flex flex-wrap gap-4 items-center text-sm text-gray-400">
            <span>15 октября 2023</span>
            <span>•</span>
            <span>5 мин чтения</span>
            <span>•</span>
           <div class="flex items-center gap-2 flex-wrap">
                <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm">События</p>
                <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm">Дети</p>
            </div>
        </div>
        <p class="text-3xl font-semibold font-mono">Открытие нового детского зала в центральном филиале</p>
        <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Omnis, beatae tempora! Dolorum dolorem molestiae iure itaque ipsam corporis suscipit provident quo, doloremque quae, natus sunt! Repellat eaque natus deserunt expedita.</p>
        <div class="w-full h-px bg-gray-300"></div>
        <div class="flex items-center gap-4">
            <p class="text-gray-500 font-medium">Поделиться:</p>
            <button>
                <Icon class="text-3xl" name="logos:telegram"/>
            </button>
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Другие новости</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink to="/news/new-1" class="flex flex-col gap-4 border border-gray-200 bg-white rounded-xl p-4 transition-all duration-500 hover:shadow-md hover:border-amber-300" v-for="n in 4">
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
</template>

<script setup>

</script>