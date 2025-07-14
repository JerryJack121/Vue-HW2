<script setup>
import { ref } from "vue";
import ItemModal from "./ItemModal.vue";

const props = defineProps({
    showItems: Array,
});

const emits = defineEmits(["addCollect"]);

// 點擊詳細資料的項目
const selectedItem = ref(null);

const openModal = (item) => {
    selectedItem.value = item;
};

const closeModal = () => {
    selectedItem.value = null;
};
</script>

<template>
    <h2 class="text-xl my-3">查詢結果</h2>
    <div v-if="showItems.length === 0">查無結果</div>
    <div v-else class="overflow-y-auto max-h-96">
        <table class="table table-pin-rows">
            <thead>
                <tr>
                    <th class="w-3/5">場站名稱</th>
                    <th class="w-1/5">詳細資料</th>
                    <th class="w-1/5">加入收藏</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in showItems" :key="item.sno">
                    <td>
                        {{ item.sna }}
                    </td>
                    <td>
                        <button
                            class="btn btn-primary btn-sm"
                            @click="openModal(item)"
                        >
                            詳細資料
                        </button>
                    </td>
                    <td>
                        <button
                            class="btn btn-secondary btn-sm"
                            @click="$emit('addCollect', item)"
                        >
                            加入收藏
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>

    <ItemModal v-if="selectedItem" :item="selectedItem" @close="closeModal" />
</template>

<style scoped></style>
