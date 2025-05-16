<template>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">О компании</p>
        <div class="flex max-lg:flex-col items-center gap-6">
            <p class="w-full lg:w-1/2 text-xl font-medium">Мы создаем современную, удобную и технологичную библиотечную систему, которая объединяет читателей, книги и библиотекарей в едином цифровом пространстве. Наша цель — сделать доступ к знаниям простым, быстрым и комфортным для каждого.</p>
            <img src="/images/about/main.jpg" alt="" class="rounded-xl w-full lg:w-1/2">
        </div>
    </div>
    <div class="flex flex-col gap-6">
        <p class="mainHeading">Наша философия</p>
        <div class="flex flex-col gap-4">
            <div class="flex flex-col gap-2 p-4 rounded-xl bg-white border border-gray-300 shadow-md transition-all duration-500 hover:-translate-y-2">
                <p class="text-xl font-mono font-semibold text-[#131313]/80">📖 Книги в цифровую эпоху</p>
                <p>Мы объединяем традиционные библиотечные ценности с современными технологиями</p>
            </div>
            <div class="flex flex-col gap-2 p-4 rounded-xl bg-white border border-gray-300 shadow-md transition-all duration-500 hover:-translate-y-2">
                <p class="text-xl font-mono font-semibold text-[#131313]/80">⚡ Быстрый доступ к знаниям</p>
                <p>Наша система позволяет находить нужные книги за секунды из любой точки города</p>
            </div>
            <div class="flex flex-col gap-2 p-4 rounded-xl bg-white border border-gray-300 shadow-md transition-all duration-500 hover:-translate-y-2">
                <p class="text-xl font-mono font-semibold text-[#131313]/80">🔍 Умный поиск</p>
                <p>Расширенные фильтры помогают находить литературу по любым критериям</p>
            </div>
            <div class="flex flex-col gap-2 p-4 rounded-xl bg-white border border-gray-300 shadow-md transition-all duration-500 hover:-translate-y-2">
                <p class="text-xl font-mono font-semibold text-[#131313]/80">🤝 Сообщество читателей</p>
                <p>Мы создаем цифровое пространство для обмена мнениями и рекомендациями</p>
            </div>
        </div>
    </div>
    <div class="relative flex flex-col gap-6">
        <iframe class="w-full h-[400px] rounded-xl border border-gray-300 shadow-md lg:h-[500px]"
            src="https://yandex.ru/map-widget/v1/?um=constructor%3Ab5b71122d59bf321fa1fb00171146b95ba02bcfc87108b9fa87ff29b94af771d&source=constructor"
            frameborder="0"></iframe>
        <div
            class="flex flex-col gap-4 lg:w-3/4 lg:mx-auto lg:-mt-16 lg:bg-white lg:p-6 lg:rounded-xl lg:shadow-lg">
            <p class="mainHeading">Контакты</p>
            <div class="flex flex-col lg:flex-row lg:flex-wrap gap-4 lg:gap-6">
                <div class="flex flex-col gap-2">
                    <p class="text-base">Адрес</p>
                    <p class="text-xl font-semibold font-mono">улица Пушкина, дом 1</p>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Режим работы</p>
                    <p class="mt-2 text-xl font-semibold font-mono">Пн-Пт: с 9:00 до 21:00</p>
                    <p class="text-xl font-semibold font-mono">Cб-Вс: с 12:00 до 18:00</p>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Email</p>
                    <NuxtLink to="mailto:library@mail.ru"
                        class="text-xl font-semibold font-mono transition-all duration-500 hover:text-amber-500">
                        library@mail.ru</NuxtLink>
                </div>
                <div class="flex flex-col gap-2">
                    <p class="text-base">Телефон для связи</p>
                    <NuxtLink to="tel:8 800-555-35-35"
                        class="text-xl font-semibold font-mono transition-all duration-500 hover:text-amber-500">
                        8 800-555-35-35</NuxtLink>
                </div>
                <div class="flex items-center gap-2">
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-amber-500">
                        <Icon class="text-3xl" name="mdi:vk" />
                    </NuxtLink>
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-amber-500">
                        <Icon class="text-3xl" name="ic:baseline-telegram" />
                    </NuxtLink>
                    <NuxtLink to="/" target="_blank" class="transition-all duration-500 hover:text-amber-500">
                        <Icon class="text-3xl" name="basil:instagram-outline" />
                    </NuxtLink>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
/* название и язык страницы */
useSeoMeta({
    title: 'О компании',
    lang: 'ru'
})
</script>