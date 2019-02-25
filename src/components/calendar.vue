<template>
    <div class="calendar-page">
        <header>
            <button @click="prevWeek" class="main-button">Previous</button>
            <div class="page-title">
                <h1>{{year}}</h1>
                <a href="#" @click="print" class="print">Print weekly calendar</a>
            </div>
            <button v-on:click="nextWeek" class="main-button">Next</button>
        </header>
        <div class="container">
            <div class="table">
                <div class="head">
                    <div class="table-cell"></div>
                    <div class="thead" v-for=" day in days" :key="day.date">{{day.date | moment("ddd, D")}}</div>
                </div>
                <div class="tbody">
                    <div class="table-row">
                        <div class="morning column table-cell">Morning shift</div>
                        <div class="table-cell" v-for="day in days" :key="day.date">
                            <tank @patchtank="patch" shift="morning" v-bind:date="day.date"
                                  v-bind:reserved="day.info.morning.booked"
                                  v-bind:available="day.info.morning.total" v-bind:custom="day.info.morning.custom"/>
                        </div>
                    </div>
                    <div class="table-row">
                        <div class="afternoon column table-cell">Afternoon shift</div>
                        <div class="table-cell" v-for="day in days" :key="day.date">
                            <tank @patchtank="patch" shift="afternoon" v-bind:date="day.date"
                                  v-bind:reserved="day.info.afternoon.booked"
                                  v-bind:available="day.info.afternoon.total"
                                  v-bind:custom="day.info.afternoon.custom"/>
                        </div>
                    </div>
                    <div class="table-row">
                        <div class="evening column table-cell">Evening shift</div>
                        <div class="table-cell" v-for="day in days" :key="day.date">
                            <tank @patchtank="patch" shift="evening" v-bind:date="day.date"
                                  v-bind:reserved="day.info.evening.booked"
                                  v-bind:available="day.info.evening.total" v-bind:custom="day.info.evening.custom"/>
                        </div>
                    </div>
                    <div class="table-row">
                        <div class="night column table-cell">Night shift</div>
                        <div class="table-cell" v-for="day in days" :key="day.date">
                            <tank @patchtank="patch" shift="night" v-bind:date="day.date"
                                  v-bind:reserved="day.info.night.booked"
                                  v-bind:available="day.info.night.total" v-bind:custom="day.info.night.custom"/>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>


    import axios from 'axios';
    import tank from './tank';
    import moment from 'moment';

    export default {
        name: "calendar",
        components: {
            tank,
        },
        data() {
            return {
                days: null,
                week: moment().format('YYYY-MM-DD'),
                year: moment().format('MMMM YYYY'),

            }
        },
        methods: {
            print(value) {
                this.print = value;
                window.print();
            },
            prevWeek: function () {
                this.week = moment(this.week).subtract(1, 'w').format('YYYY-MM-DD')
                axios
                    .get('http://localhost:3000/availability?date=' + this.week)
                    .then(response => (this.days = response.data))
            },
            nextWeek: function () {
                this.week = moment(this.week).add(1, 'w').format('YYYY-MM-DD')
                axios
                    .get('http://localhost:3000/availability?date=' + this.week)
                    .then(response => (this.days = response.data))
            },
            patch: function (value) {
                axios.patch('http://localhost:3000/availability/' + value.date + '/' + value.shift,
                    {
                        "custom": value.custom
                    })
                    .then(response => {
                            axios
                                .get('http://localhost:3000/availability?date=' + this.week)
                                .then(response => (this.days = response.data))

                        }
                    )
                    .catch(error => alert("ERROR " + error))
            }
        },


        mounted() {
            axios
                .get('http://localhost:3000/availability?date=' + this.week)
                .then(response => (this.days = response.data))

        },

    }

</script>

<style lang="scss">
    .calendar-page {
        width: 100%;
        header {
            display: flex;
            justify-content: center;
            margin: 50px 0;
            .main-button {
                background-color: $buttonColor;
                color: $lightColor;
                padding: 10px 25px;
                border-radius: 5px;
                font-weight: 600;
                outline: none;
                border: none;
                align-self: center;
                &:first-child {
                    &:before {
                        content: '<';
                        font-size: 14px;
                        padding-right: 23px;
                        font-weight: 600;
                    }
                }
                &:last-child {
                    &:after {
                        content: '>';
                        font-size: 14px;
                        padding-left: 46px;
                        font-weight: 600;
                    }
                }
            }
            .page-title {
                padding: 0 50px;
                font-size: $font-size-sm;
                text-align: center;
                color: $baseColor;
                .print {
                    display: block;
                    margin-top: 5px;
                    font-size: $font-size-sm;
                    text-decoration: none;
                    color: $linkColor;
                    font-weight: 600;
                }
            }
        }
        .container {
            width: 90%;
            max-width: 1200px;
            margin: 0 auto;
            overflow-x: auto;

            .table {
                width: 100%;
                text-align: center;
                display: flex;
                flex-direction: column;
                .head {
                    display: flex;
                    align-self: center;
                    justify-content: center;
                    border: none;
                    flex-direction: row;
                    margin: 0;
                    flex: 1;
                    .thead {
                        flex-direction: row;
                        border-bottom: 1px solid $tableColor;
                        flex: 1;
                        min-width: 150px;
                        padding: 10px;
                    }
                    .table-cell {
                        border: none;
                        align-self: center;
                        justify-content: center;
                        margin: 0;
                        flex: 1;
                        min-width: 150px;
                    }
                }
                .table-row {
                    display: flex;
                    flex-direction: row;
                    .table-cell {
                        justify-content: center;
                        vertical-align: middle;
                        align-items: center;
                        flex: 1;
                        min-width: 150px;
                        &:first-child {
                            border: none;
                            border-right: 1px solid $tableColor;
                            flex: 1;
                            min-width: 150px;
                        }
                    }
                }
                .tbody {
                    align-self: center;
                    .column {
                        min-height:100px;
                        flex: 1;
                        justify-content: flex-start;
                        text-align:left;
                    }
                }
                .table-cell {
                    align-self: center;
                    border-right: 1px solid $tableColor;
                    border-bottom: 1px solid $tableColor;
                    display: flex;
                    vertical-align: middle;
                    margin: 0;
                    position: relative;
                    flex: 1;
                    min-width: 150px;
                }
            }
        }
    }

</style>