<template>
    <header class="py-4 w-full grid-container text-xl">
        <div class="flex items-center justify-between gap-6 p-4 rounded-xl shadow-[0px_0px_13px_-7px_black]">
            <NuxtLink to="/" class="flex items-center gap-4 transition-all duration-500 hover:opacity-65">
                <Icon class="text-3xl text-amber-500" name="ph:books-duotone"/>
                <span class="font-medium font-mono">Library</span>
            </NuxtLink>
            <div class="flex items-center gap-6 max-lg:flex-col max-lg:fixed max-lg:py-6 max-lg:w-full max-lg:bg-white max-lg:z-[6] max-lg:left-0 max-lg:rounded-t-xl transition-all duration-500" :class="isMenuShow ? 'max-lg:bottom-0' : 'max-lg:top-full max-lg:translate-y-full'">
                <NuxtLink to="/" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Главная</NuxtLink>
                <NuxtLink to="/catalog" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Каталог</NuxtLink>
                <NuxtLink to="/about" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">О нас</NuxtLink>
                <NuxtLink to="/news" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Новости</NuxtLink>
                <NuxtLink v-if="role === 'admin'" to="/admin" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Админ-панель</NuxtLink>
                <NuxtLink to="/auth" class="transition-all duration-500 hover:opacity-65 flex">
                    <Icon class="text-3xl text-amber-500" name="material-symbols:person"/>
                </NuxtLink>
            </div>
            <button class="lg:hidden flex" @click="isMenuShow = !isMenuShow">
                <Icon class="text-3xl text-amber-500" name="ph:list-bold"/>         
            </button>
        </div>
        <div @click="isMenuShow = false" :class="{'max-lg:hidden' : !isMenuShow}" class="inset-0 fixed bg-black/30 lg:hidden max-lg:z-[5]"></div>
        <button type="button" @click="messageTitle = null" class="fixed top-10 right-10 z-[11] cursor-pointer flex items-center gap-2 px-6 py-2 text-lg rounded-xl w-fit font-medium font-mono bg-white border border-[#131313]/10 shadow-[0px_0px_13px_-7px_#131313]" :class="messageType ? ' text-[#131313]/80' : 'text-red-500'" v-if="messageTitle">
            <Icon class="text-3xl" name="material-symbols:close-small-rounded"/>
            <span>{{messageTitle}}</span>
        </button>
    </header>
</template>

<script setup>
/* мобильное меню */
const isMenuShow = ref(false)


/* закрытие мобильного меню */
const nuxtApp = useNuxtApp()
nuxtApp.hook('page:start', () => {
    isMenuShow.value = false
})


/* создание сообщений */
const { messageTitle, messageType } = storeToRefs(useMessagesStore())


/* хранилище пользователей */
const userStore = useUserStore()
</script>