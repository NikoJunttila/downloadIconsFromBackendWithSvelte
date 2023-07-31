<style lang="scss">
    
// n is the number of stars required
@function multiple-box-shadow($n) {
  $value: '#{random(2000)}px #{random(2000)}px #FFF';
  @for $i from 2 through $n {
    $value: '#{$value}, #{random(2000)}px #{random(2000)}px #FFF';
  }
  @return unquote($value);
}

$shadows-small:  multiple-box-shadow(700);
$shadows-medium: multiple-box-shadow(200);
$shadows-big:    multiple-box-shadow(100);

section {
top:-10px;
pointer-events: none;
  height: 100%;
  position:absolute;
  width: 100vw;
  background-color:rgba(0, 0, 0, 0.2);
overflow: hidden;
}
    
#stars {
  width: 1px;
  height: 1px;
  background: transparent;
  box-shadow: $shadows-small;
  animation: animStar 50s linear infinite;
    
  &:after {
    content: " ";
    position: absolute;
    top: 2000px;
    width: 1px;
    height: 1px;
    background: transparent;
    box-shadow: $shadows-small;
  }
}
    
#stars2 {
  width: 2px;
  height: 2px;
  background: transparent;
  box-shadow: $shadows-medium;
  animation: animStar 100s linear infinite;
    
  &:after {
    content: " ";
    position: absolute;
    top: 2000px;
    width: 2px;
    height: 2px;
    background: transparent;
    box-shadow: $shadows-medium;
  }
}
    
#stars3 {
  width: 3px;
  height: 3px;
  background: transparent;
  box-shadow: $shadows-big;
  animation: animStar 150s linear infinite;
    
  &:after {
    content: " ";
    position: absolute;
    bottom: 2000px;
    width: 3px;
    height: 3px;
    background: transparent;
    box-shadow: $shadows-big;
  }
}


@keyframes animStar {
  from {
    transform: translateY(-2000px);
  }
  to {
    transform: translateY(0);
  }
}

</style>
<section>
<div id="stars"></div>
<div id="stars2"></div>
<div id="stars3"></div>
</section>