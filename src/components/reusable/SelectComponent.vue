<template>
    <section class="select" :class="{'open':open, 'disabled':disabled}">
        <figure class="arrow">
            <i class="fa fa-chevron-down"></i>
        </figure>

        <figure class="selected-option" v-on:click="toggle">
            {{parse(selectedOption)}}
        </figure>

        <section class="options">
            <figure class="option" v-for="item in options" v-on:click="select(item)">
                <img v-if="imgParser" :src="imgParser(item)" />
                {{parse(item)}}
            </figure>
        </section>
    </section>
</template>

<script>
    export default {
        data(){ return {
            selectedOption:this.selected || this.placeholder || this.options[0],
            open:false,
        }},
        methods: {
            toggle(){
                if(this.disabled) return false;
                this.open = !this.open
            },
            parse(item){
                if(typeof item === 'string') return item;
                if(this.parser) return this.parser(item);

                let props = this.prop.split(".");
                const lastKey = props.pop();
                return props.reduce((obj,key)=> obj[key], item)[lastKey];
            },
            select(item){
                this.selectedOption = item;
                this.open = false;
                this.$emit('changed', this.selectedOption)
            }
        },
        props:['placeholder', 'options', 'selected', 'prop', 'parser', 'disabled', 'imgParser'],
        watch:{
            input(){ this.emit(); },
            text(){ this.input = this.text; },
            disabled(isDisabled){ if(isDisabled) this.open = false; },
            selected(){ this.selectedOption = this.selected; }
        }
    }
</script>

<style lang="scss">
    @import "../../_variables";
    .select {
        cursor: pointer;
        height:50px;
        position: relative;
        width:100%;
        font-family:'Raleway',sans-serif;
        font-size:14px;
        border:1px solid $mid-light-grey;
        border-radius:4px;
        background:#fff;
        transition:background 0.2s ease;

        &.disabled {
            background: #f5f5f5;
        }

        &:not(:first-child){
            margin-top:10px;
        }

        .arrow {
            position:absolute;
            right:0;
            height:50px;
            line-height:50px;
            padding:0 15px;
            color:$mid-dark-grey;
            opacity:0.3;
            transition:opacity 0.2s ease;
            pointer-events: none;
        }

        .selected-option {
            height:50px;
            line-height:50px;
            width:100%;
            padding:0 35px 0 15px;
            overflow: hidden;
        }

        .options {
            position:absolute;
            top:48px;
            background:#fff;
            border:1px solid $mid-light-grey;
            border-bottom-right-radius:4px;
            border-bottom-left-radius:4px;
            left:-1px; right:-1px;
            box-shadow:0 8px 16px $light-grey;
            visibility:hidden;
            opacity:0;
            max-height:0;
            overflow:auto;
            transition:all 0.2s ease, max-height 0.5s ease;
            transition-property: visibility, box-shadow, opacity, max-height;
            z-index:2;

            .option {
                padding:10px;
                font-size:11px;
                background:transparent;
                transition:background 0.2s ease;

                img {
                    width:16px;
                    height:16px;
                    vertical-align: middle;
                    margin-right:5px;
                }

                &:not(:last-child){
                    border-bottom:1px solid $light-grey;
                }

                &:hover {
                    background:$light-grey;
                }
            }
        }

        &.open {
            border-bottom-right-radius:0;
            border-bottom-left-radius:0;
            background:$light-grey;

            .options {
                box-shadow:0 8px 16px rgba(0,0,0,0.15);
                visibility:visible;
                opacity:1;
                max-height:220px;
            }

            .arrow {
                opacity:1;
            }
        }

        &:hover {

            .arrow {
                opacity:1;
            }
        }
    }
</style>