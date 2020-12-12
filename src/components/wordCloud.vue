<template>
    <div :style="{background:'url('+require('../static/background.jpg')+')'}">
        <div style="height:5vh;width:100vw;">
            <!-- <div style="box-shadow: 0px 0px 50px #fff;width:50px;height:50px;background:white"></div> -->
        </div>
        <div id="wordcloud" style="min-height:95vh;width:100vw" @click="clickWordcloud">
        </div>
    </div>
</template>

<script>
import Js2WordCloud from 'js2wordcloud'
export default {
    data(){
        return{
            wc:undefined,
            tooltip:"",
            maleWords:[],
            femaleWords:[]
        }
    },
    mounted(){
        this.initWordsFromJson()

        this.wc = new Js2WordCloud(document.getElementById('wordcloud'))
        var option = {
            tooltip: {
                show: true
            },
            list: this.maleWords,
            color: 'white',
            fontFamily:'handfont',
            minFontSize:20,
            maxFontSize:140,
            backgroundColor:"rgba(0,0,0,0)",
            shape:'triangle'
        }
        this.wc.setOption(option)
        this.tooltip = this.wc._tooltip
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