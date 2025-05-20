<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">📰 Библиотечные новости</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink :to="`/news/new-${article.id}`" v-for="article in news" :key="article.id" class="flex flex-col gap-4 border border-gray-200 bg-white rounded-xl p-4 transition-all duration-500 hover:shadow-md hover:border-amber-300">
                <div class="flex items-center gap-2 text-sm text-gray-400">
                    <span>{{ new Date(article.date).toLocaleDateString() }}</span>
                    <span>•</span>
                    <span>{{ article.reading_time }}</span>
                </div>
                <p class="text-xl font-semibold font-mono">{{ article.title }}</p>
                <p class="text-gray-600 line-clamp-2 mt-auto">{{ article.content }}</p>
                <div class="flex items-center gap-2 flex-wrap">
                    <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm" v-for="tag in article.tags">{{ tag }}</p>
                </div>
            </NuxtLink>
        </div>
    </div>
   <!--  <div class="flex justify-center gap-2">
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
    </div> -->
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Новости',
    lang: 'ru'
})


/* подключение БД */
const supabase = useSupabaseClient()


/* получение данных */
const news = ref([])
const loadNews = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()

    news.value = data || []
}


/* первоначальная загрузка */
onMounted(() => {
    loadNews()
})
</script>