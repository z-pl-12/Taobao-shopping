<script setup>
</script>

<template>
  <router-view />
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: Arial, sans-serif;
  background-color: #fff;
  width: 100%;
  min-height: 100vh;
  margin: 0;
  padding: 0;
}

.container {
  width: 100%;
  max-width: 100%;
  margin: 0;
  padding: 0 20px;
}

.header {
  background-color: #fff;
  border-bottom: 1px solid #e0e0e0;
  padding: 10px 0;
  position: sticky;
  top: 0;
  z-index: 1000;
}

.footer {
  background-color: #fff;
  border-top: 1px solid #e0e0e0;
  padding: 20px 0;
  margin-top: 20px;
}

/* 通用按钮样式 */
.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 14px;
}

.btn-primary {
  background-color: #ff4400;
  color: #fff;
}

.btn-primary:hover {
  background-color: #ff6600;
}

/* 通用输入框样式 */
.input {
  padding: 6px 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
  font-size: 14px;
}

/* 通用商品卡片样式 */
.product-card {
  background-color: #fff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  transition: box-shadow 0.3s ease;
}

.product-card:hover {
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
}

.product-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.product-info {
  padding: 12px;
}

.product-title {
  font-size: 14px;
  line-height: 1.4;
  margin-bottom: 8px;
  color: #333;
}

.product-price {
  font-size: 16px;
  font-weight: bold;
  color: #ff4400;
  margin-bottom: 8px;
}

.product-sales {
  font-size: 12px;
  color: #999;
}

/* 通用布局样式 */
.flex {
  display: flex;
}

.flex-between {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.flex-center {
  display: flex;
  align-items: center;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 16px;
}

/* 通用间距样式 */
.mt-10 {
  margin-top: 10px;
}

.mt-20 {
  margin-top: 20px;
}

.mb-10 {
  margin-bottom: 10px;
}

.mb-20 {
  margin-bottom: 20px;
}

/* 通用颜色样式 */
.text-primary {
  color: #ff4400;
}

.text-gray {
  color: #999;
}

/* 通用边框样式 */
.border {
  border: 1px solid #e0e0e0;
}

.border-bottom {
  border-bottom: 1px solid #e0e0e0;
}

/* 通用阴影样式 */
.shadow {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
</style>