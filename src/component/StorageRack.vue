<template>
  <div
    :id="'storge-rack-' + index"
    :style="'margin-top: 30px; border-top: black 1px solid;'"
  >
    <!-- inventory show -->
    <h1>Kệ {{ index }}</h1>

    <div v-for="(group, groupIndex) in groups" :key="groupIndex">
      <h3>Tầng {{ group.index }}</h3>
      <ul>
        <li v-for="(item, itemIndex) in group.items" :key="itemIndex">
          <p>Tên sản phẩm: {{ item.name }}</p>
          <p>Mã: {{ item.model }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "StorageRack",
  props: {
    index: {
      type: Number,
      required: true, // Thêm yêu cầu nếu cần thiết
      default: -1,
    },
    groups: {
      type: Array,
      required: false, // Nếu cần thiết
      default: () => [], // Giá trị mặc định là mảng rỗng
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
