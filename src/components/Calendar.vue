<template>
    <div class="calendar-page">
        <header>
            <button @click="prevWeek" class="main-button">Previous</button>
            <div class="page-title">
                <h1>{{year}}</h1>
                <button @click="print" class="print">Print weekly calendar</button>
            </div>
            <button @click="nextWeek" class="main-button">Next</button>
        </header>
        <div class="container">
            <table>
                <thead>
                <tr>
                    <th></th>
                    <th v-for="(date, i) in transformedData.date" :key="i">{{ date }}</th>
                </tr>
                </thead>
                <tbody>
                <tr>
                    <td>Morning shift</td>
                    <td v-for="(day, i) in transformedData.morning" :key="i">
                        <tank @patchtank="patch" shift="morning" :date="transformedData.date[i]"
                              :reserved="day.booked"
                              :available="day.total" :custom="Number.parseInt(day.custom, 10)"/>
                    </td>
                </tr>
                <tr>
                    <td>Afternoon shift</td>
                    <td v-for="(day, i) in transformedData.afternoon" :key="i">
                        <tank @patchtank="patch" shift="afternoon" :date="transformedData.date[i]"
                              :reserved="day.booked"
                              :available="day.total" :custom="Number.parseInt(day.custom, 10)"/>
                    </td>
                </tr>
                <tr>
                    <td>Evening shift</td>
                    <td v-for="(day, i) in transformedData.evening" :key="i">
                        <tank @patchtank="patch" shift="evening" :date="transformedData.date[i]"
                              :reserved="day.booked"
                              :available="day.total" :custom="Number.parseInt(day.custom, 10)"/>
                    </td>
                </tr>
                <tr>
                    <td>Night shift</td>
                    <td v-for="(day, i) in transformedData.night" :key="i">
                        <tank @patchtank="patch" shift="night" :date="transformedData.date[i]"
                              :reserved="day.booked"
                              :available="day.total" :custom="Number.parseInt(day.custom, 10)"/>
                    </td>
                </tr>

                </tbody>
            </table>
        </div>
        <div v-if="error" class="error">
            <p>{{ error }}</p>
            <button @click="error = null" class="error__close">X</button>
        </div>
    </div>
</template>

<script>

    import axios from 'axios';
    import Tank from './Tank';
    import moment from 'moment';

    export default {
        components: {
            Tank,
        },
        data() {
            return {
                days: [],
                week: moment().format('YYYY-MM-DD'),
                year: moment().format('MMMM YYYY'),
                error: null,

            }
        },
        computed: {
            transformedData() {
                /**
                 * Evo jedna ideja kako napraviti transformaciju. Posto su nam potrebni redovi ajde da za svaki
                 * red napravimo niz i u njega ubacujemo odgovarajuce vrednosti. U sustini ovo je neka vrsta
                 * grupisanja po kljucu.
                 *
                 * @type {{date: Array, afternoon: Array, night: Array, evening: Array, morning: Array}}
                 */
                const data = {
                    date: [],
                    morning: [],
                    afternoon: [],
                    evening: [],
                    night: [],
                };

                /**
                 * Najprostije resenje je da prodjemo kroz niz i za svaki datum ubacimo odgovaracuje vrednosti.
                 *
                 */

                this.days.forEach(item => {
                    data.date.push(item.date)
                    data.morning.push(item.info.morning)
                    data.afternoon.push(item.info.afternoon)
                    data.evening.push(item.info.evening)
                    data.night.push(item.info.night)
                })

                return data;
            }
        },
        methods: {
            print() {
                window.print();
            },
            prevWeek() {
                this.week = moment(this.week).subtract(1, 'w').format('YYYY-MM-DD')
                this.getWeek();

            },
            nextWeek() {
                this.week = moment(this.week).add(1, 'w').format('YYYY-MM-DD')
                this.getWeek();

            },
            getWeek() {
                axios
                    .get(`http://localhost:3000/availability?date=${this.week}`)
                    .then(response => (this.days = response.data))
            },
            async patch(value) {
                try {
                    await axios.patch(`http://localhost:3000/availability/${value.date}/${value.shift}`, {custom: value.custom})
                    const days = await axios.get(`http://localhost:3000/availability?date=${this.week}`)
                    this.days = days.data
                } catch (error) {
                    this.error = error
                }

            }
        },

        mounted() {
            axios
                .get(`http://localhost:3000/availability?date=${this.week}`)
                .then(response => this.days = response.data)

        },

    }

</script>

<style lang="scss" scoped>

    $error-title-font-size: 14px;
    $error-text-font-size: 12px;
    $error-top-offset: 20px;
    $error-padding-y: 1em;
    $error-padding-x: 1em;
    $error-border-radius: 0.25em;

    .error {
        position: fixed;
        top: $error-top-offset;
        left: 50%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: $error-padding-y $error-padding-x;
        min-width: 600px;
        max-width: 70%;
        transform: translateX(-50%);

        border-radius: $error-border-radius;
        color: #FFFFFF;
        background: linear-gradient(135deg, #ED6273, #E6475A);
        box-shadow: 0 8px 16px -8px #ED6273;

        z-index: 17;
    }


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
                            padding: 0 40px;
                        }
                    }
                }

                .tbody {
                    align-self: center;

                    .column {
                        min-height: 100px;
                        flex: 1;
                        justify-content: flex-start;
                        text-align: left;
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
                }
            }
        }
    }

</style>
