<template>
    <div class="col-sm-6 col-md-4">
        <div
                class="panel"
                :class="[worthSelling == 1 ? 'panel-success' : worthSelling == 0 ? 'panel-warning' : 'panel-danger']"
        >
            <div class="panel-heading">
                <h3 class="panel-title">
                    {{ stock.name }}
                    <small>(Price: {{ stock.price }} | Quantity {{ stock.quantity }} | BoughtAt {{ stock.boughtAt }})</small>
                </h3>
            </div>
            <div class="panel-body">
                <div class="pull-left">
                    <input
                            type="number"
                            class="form-control"
                            placeholder="Quantity"
                            v-model="quantity"
                            :class="{danger: insufficientQuantity}"
                    >
                </div>
                <div class="pull-right">
                    <button class="btn"
                            @click="sellStock"
                            :disabled="insufficientQuantity || quantity <= 0"
                    >{{ insufficientQuantity ? 'No stocks' : 'Sell' }}
                    </button>
                    <button class="btn"
                            @click="sellAllStock"
                    >Sell All
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import {mapActions} from 'vuex';

    export default {
        props: ['stock'],
        data() {
            return {
                quantity: 0
            }
        },
        computed: {
            insufficientQuantity() {
                return this.quantity > this.stock.quantity;
            },
            worthSelling() {
                return this.stock.price > this.stock.boughtAt ? 1 : this.stock.price == this.stock.boughtAt ? 0 : -1;
            }
        },
        methods: {
            ...mapActions({
                placeSellOrder: 'sellStock'
            }),
            sellStock() {
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.quantity
                };
                this.placeSellOrder(order);
                this.quantity = 0;
            },
            sellAllStock() {
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: this.stock.quantity
                };
                this.placeSellOrder(order);
                this.quantity = 0;
            }
        }
    }
</script>

<style scoped>
    .danger {
        border: 1px solid red;
    }
</style>
