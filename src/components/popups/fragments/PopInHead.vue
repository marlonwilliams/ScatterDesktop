<template>
    <section class="pop-in-head">
        <section>
            <figure class="bubble-icon">
                <i class="fa " :class="`fa-${nextPopIn.data.props.icon}`"></i>
            </figure>
        </section>
        <section>
            <figure class="title">{{nextPopIn.data.props.title}}</figure>
            <figure class="description">{{nextPopIn.data.props.description}}</figure>
        </section>
        <section v-if="nextPopIn.data.props.hasOwnProperty('buttonText')">
            <btn v-if="nextPopIn.data.props.hasOwnProperty('denyButtonText')" :text="nextPopIn.data.props.denyButtonText" red="1" v-on:clicked="returnResult(false)"></btn>
            <btn v-if="nextPopIn.data.props.buttonText.length" :text="nextPopIn.data.props.buttonText" :red="!nextPopIn.data.props.hasOwnProperty('denyButtonText')" v-on:clicked="returnResult(true)"></btn>
        </section>
    </section>
</template>

<script>
    import { mapActions, mapGetters, mapState } from 'vuex'
    import * as Actions from '../../../store/constants';

    export default {
        computed:{
            ...mapGetters([
                'nextPopIn'
            ])
        },
        methods:{
            returnResult(truthy){
                this.nextPopIn.data.callback(truthy);
                this[Actions.RELEASE_POPUP](this.nextPopIn);
            },
            ...mapActions([
                Actions.RELEASE_POPUP
            ])
        }
    }
</script>

<style scoped lang="scss" rel="stylesheet/scss">
    @import "../../../_variables";

</style>