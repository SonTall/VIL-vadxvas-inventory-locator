import
<template>
  <div id="app">
    <div class="search-menu">
      <!-- <div class="search-menu__name">
        <label for="inp-seach-by-name"> Tìm kiếm theo tên sản phẩm</label> </br>
        <input
          id="inp-seach-by-name"
          type="text"
          v-model="searchByName"
          placeholder="Tên sản phẩm"
          @input="filterStorageRacks"
        />
      </div> -->
      <div class="search-menu__model">
        <label for="inp-seach-by-model"> Tìm kiếm theo mã sản phẩm</label> </br>
        <input
          id="inp-seach-by-model"
          type="text"
          v-model="searchByModel"
          placeholder="Mã sản phẩm"
          @input="findItem"
        />
      </div>

      <div v-if="filteredRack !== null" :style="'display: block; padding: 10px; background: orange; margin-top: 20px;'">
        <h3>Thông tin sản phẩm:</h3>
        <p>Tên sản phẩm: {{this.filteredRack.name}}</p>
        <p>Mã: {{this.filteredRack.model}}</p>
        <p>Kệ {{this.filteredRack.rack}}</p>
        <p>Tầng {{this.filteredRack.group}}</p>
      </div>
    </div>
    
    <div class="storage-rack-list">
    <div :style="'display: block; margin-top: 20px;'">
      <button :style="'margin-right: 3px;'" @click="getAllItem">Tất cả</button>
      <button :style="'margin-right: 3px;'" v-for="(rack, rackIndex) in rawItems" :key="rackIndex" @click="filterStorageRack(rackIndex)">
        Kệ {{rackIndex + 1}}
      </button>
      
    </div>
    <div v-for="(rack, rackIndex) in this.storageRacks" :key="rackIndex">
      <StorageRack :index="rack.index" :groups="rack.groups" />
    </div>
    </div>
  </div>
</template>

<script>
import StorageRack from "./component/StorageRack.vue";
import inventories from "@/assets/inventories.json";

export default {
  name: "App",
  components: { StorageRack },
  data() {
    return {
      rawItems: inventories,
      storageRacks: inventories,
      filteredRack: null,
      searchByModel: '',
      searchByName: '',
    };
  },
    methods: {
      findItem() {
        if (this.searchByModel != '') {
          for (let rackIndex = 0; rackIndex < this.storageRacks.length; rackIndex++) {
            const groups = this.storageRacks[rackIndex].groups
            for (let groupIndex = 0; groupIndex < groups.length; groupIndex++) {
              const items = groups[groupIndex].items;
              for (let itemIndex = 0; itemIndex < items.length; itemIndex++) {
                const item = items[itemIndex];
                if (item.model !== '' && item.model.toLowerCase() == this.searchByModel.trim().toLowerCase()) {
                    this.filteredRack = {
                      rack: rackIndex + 1,
                      group: groupIndex + 1,
                      model: item.model,
                      name: item.name,
                    };

                    return;
                  }
              }
            }          
          }

          this.filteredRack = null;
        }
      
        //  if (searchByModel != '') {
        //     this.filteredRack = this.filteredRacks.filter(rack => 
        //     rack.model.toLowerCase().includes(this.searchByModel.toLowerCase())
        //   );
        // }
      },

      filterStorageRack(rackIndex) {
        this.getAllItem();
        this.storageRacks = this.storageRacks.filter(rack => rack.index == rackIndex + 1);
      },

      getAllItem() {
        this.storageRacks = inventories;
      }
  },
  mounted() {
    this.filteredRack = null;
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
