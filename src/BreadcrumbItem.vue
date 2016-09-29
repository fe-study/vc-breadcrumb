<template>
    <li>
        <a v-if="item.url" :href="item.url">{{ item.text }}</a>
        <slot><span v-else>{{ item.text }}</span></slot>
        <slot name="separator"><span class="separator">{{ separator }}</span></slot>
    </li>
</template>

<style>
</style>

<script>
export default {
    name: 'vc-breadcrumb-item',
    props: {
        text: String,
        url: String
    },
    data () {
        return {
            item: {}
        }
    },
    ready () {
        let text
        try {
            this.text = this._slotContents.default.textContent
        } catch (e) { console.error(e) } 
        this.item.text = this.text 
        this.item.url = this.url
        this.$parent.routes.push(this.item)
    },
    computed: {
        separator () {
            return this.$parent.separator
        }
    }
}
</script>
