<template>
    <div class="flex max-lg:flex-col gap-6 w-full">
        <div class="flex flex-col gap-6 w-full lg:w-1/2 bg-white p-6 shadow-md rounded-xl border border-gray-200 h-fit">
            <img src="/images/about/main.jpg" alt="" class="w-full rounded-lg shadow-lg">
            <div class="flex items-center gap-2">
                <div class="w-3 h-3 rounded-full animate-pulse bg-green-500"></div>
                <p class="font-medium">Доступно {{ book?.quantity }} шт.</p>
            </div>
            <button class="w-full bg-amber-500 hover:opacity-60 text-white py-1.5 px-4 rounded-xl transition-all duration-500 font-medium">
                Забронировать
            </button>
        </div>
        <div class="flex flex-col gap-6 w-full lg:w-1/2 bg-white p-6 shadow-md rounded-xl border border-gray-200 h-fit">
            <button @click="goBack" class="w-fit self-end bg-amber-500 hover:opacity-60 text-white py-1.5 px-4 rounded-xl transition-all duration-500 font-medium">Назад</button>
            <p class="text-3xl font-semibold font-mono">{{ book?.title }}</p>
            <p class="text-xl text-gray-500">{{ book?.author }}</p>
            <div class="grid grid-cols-2 md:grid-cols-3 gap-4">
                <div>
                    <p class="text-sm text-gray-500">Год издания</p>
                    <p class="font-medium">{{ book?.year }}</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Язык</p>
                    <p class="font-medium">{{ book?.language }}</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Страниц</p>
                    <p class="font-medium">{{ book?.pages }}</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Жанр</p>
                    <p class="font-medium">Роман, Классика</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Возраст</p>
                    <p class="font-medium">{{ book?.age_rating }}</p>
                </div>
                <div>
                    <p class="text-sm text-gray-500">Вес</p>
                    <p class="font-medium">{{ book?.weight }} г</p>
                </div>
            </div>
            <div class="flex flex-col gap-2">
                <p class="text-xl font-semibold font-mono">Описание</p>
                <div class="w-full h-px bg-gray-300"></div>
                <p class="text-gray-500">{{ book?.description }}</p>   
            </div>
            <!-- <div class="flex flex-col gap-2">
                <p class="text-xl font-semibold font-mono">Детали</p>
                <div class="w-full h-px bg-gray-300"></div>
                <p><span class="font-medium">ISBN:</span> 978-5-699-12345-6</p>
                <p><span class="font-medium">Издательство:</span> АСТ</p>
            </div> -->
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Ещё книги</p>
        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
            <NuxtLink :to="`/catalog/book-${book.id}`" class="flex flex-col rounded-xl overflow-hidden bg-white shadow-lg transition-all duration-500 border hover:border-amber-500 p-4 gap-4" v-for="book in randomBooks">
                <img src="/images/about/main.jpg" alt="" class="w-full aspect-video object-cover rounded-lg">
                <span class="text-xl font-medium font-mono">{{ book.title }}</span>
                <span class="mt-auto">{{ book.author }}</span>
            </NuxtLink>
        </div>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Страница книги',
    lang: 'ru'
})


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* подключение роута */
const route = useRoute()


/* получение данных */
const book = ref(null)
const loadBook = async() => {
    const { data, error } = await supabase
    .from('books')
    .select()
    .eq('id', route.params.id)
    .single()

    book.value = data || null
}


/* функция возвращения назад */
const goBack = () => {
  if (window.history.length > 1) {
    router.back()
  } else {
    router.push('/')
  }
}


/* случайные книги */
const randomBooks = ref([])
const loadRandomBooks = async() => {
    const { data, error } = await supabase
    .from('books')
    .select()
    .order('id', { ascending: Math.random() > 0.5 })
    .limit(4)

    randomBooks.value = data || []
}


/* первоначальная загрузка */
onMounted(() => {
    loadBook()
    loadRandomBooks()
})
</script>