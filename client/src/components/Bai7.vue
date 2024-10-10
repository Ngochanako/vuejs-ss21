<template>
   <div>
    <v-btn color="primary">Thêm mới sản phẩm</v-btn>
    <div>
  <v-table
    fixed-header
  >
    <thead>
      <tr>
        <th class="text-left">
          Index
        </th>
        <th class="text-left">
          Tên
        </th>
        <th class="text-left">
          Giá
        </th>
        <th class="text-left">
          Số lượng
        </th>
        <th class="text-left">
          Chức năng
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="(item,index) in list"
        :key="index"
      >
        <td>{{ index+1 }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.price}}</td>
        <td>{{ item.quantity }}</td>
        <td>
            <v-row>
                <v-col cols="3">
                    <v-btn color="primary" @click="updateProduct(item.id)">Sửa</v-btn>
                </v-col>
                <v-col cols="3">
                    <v-btn color="error" @click="deleteProduct(item.id)">Xóa</v-btn>
                </v-col>
            </v-row>
        </td>
      </tr>
    </tbody>
  </v-table>
</div>
   </div>
</template>

<script setup>
import { onMounted, reactive } from "vue";

const list=reactive([]);
const getAllProduct=async()=>{
    try {
        const res=await fetch('http://localhost:3000/products');
        const data=await res.json();
        list.push(...data);
    } catch (error) {
        console.log(error);       
    } 
}
onMounted(()=>{
  getAllProduct();
})
const removeProductById=async(id)=>{
    try {
        const res=await fetch(`http://localhost:3000/products/${id}`,{
            method:'DELETE',
        });
        const data=await res.json();
        console.log('Xóa sản phẩm thành công');
        
        console.log(data);
        
    } catch (error) {
        console.log(error);
        
    }
}
const deleteProduct=(id)=>{
    window.confirm('Are you sure you want to delete this product');
    removeProductById(id);
    list.splice(list.findIndex(item=>item.id===id),1);
}
</script>

<style>

</style>