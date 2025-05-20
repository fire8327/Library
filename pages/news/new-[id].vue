<template>
    <div class="flex flex-col gap-6">
        <button @click="goBack" class="bg-amber-600 transition-all duration-500 w-fit text-white px-6 py-2 rounded-xl hover:opacity-60">
            Назад
        </button>
        <div class="flex flex-col gap-6 bg-white overflow-hidden rounded-xl p-4 h-fit">
            <div class="flex flex-wrap gap-4 items-center text-sm text-gray-400">
                <span>{{ new Date(article?.date).toLocaleDateString() }}</span>
                <span>•</span>
                <span>{{ article?.reading_time }}</span>
                <span>•</span>
               <div class="flex items-center gap-2 flex-wrap">
                    <p class="py-1 px-4 bg-gray-100 text-gray-500 rounded-full text-sm" v-for="tag in article?.tags">{{ tag }}</p>
                </div>
            </div>
            <p class="text-3xl font-semibold font-mono">{{ article?.title }}</p>
            <p>{{ article?.content }}</p>
            <div class="w-full h-px bg-gray-300"></div>
            <div class="flex items-center gap-4">
                <p class="text-gray-500 font-medium">Поделиться:</p>
                <a :href="`https://t.me/share/url?url=${encodeURIComponent(currentUrl)}&text=${encodeURIComponent(title)}`"
                target="_blank"
                rel="noopener noreferrer">
                    <Icon class="text-3xl" name="logos:telegram"/>
                </a>
            </div>
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Другие новости</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink :to="`/news/new-${article.id}`" v-for="article in randomNews" :key="article.id" class="flex flex-col gap-4 border border-gray-200 bg-white rounded-xl p-4 transition-all duration-500 hover:shadow-md hover:border-amber-300">
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
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Страница новости',
    lang: 'ru'
})


/* подключение БД и роута */
const supabase = useSupabaseClient()
const route = useRoute()


/* функция возвращения назад */
const router = useRouter()
const goBack = () => {
  if (window.history.length > 1) {
    router.back()
  } else {
    router.push('/')
  }
}


/* поделиться */
const currentUrl = `https://delicate-starship-0a7654.netlify.app${route.fullPath}`
const title = ref("")


/* получение данных */
const article = ref(null)
const loadArticle = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()
    .eq('id', route.params.id)
    .single()

    article.value = data || null
    title.value = data?.title || ""
}


/* случайные 4 новости */
const randomNews = ref([])
const loadRandomNews = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()
    .order('id', { ascending: Math.random() > 0.5 })
    .limit(4)

    randomNews.value = data || []
}


/* первоначальная загрузка */
onMounted(() => {
    loadArticle()
    loadRandomNews()
})
</script>