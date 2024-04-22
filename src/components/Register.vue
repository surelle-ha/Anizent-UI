<template>
    <Button label="Join Now" icon="pi pi-discord" class="font-bold px-5 py-3 p-button-raised p-button-rounded white-space-nowrap" @click="openAuthReg = !openAuthReg"></Button>

    <Dialog v-model:visible="openAuthReg" modal header="Register" :style="{ width: '25rem' }" :pt="{
        root: {
            style: 'border:none;'
        },
    }">
        <span class="p-text-secondary block mb-5">Create your account to save your progress and earn points.</span>
        <div class="flex align-items-center gap-3 mb-3">
            <label for="username" class="font-semibold w-6rem">Username</label>
            <InputText id="username" v-model='username' class="flex-auto" autocomplete="off" />
        </div>
        <div class="flex align-items-center gap-3 mb-3">
            <label for="email" class="font-semibold w-6rem">Email</label>
            <InputText id="email" v-model='email' class="flex-auto" autocomplete="off" />
        </div>
        <div class="flex align-items-center gap-3 mb-5">
            <label for="password" class="font-semibold w-6rem">Password</label>
            <Password id="password" v-model='password' class="flex-auto" autocomplete="off" promptLabel="Choose a password" weakLabel="Too simple" mediumLabel="Average complexity" strongLabel="Complex password" />
        </div>
        <div class="flex justify-content-end gap-2">
            <Button type="button" label="Cancel" severity="secondary" @click="visible = false"></Button>
            <Button type="button" label="Save" @click="visible = false"></Button>
        </div>
    </Dialog>
</template>

<script setup>
import { ref } from 'vue';

const openAuthReg = ref(false);
const email = ref('');
const username = ref('');
const password = ref('');

</script>

<style scoped>

</style>