<template>
    <div class="alert-container">
        <div class="alert">
            <h2>{{ alertMessage }}</h2>
        </div>        
    </div>
</template>

<script>
export default {
    name: 'Alert',
    props: {
        alertMessage: String
    },
}
</script>

<style>
.alert-container {
    display: flex;
    align-items: center;
    justify-content: center;
}
.alert {
    margin-top: 20px;
    background-color:navy;
    color: #fff;
    min-width: 50vw;
}
</style>