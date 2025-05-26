<template>
    <div class="flex flex-col gap-6">
        <div class="flex lg:items-center lg:justify-between max-lg:flex-col w-full gap-4">
            <p class="mainHeading">Каталог</p>
            <button v-if="searchQuery && searchQuery.length > 0" @click="clearSearchQuery" type="button" class="px-4 py-1.5 border border-amber-500 bg-amber-500 text-white rounded-full w-fit text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Показать всё</button>
        </div>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <div class="flex flex-col rounded-xl overflow-hidden bg-white shadow-lg" v-for="book in books">
                <div class="relative w-full">
                    <img :src="getPublicFileUrl(book.image)" alt="" class="w-full aspect-[7/8] object-cover object-top">
                    <p class="absolute top-4 right-4 rounded-xl bg-sky-500 text-white py-1 px-4 text-sm font-medium">{{ book.genre[0] }}</p>
                </div>
                <div class="flex flex-col gap-4 p-4 grow">
                    <p class="text-xl font-medium font-mono">{{ book.title }}</p>
                    <p class="mt-auto">{{ book.author }}</p>
                    <p>Количество страниц: {{ book.pages }}</p>
                    <NuxtLink :to="`/catalog/book-${book.id}`" class="py-1 px-4 w-full transition-all duration-500 rounded-xl text-center bg-amber-500 text-white border border-amber-500 hover:text-amber-500 hover:bg-transparent">Подробнее</NuxtLink>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Каталог',
    lang: 'ru'
})


/* подключение БД и хранилища */
const supabase = useSupabaseClient()
const { searchQuery } = storeToRefs(useSearchStore())


/* получение данных */
const books = ref([])
const loadBooks = async() => {
    let query = supabase.from('books').select()

    if(searchQuery.value) {
        query = query.or(`title.ilike.%${searchQuery.value}%,author.ilike.%${searchQuery.value}%`)
    }

    const { data, error } = await query

    books.value = data || []
}

// отмена поиска
const clearSearchQuery = () => {
    searchQuery.value = ''
    loadBooks()
}


/* получение url */
const getPublicFileUrl = (filePath, bucket = 'files/covers') => {
    if (!filePath) return ''

    // Получаем публичный URL из Supabase Storage
    const { data } = supabase
    .storage
    .from(bucket)
    .getPublicUrl(filePath)

    return data.publicUrl
}


/* первоначальная загрузка */
onMounted(() => {
    loadBooks()
})
</script>