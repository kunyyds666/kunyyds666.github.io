# Vue3登录注册界面快速搭建



<script setup></script>

<script lang="ts" setup>
import { ref } from "vue";
const input = ref("");
const password = ref("");
</script>


<template>
  <div class="common-layout">
    <el-container>

        <el-main>
          <div class="index_form">
            <el-input
              v-model="input"
              style="width: 240px"
              placeholder="Please input"
            />
            <br /><br />
            <el-input
              v-model="password"
              style="width: 240px"
              type="password"
              placeholder="Please input password"
              show-password
            />
          </div>
        </el-main>
      
    </el-container>
  </div>
</template>


<style scoped>
.common-layout {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
}
.el-container {
  height: 100%;


  background-image: url(./assets/index_1.jpg);
  background-size: 100% 100%;
  background-attachment: fixed;
  border-radius: 60px;
}


.index_form {
  width: 100px;
  height: 100px;


  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -50px;
  margin-top: -50px;
}
</style>

