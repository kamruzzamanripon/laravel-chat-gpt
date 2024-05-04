<script setup>
import ChatLayout from "@/Layouts/ChatLayout.vue";
import { Link, useForm } from "@inertiajs/vue3";
import { onMounted, ref } from 'vue';
import ChatContent from "../../Components/ChatContent.vue";

const promtInput = ref(null);
const chatContainer = ref(null);
const props = defineProps({
    messages: Array,
    chat: null | Object
})
const form = useForm({
    promt: "",
});

const submit = () => {
    const url = props.chat ? `/chat/${props.chat?.id}` : '/chat';
    form.post(url, {
        onFinish: () => clear()
    });
};

const scrollToBottom = () => {
    if (props.chat) {
        const el = chatContainer.value;
        console.log('scrollToBottom', el.scrollHeight)
        el.scrollTop = el.scrollHeight;

    }
}

const clear = () => {
    form.promt = "";
    promtInput.value.focus();
    scrollToBottom();
}

onMounted(() => {
    clear();
})

</script>

<template>
    <div class="overflow-auto h-[82vh] pb-10" ref="chatContainer">
        <ChatLayout>

            <template #aside>
                <div class="p-2">
                    <div
                        class="px-4 py-2 my-2 flex justify-between font-semibold hover:bg-gray-700 hover:text-white rounded-lg duration-200">
                        <Link :href="`/chat`">New Chat</Link>
                    </div>
                    <template v-for="message in messages" :key="message.id">

                        <li
                            class="px-4 py-2 my-2 flex justify-between font-semibold hover:bg-gray-700 hover:text-white rounded-lg duration-200">
                            <Link :href="`/chat/${message.id}`">{{ message.context[0].content }}</Link>
                        </li>
                    </template>
                </div>
            </template>

            <template #content>
                <div>
                    <div v-for="(content, index) in chat?.context">
                        <ChatContent :content="content" />
                    </div>
                </div>

            </template>


            <template #form>
                <section class="px-6 top-0">
                    <div class="w-full">
                        <div class="relative flex-1 flex items-center">
                            <input type="text" class="w-full bg-slate-700 text-white rounded-lg h-16"
                                placeholder="Asked to AI" v-model="form.promt" @keyup.enter="submit"
                                :disabled="form.processing" ref="promtInput" />
                            <div class="absolute inset-y-0 right-0 flex items-center pl-3">
                                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                    stroke-width="1.5" stroke="currentColor" class="w-6 h-6 -ml-8 text-slate-200"
                                    v-if="!form.processing">
                                    <path stroke-linecap="round" stroke-linejoin="round"
                                        d="M6 12 3.269 3.125A59.769 59.769 0 0 1 21.485 12 59.768 59.768 0 0 1 3.27 20.875L5.999 12Zm0 0h7.5" />
                                </svg>
                                <p v-else class="text-white">Loading....</p>
                            </div>
                        </div>
                    </div>
                </section>
            </template>



        </ChatLayout>
    </div>
</template>
