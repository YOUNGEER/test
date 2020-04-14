## snapHelper—指定targetview

- 作用：将RV中的View滑动到指定点或者容器中的任何像素点，实现类有：LinearSnapHelper和PagerSnapHelper
- 抽象类 继承RecyclerView.OnFlingListener
- 需要和LayoutManager结合使用，该LM必须实现了RecyclerView.SmoothScroller.ScrollVectorProvider接口，比如LinearLayoutManger就可以配合使用
- 大致流程：attach关联scrollerListenr，onFling计算得到position并滑动，scrollerListener计算偏移，滑动
- 重要方法

— attachToRecyclerView:将SnapHepler 关联到指定的RecyclerView上

— onFling:实现RV的onFling方法，一些判断null后，实际调用snapFromFling

— snapFromFling

​	— createScroller：创建SmoothScroller对象

​	— findTargetSnapPosition：子类需要实现该方法，获得targetPosition

​	— 最后调用下面方法实现

```
smoothScroller.setTargetPosition(targetPosition);
layoutManager.startSmoothScroll(smoothScroller);
```

— snapToTargetExistingView：滑动到目标view

​	— findSnapView：子类实现该方法，提供一个指定的view来对其

​	— calculateDistanceToFinalSnap：子类实现该方法，计算滑动的记录，RV进行滑动



## LinearSnapHelper

- 作用：可以使当前item居中显示

