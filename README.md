function iMove(obj, json, fn) {
    
    //定义标杆，true表示所有变化都达到理想值
    var flag = true;

    //清除定时器，防止重复触发
    clearInterval(obj.timer);

    obj.timer = setInterval(function() {

        //for-in循环提取json里面的值
        for (var iAttr in json) {

            //判断改变的属性参数时透明度还是长宽
            if (iAttr == 'opacity') {
                var iNum = Math.round(parseFloat(getStyle(obj, iAttr)) * 100);
            } else {
                var iNum = parseInt(getStyle(obj, iAttr));
            }

            //计算速度
            var speed = (json[iAttr] - iNum) / 10;
            speed = speed > 0 ? Math.ceil(speed) : Math.floor(speed);

            //执行函数
            if (iNum != json[iAttr]) {
                flag = false;

                if (iAttr == 'opacity') {
                    obj.style.filter = 'alpha(opacity' + (iNum + speed) + ')';
                    obj.style.opacity = (iNum + speed) / 100;
                } else {
                    obj.style[iAttr] = iNum + speed + 'px';
                }

            }
            if (flag) {
                clearInterval(obj.timer);

                //判断是否有链式执行函数
                if (fn) {
                    fn();
                }
            }
        }
    }, 30)



    //获取当前属性函数
    function getStyle(obj, attr) {
        if (obj.currentStyle) {
            return obj.currentStyle[attr];
        } else {
            return getComputedStyle(obj, false)[attr];
        }
    }



}
