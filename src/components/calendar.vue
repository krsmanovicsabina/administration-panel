<template>
    <div class="calendar-page">
        <header>
            <button v-on:click="prevWeek" class="previous-button">Previous</button>
            <button v-on:click="nextWeek" class="next-button">Next</button>
            <h1>{{year}}</h1>
            <a href="#" @click="print" class="print">Print weekly calendar</a>
        </header>
        <modal></modal>

        <table>
            <thead>
            <td></td>
            <td class="thead" v-for=" day in days" :key="day.date">{{day.date | moment("ddd, D")}}</td>
            </thead>
            <tbody>
            <tr>
                <td class="morning">Morning shift</td>
                <td v-for="day in days" :key="day.date">
                    <tank @patchtank="patch" shift="morning" v-bind:date="day.date"
                          v-bind:reserved="day.info.morning.booked"
                          v-bind:available="day.info.morning.total" v-bind:custom="day.info.morning.custom"/>
                </td>
                <td class="afternoon">Afternoon shift</td>
                <td v-for="day in days" :key="day.date">
                    <tank @patchtank="patch" shift="afternoon" v-bind:date="day.date"
                          v-bind:reserved="day.info.afternoon.booked"
                          v-bind:available="day.info.afternoon.total" v-bind:custom="day.info.afternoon.custom"/>
                </td>
                <td class="evening">Evening shift</td>
                <td v-for="day in days" :key="day.date">
                    <tank @patchtank="patch" shift="evening" v-bind:date="day.date"
                          v-bind:reserved="day.info.evening.booked"
                          v-bind:available="day.info.evening.total" v-bind:custom="day.info.evening.custom"/>
                </td>
                <td class="night">Night shift</td>
                <td v-for="day in days" :key="day.date">
                    <tank @patchtank="patch" shift="night" v-bind:date="day.date"
                          v-bind:reserved="day.info.night.booked"
                          v-bind:available="day.info.night.total" v-bind:custom="day.info.night.custom"/>
                </td>
            </tr>
            </tbody>
        </table>
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
            print(value){
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

<style scoped>

    h1 {
        font-size: 22px;
    }
    header {
        text-align: center;
        margin: 50px 0 50px;
    }
    header button {
    }
    .previous-button {
        position: absolute;
        margin-top: 15px;
        margin-left: -300px;
        background-color: #5495C4;
        color: #ffffff;
        width: 130px;
        height: 40px;
        border-radius: 5px;
        font-weight: 600;
        outline: none;
        border: none;
    }
    .next-button {
        position: absolute;
        margin-left: 170px;
        margin-top: 15px;
        width: 130px;
        height: 40px;
        color: #ffffff;
        background-color: #5495C4;
        border-radius: 5px;
        font-weight: 600;
        outline: none;
        border: none;
    }
    table {
        text-align: center;
        margin: 0 auto;
    }

    thead {
        display: flex;
        align-self: center;
        justify-content: center;
        border: none;
    }
    thead td {
        border: none;
        align-self: center;
        justify-content: center;
        height: 50px;
    }
    td {
        width: 150px;
        align-self: center;
        border: 1px solid #F4F2F2;
        height: 100px;
        margin: 0 auto;
        display: flex;
        vertical-align: middle;
        justify-content: center;
        position: relative;
    }
    tr {
        display: grid;
        grid-template-columns: repeat(8, 0fr);
    }
    tr td {
        padding-top: 8px;
    }
    tbody {
        align-self: center;
        margin: 0 auto;
    }
    .morning, .afternoon, .evening, .night {
        border: none;
        padding-top: 40px;
    }
    .previous-button::before {
        content: '<';
        font-size: 14px;
        padding-right: 23px;
        font-weight: 600;

    }
    .next-button::after {
        content: '>';
        font-size: 14px;
        padding-left: 46px;
        font-weight: 600;

    }

    .print {
        text-decoration: none;
        color: #8FBAD9;
        font-weight: 600;
        cursor: pointer;
    }
</style>