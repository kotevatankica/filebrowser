<script>
import { ref } from 'vue';
export default {
    name: 'ShowImage',
    props: {
        image: {
            type: Object,
            default: null
        }
    },

    setup(props) {
        const image = ref(props.image);
        const shown = ref(false);
        return {
            image, shown
        }
    },
    computed: {
        computedLink() {
            if (this.shown)
                return this.image.full_url;
            else
                return '';
        }
    },
    methods: {
        showImage() {

            this.shown = true;

        },
        closeImage() {
            this.shown = false;

        }
    },
}

</script>

<template>
    <a href="#" @click.prevent="showImage" class="link-interaction">Show Image</a>
    <div :class="{ 'full-screen': true, 'shown': shown }">
        <div class="image-controls">
            <a href="#" @click.prevent="closeImage">Close Image</a>
        </div>
        <div class="image-controls">
            <a :href="computedLink" target="_blank">{{ computedLink }}</a>
        </div>
        <img :src="computedLink" :alt="image.name" class="qr" />
    </div>

</template>

<style>
.link-interaction {
    text-align: center;
    margin-left: 1rem;

}

.full-screen {
    width: 100vw;
    height: 100vh;
    position: fixed;
    top: 0;
    left: 0;
    display: none;
    flex-direction: column;
    background: rgb(29, 29, 29);
}

.full-screen.shown {
    display: flex;
}

.image-controls {
    display: flex;
    justify-content: center;
    padding-top: 10px;
}

.qr {
    display: block;
    margin: auto;
    max-height: 70vh;
    max-width: 95vw;
}
</style>