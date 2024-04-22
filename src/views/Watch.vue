<template>
    <div class="container px-6 py-0">
        
        <Panel
            class="mb-4"
            :pt="{ root: { style: 'border:none;' } }"
        >

            <Toolbar :pt="{
                root: {
                    class: 'border-none mb-4'
                }
            }">
                <template #start>
                    <Button icon="pi pi-moon" class="mr-2" severity="secondary" />
                    <Button icon="pi pi-link" class="mr-2" severity="secondary" />
                    <Button icon="pi pi-save" severity="secondary" />
                </template>

                <template #center>
                    <IconField iconPosition="left">
                        <InputIcon>
                            <i class="pi pi-search" />
                        </InputIcon>
                        <!-- Make this server selection -->
                        <Dropdown v-model="selectedServer" @change="updateVideoSource" :options="servers"
                            optionLabel="name" optionValue="name" placeholder="Select a quality"
                            class="w-full md:w-14rem" />
                    </IconField>
                </template>

                <template #end>
                    <Dropdown v-model="selectedQuality" @change="updateVideoSource" :options="videoSources"
                        optionLabel="quality" optionValue="url" placeholder="Select a quality"
                        class="w-full md:w-14rem" />
                </template>
            </Toolbar>

            <div class="card text-right mx-6">
                <div class="flex flex-wrap justify-content-center">
                    <div class="w-full xl:w-8 flex flex-column justify-content-center lg:pr-0 align-items-center xl:align-items-stretch">
                        <video ref="videoElement" controls></video>
                    </div>
                </div>
            </div>

            <Toolbar style="border-radius: 3rem; padding: 1rem 1rem 1rem 1.5rem" :pt="{
                root: {
                    class: 'border-none mt-4'
                }
            }">
                <template #start>
                    <div class="flex align-items-center gap-2">
                        <svg width="35" height="40" viewBox="0 0 35 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path d="..." fill="var(--text-color)" />
                            <path d="..." fill="var(--surface-card)" />
                        </svg>
                    </div>
                </template>

                <template #end>
                    <div class="flex align-items-center gap-2">
                        <Button label="Now Playing" severity="contrast" size="small" />
                    </div>
                </template>
            </Toolbar>
        </Panel>
    </div>
</template>

<script setup>
import { watch, computed, ref, onMounted, nextTick } from 'vue';
import axios from 'axios';
import { useToast } from 'primevue/usetoast';
import Hls from 'hls.js';
import { useRoute } from 'vue-router';

const toast = useToast();
const route = useRoute();
const videoElement = ref(null);
const isLoading = ref(false);
const selectedSeriesDetails = ref([]);
const selectedQuality = ref('');
const selectedServer = ref('vidstreaming');
const servers = ref([
    {
        name: 'gogocdn'
    },
    {
        name: 'streamsb'
    },
    {
        name: 'vidstreaming'
    }
]);

const videoSources = computed(() => selectedSeriesDetails.value.sources || []);
const url_query = computed(() => route.query.episode_id);

watch([selectedServer, selectedQuality], (newValues, oldValues) => {
    const [newServer, newQuality] = newValues;
    const [oldServer, oldQuality] = oldValues;

    if (newServer !== oldServer) {
        getSeriesDetails(url_query.value, newServer);
    }

    if (newQuality !== oldQuality) {
        updateVideoSource();
    }
}, { immediate: true });

onMounted(() => {
    if (url_query.value) {
        getSeriesDetails(url_query.value, selectedServer.value);
    }
});

async function getSeriesDetails(id, server) {
    try {
        isLoading.value = true;
        const url = `${process.env.VUE_APP_MEDIA_API_URL}/anime/gogoanime/watch/${id}?server=${server}`;
        console.log('Fetching series details from:', url);  // Debugging line
        const response = await axios.get(url);
        selectedSeriesDetails.value = response.data;
        if (videoSources.value.length > 0) {
            selectedQuality.value = videoSources.value.find(source => source.quality === '720p')?.url || videoSources.value[0].url;
        }
        isLoading.value = false;
    } catch (error) {
        isLoading.value = false;
        console.error('Error fetching series details with server:', server, error);
        toast.add({
            severity: 'error',
            summary: 'Error fetching video details',
            detail: `Error: ${error.message}. Please try again later.`,
            life: 3000,
        });
    }
}

function updateVideoSource() {
    nextTick(() => {
        if (videoElement.value) {
            if (Hls.isSupported()) {
                const hls = new Hls();
                hls.loadSource(selectedQuality.value);
                hls.attachMedia(videoElement.value);
                hls.on(Hls.Events.MANIFEST_PARSED, () => {
                    // videoElement.value.play();
                });
                hls.on(Hls.Events.ERROR, (event, data) => {
                    console.error('HLS.js error:', data);
                });
            } else if (videoElement.value.canPlayType('application/vnd.apple.mpegurl')) {
                videoElement.value.src = selectedQuality.value;
                videoElement.value.load(); // Ensure the video is reloaded with the new source
                // videoElement.value.play();
            }
        }
    });
}
</script>



<style scoped>
select {
    margin-bottom: 10px;
}
</style>

