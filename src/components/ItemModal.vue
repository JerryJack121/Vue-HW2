<script setup>
import { ref } from "vue";

const props = defineProps({
    item: Object,
});

const emits = defineEmits(["close"]);
// console.log("ItemModal props: ", props.item);

const closeModal = () => {
    emits("close");
};
</script>

<template>
    <dialog class="modal modal-open">
        <div class="modal-box">
            <h3 class="font-bold text-lg mb-4">{{ item?.sna }}</h3>
            <div class="space-y-2">
                <p><strong>場站編號：</strong>{{ item?.sno }}</p>
                <p><strong>場站區域：</strong>{{ item?.sarea }}</p>
                <p><strong>場站地址：</strong>{{ item?.ar }}</p>
                <p><strong>總車位數：</strong>{{ item?.Quantity }}</p>
                <p>
                    <strong>可借車輛數：</strong
                    >{{ item?.available_rent_bikes }}
                </p>
                <p>
                    <strong>可還車位數：</strong
                    >{{ item?.available_return_bikes }}
                </p>
                <p>
                    <strong>營運狀態：</strong
                    >{{ item?.act === "1" ? "正常營運" : "暫停營運" }}
                </p>
                <p><strong>更新時間：</strong>{{ item?.mday }}</p>
            </div>
            <div class="modal-action">
                <form method="dialog">
                    <button class="btn" @click="closeModal">Close</button>
                </form>
            </div>
        </div>
    </dialog>
</template>

<style scoped></style>
