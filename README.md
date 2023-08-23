# use availability in Cesium.Model.fromGltf
# 使用函数在Cesium.Model.fromGltf中实现类似enitiy的availability


```bash
/**
     * @param lon  longitude 经度
     * @param lat latitude 纬度
     * @param height height 高度
     * @param starttime starttime 开始时间 type like 数据类型这样Cesium.JulianDate.fromDate(new Date(2023,6,13,2,8,0))
     * @param stoptime stoptime 结束时间 
     * @param modelurl modelurl 模型路径
     *  
     **/
    function loaddynamicModel(lon,lat,height,starttime,stoptime,modelurl){
        let model=viewer.scene.primitives.add(Cesium.Model.fromGltf({
            url:modelurl,
            modelMatrix:Cesium.Transforms.eastNorthUpToFixedFrame(
                Cesium.Cartesian3.fromDegrees(lon,lat,height)
            )
        }))
        model.readyPromise.then((model)=>{
            model.activeAnimations.addAll({
                multiplier:1,
                startTime:starttime,
                loop:Cesium.ModelAnimationLoop.NONE
            })
            model.show=false
        })
        const lisener=()=>{
            if (viewer.clock.currentTime.secondsOfDay>=starttime.secondsOfDay&&viewer.clock.currentTime.secondsOfDay<=stoptime.secondsOfDay) {
                model.show=true
            }else{
                model.show=false
            }
        }
        viewer.clock.onTick.addEventListener(lisener)
    }

```
