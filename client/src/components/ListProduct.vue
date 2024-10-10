<template>
    <div>
        <button @click="handleDelete">Delete Product</button>
        <br>
        <button @click="handleAdd">Add Product </button>
        <br>
        <button @click="handleUpdate">Update Product </button>
    </div>
</template>

<script setup>
import {onMounted, reactive, ref} from 'vue'
const list=reactive([])
const getAllProduct=async()=>{
    try {
        const res=await fetch('http://localhost:3000/products');
        const data=await res.json();
        list.push(...data);
    } catch (error) {
        console.log(error);       
    } 
}
const getAllProductById=async(id)=>{
    try {
        const res=await fetch(`http://localhost:3000/products/${id}`);
        const data=await res.json();
        console.log(data);   
    } catch (error) {
        console.log('Không tìm thấy bản ghi');     
    } 
}
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
const createProduct=async(product)=>{
    try {
        const res=await fetch('http://localhost:3000/products',{
            method:'POST',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(product),
        });
        const data=await res.json();
        console.log('Thêm sản phẩm thành công');
        
        console.log(data);
        
    } catch (error) {
        console.log(error);
        
    }
}
const updateProductById=async(product)=>{
    try {
        const res=await fetch(`http://localhost:3000/products/${product.id}`,{
            method:'PUT',
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(product),
        });
        const data=await res.json();
        console.log('Cập nhật sản phẩm thành công');
        
        console.log(data);
        
    } catch (error) {
        console.log(error);
        
    }
}
onMounted(()=>{
    getAllProduct();
    console.log(list);  
    getAllProductById(1); 

})
const handleDelete=()=>{
    removeProductById(5);
}
const handleAdd=()=>{
    const newProduct={
       id: new Date().getTime(),
       name: 'Product 6',
       price: 10000,
       quantity: 1,
    }
    createProduct(newProduct);
}
const handleUpdate=()=>{
    const updatedProduct={
        id: 5,
        name: 'Product 5 Updated',
        price: 15000,
        quantity: 2,
    }
    updateProductById(updatedProduct);
}
</script>

<style>

</style>