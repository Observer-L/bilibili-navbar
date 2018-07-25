<template>
    <div class="bili-wrapper">
        <div class="floor" v-for="(item, index) in floors" :id=item.id>
            <h1>{{item.name}}</h1>
        </div>
    </div>
</template>
<script>
export default {
    props: {
        floors: {
            type: Array
        }
    }
}
</script>
<style lang="less" scoped>  
.bili-wrapper {
    margin: 0 auto;
    width: 1160px;
}
.floor {
    height: 460px;
    &:nth-child(even) {
        color: #fff;
        background: salmon;
    }
    h1 {
        position: relative;
        top: 50%;
        transform: translateY(-50%);
        margin: 0;
        font-size: 4rem;
        text-align: center;
    }
}
</style>
