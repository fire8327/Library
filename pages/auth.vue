<template>
    <FormKit @submit="authUser" type="form" :actions="false" messages-class="hidden" form-class="flex flex-col gap-6 items-center justify-center grow">
        <p class="mainHeading">Вход</p>
        <div class="flex items-center lg:items-start gap-4 max-lg:flex-col w-full md:w-2/3 lg:w-1/2">
            <FormKit v-model="userForm.login" validation="required" messages-class="text-[#E9556D] font-mono" type="text" placeholder="Логин" name="Логин" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
            <FormKit v-model="userForm.password" validation="required|length:6" messages-class="text-[#E9556D] font-mono" type="password" placeholder="······" name="Пароль" outer-class="w-full lg:w-1/2" input-class="focus:outline-none px-4 py-2 bg-white rounded-xl border border-transparent w-full transition-all duration-500 focus:border-amber-500 shadow-md"/>
        </div>
        <button type="submit" :disabled="isAuthDisabled" :class="{ 'opacity-50 cursor-not-allowed': isAuthDisabled }" class="px-4 py-2 border border-amber-500 bg-amber-500 text-white rounded-full w-[160px] text-center transition-all duration-500 hover:text-amber-500 hover:bg-transparent">Вход</button>
        <div class="flex items-center justify-center gap-4 w-full md:w-2/3 lg:w-1/2 my-10">
            <div class="w-1/3 h-px bg-amber-500"></div>  
            <p class="font-semibold font-mono tracking-widest text-amber-500">или</p>
            <div class="w-1/3 h-px bg-amber-500"></div>  
        </div>
        <NuxtLink to="/reg" class="mx-auto px-4 py-2 border border-amber-500 text-amber-500 rounded-full w-[160px] text-center transition-all duration-500 hover:text-white hover:bg-amber-500">Регистрация</NuxtLink>
    </FormKit>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'Вход',
    lang: 'ru'
})


/* создание пользователя */
const userForm = ref({
    login: "",
    password: ""
})


/* создание сообщений и подключение хранилищ */
const { showMessage } = useMessagesStore()
const { login, loadProfileData } = useUserStore()


/* подключение БД и роутера */
const supabase = useSupabaseClient()
const router = useRouter()


/* вход */
const isAuthDisabled = ref(false)
const authUser = async() => {  
    isAuthDisabled.value = true
    const { data: user, error } = await supabase
    .from('users')
    .select("*")
    .eq('login', userForm.value.login)
    .single()

    if (!user) {
        userForm.value.login = ""
        isAuthDisabled.value = false
        return showMessage("Неверный логин!", false)              
    }

    if (userForm.value.password !== user.password) {
        userForm.value.password = ""
        isAuthDisabled.value = false
        return showMessage('Неверно введен пароль!', false)            
    }

    showMessage('Успешный вход!', true)
    login(user.id, user.role)
    isAuthDisabled.value = false

    if(user.role === 'user') {
        router.push('/profile')
    }
    if(user.role === 'admin') {
        router.push('/admin')
    }
} 
</script>