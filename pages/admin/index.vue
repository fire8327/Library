<template>
    <div class="flex flex-col gap-y">
        <p class="mainHeading">Добавление новости</p>
        <FormKit @submit="addNew" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
            <FormKit v-model="newForm.title" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Заголовок" name="Заголовок" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="newForm.content" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Контент" name="Контент" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="newForm.date" validation="required" messages-class="text-[#E9556D] font-mono" type="date" placeholder="Дата" name="Дата" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="newForm.reading_time" validation="required|number" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Мин чтения" name="Мин чтения" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            </div>
            <FormKit v-model="tagsInput" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Напишите теги через запятую" name="Теги" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <div class="flex flex-col gap-2 w-full md:w-2/3 lg:w-1/2" v-if="newForm.tags && newForm.tags.length > 0">
                    <p>Теги</p>
                    <div class="w-full flex items-center gap-2 flex-wrap">
                        <button type="button" @click="removeTag(index)" class="cursor-pointer flex items-center gap-2 py-1.5 px-4 rounded-xl bg-amber-500 text-white text-sm uppercase" v-for="(tag, index) in newForm.tags" :key="index">
                            <span>{{ tag }}</span>
                            <Icon class="text-2xl" name="material-symbols-light:close-small-rounded"/>
                        </button>
                    </div>    
                </div>
            <button :disabled="isNewDisabled" :class="{'opacity-50 cursor-not-allowed' : isNewDisabled}" type="submit" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Добавить</button>
        </FormKit>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Управление новостями</p>
        <div class="grid grid-cols-1 mg:grid-cols-2 lg:grid-cols-4 gap-6" v-if="news && news.length > 0">
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
        <p class="text-center text-xl font-semibold font-mono" v-else>Новостей нет</p>
    </div>
    <div class="flex flex-col gap-y">
        <p class="mainHeading">Добавление книги</p>
        <FormKit @submit="addBook" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
            <FormKit v-model="bookForm.title" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Наименование" name="Наименование" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="bookForm.description" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Описание" name="Описание" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="bookForm.pages" validation="required|number" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Кол-во страниц" name="Кол-во страниц" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="bookForm.weight" validation="required|number" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Вес" name="Вес" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="bookForm.quantity" validation="required|number" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Кол-во шт" name="Кол-во шт" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            </div>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="bookForm.year" validation="required|number" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Год выпуска" name="Год выпуска" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="bookForm.age_rating" validation="required" :options="['6+','12+','16+','18+']" messages-class="text-[#E9556D] font-mono" type="select" placeholder="Возрастной рейтинг" name="Возрастной рейтинг" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="bookForm.language" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Язык" name="Язык" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            </div>
            <FormKit v-model="bookForm.author" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Автор" name="Автор" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit @change="(e) => { coverFile = e.target.files[0] }" accept="image/*" validation="required" messages-class="text-[#E9556D] font-mono" type="file" placeholder="Обложка" name="Обложка" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="genresInput" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Напишите жанры через запятую" name="Жанры" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <div class="flex flex-col gap-2 w-full md:w-2/3 lg:w-1/2" v-if="bookForm.genre && bookForm.genre.length > 0">
                    <p>Жанры</p>
                    <div class="w-full flex items-center gap-2 flex-wrap">
                        <button type="button" @click="removeGenre(index)" class="cursor-pointer flex items-center gap-2 py-1.5 px-4 rounded-xl bg-amber-500 text-white text-sm uppercase" v-for="(genre, index) in bookForm.genre" :key="index">
                            <span>{{ genre }}</span>
                            <Icon class="text-2xl" name="material-symbols-light:close-small-rounded"/>
                        </button>
                    </div>    
                </div>
            <button :disabled="isBookDisabled" :class="{'opacity-50 cursor-not-allowed' : isBookDisabled}" type="submit" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Добавить</button>
        </FormKit>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Управление книгами</p>
        <div class="grid grid-cols-1 mg:grid-cols-2 lg:grid-cols-4 gap-6" v-if="books && books.length > 0">
            <div class="flex flex-col gap-4 rounded-xl p-4 shadow-lg bg-white" v-for="book in books">
                <div class="flex items-center justify-between">
                    <NuxtLink :to="`/catalog/book-${book.id}`" class="transition-all duration-500 hover:opacity-50">
                        <Icon class="text-3xl text-amber-500" name="material-symbols:eye-tracking-rounded"/>
                    </NuxtLink>
                    <button @click="deleteEntity('books', book.id)" class="transition-all duration-500 hover:opacity-50">
                        <Icon class="text-3xl text-red-500" name="material-symbols:delete-outline-rounded"/>
                    </button>
                </div>
                <img :src="getPublicFileUrl(book.image)" alt="" class="w-full aspect-[7/8] object-cover object-top rounded-lg">
                <p class="mt-auto"><span class="font-semibold font-mono">Наименовани :</span> {{ book.title }}</p>
                <p class="mt-auto line-clamp-2"><span class="font-semibold font-mono">Описание:</span> {{ book.description }}</p>
            </div>
        </div>
        <p class="text-center text-xl font-semibold font-mono" v-else>Книг нет</p>
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


/* форма новости и добавление */
const isNewDisabled = ref(false)
const newForm = ref({
    title: "",
    content: "",
    date: "",
    reading_time: null,
    tags: []
})

// добавление
const addNew = async() => {
    isNewDisabled.value = true
    newForm.value.date = new Date(newForm.value.date) 

    const { data, error } = await supabase
    .from('news')
    .insert(newForm.value)
    .select()

    if(data) {
        showMessage('Успешное добавление!', true)
        newForm.value.title = ""
        newForm.value.content = ""
        newForm.value.date = ""
        newForm.value.reading_time = null
        newForm.value.tags = []
        tagsInput.value = ""
        isNewDisabled.value = false
        loadNews()
    } else {
        showMessage('Произошла ошибка!', false)
    }
}


/* логика тегов */
const tagsInput = ref('')
const updateTag = () => {
    const tagsText = tagsInput.value.toLocaleLowerCase().trim()
    if (!tagsText) {
        newForm.value.tags = []
        return
    }

    newForm.value.tags = [
        ...new Set(
            tagsText
                .split(',')
                .map(s => s.trim())
                .filter(Boolean) // автоматически удаляет пустые строки
        )
    ]
}
watch(tagsInput, (newValue, oldValue) => {
    updateTag()
})

// удаление тегов
const removeTag = (index) => {
    newForm.value.tags.splice(index, 1)
    // обновляем textarea, чтобы оно соответствовало текущему списку тегов
    tagsInput.value = newForm.value.tags.join(', ')
}


/* получение новостей */
const news = ref([])
const loadNews = async() => {
    const { data, error } = await supabase
    .from('news')
    .select()
    .order('id', { ascending: false })

    if (error) throw error

    if (data) news.value = data 
}


/* форма книги и добавление */
const isBookDisabled = ref(false)
const bookForm = ref({
    title: "",
    description: "",
    language: "",
    year: null,
    genre: [],
    age_rating: "",
    pages: null,
    weight: null,
    image: "",
    author: "",
    quantity: null
})

//изборажение
const coverFile = ref(null)

// добавление
const addBook = async() => {
    isBookDisabled.value = true

    // изображение
    if (coverFile.value) {
        const file = coverFile.value
        const extension = file.name.split('.').pop()
        const fileName = `${Date.now()}-${Math.random().toString(36).substring(2, 7)}.${extension}`

        const { error: uploadError } = await supabase.storage
        .from('files/covers')
        .upload(`${fileName}`, file)

        if (uploadError) throw uploadError

        bookForm.value.image = fileName
    }

    const { data, error } = await supabase
    .from('books')
    .insert(bookForm.value)
    .select()

      if (data) {
        console.log(data)
        showMessage('Успешное добавление!', true)
        bookForm.value.title = ""
        bookForm.value.description = ""
        bookForm.value.language = ""
        bookForm.value.year = null
        bookForm.value.genre = []
        bookForm.value.age_rating = ""
        bookForm.value.pages = null
        bookForm.value.weight = null
        bookForm.value.image = ""
        bookForm.value.author = ""
        bookForm.value.quantity = null
        genresInput.value = ""
        loadBooks()
        isBookDisabled.value = false
    } else {     
        showMessage('Произошла ошибка!', false)
    }
}


/* логика жанров */
const genresInput = ref('')
const updateGenre = () => {
    const genresText = genresInput.value.toLocaleLowerCase().trim()
    if (!genresText) {
        bookForm.value.genre = []
        return
    }

    bookForm.value.genre = [
        ...new Set(
            genresText
                .split(',')
                .map(s => s.trim())
                .filter(Boolean) // автоматически удаляет пустые строки
        )
    ]
}
watch(genresInput, (newValue, oldValue) => {
    updateGenre()
})

// удаление жанров
const removeGenre = (index) => {
    bookForm.value.genre.splice(index, 1)
    // обновляем textarea, чтобы оно соответствовало текущему списку жанров
    genresInput.value = bookForm.value.genre.join(', ')
}


/* получение книг */
const books = ref([])
const loadBooks = async() => {
    const { data, error } = await supabase
    .from('books')
    .select()
    .order('id', { ascending: false })

    if (error) throw error

    if (data) books.value = data 
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


/* удаление сущности */
const deleteEntity = async(table, entityId) => {
    const { error } = await supabase
    .from(table)
    .delete()
    .eq('id', entityId)

    if(!error) {
        showMessage('Успешное удаление!', true)
        loadNews()
        loadBooks()
    } else {
        showMessage('Произошла ошибка!', false)
    }
          
}

/* первоначальная загрузка */
onMounted(() => {
    loadNews()
    loadBooks()
})
</script>