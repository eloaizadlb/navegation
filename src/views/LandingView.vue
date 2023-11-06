<template>
    <div class="landing-view">
        <h1>Welcome to Kangu!</h1>
        <div class="dashboard">
            <div class="data-card">
                <h2>Total Users</h2>
                <p>{{ totalUsers }}</p>
            </div>
            <div class="data-card">
                <h2>Total Revenue</h2>
                <p>{{ totalRevenue }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            totalUsers: 1000,
            totalRevenue: "$50,000"
        }
    }
}
</script>

<style>
.landing-view {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

.dashboard {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 50px;
}

.data-card {
    background-color: #f5f5f5;
    border-radius: 10px;
    padding: 20px;
    margin: 0 20px;
    text-align: center;
}

.data-card h2 {
    margin-top: 0;
}

.data-card p {
    font-size: 24px;
    font-weight: bold;
}
</style>
