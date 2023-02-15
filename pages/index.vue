<script setup>
import zxcvbn from "zxcvbn";
const section = ref("Loading");
const next = ref(0);
const name = ref("");
const password = ref("");
const passwordScore = ref(0);
const showPasswordField = ref(true);

async function promptName() {
    await new Promise((resolve) =>
        setTimeout(() => {
            section.value = "Transition";
            resolve();
        }, 1000)
    );
    await new Promise((resolve) =>
        setTimeout(() => {
            section.value = "Name";
            resolve();
        }, 750)
    );
    document.addEventListener("keydown", (e) => {
        if (e.keyCode == 13) {
            promptPassword();
        }
    });
}

async function promptPassword() {
    section.value = "Transition";
    await new Promise((resolve) =>
        setTimeout(() => {
            section.value = "Password";
            resolve();
        }, 750)
    );
}

function alertNameAndPsw() {
    alert(
        `Name: ${name.value}, Password: ${password.value}, PasswordStrength: ${passwordScore.value}`
    );
}

function checkStrength() {
    if (!password.value) {
        passwordScore.value = 0;
        return;
    }
    passwordScore.value = zxcvbn(password.value).score + 1;
}

onMounted(() => {
    section.value = "Greeting";
    promptName();
});
</script>
<style>
@import url(https://cdnjs.cloudflare.com/ajax/libs/MaterialDesign-Webfont/5.3.45/css/materialdesignicons.min.css);

.fade-enter,
.fade-enter-from,
.fade-leave-to {
    opacity: 0 !important;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}

#next {
    position: relative;
}

#next::before {
    content: "";
    position: absolute;
    width: 100%;
    height: 4px;
    border-radius: 4px;
    background-color: #000;
    bottom: 0;
    left: 0;
    transform-origin: right;
    transform: scaleX(0);
    transition: transform 0.3s ease-in-out;
}

#next:hover::before {
    transform-origin: left;
    transform: scaleX(1);
}
</style>
<template>
    <div class="bg-white flex justify-center items-center h-screen w-screen">
        <transition name="fade">
            <h1
                class="text-black text-4xl"
                id="Greeting"
                v-if="section == 'Greeting'"
            >
                Hallo
            </h1>
            <section
                id="name"
                class="flex flex-col items-center"
                v-else-if="section == 'Name'"
            >
                <h3 class="text-black text-2xl" id="name-question">
                    Wie ist dein Name?
                </h3>
                <br />
                <input
                    type="text"
                    class="text-black text-2xl border-b-2 border-black bg-white text-center"
                    v-model="name"
                    placeholder="Name"
                />
                <br />
                <button
                    class="bg-white text-black text-2xl"
                    @click="promptPassword"
                    id="next"
                >
                    Weiter
                </button>
            </section>
            <section
                class="flex flex-col items-center"
                id="password"
                v-else-if="section == 'Password'"
            >
                <h3 class="text-black text-4xl">Wie ist dein Passwort?</h3>
                <br />
                <div class="relative mb-2">
                    <input
                        :type="showPasswordField ? 'password' : 'text'"
                        id="password"
                        v-model="password"
                        class="w-full pl-3 pr-10 py-2 border-2 border-gray-200 rounded-md focus:outline-none focus:border-indigo-500 transition-colors"
                        placeholder="Password"
                        @input="checkStrength"
                    />
                    <button
                        class="block w-7 h-7 text-center text-xl leading-0 absolute top-2 right-2 text-gray-400 focus:outline-none hover:text-indigo-500 transition-colors"
                        @click.prevent="showPasswordField = !showPasswordField"
                    >
                        <i
                            class="mdi"
                            :class="`mdi-${
                                showPasswordField
                                    ? 'eye-outline'
                                    : 'eye-off-outline'
                            }`"
                        ></i>
                    </button>
                    <div class="flex -mx-1 mt-2">
                        <template v-for="(v, i) in 5" :key="i">
                            <div class="w-1/5 px-1">
                                <div
                                    class="h-2 rounded-xl transition-colors"
                                    :class="
                                        i < passwordScore
                                            ? passwordScore <= 2
                                                ? 'bg-red-400'
                                                : passwordScore <= 4
                                                ? 'bg-yellow-400'
                                                : 'bg-green-500'
                                            : 'bg-gray-200'
                                    "
                                ></div>
                            </div>
                        </template>
                    </div>
                </div>

                <br />
                <button
                    class="bg-white text-black text-2xl"
                    @click="alertNameAndPsw"
                    id="next"
                >
                    Weiter
                </button>
            </section>
        </transition>
    </div>
</template>
