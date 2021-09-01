<template>
    <div class="wrapper-content wrapper-content--fixed">
        <section>
            <div class="container">
                <h1>Users page</h1>

                <table>
                    <thead>
                        <tr>
                            <th @click="sort('name')">Name</th>
                            <th @click="sort('age')">Age</th>
                            <th @click="sort('gender')">Gender</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="user in usersSort" :key="user.id">
                            <td>
                                <img :src="user.img" :alt="user.name">
                                <span>{{ user.name }}</span>
                            </td>
                            <td>{{ user.age }}</td>
                            <td>{{ user.gender }}</td>
                        </tr>
                    </tbody>
                </table>

                <p>debug: sort: {{ currentSort }}, dir: {{ currentSortDir }}</p>

            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data () {
        return {
            users: [],
            currentSort: 'name',
            currentSortDir: 'asc'
        }
    },
    created () {
        axios
            .get('https://tocode.ru/static/_secret/courses/1/usersCrmApi.php')
                .then(response => {
                    // console.log(response.data)
                    this.users = response.data
                })
                .catch(error => {
                    console.log(error)
                })
        // this.users = [
        //     { id: 1, name: 'Jack', age: 22, gender: 'male'},
        //     { id: 2, name: 'Alex', age: 24, gender: 'male'}
        // ]
    },
    computed: {
        usersSort () {
            return this.users.sort((a, b) => {
                let mod = 1
                if (this.currentSortDir === 'desc') mod = -1
                if (a[this.currentSort] < b[this.currentSort]) return -1 * mod
                if (a[this.currentSort] > b[this.currentSort]) return 1 * mod
                return 0
            })
        }
    },
    methods: {
        sort (e) {
            if (e === this.currentSort) {
                 this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc'
            }
            this.currentSort = e
        }
    }
}
</script>

<style lang="scss" scoped>
    img {
        width: 60px;
        height: auto;
        border-radius: 50%;
        margin-right: 16px;
    }
</style>