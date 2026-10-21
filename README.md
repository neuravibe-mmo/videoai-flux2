# FLUX.2

**Frontier Visual Intelligence** — State-of-the-art image generation and editing from [Black Forest Labs](https://bfl.ai).

---

<p align="center">
<a href="https://docs.bfl.ai">API Docs</a> •
<a href="https://huggingface.co/black-forest-labs">Hugging Face</a> •
<a href="https://bfl.ai/blog">Blog</a>
</p>

This repo contains minimal inference code to run image generation & editing with our FLUX.2 open-weight models.

## News

- **[15.01.2026]** Today, we release the FLUX.2 [klein] family of models, our fastest models yet. Sub-second generation on consumer GPUs. Read more about it in our [blog post](https://bfl.ai/blog/flux2-klein-towards-interactive-visual-intelligence).
- **[25.11.2025]** We are releasing FLUX.2 [dev], a 32B parameter model for text-to-image generation, and image editing (single reference image and multiple reference images).

## Model Overview

| Name | Step-distilled | Guidance-distilled | Text-to-Image | Image Editing (Single reference) | Image Editing (Multi-reference) | License |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| [FLUX.2 [klein] 4B](https://huggingface.co/black-forest-labs/FLUX.2-klein-4B) | ✅ | ✅ | ✅ | ✅ | ✅ | [apache-2.0](https://huggingface.co/datasets/choosealicense/licenses/blob/main/markdown/apache-2.0.md) |
| [FLUX.2 [klein] 9B](https://huggingface.co/black-forest-labs/FLUX.2-klein-9B) | ✅ | ✅ | ✅ | ✅ | ✅ | [FLUX Non-Commercial License](model_licenses/LICENSE-FLUX-NON-COMMERICAL) |
| [FLUX.2 [klein] 9B KV](https://huggingface.co/black-forest-labs/FLUX.2-klein-9b-kv) | ✅ | ✅ | ✅ | ✅ | ✅ | [FLUX Non-Commercial License](model_licenses/LICENSE-FLUX-NON-COMMERICAL) |
| [FLUX.2 [klein] 4B Base](https://huggingface.co/black-forest-labs/FLUX.2-klein-base-4B) | ❌ | ❌ | ✅ | ✅ | ✅ | [apache-2.0](https://huggingface.co/datasets/choosealicense/licenses/blob/main/markdown/apache-2.0.md) |
| [FLUX.2 [klein] 9B Base](https://huggingface.co/black-forest-labs/FLUX.2-klein-base-9B) | ❌ | ❌ | ✅ | ✅ | ✅ | [FLUX Non-Commercial License](model_licenses/LICENSE-FLUX-NON-COMMERICAL) |
| [FLUX.2 [dev]](https://huggingface.co/black-forest-labs/FLUX.2-dev) | ❌ | ✅ | ✅ | ✅ | ✅ | [FLUX Non-Commercial License](model_licenses/LICENSE-FLUX-NON-COMMERICAL) |

**All models support**: Text-to-Image ✅ | Single-ref Editing ✅ | Multi-ref Editing ✅

## Which Model Should I Use?

| Need | Recommended |
|------|-------------|
| Real-time apps, interactive workflows | [klein] 4B, 9B, or 9B KV (distilled) |
| Consumer GPU (e.g. RTX 3090/4070) | [klein] 4B |
| Fine-tuning, LoRA training | [klein] Base or FLUX.2 [dev] |
| Maximum quality, no latency constraints | FLUX.2 [dev] |

## `FLUX.2 [klein]`

FLUX.2 [klein] is our fastest model family — generating and editing (multiple) images in under a second without sacrificing quality. Built for real-time applications, creative iteration, and deployment on consumer hardware.

### Key Capabilities
- **Sub-second inference** — Generate or edit images under a second on modern hardware
- **Unified generation & editing** — Text-to-image, image editing, and multi-reference in one model
- **Runs on consumer GPUs** — Klein 4B fits in ~8GB VRAM (RTX 3090/4070 and up)
- **Apache 2.0 on 4B** — Open-source, fine-tuning, and customization

### Performance

Klein models define the Pareto frontier for quality vs. latency and VRAM across text-to-image, single-reference editing, and multi-reference generation:

<p align="center">
<img src="assets/klein_benchmark.jpg" alt="FLUX.2 [klein] vs Baselines — Elo vs Latency and VRAM" width="800"/>
</p>
<sub>Higher Elo + Lower Latency/VRAM = Better.</sub>

### The Klein Family

| Model | Best For |
|:---|:---|
| **[klein] 4B** | Maximum speed, consumer hardware, edge deployment |
| **[klein] 9B** | High quality text-to-image; for image editing, 9B KV is faster at equal quality |
| **[klein] 9B KV** | Best quality-to-latency ratio, faster than 4B for multi-reference image editing via [KV caching](docs/flux2_klein_kv_cache.md) |
| **[klein] 4B Base** | Fine-tuning on limited hardware, full customization |
| **[klein] 9B Base** | Research, LoRA training, maximum output diversity |

**Distilled vs Base:**
- Use **Distilled** (4-step) for production apps and real-time generation
- Use **Base** (50-step) for fine-tuning, LoRA training, and maximum flexibility

**Licensing:** 4B models are [Apache 2.0](https://huggingface.co/datasets/choosealicense/licenses/blob/main/markdown/apache-2.0.md). 9B models use the [FLUX.2-dev Non-Commercial License](model_licenses/LICENSE-FLUX-DEV).

### Text-to-image examples

Example focused on realism 
![t2i-klein-grid](assets/t2i_klein_realism.jpg)

Example focused on output diversity
![t2i-klein-others](assets/t2i_klein_others.jpg)

### Editing examples

![i2i-klein](assets/i2i_klein.jpg)

## `FLUX.2 [dev]`

`FLUX.2 [dev]` is a 32B parameter flow matching transformer model capable of generating and editing (multiple) images. The model is released under the [FLUX.2-dev Non-Commercial License](model_licenses/LICENSE-FLUX-DEV) and can be found [here](https://huggingface.co/black-forest-labs/FLUX.2-dev).

Note that the below script for `FLUX.2 [dev]` needs considerable amount of VRAM (H100-equivalent GPU). We partnered with Hugging Face to make quantized versions that run on consumer hardware; below you can find instructions on how to run it on a RTX 4090 with a remote text encoder, for other quantization sizes and combinations, check the [diffusers quantization guide here](docs/flux2_dev_hf.md).

### Text-to-image examples

![t2i-grid](assets/teaser_generation.png)

### Editing examples

![edit-grid](assets/teaser_editing.png)

### Prompt upsampling

`FLUX.2 [dev]` benefits significantly from prompt upsampling. The inference script below offers the option to use both local prompt upsampling with the same model we use for text encoding ([`Mistral-Small-3.2-24B-Instruct-2506`](https://huggingface.co/mistralai/Mistral-Small-3.2-24B-Instruct-2506)), or alternatively, use any model on [OpenRouter](https://openrouter.ai/) via an API call.

See the [upsampling guide](docs/flux2_with_prompt_upsampling.md) for additional details and guidance on when to use upsampling.

## `FLUX.2` autoencoder

The FLUX.2 autoencoder has considerably improved over the [FLUX.1 autoencoder](https://huggingface.co/black-forest-labs/FLUX.1-dev/blob/main/ae.safetensors). The autoencoder is released under [Apache 2.0](https://huggingface.co/datasets/choosealicense/licenses/blob/main/markdown/apache-2.0.md) and can be found [here](https://huggingface.co/black-forest-labs/FLUX.2-dev/blob/main/ae.safetensors). For more information, see our [technical blogpost](https://bfl.ai/research/representation-comparison).

## Local installation

The inference code was tested on GB200 using CUDA 12.9 and Python 3.12.

```bash
python3.12 -m venv .venv
source .venv/bin/activate
pip install -e . --extra-index-url https://download.pytorch.org/whl/cu129 --no-cache-dir
```

## Run the CLI

Before running the CLI, you may download the weights from [here](https://huggingface.co/black-forest-labs/FLUX.2-dev) and set the following environment variables.

```bash
export FLUX2_MODEL_PATH="<flux2_path>"
export AE_MODEL_PATH="<ae_path>"
export KLEIN_4B_MODEL_PATH="<klein_4b_path>"
export KLEIN_4B_BASE_MODEL_PATH="<klein_4b_base_path>"
export KLEIN_9B_MODEL_PATH="<klein_9b_path>"
export KLEIN_9B_KV_MODEL_PATH="<klein_9b_kv_path>"
export KLEIN_9B_BASE_MODEL_PATH="<klein_9b_base_path>"
```

If you don't set the environment variables, the weights will be downloaded automatically.

You can start an interactive session to do both text to image generation as well as editing (one or multiple) images with the following command:

```bash
PYTHONPATH=src python scripts/cli.py
```

## Watermarking

We've added an option to embed invisible watermarks directly into the generated images
via the [invisible watermark library](https://github.com/ShieldMnt/invisible-watermark).

Additionally, we are recommending implementing a solution to mark the metadata of your outputs, such as [C2PA](https://c2pa.org/)

## Citation

If you find the provided code or models useful for your research, consider citing them as:

```bib
@misc{flux-2-2025,
    author={Black Forest Labs},
    title={{FLUX.2: Frontier Visual Intelligence}},
    year={2025},
    howpublished={\url{https://bfl.ai/blog/flux-2}},
}
```

GitHub chính thức trở nên QUÁ ĐỈNH RỒI! 🤯 Giờ đây, bạn có thể xây dựng cả một NHÀ MÁY SẢN XUẤT NỘI DUNG AI mà không cần thuê bất kỳ nhân viên nào.


🔥 10 KHO MÃ NGUỒN (REPO) CỰC KỲ MẠNH MẼ MÀ BẠN KHÔNG NÊN BỎ QUA:


1. OpenCreator – KHÔNG GIAN SÁNG TẠO NỘI DUNG BẰNG AI
Cung cấp cho Codex một không gian làm việc sáng tạo, giúp bạn viết kịch bản, tạo hình ảnh, tạo giọng đọc AI, dịch video và chỉnh sửa nội dung.
🔗 https://github.com/krillinai/OpenCreator


2. Social Media Research Skills – BỘ CÔNG CỤ NGHIÊN CỨU MẠNG XÃ HỘI
Trang bị cho Claude Code và Codex các kỹ năng tìm kiếm bài đăng nổi bật, phân tích đối thủ cạnh tranh, khai thác bình luận và tìm câu mở đầu thu hút từ lời thoại video.
🔗 https://github.com/ScrapeCreators/social-media-research-skills


3. Local Deep Research – NGHIÊN CỨU CHUYÊN SÂU
Biến một chủ đề thành báo cáo nghiên cứu bằng cách tìm kiếm thông tin trên web và trong tài liệu cá nhân. Hỗ trợ các mô hình AI chạy cục bộ và trên đám mây.
🔗 https://github.com/LearningCircuit/local-deep-research


4. FLUX.2 – TẠO VÀ CHỈNH SỬA HÌNH ẢNH BẰNG AI
Tạo hình ảnh cho thumbnail YouTube, bài đăng mạng xã hội và chiến dịch quảng cáo. Repo cung cấp các mô hình tạo ảnh, trong đó có dòng Klein 4B với trọng số theo giấy phép Apache.
🔗 https://github.com/black-forest-labs/flux2


5. Kokoro – BIẾN VĂN BẢN THÀNH GIỌNG NÓI AI
Chuyển kịch bản thành giọng đọc AI bằng mô hình tổng hợp giọng nói có trọng số mở. Phù hợp để tìm hiểu cách tích hợp thuyết minh vào quy trình sản xuất video.
🔗 https://github.com/hexgrad/kokoro


6. ACE-Step 1.5 – SÁNG TẠO ÂM NHẠC BẰNG AI
Tạo nhạc từ mô tả văn bản, sản xuất nhạc nền không lời và tạo lại những đoạn âm thanh cụ thể. Có thể chạy cục bộ trên phần cứng tương thích.
🔗 https://github.com/ace-step/ACE-Step-1.5


7. Short Video Factory – NHÀ MÁY SẢN XUẤT VIDEO NGẮN
Tự động hóa nhiều công đoạn tạo video ngắn, từ viết nội dung, tạo giọng đọc đến chỉnh sửa video và thêm phụ đề.
🔗 https://github.com/YILS-LIN/short-video-factory


8. AI YouTube Shorts Generator – TỰ ĐỘNG TẠO YOUTUBE SHORTS
Chuyển video YouTube dài thành video Shorts dọc bằng cách tự động chọn đoạn nổi bật, nhận diện lời thoại và cắt khung hình.
🔗 https://github.com/Anil-matcha/AI-Youtube-Shorts-Generator


9. Podcastfy – TẠO PODCAST BẰNG AI
Biến tài liệu nguồn thành các cuộc hội thoại podcast do AI tạo ra. Có thể tùy chỉnh lời thoại và tạo nội dung âm thanh bằng nhiều ngôn ngữ.
🔗 https://github.com/souzatharsis/podcastfy


10. Social Media Agent – TRỢ LÝ AI HỖ TRỢ ĐĂNG BÀI
Chuyển một đường link thành bản nháp bài đăng trên X và LinkedIn. Hỗ trợ xem lại, chỉnh sửa và lên lịch đăng thông qua quy trình AI Agent.
🔗 https://github.com/langchain-ai/social-media-agent


🚀 ĐIỀU ĐÁNG CHÚ Ý: Những công cụ này có thể hỗ trợ nhiều công đoạn trong quy trình sáng tạo nội dung, từ nghiên cứu ý tưởng, viết kịch bản, tạo hình ảnh, giọng đọc, nhạc nền cho đến sản xuất video ngắn.
Bạn không nhất thiết phải sử dụng tất cả. Hãy lựa chọn công cụ phù hợp với nhu cầu, cấu hình máy tính và ngân sách của mình.


🔥 NẾU ĐƯỢC TỰ ĐỘNG HÓA MỘT KHÂU TRONG QUY TRÌNH LÀM NỘI DUNG, BẠN SẼ CHỌN KHÂU NÀO ĐẦU TIÊN?


💬 Bình luận bên dưới để cùng trao đổi nhé!


#ai #prompt #xuhuong
