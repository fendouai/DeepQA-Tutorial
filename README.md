## 项目截图：

<img src="http://www.tensorflownews.com/wp-content/uploads/2017/09/chatbot_miniature.png" alt="" width="300" height="174" class="alignnone size-full wp-image-585" />

## 实测截图：

<img src="http://www.tensorflownews.com/wp-content/uploads/2017/09/Screenshot-from-2017-09-05-14-57-14.png" alt="" width="859" height="553" class="alignnone size-full wp-image-589" />

## 一步一步教程：
1.下载这个项目：
https://github.com/Conchylicultor/DeepQA

2.下载训练好的模型：

https://drive.google.com/file/d/0Bw-phsNSkq23OXRFTkNqN0JGUU0/view

（如果网址不能打开的话，今晚我会上传到百度网盘，分享到：http://www.tensorflownews.com/）

3.解压之后放在 项目 save 目录下
如图所示


<img src="http://www.tensorflownews.com/wp-content/uploads/2017/09/Screenshot-from-2017-09-05-14-52-13.png" alt="" width="423" height="205" class="alignnone size-full wp-image-587" />

4.复制 save/model-pretrainedv2/dataset-cornell-old-lenght10-filter0-vocabSize0.pkl 这个文件到  data/samples/

如图所示：

<img src="http://www.tensorflownews.com/wp-content/uploads/2017/09/Screenshot-from-2017-09-05-14-55-00.png" alt="" width="598" height="245" class="alignnone size-full wp-image-588" />

5.在项目目录执行一下命令：
```
python3 main.py --modelTag pretrainedv2 --test interactive
```
程序读取了预训练的模型之后，如图：

<img src="http://www.tensorflownews.com/wp-content/uploads/2017/09/Screenshot-from-2017-09-05-14-57-14.png" alt="" width="859" height="553" class="alignnone size-full wp-image-589" />

## 聊天机器人资源合集
项目，语聊，论文，教程
[https://github.com/fendouai/Awesome-Chatbot](https://github.com/fendouai/Awesome-Chatbot)

## 更多教程：
[http://www.tensorflownews.com/](http://www.tensorflownews.com/)



