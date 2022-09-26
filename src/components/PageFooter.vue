<template>
    <footer>
        <span>Бондарев Борислав, 211-321</span>
        <div class="scroll">
            <span>Scroll to top</span>
            <button @click="scrollToTop">
                <img src="../assets/up.svg" alt="" width="40">
            </button>
        </div>
    </footer>
</template>

<script>
    export default {
        methods: {
            scrollToTop() {
                window.scrollTo(0, 0);
            }
        },
    }
</script>

<style lang="scss" scoped>
    footer {
        margin-top: 10px;
        box-sizing: border-box;
        width: 100%;
        border: 3px solid;
        height: 70px;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        padding: 0 20px 0 20px;

        .scroll {
            display: flex;
            flex-direction: row;
            align-items: center;

            span {
                color: #00C4E0;
                font-size: 18px;
                margin-right: 10px;
            }

            button {
                border: 3px solid;
                background-color: white;
            }
        }
    }
</style>