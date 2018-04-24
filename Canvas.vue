<template>
    <div>
        <canvas id="canvas" ref="canvas"></canvas>
    </div>
</template>

<script>
export default {
  data() {
    return {
        addCircle: null,
        number: 100, // 存在的点的个数
        max_conn: 5,// 每个点最大连线数量
        mx: 0.5,// X轴移动速度(越小越快)
        my: 0.5,// Y轴移动速度(越小越快)
        dist: 150,//点吸附距离
        e_dist: 200,//鼠标吸附加速距离
    }
  },
  mounted() {
      let canvasDom = this.$refs.canvas;
      let canvasTx = canvasDom.getContext('2d');
      let w = canvas.width = canvas.offsetWidth;
      let h = canvas.height = canvas.offsetHeight;
      let circles = this.init(this.number, w, h);
      this.draw(canvasTx, w, h, circles);

      // 鼠标移入
      canvasDom.onmouseover = function(e) {
          e = e || window.event;
          if (!this.addCircle) {
              this.addCircle = {
                    x: e.clientX,
                    y: e.clientY,
                    mx: 0,
                    my: 0,
                    r: Math.random() * 5,
                    mouse: true
              }
              circles.push(this.addCircle);
          }
      }

      // 鼠标移出
      canvasDom.onmouseout = function() {
          if (this.addCircle) {
              circles.pop(this.addCircle);
              this.addCircle = null;
          }
      }

      // 鼠标移动
      canvasDom.onmousemove = function(e) {
          e = e || window.event;
          this.addCircle.x = e.clientX - 5;
          this.addCircle.y = e.clientY - 95;
      }
  }, 
  methods: {
      // 画一个圆形
      // element是canvas对象
      // item是每个圆点对象
      drawCircle(element, item) {
        element.beginPath();
        element.arc(item.x, item.y, item.r, 0, Math.PI * 2)
        element.closePath();
        element.fillStyle = 'rgba(114, 135, 224, 0.3)';
        element.fill();
      },
      // 画一个直线,并和所有点连接
      drawLine(element, item, circles) {
        let dx = item.x - circles.x; 
        let dy = item.y - circles.y;
        let d = Math.sqrt(dx * dx + dy * dy);

         if (d < this.dist) {
            item.conn.push(circles);
            if (this.max_conn && this.max_conn > 0 && this.max_conn >= item.conn.length)  this.anoutLine(element, item, circles, d);
        } 

        if (item.mouse) {
            if (d > this.dist && d <= this.e_dist) {
                circles.x += (item.x - circles.x) / 20;
                circles.y += (item.y - circles.y) / 20;
            }
            if (d <= this.e_dist) {
                element.lineWidth = 1;
                element.beginPath();
                element.moveTo(item.x, item.y);
                element.lineTo(circles.x, circles.y);
                element.closePath();
            }
        }
      },
      anoutLine(element, item, circles, d) {
            element.beginPath();
            element.moveTo(item.x, item.y);
            element.lineTo(circles.x, circles.y);
            element.closePath();

            //距离越远线越不明显
            let dist = d / 100;
            element.lineWidth = 1 - dist;
            element.strokeStyle = 'rgba(255, 255, 255, ' + (1 - dist) + ')';

            element.stroke();
      },
      // 圆圈在某个范围内移动
      // item是每个圆点对象
      moveCircle(w, h, item) {
          item.mx = (item.x < w && item.x > 0) ? item.mx : (-item.mx);
          item.my = (item.y < h && item.y > 0) ? item.my : (-item.my);
          item.x += item.mx / this.mx;
          item.y += item.my / this.my;
      },
      // 绘制图形
      // ctx是canvas对象 w h是画布宽高 circles是全部圆形集合
      draw(ctx, w, h, circles) {
        ctx.clearRect(0, 0, w, h);
        circles.forEach(cItem => {
            cItem.conn = [];
            this.moveCircle(w, h, cItem);
            this.drawCircle(ctx, cItem);
            circles.forEach(lItem => {
                this.drawLine(ctx, cItem, lItem);
            })
        })
        setTimeout(function() {
            this.draw(ctx, w, h, circles)
        }.bind(this), 30)
      },
      // 圆点数据初始化
      init(num, w, h) {
          let circles = [];
          for(let i = 0; i < num; i++) {
            // x,y为圆的位置，r为半径
            // mx,my为X，Y轴移动速度
            circles.push({
                x: Math.random() * w,
                y: Math.random() * h,
                mx: Math.random(),
                my: Math.random(),
                r: Math.random() * 5,
                conn: []
            });
          }
          return circles;
      },
  } 
}
</script>

<style>
   #canvas {
       width: 100%;
       height: 500px;
       background: #18486e;
   }
</style>


