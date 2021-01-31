<template>
    <li>
        <img
            :src="gameToBeRendered.img"
            :alt="gameToBeRendered.Name"
            />
        <div class="info">
            <h1 class="hl" v-html="searchHighlight(gameToBeRendered.Name)"></h1>
            <span v-for="type in gameToBeRendered.Types"
                  v-html="searchHighlight(type)"
                  :key="type.index"
                  :class="['type', type]">
            </span>
            <span class="type normal">MaxCP: {{ gameToBeRendered.MaxCP }}</span>
        </div>
    </li>
</template>

<script>
export default {
    props: ['gameToBeRendered', 'userSearchInput'],
    methods: {
        searchHighlight(text) {
            if (this.userSearchInput && text.length > this.userSearchInput.length) {
                let searchRegExp = new RegExp(this.userSearchInput, 'ig')
                return text.replace(searchRegExp, match => {
                 return `<span class="bg-yellow">${match}</span>`
                })
            }
            return text
        }
    }

}
</script>
