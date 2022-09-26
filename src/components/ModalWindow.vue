<template>
<div class="modal" ref="modal">
    <div class="info-window" @click.stop="">
        <h2>{{'Заказ #'+element.id}}</h2>
        <button class="close-button">
            <img src="../assets/close.svg" alt="" @click="$emit('closeModal')" width="40">
        </button>
        <span class="section-heading">Корзина</span>
        <div class="basket">
          <div class="product" v-for="product in element.products" :key="product.name">
            <span class="name">{{ product.name }}</span>
            <div class="right-side">
              <span class="number">{{ product.number }} шт.</span>
              <span class="price">{{ product.price.toLocaleString('en') }} руб</span>
            </div>
          </div> 
        </div>
        <div class="cost">
          <span class="block-heading">Итого:</span>
          <span class="cost-number">{{ element.cost.toLocaleString('en') }} руб</span>
        </div>
        <div class="buttons">
          <button @click="$emit('toDelivery', element)">
            <span>К отгрузке</span>
        </button>
          <button @click="$emit('delete', element)">
            <span>Отменить</span> 
        </button>
        </div>
    </div>
</div>
</template>

<script>
    export default {
        props: ['element'],
    }
</script>

<style lang="scss" scoped>
    html {
        $mainColor: white;
        $secondColor: black;

        .info-window {
            background-color: $mainColor;

        }
    }

    html[theme='dark'] {
        $mainColor: black;
        $secondColor: white;

        .info-window {
            background-color: $mainColor;
        }
    }

    .modal {
        position: absolute;
        z-index: 1;
        width: 100%;
        height: 100%;
        background-color: #000000aa;
        display: flex;
        justify-content: center;
        align-items: center;

        .info-window {
        width: 30%;
        border: 3px solid;
        position: relative;
        padding: 0 20px 0 20px;

            h2 {
                text-align: center;
            }

            .close-button {
                position: absolute;
                right: 20px;
                top: 20px;
                background-color: white;
            }

            .section-heading {
                font-size: 20px;
            } 

            .basket {
                display: flex;
                flex-direction: column;
                align-items: center;
                width: 100%;
                margin-top: 10px;

                .product {
                    display: flex;
                    flex-direction: row;
                    height: 40px;
                    width: 100%;
                    justify-content: space-between;
                    align-items: center;
                    border: 3px solid;

                    &:nth-child(2n) {
                        border-top: none;
                    }

                    .name {
                        margin-left: 20px;
                    }

                    .right-side {
                        display: flex;
                        flex-direction: row;

                        .number {
                            margin-right: 50px;
                        }

                        .price {
                            margin-right: 10px;
                        }
                    }
                }
            }

            .cost {
                margin-top: 20px;
                display: flex;
                flex-direction: row;
                justify-content: flex-end;
                align-items: flex-end;
                
                .block-heading {
                    margin-right: 10px;
                    font-size: 20px;
                }
            }

            .buttons {
                display: flex;
                flex-direction: row;
                justify-content: space-between;
                margin-top: 20px;
                margin-bottom: 10px;

                button {
                    width: 40%;
                    font-size: 25px;
                    border: 3px solid;

                    &:first-child span {
                        color: #00bb00;
                    }

                    &:last-child span {
                        color: red;
                    }
                }
            }
        }
    }
</style>