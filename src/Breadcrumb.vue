<template>
    <ol class="vc-breadcrumb-component breadcrumb">
        <slot name="breadcrumb-before"></slot>
        <template v-for="item in historyRoutes">
            <li>
                <a :href="item.url">{{ item.text }}</a>
                <slot><span class="separator">{{ separator }}</span></slot>
                <slot name="separator"></slot>
            </li>
        </template>
        <li class="active" v-if="activeRoute">
            {{ activeRoute.text }}
        </li>
        <slot name="breadcrumb-after"></slot>
    </ol>
</template>

<style>
.breadcrumb > li + li:before {
    padding: 0 5px;
    content: " "
}
.breadcrumb .separator {
    padding-left: 10px;
    color: #ccc;
}
</style>

<script>
export default {
    props: {
        routes: Array,
        separator: '/'
    },
    computed: {
        rs () {
            if (!Array.isArray(this.routes) && typeof this.routes === 'object') {
                let ret = []
                let keys = Object.keys(this.routes)
                for (let i = 0, len = keys.length; i < len; i++) {
                    let obj = {}
                    obj.text = keys[i]
                    obj.url = this.routes[keys[i]]
                    ret.push(obj)
                }
                return ret
            } else if (Array.isArray(this.routes)) {
                if (this.routes[0].url && this.routes[0].text) 
                    return this.routes
            } else {
                console.warn('[vcBreadcrumb warn]: pay attention to the routes you pass in!')
                return void 0
            }
        },
        historyRoutes () {
            return this.rs && this.rs.slice(0, this.rs.length - 1)
        },
        activeRoute () {
            return this.rs && this.rs[this.rs.length - 1]
        }
    }
}
</script>
