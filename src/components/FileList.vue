<script >
import { ref } from 'vue';
export default {
    name: 'file-sub-list',
    // props: ['files','images','src'],

    props: {
        files: {
            type: Array,
            default: () => []
        },
        images: {
            type: Array,
            default: () => []
        }
    },

    setup(props) {


        const files = ref(props.files);
        const show = ref(false);
        const search = ref('');

        return {
            files: files,
            show: show,
            search: search
        }
    },

    computed: {
        filteredFiles() {
            function escapeRegExp(string) {
                return string.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'); // $& means the whole matched string
            }
            let filtered = this.files.filter((file_item) => {

                const match_regex = escapeRegExp(this.search);


                var re = new RegExp(match_regex, "g");

                if (file_item.name.toLowerCase().match(re))
                    return true;
            });

            return filtered;

        }
    },
    methods: {
        onItemClick(item) {

            if (item.shown)
                item.shown = !item.shown;
            else
                item.shown = true;

        }
    }
}
</script>

<template>

    <div>
        <div v-if="files.length > 0">
            <input type="text" name="search-box" class="search-box" placeholder="Search Files" v-model="search">
        </div>
    </div>
    <template v-for="item in filteredFiles" :key="item.name" :item="item">

        <template v-if="item.type == 'dir'">
            <div class="mainName">
                <a href="#" @click="onItemClick(item, $event)">
                    <img src="../assets/icoooon.svg" alt="" class="dir-img"> </a>
                <div>
                    <div class="name">{{ item.name }} </div>
                    <div class="files"> {{ item.data.count_dir_files }} files</div>
                    <div> Size: <span> {{ item.data.get_directory_size }}</span></div>
                </div>
                <div v-if="item.subs && item.subs.length > 0" class="sub" :class="{ 'shown': item.shown }">
                    <file-sub-list :files="item.subs"></file-sub-list>
                </div>
            </div>


        </template>

        <template v-else>
            <div class="mainName">
                <img src="../assets/file.svg" alt="" class="dir-img">
                <div>
                    <div class="name bold">{{ item.name }}</div>
                    <div> Size: <span> {{ item.data.file_size }}</span></div>
                    <div><span>Last modified:</span>{{ item.data.last_modified }}</div>
                </div>
            </div>
        </template>


    </template>
</template>

<style scoped>
.mainName {
    clear: both;
    min-height: 70px;
    border-top: solid 1px #ECE9E9;
    display: flex;
    flex-flow: row wrap;
    margin-left: 15px;
    margin-top: 9px;
    color: black;
}

.name {
    color: rgb(62, 62, 142);
    font-weight: bolder;
}

.files {
    margin-right: 50px;
}

.bold {
    font-weight: 900;
}

.dir-img {
    width: 50px;
    height: 50px;
    margin-right: 10px;
}

.search-box {
    display: block;
    width: 100%;
    height: 4em;
    border: none;
    padding: 0.8em;
    background-color: white;
    color: black;
}

.sub {
    margin-left: 20px;
    border-left: solid 5px #ECE9E9;
    display: none;
}

.sub.shown {
    display: block;
}
</style>