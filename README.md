# chatglm3.openvino Demo

Here is an example of how to deploy ChatGLM3 using OpenVINO

## 1. Environment configuration

We recommend that you create a new virtual environment and then install the dependencies as follows.

```
python3 -m venv openvino_env

source openvino_env/bin/activate

python3 -m pip install --upgrade pip

pip install wheel setuptools

pip install -r requirements.txt
```

## 2. Convert model

Since the Huggingface model needs to be converted to an OpenVINO IR model, you need to download the model and convert.

```
python3 convert.py --model_id THUDM/chatglm3-6b --output {your_path}/chatglm3-6b
```

### Parameters that can be selected

* `--model_id` - path (absolute path) to be used from Huggngface_hub (https://huggingface.co/models) or the directory
  where the model is located.
* `--output` - the address where the converted model is saved
* If you have difficulty accessing `huggingface`, you can try to use `mirror-hf` to download

  Linux
     ```
     export HF_ENDPOINT=https://hf-mirror.com
     ```
  Windows Powershell
     ```
     $env:HF_ENDPOINT = "https://hf-mirror.com"
     ```
  Download model
     ```
     huggingface-cli download --resume-download --local-dir-use-symlinks False THUDM/chatglm3-6b --local-dir {your_path}/chatglm3-6b
     ```

parameter:

* `--model_id` - model_id is used to download from Huggngface_hub (https://huggingface.co/models) or the path to the
  directory where the pytorch model is located.
* `--output` - Path to save the model.

## 3. Quantitative model (optional)

```
python3 quantize.py --model_path {your_path}/chatglm3-6b --precision int4 --output {your_path}/chatglm3-6b-int4
```

### Parameters that can be selected

* `--model_path` - The path to the directory where the OpenVINO IR model is located.
* `--precision` - Quantization precision: int8 or int4.
* `--output` - Path to save the model.

## 4. Run the streaming chatbot

```
python3 chat.py --model_path {your_path}/chatglm3-6b --max_sequence_length 4096 --device CPU
```

### Parameters that can be selected

* `--model_path` - The path to the directory where the OpenVINO IR model is located.
* `--max_sequence_length` - Maximum size of output tokens.
* `--device` - The device to run inference on.

## example

```
User: Hello
AI Assistant: Hello! Is there anything I can do to help you?

User: Who are you?
ChatGLM3-6B-OpenVINO: I am an artificial intelligence assistant named ChatGLM3-6B, which was developed from a language model jointly trained by Tsinghua University’s KEG Laboratory and Zhipu AI Company in 2023. My role is to provide appropriate responses and support to users' questions and requests.

User: Please tell me an interesting story
ChatGLM3-6B-OpenVINO: Once upon a time, there was a little boy named Xiao Ming who loved animals very much. One day, while walking in the forest, he discovered a very beautiful bird. The bird was injured and could not fly. Xiao Ming felt very distressed and decided to take care of the little bird. Xiao Ming took the bird home, built a small nest for it, and found some soft grass and food. Every day, he feeds the bird and changes its water. Gradually, the bird's injury recovered and began to fly around Xiao Ming's home. They became very good friends. However, one day, Xiao Ming's parents told him that they must send Xiao Ming's little bird to the forest. Xiao Ming was very sad because he had become good friends with Xiaoniao. However, his parents told him that the bird would be more free in the forest and that he could continue to watch the bird's life in the forest. So, Xiao Ming and his parents sent the bird to the forest. The little bird is very happy because it can fly again and has many other little animal friends. Xiao Ming also felt very happy because he knew that even if he could not always have the birds, he could still appreciate their beauty in nature. From then on, Xiao Ming often came to the forest to look for birds.

User: Please give this story a title
ChatGLM3-6B-OpenVINO: "The Power of Friendship: Xiao Ming and Bird's Forest Adventure"
```

## common problem

1. Why does a huggingface link error appear when importing a local model?
    - Downgrade transformers library to version 4.37.2

2. Do I need to install the OpenVINO C++ inference engine?
    - unnecessary

3. Do I have to use Intel hardware?
    - We only tried it on Intel devices, and we recommend using x86 architecture Intel devices, including but not
      limited to:
    - Intel CPU, including personal computer CPU and server CPU.
    - Intel's discrete graphics card. For example: ARC A770 graphics card.