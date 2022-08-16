<template>
    <!-- Displays the list of links with the country names. Each link should be a vue-router-dom router-link which we will use to send the country code (alpha3Code) via the URL. -->
    <!-- Bootstrap container wrapper div -->
    <div class="container">
        <!-- Bootstrap row wrapper div -->
        <div class="row" v-if="this.countries">
            <!-- Countries List (Bootstrap column) -->
            <div class="col-5" style="max-height: 90vh; overflow: scroll">
                <div
                    class="list-group"
                    v-for="(item, idx) in countries"
                    :key="idx"
                >
                    <router-link
                        class="list-group-item list-group-item-action"
                        :to="`/list/${item.alpha3Code}`"
                    >
                        <img
                            :src="`https://flagpedia.net/data/flags/icon/72x54/${item.alpha2Code.toLowerCase()}.png`"
                            :alt="'flag from ' + item.name.official"
                        />
                        <p>{{ item.name.common }}</p>
                    </router-link>
                </div>
            </div>
            <div class="col-8">
                <!-- <CountriesDetails /> -->
                <router-view />
            </div>
        </div>
        <!-- -------------------SPINNER------------------- -->
        <div v-else class="row">
            <Spinner text="Loading page..." />
        </div>
    </div>
</template>

<script>
import Spinner from "./Spinner.vue";

export default {
    name: "CountryList",
    components: { Spinner },
    data() {
        return {
            countries: null,
            url: "https://ih-countries-api.herokuapp.com/countries",
        };
    },
    methods: {
        async fetchCountries() {
            // try {
            const response = await fetch(this.url);
            const finalResponse = await response.json();
            // console.log(finalResponse);
            this.countries = finalResponse.sort((a, b) =>
                a.name.official.localeCompare(b.name.official)
            );
            // } catch (err) {
            //     console.log(err);
            //     console.log(error.name === "AbortError");
            // } finally {
            //     console.log(`finally loading after try{{} and catch}`);
            // }
        },
    },

    created() {
        this.fetchCountries();
    },

    mounted() {
        // console.log(this.countries[0].alpha2Code);
    },
};
</script>

<style></style>
