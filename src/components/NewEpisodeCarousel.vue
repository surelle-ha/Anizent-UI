<template>
    <Panel :pt="{
        root: {
            style: 'border:none;'
        },
    }">
        <div class="card text-right px-4">
            <Carousel :value="recentEpisodes" :numVisible="6" :numScroll="3" :responsiveOptions="responsiveOptions">
                <template #item="slotProps" >
                    <div class="episode-card border-1 surface-border border-round m-2 p-3 cursor-pointer" @click="openDetails(slotProps.data.id)">
                        <div class="mb-3">
                            <div class="relative mx-auto">
                                <img height="350px" :src="slotProps.data.image" :alt="slotProps.data.title" class="w-full border-round" />
                                <Tag value="NEW" :severity="warning" class="absolute" style="left:5px; top: 5px"/>
                            </div>
                        </div>
                        <div class="mb-3 font-medium title">{{ slotProps.data.title }}</div>
                        <div class="flex justify-content-between align-items-center">
                            <div class="mt-0 font-semibold text-xl">Episode {{ slotProps.data.episodeNumber }}</div>
                            <span>
                                <Button icon="pi pi-heart" severity="secondary" outlined/>
                            </span>
                        </div>
                    </div>
                </template>
            </Carousel>

            <Dialog
                v-model:visible="openSeriesDetails"
                modal
                :pt="{
                    root: 'border-none',
                    mask: {
                        style: 'backdrop-filter: blur(2px)'
                    }
                }"
            >
                <div v-if="isLoading">
                    <span class="p-text-secondary block mb-5">Anizent: Loading Assets</span>
                    <div class="flex align-items-center gap-3 mb-3">
                        <ProgressSpinner style="width: 50px; height: 50px;" strokeWidth="8" fill="#EEEEEE" animationDuration=".5s"></ProgressSpinner>
                    </div>
                </div>
                <Card style="width: 50rem; overflow: hidden" v-else>
                    <template #title>{{ selectedSeriesDetails.title }}</template>
                    <template #subtitle>{{ selectedSeriesDetails.description }}</template>
                    <template #content>
                        <div >
                            <Chip v-for="genre in selectedSeriesDetails.genres" :key="genre" :label="genre" class="ml-2" />
                        </div>
                        <DataView :value="selectedSeriesDetails.episodes">
                            <template #list="slotProps">
                                <div class="grid grid-nogutter">
                                    <div v-for="(item, index) in slotProps.items" :key="index" class="col-12">
                                        <div class="flex flex-column sm:flex-row sm:align-items-center p-4 gap-3" :class="{ 'border-top-1 surface-border': index !== 0 }">
                                            <div class="md:w-10rem relative">
                                                <img class="block xl:block mx-auto border-round w-full" :src="selectedSeriesDetails.image" :alt="item.name" />
                                                <Tag :value="item.inventoryStatus" :severity="success" class="absolute" style="left: 4px; top: 4px"></Tag>
                                            </div>
                                            <div class="flex flex-column md:flex-row justify-content-between md:align-items-center flex-1 gap-4">
                                                <div class="flex flex-row md:flex-column justify-content-between align-items-start gap-2">
                                                    <div>
                                                        <div class="text-lg font-medium text-900 mt-2">{{ selectedSeriesDetails.title }} Episode {{ item.number }}</div>
                                                    </div>
                                                </div>
                                                <div class="flex flex-column md:align-items-end gap-5">
                                                    <div class="flex flex-row-reverse md:flex-row gap-2">
                                                        <Button icon="pi pi-play" label="Watch" outlined @click="playMedia(selectedSeriesDetails.episodes[index].id, selectedSeriesDetails.id)"></Button>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </template>
                        </DataView>
                    </template>
                </Card>
            </Dialog>
        </div>
    </Panel>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from 'axios';
import { useToast } from 'primevue/usetoast';
import { useRouter } from "vue-router";

const toast = useToast();
const router = useRouter();

const isLoading = ref(false);
const recentEpisodes = ref([]);
const selectedSeriesDetails = ref([]);
const openSeriesDetails = ref(false);

const playMedia = (episode, show) => {
    router.push({ name:'watch', query: {show: show,episode_id:episode} });
}

const openDetails = (seriesId) => {
    openSeriesDetails.value = !openSeriesDetails.value;
    getSeriesDetails(seriesId);
}

const getSeriesDetails = async (id) => {
    try {
        isLoading.value = true;
        const response = await axios.get(`${process.env.VUE_APP_MEDIA_API_URL}/anime/gogoanime/info/${id}`);
        selectedSeriesDetails.value = response.data;
        console.log(response.data)
        
        isLoading.value = false;
    } catch (error) {
        isLoading.value = false;
        toast.add({ severity: 'error', summary: 'Something went wrong!', detail: `Something went wrong! ${error.message}. Please contact the developer.`, life: 3000 });
    }
}

const getRecentEpisode = async (pages) => {
    try {
        let allEpisodes = [];
        for (let i = 1; i <= pages; i++) {
            const res = await axios.get(`${process.env.VUE_APP_MEDIA_API_URL}/anime/gogoanime/recent-episodes?page=${i}`);
            allEpisodes = allEpisodes.concat(res.data.results);
        }

        if (allEpisodes.length > 0) {
            toast.add({ severity: 'info', summary: 'Success Message', detail: 'New episodes loaded', life: 3000 });
            recentEpisodes.value = allEpisodes;
            console.log('new', recentEpisodes.value);
        } else {
            toast.add({ severity: 'error', summary: 'Failed loading recent episodes', detail: 'No recent episodes found', life: 3000 });
        }
    } catch (error) {
        console.error(error);
        toast.add({ severity: 'error', summary: 'Error', detail: 'Failed loading recent episodes', life: 3000 });
    }
}

onMounted(() => {
    getRecentEpisode(3);
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