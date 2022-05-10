# EPR
Implementation of Weakly Supervised Explainable Phrasal Reasoning for Natural Language Inference

## Training
dataset_name: mnli/snli  
dataset_path: path containing MNLI/SNLI dataset  
mode: local=0, global=1, concat=2
### Data process
```
python data_process.py \
--do_train \
--do_dev \
--do_test \
--data_path ${dataset_path} \
--save_encoding_path ./save_encoding/${dataset_name}/token/
```

### Alignment
```
python aligner.py \
--do_test \
--do_train \
--do_dev \
--_lambda 0.6 \ # global ratio
--encoding_cache_path ./save_encoding/${dataset_name}/token/ \
--save_encoding_path ./save_encoding/${dataset_name}/alignment/
```

### Model training
```
python main_finetune_epr.py \
--batch_size 256 \
--mode ${mode} \
--model_name epr_model \
--alignment_cache_path ./save_encoding/${dataset_name}/alignment/ \
--model_cache_path ./save_model/${dataset_name}/ \
--token_cache_path ./save_encoding/${dataset_name}/token/ \
--lr 5e-5 \
--is_train
```

## Checkpoint
https://drive.google.com/file/d/12Z990_X3Ocu5_Ixgmm8xXorZIGWdY_ud/view?usp=sharing

## Evaluation
### Phrasal prediction
```
python explain_epr.py \
--alignment_cache_path ./save_encoding/${dataset_name}/alignment/ \
--model_cache_path ./save_model/${dataset_name}/ \
--token_cache_path ./save_encoding/${dataset_name}/token/ \
--model_name epr_model \
--mode ${mode} \
--result_file ./text_file/result.json \
--dataset ${dataset_name}
```

### Calculate F score
```
python micro_eva.py
```
