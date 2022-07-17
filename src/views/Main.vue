<<template>
    <div class="main-page">
        <div class="workblock">
            <span :class="myState(0)"><b>20:00</b><br><small>Work</small></span>
            <span :class="myState(1)"><b>05:00</b><br><small>Rest</small></span>
            <span :class="myState(2)"><b>20:00</b><br><small>Work</small></span>
            <span :class="myState(3)"><b>05:00</b><br><small>Rest</small></span>
            <span :class="myState(4)"><b>20:00</b><br><small>Work</small></span>
            <span :class="myState(5)"><b>05:00</b><br><small>Rest</small></span>
            <span :class="myState(6)"><b>20:00</b><br><small>Work</small></span>
            <span :class="myState(7)"><b>25:00</b><br><small>Rest</small></span>
        </div>
        <hr>
        <h1><small>Current Block: </small> {{currentTimeString}} </h1>
        <hr>
        <h1><small>Total Time: </small> {{totalTimeString}} </h1>
        <hr>
        <button :class="buttonState" @click="buttonClicked">{{buttonState}}</button>
    </div>
</template>

<script>
export default {
    data: function(){
        return {
            activeBlock: -1,
            currentTimeString: '01:30 of 20:00 Work',
            totalTimeString: '00:21:30 of 2:00:00',
            buttonState: 'Start',
            mainTimer: undefined,
            currentTime: 0,
            sound: undefined
        }
    },
    mounted: function(){
        this.sound = new Audio(require('@/assets/notification.mp3'));
        this.STOP();
    },
    methods: {
        START: function(){
            this.activeBlock = 0;
            this.mainTimer = setInterval(() => {
                this.currentTime += 1;
                this.updateCurrentBlock();
                this.updateTotalBlock();
            }, 1000);
        },
        updateCurrentBlock: function(){
            //Block 1 : 20 : 1200
            if(parseInt(this.currentTime) < 1200){
                this.activeBlock = 0;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 0).substring(3, 8) + ' of 20:00 Work';
                return null;
            }
            if(parseInt(this.currentTime) == 1200) this.sound.play();
            //Block 2 : 05 : 1500
            if(parseInt(this.currentTime) < 1500){
                this.activeBlock = 1;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 1200).substring(3, 8) + ' of 05:00 Rest';
                return null;
            }
            if(parseInt(this.currentTime) == 1500) this.sound.play();
            //Block 3 : 20 : 2700
            if(parseInt(this.currentTime) < 2700){
                this.activeBlock = 2;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 1500).substring(3, 8) + ' of 20:00 Work';
                return null;
            }
            if(parseInt(this.currentTime) == 2700) this.sound.play();
            //Block 4 : 05 : 3000
            if(parseInt(this.currentTime) < 3000){
                this.activeBlock = 3;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 2700).substring(3, 8) + ' of 05:00 Rest';
                return null;
            }
            if(parseInt(this.currentTime) == 3000) this.sound.play();
            //Block 5 : 20 : 4200
            if(parseInt(this.currentTime) < 4200){
                this.activeBlock = 4;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 3000).substring(3, 8) + ' of 20:00 Work';
                return null;
            }
            if(parseInt(this.currentTime) == 4200) this.sound.play();
            //Block 6 : 05 : 4500
            if(parseInt(this.currentTime) < 4500){
                this.activeBlock = 5;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 4200).substring(3, 8) + ' of 05:00 Rest';
                return null;
            }
            if(parseInt(this.currentTime) == 4500) this.sound.play();
            //Block 7 : 20 : 5700
            if(parseInt(this.currentTime) < 5700){
                this.activeBlock = 6;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 4500).substring(3, 8) + ' of 20:00 Work';
                return null;
            }
            if(parseInt(this.currentTime) == 5700) this.sound.play();
            //Block 8 : 25 : 7200
            if(parseInt(this.currentTime) < 7200){
                this.activeBlock = 7;
                this.currentTimeString = this.toHHMMSS(this.currentTime - 5700).substring(3, 8) + ' of 25:00 Rest';
                return null;
            }
            if(parseInt(this.currentTime) == 7200) this.sound.play();
            if(parseInt(this.currentTime) >= 7200){
                alert("Micro Sprint is done. Reset Timer to start again.");
                this.STOP();
            }
        },
        updateTotalBlock: function(){
            this.totalTimeString = this.toHHMMSS(this.currentTime) + ' of 02:00:00';
        },
        toHHMMSS: function (num) {
            var sec_num = parseInt(num, 10); // don't forget the second param
            var hours   = Math.floor(sec_num / 3600);
            var minutes = Math.floor((sec_num - (hours * 3600)) / 60);
            var seconds = sec_num - (hours * 3600) - (minutes * 60);

            if (hours   < 10) {hours   = "0"+hours;}
            if (minutes < 10) {minutes = "0"+minutes;}
            if (seconds < 10) {seconds = "0"+seconds;}
            return hours+':'+minutes+':'+seconds;
        },
        STOP: function(){
            clearInterval(this.mainTimer);
            this.currentTime = 0;
            this.activeBlock = -1;
            this.currentTimeString = '-- of --';
            this.totalTimeString = '-- of --';
            console.log("STOPPED");
        },
        buttonClicked: function(){
            if(this.buttonState == "Start"){
                this.buttonState = "Stop";
                //Start The Timer
                this.START();
            }else{
                this.buttonState = "Start";
                //Stop The Timer
                var c = confirm("Are you sure you want to stop the timer and loose all progress?");
                if(c) this.STOP();
            }
            this.sound.play();
        },
        myState: function(i){
            if(this.activeBlock == -1) return '';
            if(i == this.activeBlock) return 'active';
            if(i < this.activeBlock) return 'done';
            if(i > this.activeBlock) return 'pending';
        }
    },
    watch: {
	    'graphData': {
            deep: true
        }
    }
}
</script>

<style lang="scss">
    @keyframes blinkActive{
        0%{
            border-color: #a1efe4;
            color: #a1efe4;
        }
        100%{
            border-color: #ae81ff;
            color: #ae81ff;
        }
    }
    .main-page{
        border: #a1efe4 1px solid;
        padding: 3em;
        border-radius: 5px;
        hr{
            border-color: #a1efe4;
        }
        button{
            width: 100%;
            font-size: 3em;
            border-radius: 5px;
            background-color: #a6e22e;
            border: none;
            color: #FFFFFF;
            padding: 20px;
            text-align: center;
            -webkit-transition-duration: 0.4s; /* Safari */
            transition-duration: 0.4s;
            text-decoration: none;
            overflow: hidden;
            cursor: pointer;
            color: #232328;
            &:hover{
                color: #d3d3d8;
                background: #56820e;
            }
        }
        .Stop{
            background: #f92672;
            &:hover{
                color: #d3d3d8;
                background: #fd5ff0;
            }
        }
        h1{
            text-align: center;
            small{
                font-size: 0.6em;
            }
        }
        .workblock{
            display: flex;
            span{
                margin: 1em;
                text-align: center;
                background: #424248;
                padding: 0.5em;
                border-radius: 4px;
                border: #f3f3f8 1px solid;
                transition: 0.5s;
                b{
                    font-size: 1.5em;
                }
            }
            .active{
                animation-name: blinkActive;
                animation-duration: 1s;
                animation-iteration-count: infinite;
                animation-direction: alternate;
            }
            .done{
                border-color: #a6e22e;
                color: #a6e22e;
            }
            .pending{
                border-color: #fd971f;
                color: #fd971f;
            }
        }
    }
</style>