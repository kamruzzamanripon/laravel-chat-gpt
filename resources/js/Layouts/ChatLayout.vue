<script setup>
</script>

<template>
    <aside
        class="w-full lg:w-[25%] xl:w-[20%] 2xl:w-[15%] fixed z-10 top-0 pb-3 px-4 flex flex-col justify-between h-screen border bg-gray-200">
        <div class="">
            <slot name="aside" />
        </div>
    </aside>
    <div class="ml-auto mb-6 lg:w-[75%] xl:w-[80%] 2xl:w-[85%] w-full h-full">
        <div class="pt-6 px-5">
            <main>
                <slot name="content" />
                <slot></slot>

            </main>
        </div>
        <div class="fixed z-20 w-full lg:w-[75%] xl:w-[80%] 2xl:w-[85%] bottom-0 h-[15vh]  bg-slate-900 bg-opacity-60 py-4">
            <slot name="form" />

        </div>
    </div>
</template>