<!DOCTYPE html>
<html><head>
<meta http-equiv="content-type" content="text/html; charset=windows-1252"><style>#g{background:black;margin:auto;display:block;}</style></head><body><canvas id="g" width="640px" height="480px"></canvas><script>const area_w=640,area_h=480,tile_sz=32,area_tile_w=area_w/tile_sz,area_tile_h=area_h/tile_sz,padding=tile_sz/4,food_padding=tile_sz/8,max_snake=area_tile_w*area_tile_h,D_UP=38,D_DOWN=40,D_LEFT=37,D_RIGHT=39;var canvas=document.getElementById("g"),ctx=canvas.getContext("2d"),phys_timer=null,dir=0,started=!1,pos=[{x:Math.round(area_tile_w/2),y:Math.round(area_tile_h/2)}],food=null,ctrl_stack=[],forgive=!1;function pos_equal(e,a){return e.x==a.x&&e.y==a.y}function place_food(){var e;do{food={x:Math.floor(Math.random()*area_tile_w),y:Math.floor(Math.random()*area_tile_h)},e=!1;for(var a=0;a<pos.length;a++)if(pos_equal(pos[a],food)){e=!0;break}}while(e)}function draw_internal(){if(ctx.clearRect(0,0,area_w,area_h),ctx.fillStyle="white",pos.length>1)for(var e=1;e<pos.length;e++){var a=pos[e],t=pos[e-1],i=Math.min(a.x,t.x),_=Math.max(a.x,t.x),o=Math.min(a.y,t.y),l=Math.max(a.y,t.y),r=i*tile_sz+padding,n=o*tile_sz+padding,s=(1+_-i)*tile_sz-2*padding,d=(1+l-o)*tile_sz-2*padding;0==i&&_==area_tile_w-1?(ctx.fillRect(0,n,tile_sz-padding,d),ctx.fillRect(area_w-tile_sz+padding,n,tile_sz-padding,d)):0==o&&l==area_tile_h-1?(ctx.fillRect(r,0,s,tile_sz-padding),ctx.fillRect(r,area_h-tile_sz+padding,s,tile_sz-padding)):ctx.fillRect(r,n,s,d)}else{var c=pos[0];ctx.fillRect(c.x*tile_sz+padding,c.y*tile_sz+padding,tile_sz-2*padding,tile_sz-2*padding)}food&&(ctx.fillStyle="lime",ctx.fillRect(food.x*tile_sz+food_padding,food.y*tile_sz+food_padding,tile_sz-2*food_padding,tile_sz-2*food_padding))}function draw_loop(){draw_internal(),window.requestAnimationFrame(draw_loop)}function end_game(e){clearInterval(phys_timer),setTimeout(function(){alert(e)},250)}function physics_loop(){for(var e=Object.assign({},pos[pos.length-1]);ctrl_stack.length>0;){var a=ctrl_stack[0];if(ctrl_stack.shift(),!dir_is_opposing(dir,a)){dir=a;break}}switch(dir){case D_UP:e.y--;break;case D_DOWN:e.y++;break;case D_LEFT:e.x--;break;case D_RIGHT:e.x++}e.x<0?e.x=area_tile_w-1:e.x>=area_tile_w&&(e.x=0),e.y<0?e.y=area_tile_h-1:e.y>=area_tile_h&&(e.y=0);var t=!1,i=pos_equal(e,food);if(!i){for(var _=1;_<pos.length;_++)if(pos_equal(pos[_],e)){t=!0;break}t&&(forgive?end_game("Game over!"):forgive=!0)}t||(i||pos.shift(),pos.push(e),i&&(pos.length==max_snake?(food=null,end_game("YOU'RE WINNER !")):place_food()),forgive=!1)}function start_game(){phys_timer=setInterval(physics_loop,150),window.requestAnimationFrame(draw_loop)}function dir_is_opposing(e,a){return e==D_DOWN&&a==D_UP||a==D_DOWN&&e==D_UP||e==D_LEFT&&a==D_RIGHT||a==D_LEFT&&e==D_RIGHT}document.addEventListener("keydown",function(e){switch(e.keyCode){case D_UP:case D_LEFT:case D_RIGHT:case D_DOWN:ctrl_stack.push(e.keyCode)}!started&&ctrl_stack.length>0&&(started=!0,start_game())}),place_food(),draw_internal();</script>
</body></html>
