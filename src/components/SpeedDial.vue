
<template>
    <div :style="{ position: 'fixed', right: '0', bottom: '0', height: '500px' }">
      <SpeedDial :model="items" :radius="120" type="quarter-circle" showIcon="pi pi-pencil" hideIcon="pi pi-times" direction="up-left" :style="{ right: '10px', bottom: '10px' }" />
    </div>

    <Button label="Show" @click="displayDonate = true" />

    <Dialog v-model:visible="displayDonate" modal header="Edit Profile" :style="{ width: '25rem' }">
        <div>
            <stripe-checkout
            ref="checkoutRef"
            mode="payment"
            :pk="publishableKey"
            :line-items="lineItems"
            :success-url="successURL"
            :cancel-url="cancelURL"
            @loading="v => loading = v"
            />
            <button @click="submit">Pay now!</button>
        </div>
    </Dialog>
    
</template>

<script setup>
import { ref } from 'vue';
import { useToast } from 'primevue/usetoast';
import { useRouter } from 'vue-router';
import { StripeCheckout } from '@vue-stripe/vue-stripe';

const toast = useToast();
const router = useRouter();

const displayDonate = ref(false)

const items = ref([
    {
        label: 'Add',
        icon: 'pi pi-dollar',
        command: () => {
            displayDonate.value = !displayDonate.value
        }
    },
    {
        label: 'Update',
        icon: 'pi pi-refresh',
        command: () => {
            toast.add({ severity: 'success', summary: 'Update', detail: 'Data Updated' });
        }
    },
    {
        label: 'Delete',
        icon: 'pi pi-trash',
        command: () => {
            toast.add({ severity: 'error', summary: 'Delete', detail: 'Data Deleted' });
        }
    },
    {
        label: 'Upload',
        icon: 'pi pi-upload',
        command: () => {
            router.push('/fileupload');
        }
    },
    {
        label: 'Vue Website',
        icon: 'pi pi-external-link',
        command: () => {
            window.location.href = 'https://vuejs.org/'
        }
    }
])
</script>
