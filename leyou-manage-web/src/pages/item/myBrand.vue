<template>
  <div>

    <template>
      <v-card>
        <v-card-title>
          <v-btn color="primary">新增品牌</v-btn>
          <!--空间隔离组件-->
          <v-spacer />
          <!--搜索框，与search属性关联-->
          <v-text-field label="输入关键字搜索"  append-icon="search" hide-details v-model="key"/>
        </v-card-title>
        <v-data-table
          :headers="headers"
          :items="brands"
          :pagination.sync="pagination"
          :total-items="totalBrands"
          :loading="loading"
          class="elevation-1"
        >
          <template slot="items" slot-scope="props">
            <td class="text-xs-center">{{ props.item.id }}</td>
            <td class="text-xs-center">{{ props.item.name }}</td>
            <td class="text-xs-center"><img v-if="props.item.image" :src="props.item.image" width="130" height="40"/></td>
            <td class="text-xs-center">{{ props.item.letter }}</td>
            <td class="text-xs-center">
              <v-icon small class="mr-2" @click="editItem(props.item)">
                edit
              </v-icon>
              <v-icon small @click="deleteItem(props.item)">
                delete
              </v-icon>
            </td>
          </template>
        </v-data-table>
      </v-card>
    </template>
  </div>
</template>

<script>
  export default {
    name: "myBrand",
    data () {
      return {
        headers: [ // 头信息
          {text: 'id', align: 'center', value: 'id'},
          {text: '名称', align: 'center', value: 'name', sortable: false},
          {text: 'LOGO', align: 'center', value: 'image', sortable: false},
          {text: '首字母', align: 'center', value: 'letter'},
          {text: '操作', align: 'center', value: 'id', sortable: false }
        ],
        brands: [], // 当前页品牌数据
        loading: true, // 是否在加载中
        totalBrands: 0, // 总条数
        pagination: {}, // 分页信息
        key:"",//搜索条件
      }
    },
    created(){
      this.brands = [
        {id:1,name:"oppo",image:"1",letter:"0"},
        {id:1,name:"oppo",image:"1",letter:"0"},
        {id:1,name:"oppo",image:"1",letter:"0"},
        {id:1,name:"oppo",image:"1",letter:"0"},
      ];
      this.totalBrands = 15;


    },
    watch:{
      key(){
        this.pagination.page=1;
      },
      pagination:{
        deep:true,
        handler(){
          this.loadBrands();
        }
      }
    },

    methods: {
      loadBrands() {
        this.loading = true
        this.$http.get("/item/brand/page",{
          params:{
            page:this.pagination.page,//当前页
            rows:this.pagination.rowsPerPage,//每页大小
            sortBy:this.pagination.sortBy,//排序字段
            desc:this.pagination.descending,//是否降序
            key:this.key//搜索条件
          }
        }).then(resp =>{
          this.brands = resp.data.items;
          this.totalBrands = resp.data.total;
        })
        this.loading = false
      }
    }
  }
</script>

<!-- scoped:当前样式只作用于当前组件的节点 -->
<style scoped>

</style>
