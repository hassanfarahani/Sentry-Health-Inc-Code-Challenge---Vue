<template>
    <div>
        <label for="maxCP" class="max-cp">
            <input type="checkbox" id="maxCP" v-model="boxChecked" />
            <small>
                Maximum Combat Points
            </small>
        </label>
        <input type="text" class="input" placeholder="Pokemon or type" v-model="userSearchInput" />
        <div v-if="!userSearchResults" class="loader"></div>
        <ul v-if="userSearchInput" class="suggestions">
            <Game v-for="game in filteredResults" :gameToBeRendered="game" :userSearchInput="userSearchInput" :key="game.Number" />
        </ul>
        <ul v-if="!filteredResults.length">
            <li>
                <img
                    src="https://cyndiquil721.files.wordpress.com/2014/02/missingno.png"
                    alt="No results"
                />
                <div class="info">
                    <h1 class="no-results">
                        No results
                    </h1>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
// eslint-disable-next-line no-unused-vars
// const URL_PATH =
//     'https://gist.githubusercontent.com/bar0191/fae6084225b608f25e98b733864a102b/raw/dea83ea9cf4a8a6022bfc89a8ae8df5ab05b6dcc/pokemon.json';
import Game from './Game'

export default {
    name: 'App',
    data() {
        return {
            userSearchInput: null,
            userSearchResults: null,
            boxChecked: false
        }
    },
    async created() {
        let response  = await fetch('https://gist.githubusercontent.com/bar0191/fae6084225b608f25e98b733864a102b/raw/dea83ea9cf4a8a6022bfc89a8ae8df5ab05b6dcc/pokemon.json')
        let results = await response.json()
        this.userSearchResults = results
    },
    computed: {
        filteredResults() {
            let filteredSearchResults = []
            if (this.userSearchResults && this.userSearchInput) {
                filteredSearchResults = this.userSearchResults.filter(game => {
                    return game.Name.toLowerCase().includes(this.userSearchInput.toLowerCase())
                })
                this.userSearchResults.forEach(game => {
                    for (let i=0; i<game.Types.length; i++) {
                        if (game.Types[i].toLowerCase().includes(this.userSearchInput.toLowerCase())) {
                            if (filteredSearchResults.length === 0) {
                                filteredSearchResults.push(game)
                            } else if (!filteredSearchResults.some(filteredGame => filteredGame.Number === game.Number)) {
                                filteredSearchResults.push(game)
                            }
                            break
                        }
                    }
                })
            }
            if (this.boxChecked) {
                let sortedResults = filteredSearchResults.sort((a,b) => b.MaxCP - a.MaxCP)
                return sortedResults.filter((game, index) => index < 4)
            }
            return filteredSearchResults.filter((game, index) => index < 4)
        }
    },
    components: {
        Game
    }
};
</script>
