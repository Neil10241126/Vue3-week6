<template>
  <div>
    <div class="h1 text-primary">
      這是前台
    </div>
    <div class="btn-group">
      <router-link to="/home" class="btn btn-outline-primary">首頁</router-link>
      <router-link to="/products" class="btn btn-outline-primary">產品列表</router-link>
      <router-link to="/cart" class="btn btn-outline-primary me-2">購物車</router-link>
    </div>
      <router-link to="/login" class="btn btn-outline-success">登入</router-link>
  </div>
  <hr>
  <router-view></router-view>
</template>
