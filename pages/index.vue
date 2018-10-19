<template>
    <div>
        <div class="features">
            <div v-for="feature in features" :key="feature.title" class="feature">
                <p class="feature-title">{{ feature.title }}</p>
                <hr class="divider"/>
                <p class="desc" v-html="feature.desc"></p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'Home',
        resource: 'Features',
        data() {
            return {
                features: [],
                title: 'Home'
            };
        },
        head () {
            return {
                title: `Prinz Eugen | ${this.title}`,
                meta: [
                    { hid: 'og-title', property: 'og:title', content: `Prinz Eugen | ${this.title}` },
                    { hid: 'og-url', property: 'og:url', content: 'https://prinz-eugen.info' },
                    { hid: 'twitter-title', name: 'twitter:title', content: `Prinz Eugen | ${this.title}` }
                ]
            }
        },
        async beforeMount() {
            await this.$utils.sleep(1);
            this.$store.commit('updateTitle', 'Home');

            try {
                this.features = await this.$getResource('features');
            } catch (e) {
                console.error(e);
                this.$utils.alertError(e.message);
            }
        }
    };
</script>

<style lang="sass">
    @import "../assets/sass/main.sass"

    .features
        display: flex
        justify-content: space-around
        flex-wrap: wrap
        margin: 2rem 0
        & > p
            padding: 8px
        & > .feature
            width: calc((100% - 1rem) / 3)
            min-width: 260px
            margin: .5rem .1rem .5rem .1rem
            align-self: flex-start
            & > .feature-title
                text-align: center
                padding: 7px 8px 4px
                font-size: 20px
                font-weight: 500
                color: $title
            & > .desc
                text-align: center
                color: $text
                margin: 0 1.5rem 0 1.5rem

    .divider
        height: 3px
        margin: .5rem 1.5rem .5rem 1.5rem
        padding: 0
        display: block
        border: 0
        transition: margin-left .3s cubic-bezier(.4, 0, .2, 1)
        will-change: margin-left
</style>