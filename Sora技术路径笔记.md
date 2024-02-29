# Sora 模型训练过程

Sora 是 OpenAI 开发的人工智能模型，可以根据文本指令创建逼真而富有想象力的场景。它是视觉数据的通用模型，能够生成不同时长、长宽比和分辨率的视频和图像，最高可生成整整一分钟的高清视频。Sora 的训练过程包括将视觉数据转化为patches。这些patches类似于大型语言模型中的tokens，是一种高度可扩展的有效表示方法，可用于在各种类型的视频和图像上训练生成模型。

# Sora 可能使用的技术

1. SORA 支持不同长度、不同分辨率的输入。NaViT:多个patches打包成一个单一序列实现可变分辨率。使用不同分辨率、不同时长的视频进行训练保证推理时在不同长度和分辨率上的效果带来大量的计算负载不均衡可能使用Google的NaVit相关技术降低计算量支持动态输入
2. Peebles 在ICCV上发表了一篇 Dit 的工作该文章在 Technical Report的 Reference中给出。结合 Diffusion Model 和 Transformer，通过 Scale up Model 提升图像生成质量图像的scaling技术运用到视频场景非常直观，可以确定是 SORA 的技术之一。

引用自Datawhale开源课程:
- [学习手册](https://datawhaler.feishu.cn/wiki/LxSCw0EyRidru1kFkttc1jNQnnh)
- [开营直播，sora技术原理详解回放](https://www.bilibili.com/video/BV1wm411f7gf)
- [Sora硬核解读](https://www.bilibili.com/video/BV1KZ42127GP)

# 视觉转换器（ViT）简介

视觉转换器（ViT）是一种利用注意力和图像片段来执行图像分类的模型架构。图像被分割成固定大小的片段，然后对每个片段进行线性嵌入。然后添加位置嵌入，并将得到的向量序列输入标准变换器编码器。在不同的图像识别计算机视觉任务中，ViT 模型最近已成为卷积神经网络（CNN）的竞争性替代品。

[更多关于ViT的信息](https://paperswithcode.com/method/vision-transformer)

# 关于 NaVit 的知识

NaVit 通过引入 "Patch n' Pack "方法扩展了视觉转换器的功能，允许以原始分辨率和长宽比有效处理图像。这种方法不仅提高了计算效率，还改善了模型在不同任务中的性能。通过采用连续标记删除和分辨率采样等策略，NaVit 展示了适应各种图像分辨率的卓越能力，为视觉变换器的灵活性和效率树立了新的标杆。

# Spacetime Latent Patches

Spacetime Latent Patches用于 Sora 模型。给定一个压缩输入视频，Sora 会提取一系列Spacetime Patches作为转换tokens。这一方案也适用于图像，因为图像只是具有单帧的视频。基于token的表示法使 Sora 能够在不同分辨率、持续时间和长宽比的视频和图像上进行训练。

# Diffusion Transformer

扩散变换器（DiTs）是基于Transformer架构的一类新型扩散模型。它们训练图像的

潜在扩散模型，用在latent patches上运行的transformer取代常用的 U-Net 主干网。通过增加Transformer深度/宽度或增加输入标记的数量，Gflops 越高的 DiT 始终具有较低的 Frechet Inception Distance (FID)，表明其性能更好。在以类别为条件的 ImageNet 基准上，最大的 DiT 模型优于所有先前的扩散模型。

[阅读更多关于Diffusion Transformer](https://arxiv.org/abs/2212.13771)
```
