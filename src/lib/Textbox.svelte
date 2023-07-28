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
<button class="border-solid border border-[#a71717] mt-3 rounded bg-blue-900 hover:bg-blue-500 p-2 text-lg ease-in duration-300" on:click={stripSpecialCharacters}>add</button>
</div>

<style>
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
</style>