<template>
    <transition-group name="fade" tag="div" @beforeEnter="before" @enter="enter" @leave="leave">
        <div class="row mb-3 align-items-center" v-for="(item, index) in showItem" :key="item.id" :data-index="index">
            <div class="col-sm-4 mb-4">
                <img :src="item.image" :alt="item.name" class="img-fluid d-block">
                <div class="d-flex justify-content-center m-auto">
                    <label class="mr-2 mt-2">Add to cart</label>
                    <button class="btn btn-info" @click="$emit('add-item', item)">+</button>
                  </div>
            </div>
            <div class="col">
                <h3 class="text-info">{{ item.name }}</h3>
                <p class="mb-0">{{ item.description }}</p>
                <div class="h5 float-right">
                    <i class="fas fa-dollar-sign"></i>
                    <price :value="Number(item.price)"></price>
                </div>
            </div>
        </div>
    </transition-group>
</template>

<script>
import Price from "./Price.vue";

export default {
    name:"product-list",
    components:{
Price
    },
    props: ["products", "maximum"],
    computed:{
        showItem: function() {
            let max = this.maximum;
            return this.products.filter(function(item){
                return item.price <= max;
            });
        }
    },
    methods: {
    before: function (el) {
        el.className = "d-none";
      },
      enter: function (el) {
        var delay = el.dataset.index * 100;
        setTimeout(function () {
          el.className = "row d-flex mb-3 align-items-center animated slideInUp";
        }, delay);
      },
      leave: function (el) {
        var delay = el.dataset.index * 100;
        setTimeout(function () {
          el.className =
            "row d-flex mb-3 align-items-center animated zoomOutDown";
        }, delay);
      },
  },
}

</script>