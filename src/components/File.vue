<script >
import axios from "axios";
import FileList from './FileList.vue';
import { ref } from "vue";
import Datepicker from '@vuepic/vue-datepicker';
import '@vuepic/vue-datepicker/dist/main.css';

export default {
    components: {
        FileList,
        Datepicker
    },
    setup() {
        const files = ref([]);
        const starting_date = ref(null);
        const ending_date = ref(null);


        return {
            files: files, starting_date: starting_date, ending_date: ending_date
        }
    },
    methods: {
        async grabData() {
            let starting_date_local = null;
            let ending_date_local = null;

            if (this.starting_date) {
                starting_date_local = new Date(this.starting_date) / 1000;
            }

            if (this.ending_date) {
                ending_date_local = new Date(this.ending_date) / 1000;
            }
            let params = {
                cut_date: starting_date_local,
                cut_date_end: ending_date_local
            };


            const username = 'adamantus';
            const password = 'lemon7';
            const resp = await axios.get('https://php72.afk.mk/images/api.php', {
                params: params,
                headers: {
                    authorization: "Basic " + btoa(username + ":" + password)
                }
            });
            this.files = resp.data;

        },

        async handleData() {
            await this.grabData();
        }

    },

    async mounted() {

        this.loaded = false;
        await this.grabData();
    }
}
</script>

<template>
    <div>
        <div>
            <span>Older than:</span>
            <Datepicker v-model="starting_date" @update:modelValue="handleData" />
        </div>
        <br>
        <div>
            <span>Younger than: </span>
            <Datepicker v-model="ending_date" @update:modelValue="handleData" />
        </div>
    </div>
    <FileList v-for="item in files" :item="item" />
</template>

<style scoped>
span {
    color: black;
}
</style>