<section class="relative w-[200px] h-[200px] text-center">
<div> </div>
<div></div>
<div></div>
<h1 class="absolute text-center">Loading...</h1>
</section>

<style>
    section div:nth-child(1){
        border-top: 7px solid red;
        border-left: 6px solid yellow;
        animation: rotating 4s infinite linear;
    }
    section div:nth-child(2){
        border-top: 4px solid rgb(157, 224, 12);
        border-left: 5px solid rgb(187, 10, 134);
        scale: 90%;
        animation: rotating 2s infinite linear;
    }
    section div:nth-child(3){
        border-top: 3px solid rgb(252, 0, 0);
        border-bottom: 3px solid rgb(2, 10, 255);
        scale: 75%;
        animation: rotating 1.5s infinite linear;
    }
    div{
        position: absolute;
        text-align: center;
        width: 200px;
        content: "";
        height: 200px;
        border-radius: 100px;
        background-color: inherit;
        
    }
    h1{
        font-size: 1.2rem;
        top: 85px;
        left: 55px;
        animation: text 10s infinite linear;
    }
    @keyframes text {
        0% {
            color: red;
            opacity: 1;
        }

        50% {
            color: blue;
            opacity: 0.4;
        }

        100% {
            color: gold;
            opacity: 1;
        }
    }

    @keyframes rotating{
        0% {
            transform: rotate(0deg);
            opacity: 1;
        }
        50% {
            transform: rotate(180deg);
            opacity: 0.1;
        }
        100% {
            transform: rotate(360deg);
            opacity: 1;
        }
    }
</style>