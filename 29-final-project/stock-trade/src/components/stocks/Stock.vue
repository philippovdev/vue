<template>
    <div class="col-sm-6 col-md-4">
        <div class="card border-success mt-4">
            <div class="card-header text-success">
                <h3>{{ stock.name }} <small class="lead">Price: {{ stock.price }}$</small></h3>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="float-left ml-2">
                        <input
                                type="number"
                                class="form-group form-control mr-2"
                                placeholder="Quantity"
                                v-model="quantity"
                                :class="{ danger: insufficientFunds }"
                        >
                    </div>
                    <div class="sm-float-right ml-auto mr-2">
                        <button class="btn btn-success"
                                @click="buyStock"
                                :disabled="insufficientFunds || quantity <= 0 || !Number.isInteger(Number(quantity))"
                                >{{ insufficientFunds ? 'Insufficient Funds' : 'Buy' }}
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Stock",
        props: ['stock'],
        data(){
            return {
                quantity: 0
            }
        },
        computed: {
            funds(){
                return this.$store.getters.funds;
            },
            insufficientFunds(){
                return this.quantity * this.stock.price > this.funds;
            }
        },
        methods: {
            buyStock(){
                const order = {
                    stockId: this.stock.id,
                    stockPrice: this.stock.price,
                    quantity: Number(this.quantity),
                };
                this.$store.dispatch('buyStock', order);
                this.quantity = 0;
            }
        }
    }
</script>

<style scoped>
    small{
        font-size: .8rem;
    }
    input{
        margin-right: 2rem;
    }
    .danger{

    }
</style>