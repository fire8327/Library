<template>
    <div class="flex flex-col gap-y">
        <p class="mainHeading">Добавление новости</p>
        <FormKit v-model="resetNewForm" @submit="addNew" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
            <FormKit v-model="newForm.title" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Заголовок" name="Заголовок" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="newForm.content" validation="required" messages-class="text-[#E9556D] font-mono" type="textarea" placeholder="Контент" name="Контент" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="newForm.date" validation="required" messages-class="text-[#E9556D] font-mono" type="date" placeholder="Дата" name="Дата" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="newForm.reading_time" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Мин чтения" name="Мин чтения" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
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
            <button type="submit" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Добавить</button>
        </FormKit>
    </div>
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


/* форма новости и добавление */
const resetNewForm = ref()
const newForm = ref({
    title: "",
    content: "",
    date: "",
    reading_time: null,
    tags: []
})

// добавление
const addNew = async() => {
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
        loadNews()
    } else {
        showMessage('Произошла ошибка!', false)
    }
}


/* логика навыков */
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

// удаление навыков
const removeTag = (index) => {
    newForm.value.tags.splice(index, 1)
    // обновляем textarea, чтобы оно соответствовало текущему списку навыков
    tagsInput.value = newForm.value.tags.join(', ')
}



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