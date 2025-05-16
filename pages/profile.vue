<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Личные данные</p>
        <FormKit type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="userForm.surname" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Фамилия" name="Фамилия" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="userForm.name" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Имя" name="Имя" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="userForm.patronymic" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Отчество" name="Отчество" outer-class="w-full lg:w-1/3" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            </div>
            <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
                <FormKit v-model="userForm.login" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Логин" name="Логин" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
                <FormKit v-model="userForm.password" validation="required|length:6" messages-class="text-[#E9556D] font-mono" type="password" placeholder="······" name="Пароль" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            </div>
            <FormKit v-model="userForm.phone" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Телефон" name="Телефон" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="userForm.email" validation="required|email" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Email" name="Email" outer-class="w-full md:w-2/3 lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <button @click="handleSave" :disabled="isLoading" :class="{ 'opacity-50 cursor-not-allowed': isLoading }" type="button" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Сохранить</button>
        </FormKit>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Выход из аккаунта</p>
        <button @click="logout" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Выйти</button>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Профиль',
    lang: 'ru'
})


/* проверка роли и создание сообщений */
const userStore = useUserStore()
const { id:userId, role, logout } = useUserStore()
const { showMessage } = useMessagesStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* загрузка */
const isLoading = ref(false)


/* форма пользователя */
const userForm = ref({
    name: '',
    surname: '',
    patronymic: '',
    login: '',
    password: '',
    phone: '',
    email: ''
})


/* данные профиля и их загрузка */
const loadProfileData = async () => {
    const { data, error } = await supabase
    .from('users')
    .select()
    .eq('id', userId)
    .single()

    if (error) throw error

    if (data) userForm.value = data 
}


/* сохранение профиля */
const saveProfile = async () => {
    isLoading.value = true
    try {
        const { error } = await supabase
        .from('users')
        .update(userForm.value)
        .eq('id', userId)

        if (error) throw error

        await loadProfileData()
        showMessage('Профиль обновлён!', true)
    } catch (error) {
        showMessage('Ошибка при сохранении: ' + error.message, false)
    } finally {
        isLoading.value = false
    }
}


/* подтверждение сохранения */
const saveClickCount = ref(0)

const handleSave = () => {
  saveClickCount.value++
  
  if (saveClickCount.value === 1) {
    showMessage('Нажмите еще раз для подтверждения', true)
    setTimeout(() => saveClickCount.value = 0, 5000)
  } else if (saveClickCount.value >= 2) {
    saveClickCount.value = 0
    saveProfile()
  }
}


/* первоначальная загрузка */
onMounted(() => {
    loadProfileData()
})
</script>