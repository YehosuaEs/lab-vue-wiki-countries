<template>
    <!-- Is the component that we will render via the vue-router's Route and will be receiving the country code (alpha3Code) via the URL. -->
    <div v-if="this.alpha2Code && this.borders">
        <img
            :src="`https://flagcdn.com/${alpha2Code.toLowerCase()}.svg`"
            alt="country flag"
            style="width: 300px"
        />
        <h1>
            {{ name }}
        </h1>
        <table class="table">
            <thead></thead>
            <tbody>
                <tr>
                    <td style="width: 30%">Capital</td>

                    <td>{{ capital }}</td>
                </tr>
                <tr>
                    <td>Area</td>

                    <td>{{ area }} km <sup>2</sup></td>
                </tr>
                <tr>
                    <td>Borders</td>
                    <td v-if="borders.length === 0">
                        <ul>
                            <li>This Coutry has not borders</li>
                        </ul>
                    </td>
                    <td v-else>
                        <ul v-for="(countryBorder, idx) in borders" :key="idx">
                            <li>
                                <router-link :to="`${countryBorder}`">
                                    {{ this.newObj.get(countryBorder) }}
                                </router-link>
                            </li>
                        </ul>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <!-- -------------------SPINNER------------------- -->
    <div v-else class="col-12 text-center pt-5">
        <Spinner text="Loading info about Country... " />
    </div>
</template>

<script>
import Spinner from "./Spinner.vue";
export default {
    name: "CountryDetails",
    components: { Spinner },
    data() {
        return {
            url: "https://ih-countries-api.herokuapp.com/countries/",
            countryData: {},

            name: "",
            area: "",
            capital: "",
            borders: [],
            alpha3Code: "",
            alpha2Code: "",

            newObj: new Map(),
            // item: this.countryData.filter(
            //     (element) =>
            //         element.alpha3Code === this.$route.params.alpha3Code
            // )[0],
        };
    },
    methods: {
        async getCountryByAlphaCode() {
            try {
                this.alpha3Code = this.$route.params.alpha3Code;
                // const response = await fetch(this.url);
                const [response, responseArray] = await Promise.all([
                    fetch(`${this.url}${this.alpha3Code}`),
                    fetch(`${this.url}`),
                ]);
                const finalResponse = await response.json();
                const finalResponseArray = await responseArray.json();
                this.name = finalResponse.name.common;
                this.area = finalResponse.area;
                this.capital = finalResponse.capital[0];
                this.borders = finalResponse.borders;
                this.alpha2Code = finalResponse.alpha2Code;
                finalResponseArray.map((item) => {
                    this.newObj.set(item.alpha3Code, item.name.common);
                });
            } catch (err) {
                console.log(err.name + " in Country Details");
            } finally {
                console.log(
                    `finally loading after try{{} and catch} in Country Details`
                );
            }
        },
    },
    beforeCreate() {},
    beforeMount() {
        this.getCountryByAlphaCode();
    },
    mounted() {},

    computed: {
        countryCode() {
            return this.$route.params.alpha3Code;
        },
    },

    watch: {
        countryCode(newCountryCode) {
            this.getCountryByAlphaCode();
        },
    },
};
</script>

<style scoped>
li {
    list-style-type: none;
    padding: 0;
    margin: 0;
}
</style>
