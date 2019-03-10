默认情况下在容器中创建的所有文件都存储在一个可写的容器层。这意味着：

* 当容器不存在的时候，数据也不是持久化的，并且如果其他进程需要这个数据，也很难在容器之外获得它
* 容器的可写数据层是和运行容器的host主机紧密联系的。即不容易将数据迁移到其他地方
* 写入到容器的可写入层需要使用一个存储驱动来管理文件系统。这个存储驱动使用Linux内核提供了一个统一文件系统（union filesystem）。和使用数据卷存储数据（即直接将数据写入到主机文件系统）方式相比较，这个扩展抽象降低了数据读写性能。

> 注意：除了静态数据，不要将任何数据存储在Docker的默认读写数据层。线上生产数据，特别是日志文件，应该通过数据卷来存储，以获得最大的存储性能。
>
> 容器的层次文件系统由于需要记录数据层差异，性能非常低。只有直接映射到物理主机的数据卷才绕过了这种层次文件系统，获得了原生的存储性能。