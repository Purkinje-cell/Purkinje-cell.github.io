### Git 冲突处理

在多人协作项目时，常常会碰到的一个场景是两个人同时对同一个文件进行了不同的修改。
#### 使用 merge 
1. 两人同时修改一个文件，一个添加行，一个修改另一行 pull时merge auto merge use 'ort' strategy 不需要单独处理冲突
2. 两人修改了同一行显示冲突
3. 两个分支一个删除文件，另一个对其进行修改，merge时保留修改的文件，但需要进一步确认
4. 两个分支其中一个修改了文件的第一行和最后一行，另一个修改了第一行和倒数第二行，merge后git 两处修改的版本均被保留（倒数两行的修改被识别为一处）需要手动处理冲突后 git add、git commit 冲突的文件之后方可切换至其他分支进行操作。
#### 在分支上创建/修改文件后未保存造成的影响
1. 在某一在分支中创建文件后未保存（未add+commit) 就checkout 到其他分支，可以在新的分支上正常add commit 未保存的文件。
2. 在某一分支中修改文件后未保存（未add+commit），再checkout 到其他分支，则所进行的修改丢失。* 修改文件后务必记得保存文件/*
### Markdown 图床配置

在使用 Markdown 撰写讨论报告时，我们发现了一个问题：Markdown 对于插入图片的支持比较复杂，需要指定本地图片的路径或网络图片的 URL。因此，我们上传至 Github 的 Markdown 报告中无法显示图片。我们搜索了 Markdown 图片支持的解决方案，发现图床是一种较好的方式。

**图床（Image Hosting)** 是一个在线存储和分享图片的服务。它允许用户将图片上传到图床服务器上，并通过生成的链接或嵌入代码将图片分享给其他人或展示在不同的平台上。图床为用户提供了一个方便、快捷的图片存储和分享方式，不需要担心存储空间和带宽问题。用户可以通过图床服务轻松上传、管理和分享图片，而不必担心占用自己的硬盘空间或消耗大量带宽。这对于网站制作、论坛交流、博客撰写等场景非常有用，因为图片通常占用很大的存储空间和带宽。使用图床服务，用户可以将图片托管到第三方服务器上，让访问者直接从图床服务器获取图片，减轻了原始服务器的压力。

这里我们根据网上的教程, 使用 GitHub 创建一个图床。[如何利用 Github 搭建自己的免费图床？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/347342082)

首先，在 Github 上新建一个仓库 Images（此处命名可以修改），并在 Settings -> Developer Settings -> Personal access tokens 里新创建一个 Token，用于 PicGo 软件直接使用 API 访问 Github。

![image.png](https://raw.githubusercontent.com/Purkinje-cell/Images/master/20230427160824.png)

之后，从 GitHub 上下载 PicGo 软件，并按照教程配置它的 Github 仓库名和分支名，并粘贴之前设置好的 Token。使用 PicGo 控制图片上传的好处是它能够较为方便的设置不同图床的上传，以及在上传后可以自动复制 Markdown 类型的链接到剪贴板，从而减小了工作量。

![image.png](https://raw.githubusercontent.com/Purkinje-cell/Images/master/20230427161524.png)

之后，将 Markdown 中图片的格式从引用本地文件改为引用 GitHub 上的 URL。再将 Markdown 上传到 Github，发现已经能够显示图片。

目前网上有许多免费的图床，如 SM. MS。或者可以使用腾讯云存储服务来进行图床的配置。使用腾讯云的好处是上传速度较快，不会遇到 GitHub 需要 VPN 的问题。
