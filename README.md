# LUFFY Development Repository

> 🚧 **Development Branch** - This is the main development repository for LUFFY (Learning to Reason Under Off‑Policy Guidance)

## About LUFFY

LUFFY is a reinforcement learning framework that bridges the gap between zero-RL and imitation learning by incorporating off-policy reasoning traces into the training process. This repository contains the core implementation and development work.

## 🔧 Development Status

This repository is under active development. Many features are currently being implemented or need refactoring.

## 🚀 Quick Start

⚠️ **Note**: This development version has incomplete implementations. Many features are marked as TODO and need to be completed before production use.

```bash
# Clone the repository
git clone <repository-url>
cd LUFFY

# Install dependencies
pip install -r luffy/requirements.txt

# Note: Some functionality is incomplete - check TODO list below for details
```

## 📁 Repository Structure

```
LUFFY/
├── luffy/                 # Core framework
│   ├── deepscaler/        # Scaling utilities (⚠️ API integration needed)
│   ├── verl/              # RL training components (⚠️ Some features incomplete)
│   └── ...
├── data/                  # Training data and scripts
├── eval_scripts/          # Evaluation utilities
├── exp_scripts/           # Experiment scripts
└── README.md              # This file
```

## ⚠️ Development Notes

- This is a **development version** with incomplete implementations
- Many functions contain TODO markers indicating pending work
- API integrations (OpenAI, Gemini) are currently placeholder implementations
- FSDP and distributed training features need completion


### 🔴 High Priority TODOs

- **API Integration**: OpenAI and Gemini API implementations need completion
- **Reward System**: Parallel processing and validation for reward computation  
- **FSDP Training**: Model loading and distributed training setup
- **Data Processing**: Batch dimension operations and tensor reshaping

### 📝 Complete TODO List

**luffy/deepscaler/utils.py**
- Line 45: Add logging for API calls and errors
- Line 46: Support batch processing for multiple prompts
- Line 47: Add timeout configuration for API calls
- Line 107: Implement Vertex AI initialization and authentication
- Line 108: Configure safety settings for content generation
- Line 109: Set up GenerativeModel with proper system instructions
- Line 110: Implement retry logic with exponential backoff
- Line 111: Add comprehensive error handling for API access issues
- Line 112: Handle rate limiting and quota management
- Line 113: Implement response validation and text extraction
- Line 114: Add support for different generation configurations

**luffy/test.py**
- Line 1590: add smaller page sizes when https://github.com/Dao-AILab/flash-attention/pull/824 is merged

**luffy/verl/examples/split_placement/split_monkey_patch.py**
- Line 141: make a canonical logger that supports various backend

**luffy/verl/tests/e2e/check_results.py**
- Line 21: this function needs error handling

**luffy/verl/tests/model/test_transformer.py**
- Line 22: (sgm): add more models for test
- Line 50: (sgm): we can construct the position_ids_rmpad here
- Line 111: (sgm): we can construct the position_ids_rmpad here

**luffy/verl/tests/model/test_transformers_ulysses.py**
- Line 34: (sgm): add more models for test
- Line 81: (sgm): we can construct the position_ids_rmpad here
- Line 159: (sgm): we can construct the position_ids_rmpad here

**luffy/verl/tests/ray/test_high_level_scheduling_api.py**
- Line 25: pass *args and **kwargs is bug prone and not very convincing

**luffy/verl/tests/ray/test_worker_group_basics.py**
- Line 43: pass *args and **kwargs is bug prone and not very convincing

**luffy/verl/verl/mix_src/mix_fsdp_worker.py**
- Line 54: (sgm): support FSDP hybrid shard for larger model
- Line 83: it seems that manual offload is slowly than FSDP offload
- Line 123: (zhangchi.usc1992): 1. support create from random initialized model. 2. Support init with FSDP directly
- Line 199: (zhangchi.usc1992, shengguangming) fix me. Current, auto_wrap_policy causes HFRollout to hang in Gemma
- Line 207: add transformer policy
- Line 226: add more optimizer args into config
- Line 252: (sgm): support FSDP hybrid shard for larger model
- Line 263: a sharding manager that do nothing?
- Line 391: here, we should return all metrics
- Line 517: support DCP and save sharded checkpoints

**luffy/verl/verl/mix_src/mix_trainer.py**
- Line 90: add other ways to estimate advantages
- Line 168: support each role have individual ray_worker_group_cls,
- Line 293: we have to make sure the batch size is divisible by the dp size
- Line 599: make a canonical logger that supports various backend
- Line 637: add response length

**luffy/verl/verl/mix_src/mix_trainer_acc_rebatch.py**
- Line 63: we have to make sure the batch size is divisible by the dp size
- Line 437: make a canonical logger that supports various backend
- Line 592: check path
- Line 628: from remote not implemented yet

**luffy/verl/verl/models/llama/megatron/layers/parallel_attention.py**
- Line 380: llama does not have dropout in the config??

**luffy/verl/verl/models/llama/megatron/layers/parallel_decoder.py**
- Line 78: add sequence parallel operator reduce_scatter here
- Line 86: add sequence parallel operator all_gather here
- Line 90: add sequence parallel operator reduce_scatter here

**luffy/verl/verl/models/llama/megatron/modeling_llama_megatron.py**
- Line 330: for better performance, the sp padding should be removed at each layer. Not sure the performance gap
- Line 588: for better performance, the sp padding should be removed at each layer. Not sure the performance gap

**luffy/verl/verl/models/registry.py**
- Line 21: (sgm): HF may supported more than listed here, we should add more after testing

**luffy/verl/verl/models/transformers/llama.py**
- Line 88: These transpose are quite inefficient but Flash Attention requires the layout [batch_size, sequence_length, num_heads, head_dim]. We would need to refactor the KV cache

**luffy/verl/verl/protocol.py**
- Line 114: Optimize memory usage during tensor reshaping
- Line 115: Add support for different tensor types and shapes
- Line 136: Optimize tensor view operations for performance
- Line 137: Add error handling for invalid batch dimensions
- Line 169: (zhangchi.usc1992) add consistency check
- Line 265: we can actually lift this restriction if needed
- Line 351: (zhangchi.usc1992) whether to copy

**luffy/verl/verl/single_controller/ray/base.py**
- Line 439: create a class with customizable name

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/arg_utils.py**
- Line 64: (shengguangming): delete the unused args
- Line 147: (woosuk): Support fine-grained seeds (e.g., seed per request).

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/llm.py**
- Line 237: (shengguangming): maybe we can hack the autoregressive logics without only apply post process for better performance
- Line 241: (sgm): we can optimize it by making the dataloader yield List[int] without padding.
- Line 257: (shengguangming): can be optimzied by rewrite the Sampler._get_logprobs() logits

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/llm_engine_sp.py**
- Line 99: (woosuk): Print more configs in debug mode.
- Line 101: currently is hfconfig
- Line 112: (shengguangming): maybe we can choose init here or from arguments
- Line 145: check get_lora_tokenizer func
- Line 586: check this input
- Line 661: we may not need to decode

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/model_loader.py**
- Line 67: (shengguangming): latest commit in vllm fix awq for this function and add load_weights
- Line 96: (pad to be divided by 4)
- Line 224: (zhuohan): Change the get_logits part to a separate stage.

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/tokenizer.py**
- Line 56: (sgm): the lora tokenizer is also passed, but may be different

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/weight_loaders.py**
- Line 62: check megatron
- Line 84: need to implement a general way to deal with prefix

**luffy/verl/verl/third_party/vllm/vllm_v_0_3_1/worker.py**
- Line 109: do not use cupy
- Line 209: (woosuk): Profile swapping overhead and optimize if needed.
- Line 291: (shengguangming): maybe we should also flag the megatron is initialized

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/arg_utils.py**
- Line 109: (shengguangming): delete the unused args
- Line 192: (woosuk): Support fine-grained seeds (e.g., seed per request).
- Line 257: spec config

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/config.py**
- Line 136: for multimodal model

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/llm.py**
- Line 268: (shengguangming): maybe we can hack the autoregressive logics without only apply post process for better performance
- Line 272: (sgm): we can optimize it by making the dataloader yield List[int] without padding.
- Line 288: (shengguangming): can be optimzied by rewrite the Sampler._get_logprobs() logits

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/llm_engine_sp.py**
- Line 128: (woosuk): Print more configs in debug mode.
- Line 130: currently is hfconfig
- Line 143: (shengguangming): maybe we can choose init here or from arguments
- Line 145: check tokenizer class
- Line 153: don't know what's the usage
- Line 228: (sgm): add for verl but we may not tokenizer in Rollout
- Line 237: check whether we should rebuild the CUDAGraph every iter when offload/load KVCache

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/megatron_weight_loaders.py**
- Line 67: check megatron
- Line 254: need to implement a general way to deal with prefix
- Line 272: (shengguangming): latest commit in vllm fix awq for this function and add load_weights
- Line 325: (pad to be divided by 4)
- Line 337: remove dependencies from megatron

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/model_loader.py**
- Line 141: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm
- Line 226: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/model_runner.py**
- Line 274: (sgm): perform sampling on rank 0

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/parallel_state.py**
- Line 236: this will hang
- Line 245: will hang when used with device mesh
- Line 247: init using device mesh

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/spmd_gpu_executor.py**
- Line 62: (sgm): verl not support speculative decode now
- Line 208: (sgm): not implemented async executor yet

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/tokenizer.py**
- Line 61: (sgm): the lora tokenizer is also passed, but may be different

**luffy/verl/verl/third_party/vllm/vllm_v_0_4_2/worker.py**
- Line 30: (sgm): check why vllm has similar file in vllm.model_executor.parallel_utils.parallel_state
- Line 270: (sgm): check whether need this

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/arg_utils.py**
- Line 53: (sgm): check this
- Line 54: (sgm): check this
- Line 143: (shengguangming): delete the unused args
- Line 226: (woosuk): Support fine-grained seeds (e.g., seed per request).
- Line 366: spec config

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/config.py**
- Line 191: check whether this is necessary

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/llm.py**
- Line 148: check usagecontext
- Line 205: (sgm): we can optimize it by making the dataloader yield List[int] without padding.
- Line 221: (shengguangming): can be optimzied by rewrite the Sampler._get_logprobs() logits

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/llm_engine_sp.py**
- Line 143: (woosuk): Print more configs in debug mode.
- Line 160: (shengguangming): maybe we can choose init here or from arguments
- Line 262: (sgm): add for verl but we may not tokenizer in Rollout
- Line 271: check whether we should rebuild the CUDAGraph every iter when offload/load KVCache

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/megatron_weight_loaders.py**
- Line 67: check megatron
- Line 254: need to implement a general way to deal with prefix
- Line 272: (shengguangming): latest commit in vllm fix awq for this function and add load_weights

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/model_loader.py**
- Line 152: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm
- Line 239: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/parallel_state.py**
- Line 94: (sgm): deviate from the v0.5.4, not pp now
- Line 138: check why True is not work in Ray trainer
- Line 165: check why True is not work in Ray trainer
- Line 177: init using device mesh (not support hybrid engine now)
- Line 249: check why True is not work in Ray trainer
- Line 253: init using device mesh (not support hybrid engine now)

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/spmd_gpu_executor.py**
- Line 65: (sgm): verl not support speculative decode now
- Line 243: (sgm): not implemented async executor yet

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/tokenizer.py**
- Line 61: (sgm): the lora tokenizer is also passed, but may be different

**luffy/verl/verl/third_party/vllm/vllm_v_0_5_4/worker.py**
- Line 29: (sgm): check why vllm has similar file in vllm.model_executor.parallel_utils.parallel_state
- Line 84: we don't need driver
- Line 103: (sgm): set correct model runner class
- Line 301: (sgm): check whether need this

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/llm.py**
- Line 147: check usagecontext
- Line 170: (sgm): we can optimize it by making the dataloader yield List[int] without padding.
- Line 186: (shengguangming): can be optimzied by rewrite the Sampler._get_logprobs() logits

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/llm_engine_sp.py**
- Line 174: (woosuk): Print more configs in debug mode.
- Line 336: (sgm): add for verl but we may not tokenizer in Rollout
- Line 345: check whether we should rebuild the CUDAGraph every iter when offload/load KVCache

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/megatron_weight_loaders.py**
- Line 68: check megatron
- Line 255: need to implement a general way to deal with prefix
- Line 273: (shengguangming): latest commit in vllm fix awq for this function and add load_weights

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/model_loader.py**
- Line 170: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm
- Line 273: (sgm): This is a hack, we need to register the load_weight() func for each model in vllm

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/parallel_state.py**
- Line 97: (sgm): deviate from the v0.5.4, not pp now
- Line 144: check why True is not work in Ray trainer
- Line 172: check why True is not work in Ray trainer
- Line 185: init using device mesh (not support hybrid engine now)
- Line 257: check why True is not work in Ray trainer
- Line 262: init using device mesh (not support hybrid engine now)

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/spmd_gpu_executor.py**
- Line 73: (sgm): verl not support speculative decode now
- Line 246: (sgm): not implemented async executor yet

**luffy/verl/verl/third_party/vllm/vllm_v_0_6_3/worker.py**
- Line 33: (sgm): check why vllm has similar file in vllm.model_executor.parallel_utils.parallel_state
- Line 92: we don't need driver
- Line 110: (sgm): set correct model runner class
- Line 311: (sgm): check whether need this

**luffy/verl/verl/trainer/fsdp_sft_trainer.py**
- Line 77: add checkpoint manager
- Line 140: (zhangchi.usc1992):
- Line 159: Implement model loading with proper initialization context
- Line 160: Add support for different model types and configurations
- Line 161: Implement memory-efficient model loading for large models
- Line 162: Add model validation and compatibility checks
- Line 165: Complete model loading implementation
- Line 166: Add support for custom model architectures
- Line 167: Implement proper dtype and attention configuration
- Line 170: Implement gradient checkpointing configuration
- Line 171: Add memory usage optimization strategies
- Line 172: Configure mixed precision training settings
- Line 173: Implement FSDP sharding and wrapping policies
- Line 174: Add CPU offloading configuration for memory optimization
- Line 175: Set up distributed training parameters properly
- Line 178: Initialize FSDP wrapped model
- Line 301: add a unified tracking
- Line 318: (zhangchi.usc1992) add back checkpoint manager. Currently, it blocks when uploading to hdfs. So very slow.

**luffy/verl/verl/trainer/main_ppo.py**
- Line 50: Implement reward computation for different data sources
- Line 53: Add support for parallel processing of reward computation
- Line 54: Implement proper sequence decoding and validation
- Line 55: Add thread-safe logging and debugging functionality
- Line 56: Optimize memory usage for large batch processing
- Line 62: Extract and validate prompt and response sequences
- Line 63: Decode sequences to text format
- Line 64: Apply appropriate reward function based on data source
- Line 65: Handle edge cases and error conditions
- Line 70: Implement batch-wise reward computation
- Line 71: Add proper error handling and validation

**luffy/verl/verl/trainer/ppo/ray_trainer.py**
- Line 129: add other ways to estimate advantages
- Line 207: add response length
- Line 330: support each role have individual ray_worker_group_cls,
- Line 379: we have to make sure the batch size is divisible by the dp size
- Line 632: check path
- Line 667: from remote not implemented yet
- Line 880: make a canonical logger that supports various backend

**luffy/verl/verl/utils/checkpoint/fsdp_checkpoint_manager.py**
- Line 101: shall we remove previous ckpt every save?
- Line 135: address optimizer is None

**luffy/verl/verl/utils/hdfs_io.py**
- Line 67: (haibin.lin):
- Line 102: (haibin.lin):

**luffy/verl/verl/utils/megatron_utils.py**
- Line 202: (sgm): check how to disable megatron timers

**luffy/verl/verl/utils/model.py**
- Line 164: we can make this faster
- Line 272: to find a better way to load mistral7b-rm lm_head

**luffy/verl/verl/utils/torch_functional.py**
- Line 362: add them back

**luffy/verl/verl/workers/actor/megatron_actor.py**
- Line 158: (zhangchi.usc1992): actually, this function should only return log_prob and this logic should be handled by user outside
- Line 225: actually, we just need to control the sampling order.
- Line 301: we may use the new schedule instead

**luffy/verl/verl/workers/critic/megatron_critic.py**
- Line 176: we may use the new schedule instead

**luffy/verl/verl/workers/fsdp_workers.py**
- Line 88: (sgm): support FSDP hybrid shard for larger model
- Line 117: it seems that manual offload is slowly than FSDP offload
- Line 157: (zhangchi.usc1992): 1. support create from random initialized model. 2. Support init with FSDP directly
- Line 225: (zhangchi.usc1992, shengguangming) fix me. Current, auto_wrap_policy causes HFRollout to hang in Gemma
- Line 233: add transformer policy
- Line 252: add more optimizer args into config
- Line 278: (sgm): support FSDP hybrid shard for larger model
- Line 289: a sharding manager that do nothing?
- Line 416: here, we should return all metrics
- Line 811: (sgm): we may need to extract it to dp_reward_model.py

**luffy/verl/verl/workers/megatron_workers.py**
- Line 106: (sgm): Currently, we only support reference model param offload
- Line 204: add more optimizer args into config
- Line 338: here, we should return all metrics
- Line 444: (sgm): support critic model offload
- Line 478: support vpp here
- Line 507: add more optimizer args into config
- Line 667: add more optimizer args into config
- Line 720: reward model use itself tokenizer instead of sft tokenizer

**luffy/verl/verl/workers/reward_model/megatron/reward_model.py**
- Line 145: (sgm): check why is bfloat16
- Line 192: actually, we just need to control the sampling order.
- Line 233: we may use the new schedule instead

**luffy/verl/verl/workers/rollout/hf_rollout.py**
- Line 98: filter out the seq with no answers like ds-chat

**luffy/verl/verl/workers/sharding_manager/fsdp_ulysses.py**
- Line 49: check how to set seed for each model
- Line 56: check how to set seed for each model

**luffy/verl/verl/workers/sharding_manager/fsdp_vllm.py**
- Line 82: offload FSDP model weights
- Line 113: Current impl doesn't consider FSDP with torch micro-dp
- Line 122: Current impl doesn't consider FSDP with torch micro-dp
- Line 130: shall we build a micro_dp group for vllm when integrating with vLLM?

**luffy/verl/verl/workers/sharding_manager/megatron_vllm.py**
- Line 76: after binding to the memory buffer, we can load the checkpoint here
- Line 253: (sgm): this may not be true for FSDP -> vLLM
- Line 323: (zhangchi.usc1992) We can consider copy non-tp weight to another infer buffer.
1. Pick a TODO item from the list above
2. Implement the functionality
3. Test your implementation
4. Update this README when TODOs are completed

