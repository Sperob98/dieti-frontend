<template>
        <div class="contenitore-dataView">
            <DataView :value="products" :layout="layout">
                <template #grid="slotProps">
                    <div class="grid grid-cols-12 gap-2">
                        <div v-for="(item, index) in aste" :key="index"
                            class="col-span-12 sm:col-span-6 md:col-span-4 xl:col-span-6 p-2">
                            <div
                                class="p-6 border border-surface-200 dark:border-surface-700 bg-surface-0 dark:bg-surface-900 rounded flex flex-col">
                                <div class="bg-surface-50 flex justify-center rounded p-4">
                                    <div class="relative mx-auto">
                                        <img class="rounded w-full"
                                            :src="`https://primefaces.org/cdn/primevue/images/product/${item.image}`"
                                            :alt="item.name" style="max-width: 300px" />
                                        <div class="absolute bg-black/70 rounded-border" style="left: 4px; top: 4px">
                                            <Tag :value="item.inventoryStatus" :severity="getSeverity(item)"></Tag>
                                        </div>
                                    </div>
                                </div>
                                <div class="pt-6">
                                    <div class="flex flex-row justify-between items-start gap-2">
                                        <div>
                                            <span class="font-medium text-surface-500 dark:text-surface-400 text-sm">{{
                                                item.categoria }}</span>
                                            <div class="text-lg font-medium mt-1">{{ item.nome }}</div>
                                        </div>
                                    </div>
                                    <div class="flex flex-col gap-6 mt-6">
                                        <span class="text-2xl font-semibold">${{ item.baseAsta }}</span>
                                        <div class="flex gap-2">
                                            <Button icon="pi pi-shopping-cart" label="Buy Now"
                                                :disabled="item.inventoryStatus === 'OUTOFSTOCK'"
                                                class="flex-auto whitespace-nowrap"></Button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </template>
            </DataView>
        </div>

        <Paginator :rows="5" :totalRecords="10" @page="onPage"></Paginator>

</template>

<script setup>
import { ref, onMounted } from "vue";
import DataView from 'primevue/dataview';
import Button from 'primevue/button';
import Tag from 'primevue/tag';
import Paginator from 'primevue/paginator';
import {postRest} from '../../scripts/RestUtils'


onMounted(() => {
   
    aste.value = postRest("public/asta/getAllAste", bodyPerLaPost);
});

const onPage = (event) => {
    console.log("click page");
};

const aste = ref();
const layout = ref('grid');
const bodyPerLaPost = {
    pagina: 1,
    elementiPerPagina: 12,
    categoria: "tutte",
    nomeProdotto: "",
    tipoAsta: [],
    caratteristicheSelezionate: [],
    prezzoMin: null,
    prezzoMax: null,
    campoOrdinamento: null,
    direzioneOrdinamento: null
}

const getSeverity = (product) => {
    switch (product.inventoryStatus) {
        case 'INSTOCK':
            return 'success';

        case 'LOWSTOCK':
            return 'warn';

        case 'OUTOFSTOCK':
            return 'danger';

        default:
            return null;
    }
}

</script>

<style scoped>

.contenitore-dataView {

    margin-left: 3rem;
    margin-right: auto;
    margin-top: 1rem;
    padding: 1rem;
}

</style>