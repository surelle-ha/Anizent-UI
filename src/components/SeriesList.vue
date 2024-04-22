
<template>
    <div class="card">
        <DataView :value="recentEpisodes" paginator :rows="5">
            <template #list="slotProps">
                <div class="grid grid-nogutter">
                    <div v-for="(item, index) in slotProps.items" :key="index" class="col-12">
                        <div class="flex flex-column sm:flex-row sm:align-items-center p-4 gap-3" :class="{ 'border-top-1 surface-border': index !== 0 }">
                            <div class="md:w-10rem relative">
                                <img class="block xl:block mx-auto border-round w-full" :src="`${item.image}`" :alt="item.title" />
                                <Tag :value="item.inventoryStatus" :severity="success" class="absolute" style="left: 4px; top: 4px"></Tag>
                            </div>
                            <div class="flex flex-column md:flex-row justify-content-between md:align-items-center flex-1 gap-4">
                                <div class="flex flex-row md:flex-column justify-content-between align-items-start gap-2">
                                    <div>
                                        <span class="font-medium text-secondary text-sm">{{ item.title }}</span>
                                        <div class="text-lg font-medium text-900 mt-2">{{ item.title }}</div>
                                    </div>
                                    <div class="surface-100 p-1" style="border-radius: 30px">
                                        <div class="surface-0 flex align-items-center gap-2 justify-content-center py-1 px-2" style="border-radius: 30px; box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.04), 0px 1px 2px 0px rgba(0, 0, 0, 0.06)">
                                            <span class="text-900 font-medium text-sm">5</span>
                                            <i class="pi pi-star-fill text-yellow-500"></i>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </template>
        </DataView>
    </div>
</template>


<script setup>
import { ref, onMounted } from "vue";
import axios from 'axios';
import { useToast } from 'primevue/usetoast';

const toast = useToast();

const recentEpisodes = ref([]);

const getRecentEpisode = async () => {
    try {
        const response = await axios.get(`${process.env.VUE_APP_MEDIA_API_URL}/anime/gogoanime/recent-episodes?page=1`);

        if(response.data.results.length > 0){
            toast.add({ severity: 'info', summary: 'Success Message', detail: 'Message Content', life: 3000 });
            recentEpisodes.value = response.data.results;
            console.log('new', response.data.results)
        }else{
            toast.add({ severity: 'error', summary: 'Failed loading recent episodes', life: 3000 });
        }
    } catch (error) {
        toast.add({ severity: 'error', summary: 'Failed loading recent episodes', life: 3000 });
    }
}

onMounted(() => {
    getRecentEpisode();
})

const responsiveOptions = ref([
    {
        breakpoint: '1400px',
        numVisible: 4,
        numScroll: 1
    },
    {
        breakpoint: '1199px',
        numVisible: 3,
        numScroll: 1
    },
    {
        breakpoint: '767px',
        numVisible: 2,
        numScroll: 1
    },
    {
        breakpoint: '575px',
        numVisible: 1,
        numScroll: 1
    }
]);
</script>

<style scoped>
.title {
    width: 200px;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
}
</style>