<template>
    <div class="slot_game_body">
        <h2>拉霸機</h2>
        <button class="intro_btn" @click="showPopup">遊戲說明</button>
        <popModel ref="popup" backgroundClose showAnimation @onShow="handleShow" @onClose="handleClose">
            <div class="game_intro_container">
                <h3>遊戲說明</h3>
                <p>＊請輸入一個最大值來啟動拉霸機。</p>
                <p>＊拉霸機將隨機從 1 到設定的數字中選擇一個數字。</p>
                <p>＊可以開啟連續抽取模式，並設定抽取次數，以達到自動連續抽取的效果。</p>
            </div>
        </popModel>
        <div class="slot_setting_container">
            <!-- 綁定最大值 -->
            <input type="text" v-model="maxValue" class="slot_game_input" placeholder="請輸入最大值" />

            <!-- 連續抽取選項 -->
            <div class="nonestop_setting df_jc_ac">
                <input type="checkbox" v-model="isNonStop" id="nonstop-checkbox">
                <label for="nonstop-checkbox">是否連續抽取</label>
            </div>

            <!-- 綁定抽取次數 -->
            <input type="text" v-model="drawCount" v-if="isNonStop" placeholder="請輸入次數" />

            <button @click="startSlotMachine">啟動拉霸機</button>
            <button @click="resetSlotMachine">啟動拉霸機</button>

            <!-- 拉霸機顯示區 -->
            <div class="slot_machine">
                <div class="slot_card">{{ slotResult }}</div>
            </div>
        </div>
    </div>
</template>

<script setup>

import { ref } from 'vue';
import popModel from './popModel.vue';

// 引入彈出視窗

const popup = ref(null);

const showPopup = () => {
    popup.value.show();
};

const handleShow = () => {
    console.log('彈出層顯示');
};

const handleClose = () => {
    console.log('彈出層關閉');
};

// 拉霸機

function startSlotMachine() {
    if (!this.maxValue) {
        alert('請輸入最大值');
        return;
    }
    const max = parseInt(this.maxValue);
    if (this.isNonStop && this.drawCount) {
        // 連續抽取
        for (let i = 0; i < parseInt(this.drawCount); i++) {
            this.slotResult = this.getRandomNumber(max);
            // 這裡可以增加延遲效果來模擬拉霸機的視覺效果
        }
    } else {
        // 單次抽取
        this.slotResult = this.getRandomNumber(max);
    }
}

function getRandomNumber(max) {
    return Math.floor(Math.random() * max) + 1;
}

</script>

<style scoped>
p,
h2,
div,
button {
    font-family: var(--chinese-font);
    letter-spacing: 0.5px;
}

h2 {
    text-align: center;
    margin: 18px 0;
}

.intro_btn {
    display: block;
    margin: 0 auto;
    font-size: 1.25rem;
    border: none;
    background: transparent;
    cursor: pointer;
    color: #262689;
    font-weight: 600;
    text-decoration: underline;
}

.game_intro_container {
    padding: 0 12px;
}

.game_intro_container h3 {
    text-align: center;
    margin: 4px 0 16px;
}

.slot_setting_container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    row-gap: 1px;
}

.slot_setting_container input[type="text"] {
    display: block;
    margin: 16px auto;
    min-width: 160px;
    max-width: 160px;
    height: 40px;
    font-size: 1.25rem;
    border: 2px solid #333;
    text-align: center;
}


.nonestop_setting input {
    display: block;
    margin: 6px;
    width: 16px;
    height: 16px;
    opacity: 0;
    cursor: pointer;
}

.nonestop_setting label {
    display: block;
    margin: 0;
    font-size: 1.15rem;
    position: relative;
}

.nonestop_setting label::before {
    display: block;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    position: absolute;
    top: 3px;
    left: -24px;
    content: "";
    pointer-events: none;
    box-sizing: border-box;
    opacity: 1;
    background: #FFF;
    border: 1px solid #575757;
}

.nonestop_setting label::after {
    display: block;
    width: 12px;
    height: 12px;
    border-radius: 50%;
    position: absolute;
    top: 7px;
    left: -20px;
    content: "";
    pointer-events: none;
    box-sizing: border-box;
    opacity: 1;
    background: #fff;
}

.nonestop_setting input:checked+label::before {
    background: #262689;
}
</style>