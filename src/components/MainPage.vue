<template>
    <main class="component-fluid">
        <header>
            <div class="header__content">
                <p class="header__title">IP Address Tracker</p>
                <form
                    class="header__input row"
                    @submit.prevent="(e) => getIPInfo(e)"
                >
                    <input
                        type="text"
                        placeholder="enter your ip.."
                        class="col-4"
                        v-model="ip"
                        required
                    />
                    <button class="input__arrow" type="submit">
                        <img src="../assets/icon-arrow.svg" />
                    </button>
                    <p class="error__msg" v-if="error">{{ error }}</p>
                </form>
            </div>
        </header>
        <section class="ip__info" v-if="!ipData && !loading">
            <p class="empty__state">Please Enter An IP Address</p>
        </section>
        <section class="ip__info" v-if="loading">
            <PulseLoader :loading="loading" color="gray" />
        </section>
        <section class="ip__info" v-if="ipData && !loading">
            <div class="ip__info__chunk">
                <p class="ip__info__title">IP ADDRESS</p>
                <p class="ip__info__result">{{ ipData?.ip }}</p>
            </div>
            <div class="ip__info__chunk">
                <p class="ip__info__title">LOCATION</p>
                <p class="ip__info__result">
                    {{ ipData?.location?.city }},
                    {{ ipData?.location?.country }}
                    {{ ipData?.location?.geonameId }}
                </p>
            </div>
            <div class="ip__info__chunk">
                <p class="ip__info__title">TIMEZONE</p>
                <p class="ip__info__result">{{ ipData?.location?.timezone }}</p>
            </div>
            <div class="ip__info__chunk">
                <p class="ip__info__title">ISP</p>
                <p class="ip__info__result">{{ ipData?.isp }}</p>
            </div>
        </section>
    </main>
    <section class="map__container row">
        <div class="col-12">
            <Map
                :lat="ipData?.location?.lat"
                :lng="ipData?.location?.lng"
                :nice="hello"
            />
        </div>
    </section>
</template>
<script>
import axios from 'axios';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import Map from './Map.vue';
export default {
    name: 'MainPage',
    data() {
        return {
            ip: '',
            ipData: '',
            loading: false,
            error: '',
            center: [37, 7749, -122, 4194],
        };
    },
    components: {
        PulseLoader,
        Map,
    },
    methods: {
        getIPInfo(e) {
            if (
                /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/.test(
                    e.target[0].value
                )
            ) {
                this.error = '';
                document
                    .getElementsByTagName('button')[0]
                    .classList.remove('error__outline');
                axios(
                    `https://geo.ipify.org/api/v2/country,city?apiKey=at_txjvfLQeGNvNJ6VzBdO5UjJBdowCy&ipAddress=${this.ip}`
                ).then((data) => {
                    this.loading = true;
                    setTimeout(() => {
                        this.loading = false;
                        this.ipData = data?.data;
                    }, 2000);
                });
            } else {
                document
                    .getElementsByTagName('button')[0]
                    .classList.add('error__outline');
                this.error = 'Invalid Ip Address';
            }
        },
    },
};
</script>
<style lang="scss" scoped>
@import '../styles/page.scss';
</style>
