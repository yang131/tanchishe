# tanchishe



<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>贪吃蛇</title>
<meta name="Author" content="Soleil" >
<style type="text/css">
	body{
		font-family:"微软雅黑";- 12*/
    background: -webkit-linear-gradient(left, #91AECE , #fff); /* Safari 5.1 - 6.0 */
    background: -o-linear-gradient(right, #91AECE, #fff); /* Opera 11.1 - 12.0 */
    background: -moz-linear-gradient(right, #91AECE, #fff); /* Firefox 3.6 - 15 */
    background: linear-gradient(to right, #91AECE , #fff); /* 标准的语法（必须放在最后） */
		}
	.box{
		
  background: -webkit-linear-gradient(left top, rgba(49,82,125,1) , rgba(83,53,63,1)); /* Safari 5.1 - 6.0 */
  background: -o-linear-gradient(bottom right, rgba(49,82,125,1), rgba(83,53,63,1)); /* Opera 11.1 - 12.0 */
  background: -moz-linear-gradient(bottom right, rgba(49,82,125,1), rgba(83,53,63,1)); /* Firefox 3.6 - 15 */
  background: linear-gradient(to bottom right, rgba(49,82,125,1) , rgba(83,53,63,1)); /* 标准的语法 */
		width:680px;
		height:680px;
		border:2px solid #2C273B;
		margin:0px auto;
		box-shadow:1px 1px 3px 1px #2C273B;
		text-align:center;
		}
		h1{text-align:center;
			color:#fff;}
		h2{text-align:center;
		color:#fff;font-size:9px;}
		
		h3{text-align:center;
		color:yellow;font-size:19px;}
</style>

</head>

<body>
	<!--游戏界面 start-->
    <div class="box">
        <h1>
            贪吃蛇
        </h1>
        <h2>
            Retro Snaker
        </h2>
        <!--画布 start-->
        <canvas id="mycanvas" width=450 height=450 style="background-color:#EBEBF3"></canvas>
        <!--画布 end-->
        <h3>
        score:<span id="score_value"></span>
        </h3>
  
    </div>
    <audio src="eat.wav" id="eat"></audio>
    <audio src="background.mp3" id="background"></audio>
    <audio src="over.wav" id="dead"></audio>
    <!--游戏界面 end-->
    <script type="text/javascript">
		var foodColor = new Array("#A6DEF7","#D9B4E8","#AEDBD6","#F0BED7");
		var foodint = 0;
		var canvas=document.getElementById("mycanvas");
		var ctx=canvas.getContext("2d");
		var score=0;
		document.getElementById("background").play();
		for(var i=0;i<30;i++){
				ctx.strokeStyle="white";
				ctx.beginPath();
				ctx.moveTo(0,i*15);
				ctx.lineTo(450,i*15);
				ctx.closePath();
				ctx.stroke();
			}
		for(var i=0;i<30;i++){
				ctx.strokeStyle="white";
				ctx.beginPath();
				ctx.moveTo(i*15,0);
				ctx.lineTo(i*15,450);
				ctx.closePath();
				ctx.stroke();
			}
		function Cell(x,y,f){
				this.x=x;
				this.y=y;
				this.f=f;
				
			}
			
		function clickpause(that){
			var values = that.val
		}
		function Food(x,y){
				this.x=x;
				this.y=y;
			}	
		var snake=[];
		var width=15;
		var head;
		var speed=300;
		var food=new Food(15,15);
		for(var i=0;i<5;i++){
				snake[i]=new Cell(i,0,-2);}
		function drawSnake(){
				ctx.clearRect(0,0,450,450);
				for(var i=0;i<snake.length;i++){
					ctx.fillStyle="#FF5757";
					if(i==snake.length-1)
					{ctx.fillStyle="#F59D05";}
					ctx.beginPath();
					ctx.rect(snake[i].x*15,snake[i].y*15,width,width);//矩形
					ctx.closePath();
					ctx.fill();
				}
				head=snake[snake.length-1];
				if(head.x==food.x&&head.y==food.y){
					document.getElementById("eat").play();
					var newCell=new Cell(head.x,head.y,head.f);
					switch(head.f){
						case 1:newCell.y--;break;
						case -1:newCell.y++;break;
						case 2:newCell.x--;break;
						case -2:newCell.x++;break;
					}
					snake[snake.length]=newCell;
					
					foodint = foodint +1;
					score = score + 1;
					if(foodint==4){
						foodint = 0;
					}
					initFood();
					if(speed!=100){
					speed=speed-50;}
				}
				
				head=snake[snake.length-1];
				drawFood();
				 gameOver();
			}
		drawSnake();
		function drawFood(){
				
				var color = foodColor[foodint];
				ctx.fillStyle=color;
				ctx.beginPath();
				ctx.rect(food.x*15,food.y*15,width,width);//矩形
				ctx.closePath();
				ctx.fill();
						
				var score_value=document.getElementById("score_value");
				score_value.innerHTML = score;
			}
		function initFood(){
			var x=Math.ceil(Math.random()*28)+1;
			var y=Math.ceil(Math.random()*28)+1;
			food.x=x;
			food.y=y;
			for(var i=0;i<snake.length;i++){
				if(snake[i].x==x&&snake[i].y==y)
					initFood();
			}
		}
		//监听键盘事件
		document.onkeydown=function(e){
				var code=e.keyCode;
				var direction=0;
				//上1 下-1 左2 右-2
				switch(code){
					case 38:direction=1;break;	
					case 39:direction=-2;break;	
					case 40:direction=-1;break;	
					case 37:direction=2;break;	
				}
				if(direction!=0){
					if(parseInt(head.f)+direction!=0)
					moveSnake(direction);
				}
			}
		function moveSnake(direction){
				var newCell=new Cell(head.x,head.y,head.f);
				newCell.f=direction;
				var newSnake=[];
				for(var i=1;i<snake.length;i++){
						newSnake[i-1]=snake[i];
					}
				newSnake[snake.length-1]=newCell;
				switch(direction){
						case 1:newCell.y--;break;
						case -1:newCell.y++;break;
						case 2:newCell.x--;break;
						case -2:newCell.x++;break;
				}
				
				snake=newSnake;
				head=newCell;
				if(gameOver){
				drawSnake();}
			}
		function gameOver(){
			//超出边框
			if(head.x>=30||head.y>=30||head.x<0||head.y<0){
				document.getElementById("dead").play();
				document.getElementById("background").pause();
				alert("Game over");
				window.location.reload();
			}
			//咬到自己
			for(var i=0;i<snake.length-1;i++){
				if(head.x==snake[i].x&&head.y==snake[i].y){
				document.getElementById("dead").play();
				document.getElementById("background").pause();
				alert("Game over");
				window.location.reload();
				}
			}
			return true;
		}
		function moveCell(){
			moveSnake(head.f);
			setTimeout("moveCell()",speed);
		}
		moveCell();
	</script>
    
</body>
</html>
