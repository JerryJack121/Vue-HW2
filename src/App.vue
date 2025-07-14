<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";
import { SEARCH_API_URL, LOCAL_STORAGE_KEY } from "./constants.js";
import SearchBar from "./components/SearchBar.vue";
import Items from "./components/Items.vue";
import Collect from "./components/Collect.vue";

const showItems = ref([]);
const collectedItems = ref([]);

// 初始化收藏列表，從 localStorage 中讀取
onMounted(() => {
    handleSearch(""); // 頁面載入時，預設查詢所有站點
    const stored = localStorage.getItem(LOCAL_STORAGE_KEY);
    if (stored) {
        collectedItems.value = JSON.parse(stored);
    }
});

const handleSearch = async (userInput) => {
    // console.log("Search ", userInput);
    try {
        const resp = await axios.get(SEARCH_API_URL);
        const data = resp.data;
        // 保留出符合名稱或是地址的站點
        showItems.value = data.filter((item) => {
            return (
                item.sna.includes(userInput) || // 場站中文名稱
                item.sarea.includes(userInput) || // 場站區域
                item.ar.includes(userInput) // 地點
            );
        });
        console.log(
            `查詢到包含 '${userInput}' 的 ${showItems.value.length} 筆資料`
        );
    } catch (error) {
        console.error("Error fetching data:", error);
    }
};

const handleAddCollect = (item) => {
    // console.log("Add to collect: ", item);
    // 檢查是否已經收藏過
    if (
        !collectedItems.value.find(
            (collectedItem) => collectedItem.sno === item.sno
        )
    ) {
        collectedItems.value.push(item);
    } else {
        alert("此場站已在收藏列表中");
    }

    // 更新 localStorage
    localStorage.setItem(
        LOCAL_STORAGE_KEY,
        JSON.stringify(collectedItems.value)
    );
};

const handleDelCollect = (itemSno) => {
    console.log("Delete from collect: ", itemSno);
    collectedItems.value = collectedItems.value.filter(
        (collectedItem) => collectedItem.sno !== itemSno
    );

    // 更新 localStorage
    localStorage.setItem(
        LOCAL_STORAGE_KEY,
        JSON.stringify(collectedItems.value)
    );
};
</script>

<template>
    <div class="mx-10 my-10">
        <SearchBar @searchClick="handleSearch" />
        <Items :showItems="showItems" @addCollect="handleAddCollect" />
        <Collect
            :collectedItems="collectedItems"
            @delCollect="handleDelCollect"
        />
    </div>
</template>

<style scoped></style>
