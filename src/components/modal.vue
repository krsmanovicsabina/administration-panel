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
                    <form id="form" @submit="slotChange">
                        <div class="box-row">
                        <span class="slot">
                            {{slots}}
                        </span>
                        <input v-model="custom" type="text" name="slots" id="slots" @keyup="validateSlots">
                        </div>
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
            available: Number,
        },
        data() {
            return {
                custom: null,
                error: null,
                transition: null,
                dateTitle: "Date:",
                shiftTitle: "Shift:",
                slots:"Slots"
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
            validateSlots: function (e) {
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

<style lang="scss">
    .modal-mask {
        position: absolute;
        top: 65px;
        background-color: transparent;
        z-index: 999;
        width: 100%;
        height: 100%;
        display: table;
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
            display: table-cell;
            vertical-align: middle;
            .modal-container {
                width: 300px;
                margin: 0px auto;
                padding: 20px 30px;
                background-color: $lightColor;
                border-radius: 2px;
                transition: none;
                font-family: $font-family-text;
                height: 330px;
                border: 1px solid $modalBorder;
                box-shadow: none;
                .modal-header h3 {
                    margin-top: 0;
                    color: $baseColor;
                    text-align: left;
                }

                .props-body {
                    padding: 20px 0 20px;
                    margin-bottom: 30px;
                    border-bottom: 1px solid $modalBorder;
                    .box-row {
                        display: table-row;

                        .props {
                            color: $modalBorder;
                            padding-bottom: 8px;
                            display: table-cell;
                            width: 50%;
                            &:first-child {
                                text-align: left;
                                width: 150px;
                            }
                            &:last-child {
                                text-align: left;
                                width: 150px;
                            }
                        }
                    }
                    .box-row {
                        display: table-row;
                        .response-data {
                            display: table-cell;
                            width: 50%;
                            text-transform: capitalize;
                            &:first-child {
                                text-align: left;
                                width: 150px;
                            }
                            &:last-child {
                                text-align: left;
                                width: 150px;
                            }
                        }
                    }
                }

                #form {
                    .box-row {
                        display: table-row;

                        .slot {
                            display: table-cell;
                            text-align: left;
                            width:250px;
                        }

                        input {
                            width: 40px;
                            height: 30px;
                           display:table-cell;
                            text-align: center;
                        }
                    }
                    .modal-footer {
                        padding-top: 20px;
                        p.error-message {
                            font-size: 14px;
                            text-align: left;
                            border-bottom: 1px solid #ABABAB;
                            padding-bottom: 10px;
                        }
                        button {
                            padding: 7px 30px;
                            background-color: #EFEFEF;
                            font-weight: 600;
                            border-color: transparent;
                            margin-top: 20px;
                            &.cancel {
                                float: left;
                            }
                            &.apply {
                                background-color: #E83D4D;
                                color: #ffffff;
                                float: right;
                            }
                        }
                    }
                }
            }
        }
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }

    .modal-enter {
        opacity: 0;
    }

    .modal-leave-active {
        opacity: 0;
    }

    .modal-enter .modal-container,
    .modal-leave-active .modal-container {
        -webkit-transform: scale(1.1);
        transform: scale(1.1);
    }


</style>