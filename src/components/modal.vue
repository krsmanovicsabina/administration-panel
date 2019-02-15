<template>
    <transition class="modal" name="modal">

        <div class="modal-mask">
            <div class="arrow-up"></div>
            <div class="arrow-white"></div>
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">
                        <slot name="header">
                            default header
                        </slot>
                    </div>
                    <div class="props-body">
                            <span class="props">
                                Shift:
                            </span>
                        <span class="shift">{{shift}}</span>
                        <span class="props">
                                Date:
                            </span>
                        <span class="date">{{date}}</span>
                    </div>
                    <form id="form" @submit="slotChange">
                        <span class="slot">
                            Slots
                        </span>
                        <input v-model="custom" type="text" name="slots" id="slots" @keyup="validateSlots">

                        <div class="modal-footer">
                            <slot name="footer">
                                <p v-if="error" class="error-message">{{error}}</p>
                                <button class=" cancel" @click="$emit('close')">
                                    Cancel
                                </button>
                                <button type="submit" class="apply">
                                    Apply
                                </button>
                            </slot>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    export default {
        name: "modal",
        props: {
            date: String,
            shift: String,
            available: Number

        },
        data() {
            return {
                custom: null,
                error: null
            }
        },
        methods: {
            slotChange: function (e) {
                if (this.custom > this.available) {
                    this.error = "Number of slots can only be decreased"
                } else {
                    this.$emit('patchmodal', this.custom)
                    this.error = null;
                    this.$emit('close');
                }
                e.preventDefault()
            },
            validateSlots:function(e){
                if (this.custom > this.available) {
                    this.error = "Number of slots can only be decreased"
                } else {
                    this.error = null;
                }
                e.preventDefault();
            }

        },
        created: function () {
            this.custom = this.available;
        }
    }
</script>

<style scoped>
    .arrow-up {
        width: 0;
        height: 0;
        border-left: 8px solid transparent;
        border-right: 8px solid transparent;
        border-bottom: 8px solid grey;
        text-align: center;
        left: 50%;
        position: absolute;
        top: -5px;
    }

    .arrow-white {
        width: 0;
        height: 0;
        border-left: 8px solid transparent;
        border-right: 8px solid transparent;
        border-bottom: 8px solid white;
        text-align: center;
        left: 50%;
        position: absolute;
        top: -4px;
    }

    .quantity::-webkit-inner-spin-button,
    .quantity::-webkit-outer-spin-button {
        -webkit-appearance: none !important;
        margin: 0;
    }

    .modal-mask {
        position: absolute;
        top: 65px;
        left: -50%;
        background-color: transparent;
    }

    .modal-container {
        height: 330px;
        border: 1px solid #ABABAB;
        box-shadow: none;
        transition: none;
    }

    .props {
        color: #A6ADB3;
    }

    .props:first-child {
        width: 50%;
        left: 0;
        display: inline-block;
        text-align: left;
    }

    .props:last-child {
        width: 50%;
        display: inline-block;
        text-align: left;

    }

    .props-body {
        padding: 30px 0 30px;
        margin-bottom: 30px;
        border-bottom: 1px solid #ABABAB;
    }

    input {
        width: 40px;
        height: 30px;
        position: absolute;
        right: 33px;

    }

    .slot {
        left: 36px;
        position: absolute;
        text-align: left;
    }

    .modal-footer {
        padding-top: 50px;
    }

    p.error-message {
        font-size: 14px;
        text-align: left;
        border-bottom: 1px solid #ABABAB;
        padding-bottom: 10px;
    }

    button.cancel {
        float: left;
        padding: 7px 30px;
        background-color: #EFEFEF;
        font-weight: 600;
        border-color: transparent;
        margin-top: 20px;

    }

    button.apply {
        padding: 7px 30px;
        background-color: #E83D4D;
        font-weight: 600;
        border-color: transparent;
        margin-top: 20px;
        color: #ffffff;
        float: right;
    }

    .shift {
        display: block;
        left: 36px;
        position: absolute;
        text-transform: capitalize;
    }

</style>