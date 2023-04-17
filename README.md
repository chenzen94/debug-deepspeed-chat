# debug-deepspeed-chat
在IDE里调试DeepSpeed-Chat。原来的DeepSpeed-Chat库需要deepspeed命令启动，没法在IDE里调试。这里增加[my_deepspeed](alpaca_rlhf/training_model/my_deepspeed.py)脚本，在IDE里用这个脚本启动每一步，就可以一步一步调试DeepSpeed-Chat了。

## Stey by Step
### Debug Using PyCharm
- [Bootstrap Script](alpaca_rlhf/training_model/my_deepspeed.py)
  - single gpu args
    - step1: --num_gpus 1 alpaca_rlhf/training_model/step1_supervised_finetuning/main.py --gradient_accumulation_steps 2 --lora_dim 128 --zero_stage 0 --deepspeed --output_dir /root/autodl-tmp/actor
    - step2: --num_gpus 1 alpaca_rlhf/training_model/step2_reward_model_finetuning/main.py --num_padding_at_beginning 0 --gradient_accumulation_steps 2 --zero_stage 0 --deepspeed --output_dir /root/autodl-tmp/critic
    - step3: alpaca_rlhf/training_model/step3_rlhf_finetuning/main.py --actor_model_name_or_path /root/autodl-tmp/actor --critic_model_name_or_path /root/autodl-tmp/critic  --actor_zero_stage 0 --critic_zero_stage 0 --num_padding_at_beginning 0 --gradient_accumulation_steps 2 --deepspeed --actor_lora_dim 128 --enable_hybrid_engine --actor_gradient_checkpointing --output_dir /root/autodl-tmp/final

### Remote Debug
- 在[Bootstrap Script](alpaca_rlhf/training_model/my_deepspeed.py)的环境变量里加入ninja所在目录
  - os.environ["PATH"] = os.environ["PATH"] + ":/root/miniconda3/bin/"