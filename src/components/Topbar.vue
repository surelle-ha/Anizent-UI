
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
                    <Avatar image="https://primefaces.org/cdn/primevue/images/avatar/amyelsner.png" shape="circle" v-badge.warning="4" v-if="true" class="cursor-pointer" @click="toggleAccount"/>

                    <OverlayPanel ref="op">
                        <div class="flex flex-column gap-3 w-25rem">
                            <div>
                                <span class="font-medium text-900 block mb-2">Search Anime</span>
                                <InputGroup>
                                    <AutoComplete v-model="selectedSeries" class="w-8rem sm:w-auto" placeholder="Search" optionLabel="title" :suggestions="filteredSeries" @complete="search" @keyup.enter="finalSearch"/>
                                    <InputGroupAddon>
                                        <i class="pi pi-search"></i>
                                    </InputGroupAddon>
                                </InputGroup>
                            </div>
                            <TabView>
                                <TabPanel header="Login">
                                    <span class="p-text-secondary block mb-5">Update your information.</span>
                                    <div class="flex align-items-center gap-3 mb-3">
                                        <label for="username" class="font-semibold w-6rem">Username</label>
                                        <InputText id="username" class="flex-auto" autocomplete="off" />
                                    </div>
                                    <div class="flex align-items-center gap-3 mb-5">
                                        <label for="email" class="font-semibold w-6rem">Email</label>
                                        <InputText id="email" class="flex-auto" autocomplete="off" />
                                    </div>
                                    <div class="flex justify-content-end gap-2">
                                        <Button type="button" label="Cancel" severity="secondary" @click="visible = false"></Button>
                                        <Button type="button" label="Save" @click="visible = false"></Button>
                                    </div>
                                </TabPanel>
                                <TabPanel header="Register">
                                    <Stepper orientation="vertical">
                                        <StepperPanel header="Header I">
                                            <template #content="{ nextCallback }">
                                                <div class="flex flex-column h-12rem">
                                                    <div class="border-2 border-dashed surface-border border-round surface-ground flex-auto flex justify-content-center align-items-center font-medium">Content I</div>
                                                </div>
                                                <div class="flex py-4">
                                                    <Button label="Next" @click="nextCallback" />
                                                </div>
                                            </template>
                                        </StepperPanel>
                                        <StepperPanel header="Header II">
                                            <template #content="{ prevCallback, nextCallback }">
                                                <div class="flex flex-column h-12rem">
                                                    <div class="border-2 border-dashed surface-border border-round surface-ground flex-auto flex justify-content-center align-items-center font-medium">Content II</div>
                                                </div>
                                                <div class="flex py-4 gap-2">
                                                    <Button label="Back" severity="secondary" @click="prevCallback" />
                                                    <Button label="Next" @click="nextCallback" />
                                                </div>
                                            </template>
                                        </StepperPanel>
                                        <StepperPanel header="Header III">
                                            <template #content="{ prevCallback }">
                                                <div class="flex flex-column h-12rem">
                                                    <div class="border-2 border-dashed surface-border border-round surface-ground flex-auto flex justify-content-center align-items-center font-medium">Content III</div>
                                                </div>
                                                <div class="flex py-4">
                                                    <Button label="Back" severity="secondary" @click="prevCallback" />
                                                </div>
                                            </template>
                                        </StepperPanel>
                                    </Stepper>
                                </TabPanel>
                            </TabView>
                        </div>
                    </OverlayPanel>

                </div>
            </template>
        </Menubar>
    </div>
</template>

<script setup>
import { onMounted, computed, ref } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";

const router = useRouter();

const series = ref();
const selectedSeries = ref();
const filteredSeries = ref();
const op = ref();

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

const toggleAccount = (event) => {
    op.value.toggle(event);
}

</script>
