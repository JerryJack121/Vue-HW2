<script setup>
import { ref } from "vue";

const emits = defineEmits(["searchClick"]);

const userInput = ref("");
const showModal = ref(true);

const onSearch = () => {
    const userInputValue = userInput.value;
    if (userInputValue === "") {
        alert("請輸入查詢關鍵字");
    } else {
        emits("searchClick", userInput.value);
        userInput.value = ""; // 清空輸入框
    }
};
</script>

<template>
    <h1 class="text-2xl">YouBike 站點查詢</h1>
    <div class="my-2">
        <input
            type="text"
            @keyup.enter="onSearch"
            v-model.trim="userInput"
            class="input"
        />
        <button @click="onSearch" class="btn">新增</button>
    </div>
</template>

<style scoped></style>
