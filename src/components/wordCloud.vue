<template>
    <div :style="{background:'url('+require('../static/background.jpg')+')'}">
        <div style="height:5vh;width:100vw;">
            <!-- <div style="box-shadow: 0px 0px 50px #fff;width:50px;height:50px;background:white"></div> -->
        </div>
        <div ref='wordcloud' id="wordcloud" style="min-height:95vh;width:100vw" @click="clickWordcloud">
        </div>
    </div>
</template>

<script>
import Js2WordCloud from 'js2wordcloud'
export default {
    props:[
        'type'
    ],
    data(){
        return{
            minFontSize:20,
            maxFontSize:140,

            wc:undefined,
            tooltip:"",
            maleWords:[],
            femaleWords:[]
        }
    },
    mounted(){
        if(window.innerWidth < 960){
            console.log('window width:'+window.innerWidth)
            this.minFontSize = 18
            this.maxFontSize = 110
        }

        this.initWordsFromJson()
        let words = []
        if(this.type === 'male'){
            words = this.maleWords
        }
        if(this.type === 'female'){
            words = this.femaleWords
        }

        this.wc = new Js2WordCloud(this.$refs.wordcloud)
        var option = {
            tooltip: {
                show: true
            },
            list: words,
            color: 'white',
            fontFamily:'handfont',
            minFontSize:this.minFontSize,
            maxFontSize:this.maxFontSize,
            backgroundColor:"rgba(0,0,0,0)",
            shape:'triangle'
        }
        setTimeout(()=>{
            this.wc.setOption(option)
            this.tooltip = this.wc._tooltip
        }, 1000)
    },
    methods:{
        initWordsFromJson(){
            let maleObj = require('../static/maleWords.json')
            let femaleObj = require('../static/femaleWords.json')
            let objToArray = (wordsObj)=>{
                let wordsArray = []
                for(let i in wordsObj){
                    wordsArray.push([i, wordsObj[i]])
                }
                wordsArray.sort(()=>Math.random(1)-0.5)
                return wordsArray
            }
            this.maleWords = objToArray(maleObj)
            this.femaleWords = objToArray(femaleObj)
        },
        clickWordcloud(){
            if(this.tooltip.style.display == 'block'){
                console.log(this.tooltip.innerHTML)
                window.open('https://s.weibo.com/weibo?q='+this.tooltip.innerHTML, '_blank')
            }
        }
    }
}
</script>