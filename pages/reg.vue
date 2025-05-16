<template>
    <FormKit @submit="regUser" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
        <p class="mainHeading">Регистрация</p>
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
        <button :disabled="isRegDisabled" :class="{ 'opacity-50 cursor-not-allowed': isRegDisabled }" type="submit" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Регистрация</button>
        <div class="flex items-center justify-center gap-4 w-full md:w-2/3 lg:w-1/2 my-10">
            <div class="w-1/3 h-px bg-amber-500"></div>  
            <p class="font-semibold font-mono tracking-widest text-amber-500">или</p>
            <div class="w-1/3 h-px bg-amber-500"></div>  
        </div>
        <NuxtLink to="/auth" class="mx-auto px-4 py-2 border border-amber-500 text-amber-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-amber-500">Вход</NuxtLink>
    </FormKit>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Регистрация',
    lang: 'ru'
})


/* создание пользователя */
const userForm = ref({
    surname: "",
    name: "",
    patronymic: "",
    login: "",
    password: "",
    phone: "",
    email: ""
})


/* создание сообщений */
const { showMessage } = useMessagesStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* регистрация пользователя */
const isRegDisabled = ref(false)
const regUser = async () => {
    isRegDisabled.value = true
    const { data: user, error: usersError } = await supabase
    .from('users')
    .select("*")
    .eq('login', userForm.value.login)
    .single()

    if (user) {
        userForm.value.login = ""
        isRegDisabled.value = false
        return showMessage("Такой  логин уже используется!", false)
    } 

    const { data, error } = await supabase
    .from('users')
    .insert(userForm.value)
    .select()

    if (data) {
        console.log(data)
        showMessage('Успешная регистрация!', true)
        isRegDisabled.value = false
        router.push('/auth')
    } else {
        console.log(userForm.value)            
        isRegDisabled.value = false
        showMessage('Произошла ошибка!', false)
    }
}
</script>