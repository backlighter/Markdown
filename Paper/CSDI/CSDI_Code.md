# Train.py
+ Train_loader、valid_loader、test_loader
+ Model CSDI_Traffic

# get_dataloader_@f
+ observed_mask 构造缺失后的mask矩阵
+ observed_value 构造缺失后的数据
+ values 未构造缺失时的数据
+ return train_loader、valid_loader、test_loader
    + train_loader包含、train_X, train_Y,  train_mask

# Traffic_Dataset
+ init
    + eval_length 滑窗长度
    + observed_values 构造缺失后的数据
    + observed_masks 构造缺失后的mask矩阵
    + gt_masks 构造缺失后的mask矩阵 此处是不是有问题？
    + ifTest_@f {如果是测试集}
        + observed_values 为真实数据
        + observed_masks = np.ones_like(self.gt_masks) 全部设置为1
+ __getitem__ 
    + 使用[]即可调用
    ```
    s = {
            "observed_data": self.observed_values[index],
            "observed_mask": self.observed_masks[index],
            "gt_mask": self.gt_masks[index],
            "timepoints": np.arange(self.eval_length),
        }
    ```



# CSDI_Traffic_@class
+ 继承CSDI_base
+ function有
    + process_data
        + (处理数据)
        + observed_data 构造缺失后的数据 窗口大小为12
        + observed_mask 构造缺失后的mask 
        + observed_tp   值为(观测值的timestamp)[ 0  1  2  3  4  5  6  7  8  9 10 11 ]
        ```
        return (
            observed_data,
            observed_mask,  # 构造缺失后的mask 
            observed_tp,    # 值为(观测值的timestamp)
            gt_mask,        # 构造缺失后的mask   ???
            for_pattern_mask,# 构造缺失后的mask 
            cut_length,    # Batch_size
        )
        ```
# CSDI_base_@class
+ init()
    + target_dim 目标维度
    + emb_time_dim  时间嵌入维度 default 128
    + emb_feature_dim 特征嵌入维度 default 16
    + is_unconditional 是否使用条件信息  True
    + target_strategy 使用的目标策略
    + embed_layer     嵌入层
    + config_diff    diffusion的配置
    + diffmodel =  diff_CSDI(config_diff,input_dim)
    + **有状态的扩散input_dim=2,无状态的扩散input_dim=1**
    ```
    if config_diff["schedule"] == "quad":
            self.beta = np.linspace(
                float(config_diff["beta_start"]) ** 0.5, float(config_diff["beta_end"]) ** 0.5, self.num_steps
            ) ** 2
        elif config_diff["schedule"] == "linear":
            self.beta = np.linspace(
                float(config_diff["beta_start"]), float(config_diff["beta_end"]), self.num_steps
            )
    ```
    ```
    Quad 调度：适用于在扩散过程中希望更平滑地调整噪声的添加，尤其是在噪声添加的早期和后期阶段。这种调度方式在某些情况下可以更好地控制扩散过程，避免过度噪声或不足噪声的情况。

    Linear 调度：适用于简单直接的噪声添加控制，线性变化的 beta 可以带来更均匀的噪声添加步骤，适合对噪声添加没有特殊要求的应用场景。
    ```

# time_embedding()
    + 这个就是Transformer中的positional Encodering
    + 




# get_randmask()
+ 产生cond_mask
    + 


# get_side_info()\get_side_info  产生两种mask 我在思考 是否这块需要删掉，因为我们之前已经做过mask了
    + 将observed_tp 输入 得到time_embed
    + 将

# calc_loss 计算diffusion在单个时间步的损失
# calc_loss_valid 这个函数用于在验证过程中计算所有时间步的平均损失


# set_input_to_diffmodel
+ 缺失数据和加噪后的缺失数据作为输出
+ total_input=缺失数据和加噪后的缺失数据 cat起来
```
def set_input_to_diffmodel(self, noisy_data, observed_data, cond_mask):
        if self.is_unconditional == True:
            total_input = noisy_data.unsqueeze(1)  # (B,1,K,L)
        else:  # cond_mask 这个地方 用于表示已经构造缺失的mask
            cond_obs = (cond_mask * observed_data).unsqueeze(1) # cond_obs==miss_data
            noisy_target = ((1 - cond_mask) * noisy_data).unsqueeze(1) #1 - cond_mask 表示缺失处，(1 - cond_mask) * noisy_data 在缺失处加噪
            # noisy_target  在缺失处加噪后的结果
            # 将 miss_data和 在缺失处加噪后的miss_data输入进去
            total_input = torch.cat([cond_obs, noisy_target], dim=1)  # (B,2,K,L)
        return total_input # 
```

# diff_model.py
# diff_CSDI
+ self.diffmodel = diff_CSDI(config_diff, input_dim)

## USE
+ Training
    + predicted = self.diffmodel(total_input, side_info, t)  # (B,K,L) #训练时调用diffmodel
    + total_input、side_info、t 
    + (B,2,N,L) 、(B,145,N,L)、(B)

+ init
    + 继承 nn.Module
    + channels = 这个地方的channel是干嘛的？？
    + diffusion_embedding = DiffusionEmbedding(num_steps,embedding_dim)
    + input_projection  = Conv1d_with_init  一个一维卷积
    + output_projection1 = Conv1d_with_init(self.channels, 1, 1)
    ```
    残差块
    self.residual_layers = nn.ModuleList(
            [
                ResidualBlock(
                    side_dim=int(config["side_dim"]),
                    channels=self.channels,
                    diffusion_embedding_dim=int(config["diffusion_embedding_dim"]),
                    nheads=int(config["nheads"]),
                )
                for _ in range(int(config["layers"]))
            ]
        )
    ```
+ forward
    + (x,cond_info,diffusion_step)
    + 

```
不使用Unet作为噪声预测器，使用了一种基于Transformer和残差块的架构来预测噪声
```


# DiffusionEmbedding
+ 
    