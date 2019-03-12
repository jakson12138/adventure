# adventure

# 前言
 在学习游戏编程模式之前，我写的许多unity小项目都只是基于组件+加简单的脚本逻辑去实现的。
 当我在想要开发一款横版2D冒险游戏的时候，根据以往的游戏经验，主角一般至少拥有以下几种行为：站立，行走，跑动，跳跃，攻击。那么在开始编写一个脚本来处理玩家的输入时，就会出现类似下面的代码：
```
 void handleInput(Input input){
     if(input == PRESS_D){
         //move to right
     }
     else if(input == PRESS_A){
         //move to left
     }
     else if(input == PRESS_SPACE){
         //jump
     }
 }
```
当然，以上代码仅仅用来控制角色移动是没有问题的，但是游戏往往在角色不同运动状态时会有不一样的贴图（或者是动画），所以代码如下：
```
 void handleInput(Input input){
     if(input == PRESS_D){
         //move to right
         //set graphics move
     }
     else if(input == PRESS_A){
         //move to left
         //set graphics move
     }
     else if(input == PRESS_SPACE){
         //jump
         //set graphics jump
     }
     else{
         //set graphics idle
     }
 }
```
显然，上面的代码是有错误的，比如在跳跃的时候按下移动键，角色没落地前，角色应该还是跳跃的贴图，而当玩家没有输入时，也不应该立即转化为站立的贴图，而是在玩家落地时改变，所以代码需要加入一个bool变量isJumping：
```
 void handleInput(Input input){
     if(input == PRESS_D){
         //move to right
         if(!isJumping) //set graphics move
     }
     else if(input == PRESS_A){
         //move to left
         if(!isJumping) //set graphics move
     }
     else if(input == PRESS_SPACE){
         //jump
         //set graphics jump
         isJumping = true;
     }
     else{
         if(!isJumping) //set graphics idle
     }
 }
```
并在物理引擎中在角色落地时将isJumping修改为false。然后我们就会发现，代码不支持角色二段跳，或者我们想要给角色加入不同的行走状态（比如跑步），再加上一个攻击状态呢？如果依旧像上面那样用很多个if...else语句以及bool变量去处理玩家输入的话，代码将会非常臃肿并且难以维护（当出现bug时去修改某个状态时，可能牵一发而动全身），所以当我在《游戏编程模式》看到状态模式时，欣喜若狂，因为这恰恰帮我解决了之前一直困扰我的问题。
