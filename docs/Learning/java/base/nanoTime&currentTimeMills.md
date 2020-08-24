# System.currentTimeMills()和System.nanoTime()区别
- System.nanoTime()的精确度更高一些，如今的硬件设备性能越来越好，如果要更精密计算执行某行代码或者某块代码所消耗的时间，该方法会测量得更精确。
- 单独获取System.nanoTime()没有什么意义，因为该值是随机的，无法表示当前的时间。如果要获取当前的时间点，用System.currentTimeMills()。
- System.currentTimeMills()得到的值能够和Date类方便的转换，jdk提供了多个接口来实现；但System.nanoTime()不行；
- System.currentTimeMills()的值是基于系统时间的，可以人为地修改；而System.nanoTime()则不能；

