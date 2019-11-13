<template>
    <div>
        <tr @click="click">
            <td><img :src='value.picture.thumbnail'/></td>
            <td>{{value.name.first}} {{value.name.last}}</td>
            <td>{{value.email}}</td>
            <td>{{value.phone}}</td>
            <td>{{value.dob.age}}</td>
        </tr>
    </div>
</template>

<script>
    export default {
        name: 'Users',
        props: {
            value: { type: Object, required: true },
        },
        methods: {
            click() {
                this.$emit('click')
            }
        }
    };
</script>

<style scoped>
    th {
        cursor: pointer;
        user-select: none;
    }

    input + label {
        margin: 0 6px;
        color: rgba(0, 0, 0, .5);
    }

    input:checked + label {
        color: black;
    }

</style>