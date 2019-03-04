<template>
    <div class="tank-component">
        <modal data-dismiss="modal" :available="slots" :shift="shift" :date="date" v-if="showModal"
               @close="showModal = false" @patchmodal="sendPatch">

            <h3 slot="header">Change availability</h3>
        </modal>
        <div :class="['circle', { 'circle--busy': isAvailable }, { 'circle--res': isReserved }]" id="show-modal"
             @click="showModal = true">
            <div class="circle-background" :style="`height: ${waterHeight}`"></div>
            <div class="circle-text">{{reserved}}/{{slots}}</div>
        </div>
        <p v-if="hasCustom"><img class="img" title="Shift disabled for this date by the clinic"
                                 src="../assets/pencil.png"></p>

    </div>
</template>

<script>
    import Modal from './Modal'

    export default {
        components: {
            Modal
        },
        props: {
            available: Number,
            reserved: Number,
            custom: Number,
            date: String,
            shift: String,
        },
        data() {
            return {
                showModal: false,

            }
        },
        methods: {
            sendPatch: function (custom) {
                const data = {date: this.date, custom: custom, shift: this.shift}
                this.$emit('patchtank', data)
            },
        },
        computed: {
            slots() {
                /**
                 * Ako uradimo ovako preko computed vrednosti nemamo potrebe za `updated` ili `mounted` hookovima,
                 * zasto sto ce ovo svakako da se updatuje kad god se promeni neka vrednost.
                 */
                return this.hasCustom >= 0 ? this.custom : this.available;
            },
            hasCustom() {
                return typeof this.custom === "number" && this.custom;
            },
            waterHeight() {
                return `${Math.floor(this.reserved / this.slots * 100)}%`
            },
            isAvailable() {
                return this.available === 0
            },
            isReserved() {
                return this.reserved === 0
            },
        }
    }
</script>

<style lang="scss">
    .tank-component {
        padding: 10px 30px;
        position: relative;

        .circle {
            position: relative;
            width: 80px;
            height: 80px;
            border: 1px solid $circleBorder;
            border-radius: 100%;
            text-align: center;
            color: $baseColor;
            display: flex;
            align-items: center;
            overflow: hidden;

            .circle-text {
                position: relative;
                z-index: 1;
                font-size: $font-size-base;
                font-family: $font-family-text;
                font-weight: 600;
                margin: 0;
                width: 100%;
            }

            &--res {
                opacity: .3;
            }

            &--busy {
                border: 1px solid red;
            }

            &-background {
                position: absolute;
                bottom: 0;
                width: 100%;
                background-color: #bec9d4;
            }
        }

        .img {
            position: absolute;
            right: 10px;
            top: 5px;
            width: 15px;
        }
    }

</style>
