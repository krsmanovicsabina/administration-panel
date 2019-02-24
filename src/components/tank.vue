<template>
    <div class="tank-component">
        <modal data-dismiss="modal" :available="slots" :shift="shift"  :date="date" v-if="showModal" @close="showModal = false" @patchmodal="sendPatch">

            <h3 slot="header">Change availability</h3>
        </modal>
        <div v-cloak class="circle " id="show-modal" @click="showModal = true" :style="water">
            <p v-cloak class="circle-text">{{reserved}}/{{slots}}</p>
        </div>
        <p v-if="custom !== null"><img class="img" title="Shift disabled for this date by the clinic"
                                       src="../assets/pencil.png"></p>

    </div>
</template>

<script>
    import modal from './modal'

    export default {
        name: "tank",
        components: {
            modal
        },
        data() {
            return {
                showModal: false,
                slots: null,
            }
        },
        created: function(){
          this.slots = this.custom !== null ? this.custom : this.available;
        },
        updated: function(){
            this.slots = this.custom !== null ? this.custom : this.available;
        },
        methods: {
            sendPatch: function (custom) {
                var data = {date: this.date, custom: custom, shift: this.shift}
                this.$emit('patchtank', data)
            },
            show() {
                this.$modal.show();

            },
            hide() {
                this.$modal.hide();

            }
        },
        props: {
            available: Number,
            reserved: Number,
            custom: Number,
            date: String,
            shift: String,
        },
        computed: {
            water: function () {

                let filled = (100 - Math.floor(this.reserved /this.slots * 100)) + '%';
                let style = {
                    background: "-webkit-linear-gradient(top, #ffffff " + filled + ",#bec9d4 " + filled + ",#bec9d4 88%,#bec9d4 100%,#bec9d4 100%)"
                };
                if (this.reserved === 0) {
                    style.opacity = "0.3";
                }
                if (this.available === 0) {
                    style.border = "1px solid red";
                }
                return style;
            },

        },
    }
</script>

<style lang="scss">
    .tank-component {
        .circle {
            width: 80px;
            height: 80px;
            border: 1px solid $circleBorder;
            border-radius: 100%;
            text-align: center;
            color: $baseColor;
            display: flex;
            -webkit-align-items: center;
            align-items: center;
            .circle-text {
                font-size: $font-size-base;
                font-family: $font-family-text;
                font-weight: 600;
                margin: 0;
                width: 100%;
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