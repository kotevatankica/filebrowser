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
        const starting_date = ref();
        const ending_date = ref();


        return {
            files: files, starting_date: starting_date, ending_date: ending_date
        }
    },

    mounted() {
        const username = 'adamantus';
        const password = 'lemon7';
        axios.get('https://php72.afk.mk/images/api.php', {
            headers: {
                authorization: "Basic " + btoa(username + ":" + password)
            }
        }).then((resp) => {
            this.files = resp.data;
        });
    }
}
</script>

<template>

    <div>
        <div>
            <span>Older than:</span>
            <Datepicker v-model="starting_date" />
        </div>
        <br>
        <div>
            <span >Younger than: </span>
            <Datepicker v-model="ending_date" />
        </div>
    </div>

    <FileList v-for="item in files" :item="item" />

</template>

<style scoped>
span {
    color: black;
}
</style>