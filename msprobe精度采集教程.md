 执行前记得打patch
 git apply 0001-add-msprobe.patch
 
``` shell
# 根据项目情况进行修改
export PYTHONPATH="/data/tiankuan/czh1/FlagScale:/data/tiankuan/czh1/FlagScale/flagscale/train:${PYTHONPATH}"
export TE_FL_PREFER=vendor
export OMP_NUM_THREADS=4
source /usr/local/Ascend/cann/set_env.sh

export NPU_ASD_ENABLE=0
export MSPROBE_ENABLE=1

export QWEN35_WORKDIR="/data/tiankuan/czh1/outputs/qwen35-tutorial"
export MSPROBE_ROOT="$QWEN35_WORKDIR/msprobe"
export MSPROBE_CONFIG_PATH="$MSPROBE_ROOT/config_tp4_pp2_smoke.json"

export QWEN35_HF_PATH="/data/tiankuan/czh1/models/models/qwen--Qwen3.5-4B/snapshots/master"
export QWEN35_DATA_PATH="$QWEN35_WORKDIR/data/energon/wds-1"
export QWEN35_VISION_ROOT="$QWEN35_WORKDIR/data/llava-first-5k"
export QWEN35_LOAD="$QWEN35_WORKDIR/checkpoints/release-tp2"
export QWEN35_OUTPUT_DIR="$QWEN35_WORKDIR/train-vendor-msprobe-tp4-pp2"
export QWEN35_VISIBLE_DEVICES="0,1,2,3,4,5,6,7"
export QWEN35_TRAIN_ITERS=2


执行训练采集
python -m flagscale.cli train qwen35 \
  --config examples/qwen35/conf/train_vendor_verify.yaml
```

新增：config_tp4_pp2_smoke.json

``` shell
{

"task": "statistics",

"dump_path": "/data/tiankuan/czh1/outputs/msprobe_dumps",

"rank": [0, 4],

"step": [0],

"level": "L0",

"async_dump": false,

"extra_info": true,

"risk_level": "FOCUS",

"statistics": {

"scope": [],

"list": [],

"tensor_list": [],

"data_mode": ["all"],

"summary_mode": "statistics"

}

}
```


