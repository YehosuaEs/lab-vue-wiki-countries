<template>
    <!-- Is the component that we will render via the vue-router's Route and will be receiving the country code (alpha3Code) via the URL. -->
    <!-- Country Details (Bootstrap column) -->
    <!-- ROUTER VIEW -->
    <!--  v-for="(item, idx) in this.countries" :key="idx" -->
    <!-- v-if="item.alpha3Code === this.$route.params.id" -->

    <!-- <div class="col-7" v-for="(item, idx) in this.country" :key="idx"> -->
    <div class="col-7">
        <img
            :src="`https://flagpedia.net/data/flags/icon/72x54/${item.alpha2Code.toLowerCase()}.png`"
            alt="country flag"
            style="width: 300px"
        />
        <h1>
            {{ item.name.common }}
        </h1>
        <table class="table">
            <thead></thead>
            <tbody>
                <tr>
                    <td style="width: 30%">Capital</td>
                    <!-- <td>Paris</td> -->
                    <td>{{ item.capital[0] }}</td>
                </tr>
                <tr>
                    <!-- <td>Area</td>
                    <td>551695 km <sup>2</sup></td> -->
                    <td>Area</td>
                    <td>{{ item.area }} km <sup>2</sup></td>
                </tr>
                <tr>
                    <td>Borders</td>
                    <td>
                        <ul
                            v-for="(countryBorder, idx) in this.item.borders"
                            :key="idx"
                        >
                            <li>
                                <a :href="`${countryBorder}`">{{
                                    this.newObj.get(countryBorder)
                                }}</a>
                            </li>
                            <!-- <li>
                                <router-link :to="`${countryBorder}`">{{
                                    countryBorder
                                }}</router-link>
                            </li> -->
                        </ul>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import countryData from "../../public/countries.json";

export default {
    name: "CountryDetails",
    data() {
        return {
            country: countryData,

            item: countryData.filter(
                (element) => element.alpha3Code === this.$route.params.id
            )[0],

            newObj: new Map(),
            // countryName: countryData.forEach((item) => {
            //     if (item.alpha3Code === this.$route.params.id) {
            //         return (this.name = item.name.common);
            //     }
            // }),
        };
    },
    method: {
        // GetCountry() {
        //     for (let country in this.countryData) {
        //         if (country.alpha3Code === this.$route.params.id) {
        //             return (this.name += country.name.common);
        //         }
        //     }
        // },
    },
    mounted() {
        this.country.map((item) => {
            this.newObj.set(item.alpha3Code, item.name.common);
        });
        console.log(this.newObj.get("IRN"));
        // console.log(this.$route.params.id);
        // console.log(Object.entries(countryData[0].alpha3Code));
        // console.log(countryData[0].capital[0]);
        // console.log(GetCountry());
        // this.country.forEach((item) => {
        //     if (item.alpha3Code === item.borders) {
        //         console.log(item.name.common);
        //         return (this.name = item.name.common);
        //     }
        // });
        // this.item.borders.forEach((item) => {});
        // console.log(this.item);
        // console.log(GetCountry());
    },
};
</script>

<style></style>
