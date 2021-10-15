# Goroutine Scheduler

## G-P-M model

- G: Goroutine (reusable); storing running stack, status and task function
- P: Processer (logical); when G can only be scheduled in P's `local runq`
- M: Machine (OS thread)

Schedule Circulation: 

![img](https://img.taohuawu.club/gallery/GMP-scheduler.png?imageView2/2/w/1280/format/jpg/interlace/1/q/100)

