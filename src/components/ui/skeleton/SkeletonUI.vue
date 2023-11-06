<template>
    <div>
        <template v-if="type === 'table'">
            <div class="skeleton-table">
                <div class="skeleton-row" v-for="n in 5" :key="n">
                    <div class="skeleton-cell"></div>
                    <div class="skeleton-cell"></div>
                    <div class="skeleton-cell"></div>
                </div>
            </div>
        </template>
        <template v-else-if="type === 'card'">
            <div class="skeleton-card">
                <div class="skeleton-image"></div>
                <div class="skeleton-content">
                    <div class="skeleton-title"></div>
                    <div class="skeleton-text"></div>
                    <div class="skeleton-text"></div>
                </div>
            </div>
        </template>
        <template v-else-if="type === 'page'">
            <div class="skeleton-page">
                <div class="skeleton-header"></div>
                <div class="skeleton-content">
                    <div class="skeleton-text"></div>
                    <div class="skeleton-text"></div>
                    <div class="skeleton-text"></div>
                </div>
                <div class="skeleton-footer"></div>
            </div>
        </template>
    </div>
</template>

<script>
export default {
    props: {
        type: {
            type: String,
            required: true
        }
    }
}
</script>

<style>
/* Skeleton styles */
.skeleton-table {
    border-collapse: collapse;
    width: 100%;
}

.skeleton-row {
    border-bottom: 1px solid #ddd;
    display: flex;
    flex-direction: row;
}

.skeleton-cell {
    background-color: #f6f7f8;
    height: 20px;
    margin-right: 10px;
    width: 100%;
}

.skeleton-card {
    display: flex;
    flex-direction: row;
    margin-bottom: 20px;
}

.skeleton-image {
    background-color: #f6f7f8;
    height: 100px;
    margin-right: 20px;
    width: 100px;
}

.skeleton-content {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}

.skeleton-title {
    background-color: #f6f7f8;
    height: 20px;
    margin-bottom: 10px;
    width: 50%;
}

.skeleton-text {
    background-color: #f6f7f8;
    height: 20px;
    margin-bottom: 10px;
    width: 100%;
}

.skeleton-page {
    display: flex;
    flex-direction: column;
    height: 100vh;
}

.skeleton-header {
    background-color: #f6f7f8;
    height: 50px;
}

.skeleton-content {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.skeleton-footer {
    background-color: #f6f7f8;
    height: 50px;
}
</style>
