<script lang="ts">
let bigString : string = ""

export let cleanedArr : string[] = []

function stripSpecialCharacters(){
  const cleaned : string = bigString.replace(/[^\w\s-]/g, ' ');
  const splitArr  = cleaned.split(" ")
  cleanedArr = cleanedArr.concat(splitArr.filter((item) => item.trim() !== ""));
  bigString = ""
}

</script>
<div class="pt-10  grid">
  <h3>Paste icon names. example below</h3>
  <h3 class="mb-4">['document-open', 'document-save', "document-open-recent"]</h3>
<textarea id="text" class="min-h-[15vh]" bind:value={bigString} />
<button class="text-white border-solid border border-[#a71717] mt-3 rounded bg-blue-900 hover:bg-blue-500 p-2 text-lg ease-in duration-300 relative" on:click={stripSpecialCharacters}>add
  <i class="star posLeft"></i>
  <i class="star posTop posRight"></i>
  <i class="star posBottom posRight"></i>
  <i class="star posBottom posLeft"></i>
  <i class="star posBottom"></i>
  <i class="star posTop"></i>
</button>
</div>

<style lang="scss">

button:hover { 
  i{
  display: inline-block;
}}
  #text {
  width:500px;
  overflow:hidden;
  background-color:#FFF;
  color:#222;
  font-weight:normal;
  font-size:1rem;
  resize:none;
  line-height:40px;
  padding-left:100px;
  padding-right:100px;
  padding-top:45px;
  padding-bottom:34px;
  background-image:url(https://static.tumblr.com/maopbtg/E9Bmgtoht/lines.png), url(https://static.tumblr.com/maopbtg/nBUmgtogx/paper.png);
  background-repeat:repeat-y, repeat;
  -webkit-border-radius:12px;
  border-radius:12px;
  -webkit-box-shadow: 0px 2px 14px #000;
  box-shadow: 0px 2px 14px #000;
  border-top:1px solid #FFF;
  border-bottom:1px solid #FFF;
}
.star {
  position:absolute;
  rotate:30deg;
  display: none;
  /* display: inline-block; */
  width: 0;
  height: 0;
  margin-left: .9em;
  margin-right: .9em;
  margin-bottom: 1.2em;
 
  border-right:  .3em solid transparent;
  border-bottom: .7em  solid #FC0;
  border-left:   .3em solid transparent;
  font-size: 8px;
  animation: appear 5s infinite linear;
  
  &:before, &:after {
    content: '';
    display: block;
    width: 0;
    height: 0;
    position: absolute;
    top: .6em;
    left: -1em;
  
    border-right:  1em solid transparent;
    border-bottom: .7em  solid #FC0;
    border-left:   1em solid transparent;
    
    transform: rotate(-35deg);
  }
  
  &:after {  
    transform: rotate(35deg);
  }
}
@keyframes appear{
  0%{opacity:0;scale:0.2}
  10%{opacity:0.5;scale:0.5}
  50% {opacity:1;scale:1;rotate: random(100-50)+deg}
  100% {scale:1.2;rotate:0deg;box-shadow: 0px 2px 15px 5px #CCBB05;}
}
.posLeft{
  left:0;
  transform: translateX(random(50)-20);
  transform: translateY(random(50)-20);
}
.posTop{
   top:0;
   transform: translateY(random(50)-20);
   transform: translateX(random(50)-20);
}
.posRight{
   right:0;
   transform: translateY(random(50)-20);
   transform: translateX(random(50)-20);
}
.posBottom{
   bottom:0;
   transform: translateY(random(50)-20);
}

</style>