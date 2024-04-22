
<template>
    <div class="card">
        <div class="flex flex-wrap gap-3 mb-5">
            <div v-for="option in positionOptions" :key="option.label" class="flex align-items-center">
                <RadioButton :value="option.value" />
                <label :for="option.label" class="ml-2"> {{ option.label }} </label>
            </div>
        </div>
        <div class="flex align-items-center mb-5">
            <Checkbox v-model="inside" inputId="inside_cbox" :binary="true"></Checkbox>
            <label for="inside_cbox" class="ml-2"> Inside </label>
        </div>
        <Galleria :value="images" :numVisible="5" containerStyle="max-width: 640px" :showThumbnails="false"
            :showIndicators="true" :changeItemOnIndicatorHover="true" :showIndicatorsOnItem="inside" :indicatorsPosition="position">
            <template #item="slotProps">
                <img :src="slotProps.item.itemImageSrc" :alt="slotProps.item.alt" style="width: 100%; display: block" />
            </template>
        </Galleria>
    </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { PhotoService } from '@/service/PhotoService';

onMounted(() => {
    PhotoService.getImages().then((data) => (images.value = data));
});

const images = ref();
const inside = ref(false);
const position = ref('bottom');
const positionOptions = ref([
    {
        label: 'Bottom',
        value: 'bottom'
    },
    {
        label: 'Top',
        value: 'top'
    },
    {
        label: 'Left',
        value: 'left'
    },
    {
        label: 'Right',
        value: 'right'
    }
]);

</script>
