# Custom StarGAN v2

## Set Up

```bash
conda create -n custom-stargan-v2 python=3.6.7
conda activate custom-stargan-v2
conda install -y pytorch=1.4.0 torchvision=0.5.0 cudatoolkit=9.0 -c pytorch
conda install x264=='1!152.20180717' ffmpeg=4.0.2 -c conda-forge
pip install -r requirements.txt
```

## Load Pretrained Model
```bash
bash download.sh pretrained-network-celeba-hq
bash download.sh pretrained-network-afhq
bash download.sh wing
```

## Preprocess Custom Images

```bash
python main.py --mode align \
               --inp_dir assets/representative/custom/male \
               --out_dir assets/representative/celeba_hq/src/male
```

## Generate Fake Images

```bash
python main.py --mode sample --num_domains 2 --resume_iter 100000 --w_hpf 1 \
               --checkpoint_dir expr/checkpoints/celeba_hq \
               --result_dir expr/results/celeba_hq \
               --src_dir assets/representative/celeba_hq/src \
               --ref_dir assets/representative/celeba_hq/ref

python main.py --mode sample --num_domains 3 --resume_iter 100000 --w_hpf 0 \
               --checkpoint_dir expr/checkpoints/afhq \
               --result_dir expr/results/afhq \
               --src_dir assets/representative/afhq/src \
               --ref_dir assets/representative/afhq/ref
```

```bash
python main.py --mode custom \
               -o starganv2_cross_celeba.jpg \
               -s assets/representative/celeba_hq/src/male/test_me.jpg \
               -r assets/representative/celeba_hq/ref/female/081871.jpg

python main.py --mode custom \
               -o starganv2_cross_afhq.jpg \
               -s assets/representative/afhq/src/cat/test_cat.jpg \
               -r assets/representative/afhq/ref/dog/flickr_dog_001072.jpg
```

## Evaluate Model Performance

```bash
python main.py --mode eval --num_domains 2 --w_hpf 1 \
               --resume_iter 100000 \
               --train_img_dir data/celeba_hq/train \
               --val_img_dir data/celeba_hq/val \
               --checkpoint_dir expr/checkpoints/celeba_hq \
               --eval_dir expr/eval/celeba_hq

python main.py --mode eval --num_domains 3 --w_hpf 0 \
               --resume_iter 100000 \
               --train_img_dir data/afhq/train \
               --val_img_dir data/afhq/val \
               --checkpoint_dir expr/checkpoints/afhq \
               --eval_dir expr/eval/afhq
```

### Weight & Bias API Key
47e2707e8eb126cec05bb252988b43f16fa5016b