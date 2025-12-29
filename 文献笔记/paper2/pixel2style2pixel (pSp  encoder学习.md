

## Question

1.  - **如何理解将真实图像嵌入到 W⁺？**  
    即，如何将任意图像通过 encoder 编码成 W⁺ latent code，使其能被 StyleGAN 生成器还原？
    
2. **W⁺ 空间是什么？为何选择 W⁺ 而不是 W？**  
    是不是每层输入不同 latent 会更好地表达细节？是否意味着 encoder 输出多个 style vector？
    
3.  **Encoder 和 StyleGAN Generator 的 latent 空间需要对齐吗？**  
    Encoder 是后训练的，那它如何确保输出的 latent code 是 StyleGAN G 能接受的？是否 StyleGAN G 是冻结的？
    
4.  **是否需要对 StyleGAN Generator 再训练以适配 encoder？**  
    如果 G 是固定的，encoder 如何学习与之匹配的 latent code？



- **是否需要对 StyleGAN Generator 再训练以适配 encoder？**  
    如果 G 是固定的，encoder 如何学习与之匹配的 latent code？

5. **pSp 中为什么不再使用判别器 D？是否完全不需要 adversarial loss？**  
    与传统 GAN 方法的最大差异在哪？为什么可以只靠感知损失等非对抗方法？
    
6. **pSp 的损失函数都有哪些？每种损失的作用是什么？**  
    感知损失、LPIPS、identity loss 等分别用于解决什么问题？它们能否代替 D 的功能？
    
7. **pSp 的结构是否通用？是否适用于非人脸图像的 inversion 或 translation？**  
    该架构迁移到非人脸领域是否存在困难？


8. **“偏离反演-编辑”的旧范式，直接做翻译任务意味着什么？**  
    如何理解“将 image2image translation 视为从输入域到 latent 域的编码问题”？
    
9. **如果输入图像不在 StyleGAN 的训练分布中（out-of-domain），pSp 如何处理？**  
    编码器如何将这类图像也嵌入到 W⁺ 中并由 StyleGAN 生成？











### 1.Projected GAN

- Projected GANs主要的贡献是多尺度判别器，多尺度判别器主要的改进是两部分：**CCM（跨通道混合）和CSM（跨尺度混合）**
- **特征投影器**:将真实的和生成的图像转化为判别器的输入空间