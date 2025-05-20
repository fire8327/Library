<template>
    <div class="flex flex-col gap-6 w-full rounded-xl py-12 px-6 bg-gradient-to-r from-yellow-500 to-amber-600 text-white h-fit">
        <p class="text-3xl font-semibold font-mono">Добро пожаловать в цифровую библиотеку!</p>
        <p class="text-xl">Откройте новую любимую книгу среди бесконечного мира историй, собранных в нашей библиотеке.</p>
        <FormKit type="form" :actions="false" messages-class="hidden" form-class="flex gap-2 items-start">
            <FormKit validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Название, автор, жанр" name="Поиск" outer-class="text-[#131313] w-full" input-class="focus:outline-none px-4 py-1.5 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <button type="submit" class="cursor-pointer py-1.5 px-4 rounded-xl bg-white w-fit transition-all duration-500 hover:opacity-60 text-[#131313] shadow-[0px_0px_13px_-7px_black]">Искать</button>
        </FormKit>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
        <NuxtLink to="/" class="bg-white transition-all duration-500 rounded-xl p-4 border border-gray-200 shadow-[0px_0px_13px_-7px_black] hover:shadow-none flex flex-col gap-2">
            <p class="text-3xl">🔍</p>
            <p class="text-xl font-mono font-medium">Каталог</p>
            <p class="text-sm text-gray-500">Все книги в одном месте</p>
        </NuxtLink>
        <NuxtLink to="/profile" class="bg-white transition-all duration-500 rounded-xl p-4 border border-gray-200 shadow-[0px_0px_13px_-7px_black] hover:shadow-none flex flex-col gap-2">
            <p class="text-3xl">👤</p>
            <p class="text-xl font-mono font-medium">Профиль</p>
            <p class="text-sm text-gray-500">Ваши книги и настройки</p>
        </NuxtLink>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">📰 Библиотечные новости</p>
        <NuxtLink :to="`/news/new-${ article.id }`" class="flex flex-col gap-4 border-b border-gray-400 p-4 transition-all duration-500 hover:opacity-50" v-for="article in news.slice(0,6)">
            <p class="text-xl font-semibold font-mono">{{ article.title }}</p>
            <p class="text-sm text-gray-500 line-clamp-2">{{ article.content }}</p>
        </NuxtLink>
        <NuxtLink to="/news" class="bg-amber-600 transition-all duration-500 w-fit text-white px-6 py-2 rounded-xl hover:opacity-60 self-end">
            Ещё новости
        </NuxtLink>
    </div>
    <div class="bg-green-50 p-4 rounded-xl w-full text-center border border-green-200 shadow-sm flex flex-col items-center gap-4">
        <p class="mainHeading">🎲 Не знаете, что почитать?</p>
        <p>Позвольте нам выбрать новость для вас</p> <!-- или книгу -->
        <button @click="goToRandomArticle" class="bg-green-600 transition-all duration-500 w-fit text-white px-6 py-2 rounded-xl hover:opacity-60">
            Крутить барабан
        </button>
    </div>
    
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Главная',
    lang: 'ru'
})


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* случайная новость */
const randomId = ref(null)
const newsLength = ref(null)

const goToRandomArticle = () => {
    randomId.value = Math.floor(Math.random() * (newsLength.value - 1) + 1)
    router.push(`/news/new-${randomId.value}`)
}


/* получение данных */
const news = ref([])
const loadNews = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()

    if(data) {
        news.value = data
        newsLength.value = data.length
    }
}


/* первоначальная загрузка */
onMounted(() => {
    loadNews()
})
</script>