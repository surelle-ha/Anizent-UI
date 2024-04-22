
<template>
    <div class="card">
        <Menubar :model="items" :pt="{
            root: {
                style: 'background: transparent;',
                class: 'border-none'
            }

        }">
            <template #start>
                <img src="@/assets/icon.png" width="50px" class="mr-6" alt="" srcset="">
            </template>
            <template #item="{ item, props, hasSubmenu, root }">
                <component
                    :is="item.actionType === 'route' ? 'router-link' : 'a'"
                    :to="item.actionType === 'route' ? item.route : undefined"
                    @click="item.actionType === 'action' ? $emit(item.action) : null"
                    v-ripple
                    class="flex align-items-center"
                    v-bind="props.action" v-if="item.show">
                    <span :class="item.icon" />
                    <span class="ml-2">{{ item.label }}</span>
                    <Badge v-if="item.badge" :class="{ 'ml-auto': !root, 'ml-2': root }" :value="item.badge" />
                    <span v-if="item.shortcut" class="ml-auto border-1 surface-border border-round surface-100 text-xs p-1">{{ item.shortcut }}</span>
                    <i v-if="hasSubmenu" :class="['pi pi-angle-down', { 'pi-angle-down ml-2': root, 'pi-angle-right ml-auto': !root }]"></i>
                </component>
            </template>
            <template #end>
                <div class="flex align-items-center gap-2">
                    <AutoComplete v-model="selectedSeries" class="w-8rem sm:w-auto" placeholder="Search" optionLabel="title" :suggestions="filteredSeries" @complete="search" @keyup.enter="finalSearch"/>
                    <Avatar image="https://primefaces.org/cdn/primevue/images/avatar/amyelsner.png" shape="circle" />
                </div>
            </template>
        </Menubar>
    </div>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

const series = ref();
const selectedSeries = ref();
const filteredSeries = ref();

const search = async (event) => {
    if (selectedSeries.value) {
        const query_result = await axios.get(`${process.env.VUE_APP_MEDIA_API_URL}/anime/gogoanime/${selectedSeries.value}?page=1`);
        series.value = query_result.data.results;
        setTimeout(() => {
            if (!event.query.trim().length) {
                filteredSeries.value = [...series.value];
            } else {
                filteredSeries.value = series.value.filter(seriesItem => {
                    return seriesItem.title.toLowerCase().startsWith(event.query.toLowerCase());
                });
            }
        }, 250);
    }
}

const finalSearch = () => {
    if(typeof selectedSeries.value === 'object' &&
    !Array.isArray(selectedSeries.value) &&
    selectedSeries.value !== null){
        router.push({path:'/search', query: { query: selectedSeries.value.title } });
    } else{
        router.push({path:'/search', query: { query: selectedSeries.value } });
    }
}

const items = computed(() => [
{
        label: 'Anizent',
        route: '/',
        icon: 'pi pi-wave-pulse',
        actionType: 'route', 
        show: true
    },
    {
        label: 'Featured',
        route: 'featured',
        icon: 'pi pi-star',
        actionType: 'route', 
        show: true
    },
    {
        label: 'Donate',
        route: 'featured',
        icon: 'pi pi-paypal',
        actionType: 'route', 
        show: true
    },

]);
</script>
