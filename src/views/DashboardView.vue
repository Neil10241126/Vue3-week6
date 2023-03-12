<template>
  <div class="h1 text-warning">
    後台頁面
  </div>
  <div class="btn-group">
    <router-link to="/home" class="btn btn-outline-warning">回到前台</router-link>
    <router-link to="/admin/products" class="btn btn-outline-warning">後台產品</router-link>
  </div>
  <hr>
  <router-view></router-view>
</template>
