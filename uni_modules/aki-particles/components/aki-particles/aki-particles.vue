<template>
    <view style=""><view id="particles" :data-style="particlestyle.style" :style="'background-color: ' + particlestyle.bgcolor + ';z-index: ' + particlestyle.zindex"></view></view>
</template>

<script>
export default {
    name: 'akira-particles',
    props: ['particlestyle'],   
    data() {
        return {};
    }
};
</script>

<script module="test" lang="renderjs">
    import particles from '../../../../uni_modules/aki-particles/static/aki-particles/particles.js';
    export default {
        data() {
            return {};
        },
        mounted() {
            //获取dataset
            let dataset=this.$ownerInstance.$el.children[0].dataset;
            //挂载路径是相对www目录而言，和上面import路径起点并不同
            let url='./uni_modules/aki-particles/static/aki-particles/particles-default.json';
            if(dataset?.style) url='./uni_modules/aki-particles/static/aki-particles/particles-' + dataset.style + '.json';
            particlesJS.load('particles', url);
        },
    };  
</script>

<style lang="scss">
	#particles{
	      position: absolute;
		  //z-index: 1;
	      width: 100%;
	      height: 100%;
	     // background-color: #000022;
	      background-repeat: no-repeat;
	      background-size: cover;
	      background-position: 50% 50%;
	}
</style>