

### Dependencies
python3.8.10 2080ti

- pip install torch==1.10.0+cu111 torchvision==0.11.0+cu111 torchaudio==0.10.0+cu111 -f https://download.pytorch.org/whl/torch_stable.html
- pip install ffmpeg-python
- pytorch3d     (tested with v0.4.0)
- 参考[adnerf](https://github.com/aurelianocyp/AD-NeRF)里安装pytorch3d

### Animating a Face Mesh from Audio

Download the [pretrained models](https://github.com/facebookresearch/meshtalk/releases/download/pretrained_models_v1.0/pretrained_models.zip) and unzip them.
Make sure your python path contains the root directory (`export PYTHONPATH=<your_meshtalk_root_directory>`).

Then, run
```
python animate_face.py --model_dir <your_pretrained_model_dir> --audio_file <your_speech_snippet.wav> --output <your_output_file.mp4>
```
See a description of command line arguments via `python animate_face.py --help`. We provide a neutral face template mesh in `assets/face_template.obj`. Note that the rendered results look slightly different than in the paper and supplemental video because we use a differnt (open source) rendering engine in this repository.

### Training your own MeshTalk version

All training code is available in the `training` directory. The training follows a two-step recipe. First, learn the latent expression code by running `train_step1.py`, second learn the autoregressive model by running `train_step2.py`.

Note that we only provide a dataloader that produces dummy data which is always zero. For your training data, implement your own data reader that produces the same assets (template mesh, mesh sequence, audio sequence) as the dummy data reader.

One potential dataset to use for MeshTalk is the [Multiface](https://github.com/facebookresearch/multiface) dataset which contains a subset (13 subjects) of the data used in the paper. The dataset includes tracked meshes and audio files.

Note that the geometries in multiface have a slightly different topology than in meshtalk. To convert geometries from multiface to meshtalk, run
```
python utils/multiface2meshtalk.py 
```
on the `.bin` files containing the vertex positions of the multiface meshes. Note that the input must be the `.bin` files from the `tracked_mesh` directories in multiface, **not** the `.obj` files. The output is a `.obj` file in the same format as `assets/face_template.obj`.

将需要转化的bin文件放在主目录下的multiface/bin文件夹中。结果会自动生成在multiface/objs文件夹中。

## License

The code and dataset are released under [CC-NC 4.0 International license](https://github.com/facebookresearch/BinauralSpeechSynthesis/blob/main/LICENSE).

## 记录
需要保存每个网格的话使用render_mine.py.在utils里面进行替换
