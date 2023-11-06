<template>
    <div class="avatar-container">
        <img :src="avatarUrl" @click="showListModal" class="avatar-image" />
        <div v-if="showModal" class="list-modal">
            <ul>
                <li v-for="(item, index) in list" :key="index" @click="item.function">{{ item.text }}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: "AvatarUI",
    props: {
        avatarUrl: {
            type: String,
            required: true,
        },
        list: {
            type: Array,
            default: () => [],
        },
    },
    data() {
        return {
            showModal: false,
        };
    },
    methods: {
        showListModal() {
            if (this.list.length > 0) {
                this.showModal = !this.showModal;
            }
        },
    },
};
</script>

<style scoped>
.avatar-container {
    position: relative;
}

.avatar-image {
    width: 35px;
    height: 35px;
    border-radius: 50%;
    cursor: pointer;
}

.list-modal {
    position: absolute;
    top: 40px;
    right: 0;
    background-color: black;
    border: 1px solid black;
    padding: 10px;
    z-index: 1;
}

.list-modal ul {
    list-style: none;
    margin: 0;
    padding: 0;
}

.list-modal li {
    cursor: pointer;
}
</style>
