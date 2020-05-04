<template>
    <div class="navbar">
        <img src="../assets/logo.png" alt="logo">
        <h1>VueNues</h1>
    </div>
</template>

<script>
export default {
    name: 'Navbar',
}
</script>

<style scoped>
    .navbar {
        display: flex;
        color: #35495e;
        justify-content: space-around;
        align-items: center;
        width: 100vw;
        padding: 10px 10px;
    }
    img {
        width: 50px;
        height: 50px;
    }
</style>