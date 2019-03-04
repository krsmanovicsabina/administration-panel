<template>
    <transition class="modal" name="modal">

        <div class="modal-mask">
            <div class="arrow-up"></div>
            <div class="arrow-white"></div>
            <div class="modal-wrapper">
                <div class="modal-container">
                    <div class="modal-header">
                        <slot name="header">
                        </slot>
                    </div>
                    <div class="props-body">
                        <div class="box-row">
                            <span class="props">
                                {{shiftTitle}}
                            </span>
                            <span class="props">
                                {{dateTitle}}
                            </span>
                        </div>
                        <div class="box-row">
                            <span class="response-data">{{shift}}</span>

                            <span class="response-data">{{date}}</span>
                        </div>
                    </div>
                    <form id="form" @submit.prevent="slotChange">
                        <div class="box-row">
                        <span class="slot">
                            {{slots}}
                        </span>
                            <input v-model="custom" type="text" name="slots" id="slots" @keyup.prevent="validateSlots">
                        </div>
                        <div class="modal-footer">
                            <slot name="footer">
                                <p v-if="error" class="error-message">{{error}}</p>
                                <div class="box-row">
                                    <button class=" cancel" @click="$emit('close')">
                                        Cancel
                                    </button>
                                    <button type="submit" class="apply">
                                        Apply
                                    </button>
                                </div>
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
        props: {
            date: String,
            shift: String,
            available: Number,
        },
        data() {
            return {
                custom: null,
                error: null,
                transition: null,
                dateTitle: "Date:",
                shiftTitle: "Shift:",
                slots: "Slots"
            }
        },
        methods: {
            slotChange() {
                if (this.custom > this.available) {
                    this.error = "Number of slots can only be decreased"
                } else {
                    this.$emit('patchmodal', this.custom)
                    this.error = null;
                    this.$emit('close');
                }
            },
            validateSlots() {
                if (this.custom > this.available) {
                    this.error = "Number of slots can only be decreased"
                } else {
                    this.error = null;
                }
            }

        },
        mounted() {
            this.custom = this.available;
        }
    }
</script>

<style lang="scss">
    .modal-mask {
        position: absolute;
        top: 65px;
        background-color: transparent;
        z-index: 999;
        transition: opacity .3s ease;
        left: -50%;

        .arrow-up {
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 8px solid grey;
            text-align: center;
            left: 50%;
            transform: translateX(-50%);
            position: absolute;
            top: -7px;
        }

        .arrow-white {
            width: 0;
            height: 0;
            border-left: 8px solid transparent;
            border-right: 8px solid transparent;
            border-bottom: 8px solid white;
            text-align: center;
            left: 50%;
            transform: translateX(-50%);
            position: absolute;
            top: -6px;
        }

        .modal-wrapper {
            max-width: 300px;

            .modal-container {
                padding: 20px 25px;
                background-color: $lightColor;
                border-radius: 2px;
                transition: none;
                font-family: $font-family-text;
                min-height: 330px;
                border: 1px solid $modalBorder;
                box-shadow: none;

                .modal-header h3 {
                    color: $baseColor;
                    text-align: left;
                }

                .props-body {
                    padding: 20px 0 20px;
                    margin-bottom: 30px;
                    border-bottom: 1px solid $modalBorder;

                    .box-row {
                        display: flex;

                        .props {
                            color: $modalBorder;
                            padding-bottom: 8px;
                            padding-right: 42px;
                        }
                    }

                    .box-row {
                        display: flex;
                        justify-content: space-between;

                        .response-data {
                            text-transform: capitalize;
                        }
                    }
                }

                #form {
                    .box-row {
                        display: flex;
                        flex-direction: row;
                        justify-content: space-between;

                        .slot {
                            text-align: left;
                        }

                        input {
                            width: 40px;
                            height: 30px;
                            text-align: center;
                        }
                    }

                    .modal-footer {
                        padding-top: 20px;

                        p.error-message {
                            font-size: 14px;
                            text-align: left;
                            border-bottom: 1px solid $tableColor;
                            padding-bottom: 10px;
                        }

                        .box-row {
                            display: flex;
                            flex-direction: row;
                            justify-content: space-between;

                            button {
                                padding: 7px 30px;
                                background-color: #EFEFEF;
                                font-weight: 600;
                                border-color: transparent;
                                margin-top: 20px;

                                &.apply {
                                    background-color: #E83D4D;
                                    color: $lightColor;
                                    margin-left: 15px;
                                }
                            }
                        }
                    }
                }
            }
        }
    }

    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

</style>
