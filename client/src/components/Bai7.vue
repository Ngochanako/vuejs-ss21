<template>
   <div>
    <!-- Modal thêm mới sản phẩm -->
     <div v-if="modalAdd" class="modal">
       <v-card width="400" class="form">
         <v-card-text>
           <v-form>
             <v-text-field v-model="product.name" label="Tên sản phẩm" required />
             <v-text-field v-model="product.price" label="Giá sản phẩm" type="number" required />
             <v-text-field v-model="product.quantity" label="Số lượng" type="number" required />
             <v-btn @click="add">Thêm</v-btn>
             <v-btn @click="closeAddModal">Hủy</v-btn>
           </v-form>
         </v-card-text>
       </v-card>
     </div>
    <v-btn @click="handleModalAdd" color="primary">Thêm mới sản phẩm</v-btn>
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
import { onMounted, reactive, ref } from "vue";
const product=reactive({
  id:'',
  name:'',
  price:0,
  quantity:0,
})
const modalAdd=ref(false);
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
//add product
const handleModalAdd=()=>{
  modalAdd.value=true;
}
const closeAddModal=()=>{
  modalAdd.value=false;
}
const add=async()=>{
  product.id=new Date().getTime();
    try {
        const res=await fetch('http://localhost:3000/products',{
            method:'POST',
            headers:{
                'Content-Type':'application/json',
            },
            body:JSON.stringify(product),
        });
        const data=await res.json();
        console.log('Thêm sản phẩm thành công');
        list.push(data);
        modalAdd.value=false;
    } catch (error) {
        console.log(error);
        
    }
}
</script>

<style>
.modal{
  position: fixed;
  background-color: rgb(41, 41, 41,0.7);
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
}
.form{
  margin: auto;
}
</style>