<template>
    <header class="py-4 w-full grid-container text-xl">
        <div class="flex items-center justify-between gap-6 p-4 rounded-xl shadow-[0px_0px_13px_-7px_black]">
            <NuxtLink to="/" class="flex items-center gap-4 transition-all duration-500 hover:opacity-65">
                <Icon class="text-3xl text-amber-500" name="ph:books-duotone"/>
                <span class="font-medium font-mono">Library</span>
            </NuxtLink>
            <div class="flex items-center gap-6">
                <NuxtLink to="/" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Главная</NuxtLink>
                <NuxtLink to="/" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">Каталог</NuxtLink>
                <NuxtLink to="/" class="flex flex-col after:w-0 after:h-px after:bg-amber-500 after:transition-all after:duration-500 hover:after:w-full">О нас</NuxtLink>
                <NuxtLink to="/" class="transition-all duration-500 hover:opacity-65 flex">
                    <Icon class="text-3xl text-amber-500" name="material-symbols:person"/>
                </NuxtLink>
            </div>
        </div>
    </header>
</template>

<script setup>

</script>