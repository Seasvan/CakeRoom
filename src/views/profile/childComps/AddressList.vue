<template>
  <div id="addressList">
    <nav-bar class="nav-bar">
      <template v-slot:left><div class="back"><img src="~assets/img/common/back.svg" alt="" @click="backClick" /></div></template>
      <template v-slot:center><div class="compile">{{navCenter}}</div></template>
    </nav-bar>
    <van-address-list v-model="chosenAddressId" 
                      :list="list"  
                      default-tag-text="默认" 
                      @add="onAdd" 
                      v-if="isShow"/>
    
    <van-address-edit
      :area-list="areaList"
      show-postal
      show-delete
      show-search-result
      show-set-default
      :area-columns-placeholder="['请选择', '请选择', '请选择']"
      @save="onSave"
      @delete="onDelete"
      @change-default = 'onDefault'
      v-else />
  </div>
</template>

<script>
import NavBar from "../../../components/common/navbar/NavBar";
import areaList from 'assets/js/area'

export default {
  name: "AddressList",
  components:{
      NavBar
  },
  data() {
    return {
      chosenAddressId: '1',
      list: [],
      isDefault: '',
      isShow:true,
      navCenter:'收货人地址',
      iid:1,
      areaList,
      addressPush:{},
    }
  },
  methods: {
    onAdd() {
      this.isShow = false 
      this.navCenter = '新增收货人地址'
    },
    // onEdit(item,index) {
    //   this.$toast("编辑地址");
    //   this.isShow = false
    // },
    //编辑
    onSave(content) {
      this.$toast("保存成功");
      console.log(content);
      this.$store.state.adressInfo = content
      this.addressPush = {
          id: this.iid++,
          name: this.$store.state.adressInfo.name,
          tel: this.$store.state.adressInfo.tel,
          address: this.$store.state.adressInfo.province + this.$store.state.adressInfo.county + this.$store.state.adressInfo.city + this.$store.state.adressInfo.addressDetail,
          isDefault: this.isDefault
      }
      this.$store.state.addressPush = this.addressPush
      //console.log(this.addressPush);
      this.list.push(this.$store.state.addressPush)
      // 默认地址改为false
      this.isDefault = false
    },
    //是否要默认地址
    onDefault(value) {
      this.isDefault = value;
    },
    onDelete() {
      this.$toast("删除成功");
      this.isShow = false
    },
    backClick() {
        if (this.isShow == true) {
          this.$router.back()
        } else {
          this.isShow = true
          this.navCenter = '收货人地址'
        }
    },
  },
};
</script>

<style scoped>
.nav-bar {
    background-color: #fff;
    margin-bottom: 20px;
}
.compile {
    color: #000;
}
.back img{
    margin-top: 12px;
    margin-right: 10px;
}
</style>