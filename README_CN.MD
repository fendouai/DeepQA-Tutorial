## 概述

这个工作尝试重现这个论文的结果 [A Neural Conversational Model](http://arxiv.org/abs/1506.05869) (aka the Google chatbot). 
它使用了循环神经网络（seq2seq 模型）来进行句子预测。它是用 python 和 TensorFlow 开发。

程序的加载主体部分是参考 Torch的  [neuralconvo](https://github.com/macournoyer/neuralconvo) from [macournoyer](https://github.com/macournoyer).

现在, DeepQA 支持一下对话语料:
 * [Cornell Movie Dialogs](http://www.cs.cornell.edu/~cristian/Cornell_Movie-Dialogs_Corpus.html) corpus (default). Already included when cloning the repository.
 * [OpenSubtitles](http://opus.lingfil.uu.se/OpenSubtitles.php) (thanks to [Eschnou](https://github.com/eschnou)). Much bigger corpus (but also noisier). To use it, follow [those instructions](data/opensubs/) and use the flag `--corpus opensubs`.
 * Supreme Court Conversation Data (thanks to [julien-c](https://github.com/julien-c)). Available using `--corpus scotus`. See the [instructions](data/scotus/) for installation.
 * [Ubuntu Dialogue Corpus](https://arxiv.org/abs/1506.08909) (thanks to [julien-c](https://github.com/julien-c)). Available using `--corpus ubuntu`. See the [instructions](data/ubuntu/) for installation.
 * Your own data (thanks to [julien-c](https://github.com/julien-c)) by using a simple custom conversation format (See [here](data/lightweight) for more info).

To speedup the training, it's also possible to use pre-trained word embeddings (thanks to [Eschnou](https://github.com/eschnou)). More info [here](data/embeddings).

## 安装
这个程序需要一下依赖(easy to install using pip: `pip3 install -r requirements.txt`):
 * python 3.5
 * tensorflow (tested with v1.0)
 * numpy
 * CUDA (for using GPU)
 * nltk (natural language toolkit for tokenized the sentences)
 * tqdm (for the nice progression bars)

你可能需要下载附带的数据让 nltk 正常工作。

```
python3 -m nltk.downloader punkt
```

Cornell 数据集已经包括了。其他的数据集查看 readme 文件到他们所在的文件夹。  (在  `data/`).

网站接口需要一些附加的包：

 * django (tested with 1.10)
 * channels
 * Redis (see [here](http://redis.io/topics/quickstart))
 * asgi_redis (at least 1.0)


Docker 安装也是支持的，更多详细的教程参考  [here](docker/README.md).

## 运行

### 聊天机器人

训练这个模型，直接运行 `main.py` 。一旦训练完成，你可以测试结果用 `main.py --test` 
 (结果生成在  'save/model/samples_predictions.txt') 或者用 `main.py --test interactive` (更有趣).

Here are some flags which could be useful. For more help and options, use `python main.py -h`:
 * `--modelTag <name>`: allow to give a name to the current model to differentiate between them when testing/training.
 * `--keepAll`: use this flag when training if when testing, you want to see the predictions at different steps (it can be interesting to see the program changes its name and age as the training progress). Warning: It can quickly take a lot of storage space if you don't increase the `--saveEvery` option.
 * `--filterVocab 20` or `--vocabularySize 30000`: Limit the vocabulary size to and optimize the performances and memory usage. Replace the words used less than 20 times by the `<unknown>` token and set a maximum vocabulary size.
 * `--verbose`: when testing, will print the sentences as they are computed.
 * `--playDataset`: show some dialogue samples from the dataset (can be use conjointly with `--createDataset` if this is the only action you want to perform).

To visualize the computational graph and the cost with [TensorBoard](https://www.tensorflow.org/how_tos/summaries_and_tensorboard/), just run `tensorboard --logdir save/`.

默认的这个网络架构是一个标准的 encoder/decoder 有两个 LSTM layers (隐藏层大小 256) ，然后 vocabulary 的 embedding size 是  32.这个网络用 ADAM 训练。最大的句子长度设置为 10个单词，但是可以增加。

## 结果
当然，这个网络并不会很擅长聊天：

    Q: Hi
    A: Hi.

    Q: What is your name ?
    A: Laura.

    Q: What does that mean ?
    A: I dunno.

    Q: How old are you ?
    A: thirty-five.

    Q: Will Google hire me ?
    A: No.

    Q: Tell me the alphabet
    A: Fuck you.

    Q: That's not nice
    A: Yeah.

这里有一些情况它并不能正确回答：

    Q: Who is Laura ?
    A: My brother.

    Q: Say 'goodbye'
    A: Alright.

    Q: What is cooking ?
    A: A channel.

    Q: Can you say no ?
    A: No.

    Q: Two plus two
    A: Manny...


## 预训练模型
### 项目截图：
![chatbot_miniature.png](http://upload-images.jianshu.io/upload_images/76451-ac9a24655ef04729.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 实测截图：

![Screenshot from 2017-09-05 14-47-52.png](http://upload-images.jianshu.io/upload_images/76451-b76442dd3de787ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 一步一步教程：
1.下载这个项目：
https://github.com/Conchylicultor/DeepQA
2.下载训练好的模型：
https://drive.google.com/file/d/0Bw-phsNSkq23OXRFTkNqN0JGUU0/view
（如果网址不能打开的话，今晚我会上传到百度网盘，分享到：http://www.tensorflownews.com/）
3.解压之后放在 项目 save 目录下
如图所示

![Screenshot from 2017-09-05 14-52-13.png](http://upload-images.jianshu.io/upload_images/76451-35330ed6f7479561.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.复制 save/model-pretrainedv2/dataset-cornell-old-lenght10-filter0-vocabSize0.pkl 这个文件到  data/samples/

如图所示：

![Screenshot from 2017-09-05 14-55-00.png](http://upload-images.jianshu.io/upload_images/76451-26821d8dcf427484.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.在项目目录执行一下命令：
```
python3 main.py --modelTag pretrainedv2 --test interactive
```
程序读取了预训练的模型之后，如图：

![Screenshot from 2017-09-05 14-57-14.png](http://upload-images.jianshu.io/upload_images/76451-77560d300add0733.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 聊天机器人资源合集
项目，语聊，论文，教程
https://github.com/fendouai/Awesome-Chatbot

### 更多教程：
[http://www.tensorflownews.com/](http://www.tensorflownews.com/)

### DeepQA
>https://github.com/Conchylicultor/DeepQA

备注：为了更加容易了解这个项目，说明部分翻译了项目的部分 readme ，主要是介绍使用预处理数据来运行这个项目。
