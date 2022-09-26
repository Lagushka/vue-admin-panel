<template>
    <div class="order-additional">
        <div class="order__body">
            <div class="left-side">
                <span class="name">
                    {{delivery ? 'Отгрузка #'+element.id : 'Заказ #'+element.id}}
                </span>
                <span v-if="delivery" class="delivery-order">
                    {{'Заказ #'+element.order}}
                </span>
            </div>
            <div class="right-side">
                <span v-if="!delivery">
                    {{element.cost.toLocaleString('en')}} руб.
                </span>
                <span v-if="delivery">
                    {{element.date}}
                </span>
                <button @click.stop="changeValue">
                    <img src="../assets/more.svg" alt="" width="35">
                </button>
            </div>
        </div>
        <div class="menu" v-if="element.menuVisibility">
            <button class="more" v-if="!delivery" @click="$emit('showInfo', element)">
                <span>Подробнее</span> 
            </button>
            <button class="to-delivery" v-if="!delivery" @click="$emit('toDelivery', element)">
                <span>К отгрузке</span>
            </button>
            <button class="cancel" @click="$emit('removeElement', element)">
                <span>Отменить</span>
            </button>
        </div>
    </div> 
</template>

<script>
    export default {
        props: ['element', 'delivery'],
        methods: {
            changeValue() {
                this.$emit('changeVisibility');
            }
        }
    }
</script>

<style lang="scss" scoped>
    .order-additional {
        position: relative;

        .order__body {
            width: 500px;
            height: 60px;
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;
            border: 3px solid;
            padding: 0 1em 0 1em;

            .left-side {
                display: flex;
                flex-direction: row;
                width: 300px;
                align-items: center;

                .delivery-order {
                    margin-left: 20px;
                    font-size: 14px;
                }
            }

            .right-side {
                display: flex;
                flex-direction: row;
                align-items: center;

                button {
                    position: relative;
                    margin-left: 20px;
                    background-color: white;
                }
            }
        }

        .menu {
            display: flex;
            position: absolute;
            z-index: 1;
            flex-direction: column;
            right: 0;
            top: 70px;

            button {
                position: relative;
                top: 0;
                width: 150px;
                font-size: 20px;
                border-color: inherit;
            }

            .to-delivery {
                    border-top: none;
                    border-bottom: none;
                    span {
                        color: #00bb00;
                    }
                }

            .cancel {
                span {
                    color: red;
                }
            }
        }
    }
</style>