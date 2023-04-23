<template lang="">
    <div 
    class="card"
    :class="{disabled: isDisabled}"
    :style="{
        height:`calc(var(--height-body) / ${Math.sqrt(numberCards)} - 16px)`,
        width: `calc((var(--height-body) / ${Math.sqrt(numberCards)}) * 3 /4 - 16px)`,
        perspective: `calc(((var(--height-body) / ${Math.sqrt(numberCards)}) * 3 /4 - 16px)*2)`
    }"
    >
        <div 
        class="card__inner" 
        :class="{'is-flipped':isFlipped}"
        @click="onFlipCard"
        >
            <div class="card__face card__face--front">
                <div 


                class="card__content"
                :style="{
                    backgroundSize: `
                    calc((var(--height-body) / ${Math.sqrt(numberCards)} - 16px)*0.3)
                    calc((var(--height-body) / ${Math.sqrt(numberCards)} - 16px)*0.3)
                    `
                }"
                >

                </div>
            </div>
            <div class="card__face card__face--back">
                <div class="card__content"
                :style="{
                    backgroundImage:`url(${require('@/assets/' + imgBackFaceUrl)})`
                }"
                >
                   
                </div>

            </div>
        </div>
    </div>
</template>
<script>
export default {
    props:{
        card:{
            type:[String,Number,Array,Object]
        },
        imgBackFaceUrl:{
            type:String,
            required:true
        },
        numberCards:{
            type:Number,
            required:true,
            default:1
        }
    },
    data() {
        return {
            isFlipped:false,
            isDisabled:false    
        }
    },
    methods: {
        // onToggleFlipCard(){
        //     this.isFlipped = !this.isFlipped;
        //     this.$emit("onFlip",this.card)
        // },

        onFlipCard(){
            // console.log("on flip")
            this.isFlipped = true;
            this.$emit("onFlip",this.card)
            this.isDisabled = true;

        },

        // flip back card
        onFlipBackCard(){
            this.isFlipped = false;
            this.isDisabled = false;

        },
        // disable flip card
        onDisableFlipCard(){
            this.isDisabled = true;
        }
    },
}
</script>
<style lang="scss">
.card {
    display: inline-block;
    margin-right: 1rem;
    margin-bottom: 1rem;
    // width: 90px;
    // height: 120px;
        &.disabled {
    cursor: none;
    pointer-events: none;
} 

}
.card__inner {
    width: 100%;
    height: 100%;
    transition: transform 0.5s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
    &.is-flipped {
        transform: rotateY(-180deg);
    }
}
.card__face {
    position: absolute;
    width: 100%;
    height: 100%;
   backface-visibility: hidden; // co hie nthi mat sau cua eleemnt hay khong
    overflow: hidden;
    border-radius: 0.5rem;
    // padding: 0.5rem;
    box-shadow: 0 3px 10px 3px rgba(0,0,0,0.3);

}
.card__face--front {
    .card__content {
        background: url("@/assets/images/icon_back.png") no-repeat center center;
        width: 100%;
        height: 100%;
    }
}
.card__face--back {
    background-color: var(--light);
    transform: rotateY(-180deg);
    .card__content {
        background-size: contain;
        background-position: center center;
        background-repeat: no-repeat;
        width: 100%;
        height: 100%;
    }

}

</style>
