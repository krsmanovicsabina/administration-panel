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
                        <input  v-model="custom" type="text" name="slots" id="slots" @keyup="validateSlots">

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
                error: null,
                transition: null
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
    .modal-mask {
        position: fixed;
        z-index: 9998;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, .5);
        display: table;
        transition: opacity .3s ease;
    }
    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }
    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }
    .modal-header h3 {
        margin-top: 0;
        color: #42b983;
    }
    .modal-body {
        margin: 20px 0;
    }
    .modal-default-button {
        float: right;
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
    }.modal-mask {
         position: fixed;
         z-index: 9998;
         top: 0;
         left: 0;
         width: 100%;
         height: 100%;
         background-color: rgba(0, 0, 0, .5);
         display: table;
         transition: opacity .3s ease;
     }
    .modal-wrapper {
        display: table-cell;
        vertical-align: middle;
    }
    .modal-container {
        width: 300px;
        margin: 0px auto;
        padding: 20px 30px;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
        transition: all .3s ease;
        font-family: Helvetica, Arial, sans-serif;
    }

    .modal-header h3 {
        margin-top: 0;
        color: #42b983;
    }

    .modal-body {
        margin: 20px 0;
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
        padding-bottom: 8px;
    }

    .props:first-child {
        width: 50%;
        left: 0;
        display: inline-block;
        text-align: left;
        margin-left: -75px;
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
        text-align:center;

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
    .date{
        display: block;
        position: absolute;
        right: 66px;
    }


</style>