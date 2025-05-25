<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Управление новостями</p>
        <div class="grid grid-cols-1 mg:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col gap-4 rounded-xl p-4 shadow-lg bg-white" v-for="article in news">
                <div class="flex items-center justify-between">
                    <NuxtLink :to="`/news/new-${article.id}`" class="transition-all duration-500 hover:opacity-50">
                        <Icon class="text-3xl text-amber-500" name="material-symbols:eye-tracking-rounded"/>
                    </NuxtLink>
                    <button @click="deleteEntity('news', article.id)" class="transition-all duration-500 hover:opacity-50">
                        <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline-rounded"/>
                    </button>
                </div>
                <p class="mt-auto line-clamp-2"><span class="font-semibold font-mono">Заголовок:</span> {{ article.title }}</p>
                <p class="line-clamp-2"><span class="font-semibold font-mono">Описание:</span> {{ article.content }}</p>
                <p><span class="font-semibold font-mono">Дата:</span> {{ new Date(article.date).toLocaleDateString() }}</p>
            </div>
        </div>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Админ-панель',
    lang: 'ru'
})


/* проверка роли и создание сообщений */
const userStore = useUserStore()
const { id:userId, role, logout } = useUserStore()
const { showMessage } = useMessagesStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()


/* получение новостей */
const news = ref([])
const loadNews = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()

    if (error) throw error

    if (data) news.value = data 
}


/* удаление новости */
const deleteEntity = async(table, entityId) => {
    const { error } = await supabase
    .from(table)
    .delete()
    .eq('id', entityId)

    if(!error) {
        showMessage('Успешное удаление!', true)
        loadNews()
    } else {
        showMessage('Произошла ошибка!', false)
    }
          
}


/* первоначальная загрузка */
onMounted(() => {
    loadNews()
})
</script>