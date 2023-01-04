<template>
    <div class="screen">
        <div class="screen__inner" :style="{
            width: `${((((1200 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) * Math.sqrt(cardsContext.length)}px`
        }">
            <card-Flip v-for="(card, index) in cardsContext" :key="index" :ref="`card-${index}`"
                :imgBackFaceUrl="`images/${card}.jpg`" :card="{ index, value: card }" @onFlip="checkRule($event)"
                :cardsContext="cardsContext" />
        </div>
    </div>
</template>

<script>
import cardFlip from "./card.vue"
export default {
    props: {
        cardsContext: {
            type: Array,
            default: function () {
                return [];
            }
        }
    },
    components: {
        cardFlip,
    },
    data() {
        return {
            rules: []
        }
    },
    methods: {
        checkRule: function (card) {
            if (this.rules.length === 2) return false;
            this.rules.push(card);
            if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
                console.log("YES");
                this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
                this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();
                this.rules = [];

                const disabledElements = document.querySelectorAll(".screen .card.disabled");
                if (disabledElements && disabledElements.length === this.cardsContext.length - 2) {
                    setTimeout(() => {
                        this.$emit("onFinish")
                    }, 1000);
                }
            } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
                console.log("NO");
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
                    this.rules = [];
                }, 800)
            } else {
                console.log("NO");
                return false;
            }
        }
    }
}
</script>

<style scoped>
.screen {
    width: 100%;
    height: 100vh;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 2;
    background-color: var(--dark);
    color: var(--light);
}

.screen__inner {
    display: flex;
    flex-wrap: wrap;
    margin: 2rem auto;
}
</style>