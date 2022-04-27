<template>
    <tr>
        <td><router-link :to="item.product.get_absolut_url">{{ item.product.name }}</router-link></td>
        <td>${{ item.product.price }}</td>
        <td>{{ item.quantity }}
            <a @click="decrementQuantity(item)" class="has-text-dark">-</a>
            <a @click="incrementQuantity(item)" class="has-text-dark">+</a>
        </td>
        <td>${{ getItemTotal(item).toFixed(2) }}</td>
        <td><button class="delete" @click="removeFromCart(item)"></button></td>
    </tr>
</template>

<script>
export default {
    name:'CartItem',
    props:{
        initialItem: Object,
    },
    data(){
        return{
            item: this.initialItem
        }
    },
    methods:{
        getItemTotal(item){
            return item.quantity * item.product.price
        },
        
        updateCart(){
            localStorage.setItem('cart', JSON.stringify(this.$store.state.cart))
        },
        incrementQuantity(item){
            item.quantity +=1

            this.updateCart()
        },
        decrementQuantity(item){
            item.quantity -=1

            if(item.quantity ===0){
                this.$emit('removeFromCart',item)
            }

            this.updateCart()
        },
        removeFromCart(item){
            this.$emit('removeFromCart',item)
            this.updateCart()

        }

    }
}
</script>

<style>

</style>