# Use_Docker


# 🚀 Common Docker Commands + FastAPI Deployment Guide

## 1. System Information
```bash
docker --version         # Check Docker version
docker info              # Docker system info
docker help              # Docker command help
```

## 2. Image Commands
```bash
docker images                      # List downloaded images
docker pull <image>               # Pull image from Docker Hub
docker build -t <image_name> .    # Build image from Dockerfile
docker rmi <image>                # Remove image
docker tag <image> <new_name>     # Tag image with new name
```

## 3. Container Commands
```bash
docker ps                         # List running containers
docker ps -a                      # List all containers (including stopped ones)
docker run <image>               # Run container from image
docker run -it <image>           # Run container with interactive terminal
docker run -d <image>            # Run container in detached mode
docker run --name <name> -p 8000:8000 <image>   # Assign name and open port
docker start <container>         # Start container
docker stop <container>          # Stop container
docker restart <container>       # Restart container
docker rm <container>            # Remove container
docker logs <container>          # View container logs
docker exec -it <container> bash # Enter container terminal
```

## 4. Volume & File System
```bash
docker volume create <name>                  # Create volume
docker volume ls                             # List volumes
docker volume inspect <volume>               # Inspect volume
docker run -v <local_path>:<container_path> <image>  # Mount volume
```

## 5. Build Image from Dockerfile
```bash
docker build -t my_app .                     # Build from Dockerfile
docker run -p 3000:3000 my_app               # Run built image
```

## 6. Docker Compose
```bash
docker-compose up                            # Start all services
docker-compose up -d                         # Start in background
docker-compose down                          # Stop and remove services
docker-compose build                         # Rebuild images
docker-compose logs                          # View logs from services
```

## 7. System Cleanup
```bash
docker system prune                           # Remove unused containers/images/volumes
docker rm $(docker ps -aq)                    # Remove all containers
docker rmi $(docker images -q)                # Remove all images
```

## ✅ Deployment Guide for FastAPI + PyTorch Model

(Sections omitted for brevity in this preview)
- Project structure
- requirements.txt
- Dockerfile
- docker build / run / test
- Limiting CPU & RAM
- GPU Docker support
- Exporting image to `.tar`
- Docker Hub upload

...

(Full details are included in the file)


===========================================
🚀 CÁC LỆNH DOCKER THÔNG DỤNG
===========================================

1. THÔNG TIN HỆ THỐNG
---------------------
docker --version           # Kiểm tra phiên bản Docker
docker info                # Thông tin hệ thống Docker
docker help                # Trợ giúp các lệnh Docker

2. IMAGE COMMANDS
---------------------
docker images                      # Liệt kê các image đã tải
docker pull <image>               # Tải image từ Docker Hub
docker build -t <tên_image> .     # Build image từ Dockerfile
docker rmi <image>                # Xoá image
docker tag <image> <new_name>     # Gán tên mới cho image

3. CONTAINER COMMANDS
---------------------
docker ps                         # Liệt kê container đang chạy
docker ps -a                      # Liệt kê tất cả container (cả đã dừng)
docker run <image>               # Chạy container từ image
docker run -it <image>           # Chạy container interactive terminal
docker run -d <image>            # Chạy container nền (detached)
docker run --name <tên> -p 8000:8000 <image>     # Gán tên và mở cổng
docker start <container>         # Khởi động lại container
docker stop <container>          # Dừng container
docker restart <container>       # Khởi động lại container
docker rm <container>            # Xoá container
docker logs <container>          # Xem log container
docker exec -it <container> bash # Vào terminal container

4. VOLUME & FILE SYSTEM
---------------------
docker volume create <tên>                   # Tạo volume
docker volume ls                             # Liệt kê volumes
docker volume inspect <volume>              # Thông tin volume
docker run -v <local_path>:<container_path> <image>   # Mount volume

5. BUILT IMAGE TỪ DOCKERFILE
---------------------
docker build -t my_app .            # Build image từ Dockerfile
docker run -p 3000:3000 my_app      # Chạy image vừa build

6. DOCKER COMPOSE
---------------------
docker-compose up                  # Chạy toàn bộ services
docker-compose up -d              # Chạy ngầm
docker-compose down               # Dừng và xoá service
docker-compose build              # Build lại các image
docker-compose logs               # Xem log từ các service

7. DỌN DẸP HỆ THỐNG
---------------------
docker system prune               # Xoá container/image/volume không dùng
docker rm $(docker ps -aq)       # Xoá toàn bộ container
docker rmi $(docker images -q)   # Xoá toàn bộ image

===========================================




Rồi, mình sẽ hướng dẫn mày **từng bước để deploy API FastAPI + PyTorch model trên Docker** thật chi tiết, gồm các phần:

---

## 🔧 1. Cấu trúc thư mục cần có

Giả sử project của mày có cấu trúc như sau:

```
Malicious_Link_AI/
├── app.py                  # Code API của mày (vừa gửi ở trên)
├── requirements.txt        # Danh sách thư viện cần cài
├── Dockerfile              # File docker build image
└── Models/
    └── Results_13_Train_95_Val_94/
        └── trained_model/  # Chứa model đã fine-tuned
```

---

## 📦 2. Tạo `requirements.txt`

Tạo file `requirements.txt` để Docker biết cần cài gì:

```txt
fastapi
uvicorn
transformers
torch
tqdm
```

> ✅ Nếu mày còn dùng thêm `pydantic`, `scikit-learn`, `numpy`, thì thêm vào luôn.

---

## 🐳 3. Tạo `Dockerfile`

Tạo file `Dockerfile` để định nghĩa image:

```Dockerfile
# Base image Python
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Copy requirements and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy project files
COPY . .

# Expose API port
EXPOSE 8000

# Run the FastAPI app with uvicorn
CMD ["uvicorn", "app:app", "--host", "0.0.0.0", "--port", "8000"]
```

> 🔥 `app:app` nghĩa là `app.py` có biến `app = FastAPI()`

---

## 🛠 4. Build Docker Image

Tại thư mục chứa `Dockerfile`, mở terminal và chạy:

```bash
docker build -t malicious_link_api .
```

* `-t malicious_link_api`: gán tên image là `malicious_link_api`
* `.`: build từ thư mục hiện tại

---

## 🚀 5. Run Docker Container

Sau khi build xong, chạy container như sau:

```bash
docker run -d -p 8000:8000 --name malicious_link_container malicious_link_api
```

* `-p 8000:8000`: ánh xạ port host → container
* `-d`: chạy ngầm (detached)
* `--name`: đặt tên cho container

---

## ✅ 6. Kiểm tra API

Vào trình duyệt mở:

```
http://localhost:8000/docs
```

→ Giao diện Swagger UI sẽ hiển thị 🎉
Mày có thể test API `POST /predict` trực tiếp ở đây.

---

## 🧠 Giải thích thêm:

| Thành phần         | Ý nghĩa                                       |
| ------------------ | --------------------------------------------- |
| `Dockerfile`       | Hướng dẫn Docker build image cho app mày      |
| `requirements.txt` | Danh sách thư viện cần cài                    |
| `uvicorn`          | Web server dùng để chạy FastAPI               |
| `EXPOSE`           | Cho phép truy cập port từ bên ngoài container |
| `CMD`              | Lệnh mặc định khi container start             |

---

## 🧪 Debug tips:

* ❌ Nếu container không start được: dùng `docker logs <container_id>` để xem lỗi
* ✅ Dùng `docker ps` để kiểm tra container đang chạy
* 🛑 Dừng container: `docker stop malicious_link_container`
* 🗑 Xoá: `docker rm -f malicious_link_container`

---

## 🧩 Gợi ý nâng cao:

* Nếu model `.bin` rất lớn → dùng `COPY` riêng hoặc mount volume.
* Nếu cần dùng GPU trong container → cần thêm `--gpus all` và base image như `nvidia/cuda:12.2.0-cudnn8-runtime-ubuntu20.04`.

---

📦 Nếu mày muốn mình tạo đầy đủ 3 file (`requirements.txt`, `Dockerfile`, `tree structure`) thì tao gửi liền. Muốn deploy lên cloud (EC2/VPS) tao cũng hướng dẫn được.

> Mày xác nhận giúp là model path `./Models/Results_13_Train_95_Val_94/trained_model/` đúng nhé.




Tao hiểu rồi Happy 👌 — mày đang chạy app trong **Docker container** và muốn:

> ✅ Tạm thời **dừng container cũ** →
> ✅ Sau đó **chạy lại với RAM giới hạn để test** (ví dụ `4g`).

---

## ✅ 1. **Stop container đang chạy**

```bash
docker stop malicious_link_container
```

(Nó sẽ dừng container nhưng không xóa)

---

## ✅ 2. **Remove container cũ (vì `--memory` không thể thay đổi sau khi đã tạo)**

```bash
docker rm malicious_link_container
```

---

## ✅ 3. **Chạy lại với giới hạn RAM & CPU**

```bash
docker run -p 8000:8000 --cpus="2.0" --memory="4g" --name malicious_link_container malicious_link_api
```

> 🧠 `--memory="4g"` giới hạn container chỉ dùng tối đa 4GB RAM
> 🧠 `--cpus="2.0"` giới hạn 2 logic CPU core (có thể là 1 core vật lý nếu CPU hỗ trợ SMT)

---

## ✅ 4. **Kiểm tra container đã bị giới hạn chưa**

### Kiểm tra từ host:

```bash
docker stats
```

→ Mày sẽ thấy container `malicious_link_container` chạy và cột `MEM USAGE` không vượt quá `4.00GiB`.

---

## 🧠 Lưu ý:

* Nếu app vượt quá RAM → container sẽ bị kill với lỗi `OutOfMemory (OOMKilled)`
* Có thể log lỗi như sau:

  ```bash
  docker inspect --format='{{.State.OOMKilled}}' malicious_link_container
  ```

---

📌 Vậy là mày đã test giới hạn RAM rồi nha.
Nếu muốn thử với 2GB, 512MB, hoặc thêm `swap`, tao có thể gợi ý thêm flag như `--memory-swap`, `--oom-kill-disable`.

Muốn simulate máy yếu kiểu cực đoan luôn không (1 core + 512MB RAM)? 😈


Tuyệt lắm Happy 😎! Dưới đây là **giải thích chi tiết từng tham số CPU và memory** trong lệnh `docker run` mà mày dùng, để mày hiểu rõ và **tự điều chỉnh theo ý muốn** khi test các môi trường giả lập phần cứng yếu:

---

## 📦 Lệnh gốc mày chạy:

```bash
docker run -d \
  -p 8000:8000 \
  --cpus="2.0" \
  --memory="4g" \
  --name malicious_link_container \
  malicious_link_api
```

---

## 🧠 Giải thích từng phần:

### ✅ `-d`

* **Chạy container dưới nền (detached mode)**
* Terminal sẽ không bị chiếm, mày vẫn có thể dùng `docker logs` hoặc `docker exec` để vào sau.

---

### ✅ `-p 8000:8000`

* **Ánh xạ cổng từ máy host → container**

  * `host:container`
* Nghĩa là khi người dùng truy cập `http://localhost:8000`, Docker sẽ chuyển tiếp đến **port 8000 bên trong container** (nơi FastAPI đang chạy).

---

### ✅ `--cpus="2.0"`

* Giới hạn container **chỉ được dùng tối đa 2 logical CPUs**
* Nếu máy mày có 8 core, container chỉ được quyền dùng **2 trong số đó**

📌 Cụ thể:

| Giá trị `--cpus` | Giải thích                             |
| ---------------- | -------------------------------------- |
| `--cpus="0.5"`   | Giới hạn nửa CPU core (rất yếu)        |
| `--cpus="1"`     | Chỉ dùng được 1 core (giả lập máy yếu) |
| `--cpus="2"`     | Dùng 2 core (bình thường)              |
| `--cpus="4"`     | Dùng 4 core (khá mạnh)                 |

→ Thường test API nên bắt đầu từ `--cpus="0.5"` đến `2.0` để thấy hiệu năng.

---

### ✅ `--memory="4g"`

* **Giới hạn container chỉ được dùng 4 GB RAM**

📌 Một số ví dụ giá trị RAM:

| Giá trị           | Giải thích                                |
| ----------------- | ----------------------------------------- |
| `--memory="512m"` | Rất yếu, dễ crash nếu model lớn           |
| `--memory="1g"`   | Tối thiểu chạy app AI nhỏ                 |
| `--memory="2g"`   | Trung bình                                |
| `--memory="4g"`   | Tương đối thoải mái cho PyTorch model vừa |
| `--memory="8g"`   | Dành cho model lớn hơn hoặc batch lớn     |

> ✅ Nếu app dùng vượt quá RAM limit → container bị kill với lỗi `OOMKilled`.

---

### ✅ `--name malicious_link_container`

* Đặt tên container → để dễ quản lý sau này (`docker stop`, `logs`, v.v.)

---

### ✅ `malicious_link_api`

* Đây là tên **image đã build từ Dockerfile**
* Nó chứa app FastAPI + model + code của mày

---

## ✅ Ví dụ test giả lập máy yếu:

### 🔹 Giả lập máy cùi:

```bash
docker run -d -p 8000:8000 --cpus="0.5" --memory="512m" --name test_low_env malicious_link_api
```

→ Dễ gây crash nếu batch size to hoặc model lớn → dùng để **test giới hạn tối thiểu cần thiết**

---

### 🔹 Giả lập máy tầm trung:

```bash
docker run -d -p 8000:8000 --cpus="1" --memory="2g" --name test_mid_env malicious_link_api
```

---

### 🔹 Giả lập server thật:

```bash
docker run -d -p 8000:8000 --cpus="4" --memory="8g" --name test_prod_env malicious_link_api
```

---

## 🔍 Theo dõi hiệu năng:

Chạy:

```bash
docker stats
```

→ Mày sẽ thấy container nào đang ngốn bao nhiêu CPU, RAM, I/O…

---

Nếu mày muốn test thêm:

* `--memory-swap` (giả lập khi hết RAM)
* `--oom-kill-disable` (vô hiệu OOM kill để debug)
* `--cpuset-cpus="0,2"` (gán cụ thể core vật lý)

Tao có thể hướng dẫn sâu hơn. Muốn test batch size to + RAM limit để xem model crash thế nào cũng vui lắm đó 😈



Việc model **không chạy trên GPU** mặc dù mày có GPU là một vấn đề phổ biến khi dùng **Docker**. Dưới đây là **giải thích và hướng xử lý cụ thể** cho mày:

---

### 🧠 **Vì sao không dùng được GPU trong Docker?**

Lý do chính là:

#### ❌ **Docker image của mày chưa hỗ trợ GPU**

* Mày đang dùng base image:

  ```Dockerfile
  FROM python:3.10-slim
  ```

  → Đây là image nhẹ **không có driver CUDA, không hỗ trợ GPU**.

---

### ✅ **Giải pháp: Dùng Docker với GPU (NVIDIA)**

Để sử dụng GPU bên trong Docker, mày cần:

---

### 🔧 1. **Cài NVIDIA Container Toolkit**

> ⚠️ Bắt buộc để Docker "nhìn thấy" GPU

* Với Windows (WSL2):
  Chạy lệnh này trong PowerShell hoặc WSL:

  ```bash
  sudo apt update
  sudo apt install -y nvidia-container-toolkit
  sudo systemctl restart docker
  ```

* Kiểm tra GPU được Docker nhận:

  ```bash
  docker run --gpus all nvidia/cuda:12.2.0-base nvidia-smi
  ```

---

### 🧱 2. **Dùng Base Image Hỗ Trợ GPU**

Thay dòng này:

```dockerfile
FROM python:3.10-slim
```

Thành:

```dockerfile
FROM nvidia/cuda:12.2.0-runtime-ubuntu20.04
```

Rồi thêm vào:

```dockerfile
RUN apt-get update && apt-get install -y python3-pip
COPY requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt
```

---

### 🐳 3. **Chạy Container Với GPU**

Thay vì:

```bash
docker run -d ...
```

Sử dụng:

```bash
docker run --gpus all -d -p 8000:8000 --name malicious_link_container malicious_link_api
```

---

### 🧪 4. **Xác minh bên trong app Python**

In thử trong `app.py`:

```python
print("[DEBUG] Is CUDA available?", torch.cuda.is_available())
print("[DEBUG] Device name:", torch.cuda.get_device_name(0) if torch.cuda.is_available() else "No GPU")
```

---

### ✅ Tóm tắt việc cần làm

| Việc cần làm                     | Mô tả                                |
| -------------------------------- | ------------------------------------ |
| ✅ Cài NVIDIA Toolkit             | Giúp Docker có thể gọi được GPU      |
| ✅ Dùng base image `nvidia/cuda`  | Hỗ trợ CUDA runtime                  |
| ✅ Gắn flag `--gpus all` khi chạy | Đảm bảo Docker gán GPU cho container |
| ✅ Xác minh bên trong code Python | Dùng `torch.cuda.is_available()`     |

---

Nếu mày cần **Dockerfile hoàn chỉnh hỗ trợ GPU**, tao có thể viết luôn. Chỉ cần nói:
👉 “Viết Dockerfile hỗ trợ GPU + Python cho tôi”



Đúng rồi Happy! Sau khi mày đã **đóng gói model AI trong Docker**, thì khi gửi sang máy khác (ví dụ máy chủ của bạn mày), **chỉ cần gửi Docker image** là đủ — không cần gửi source code, model file, hay môi trường nữa.

Tóm gọn lại các cách gửi:

---

### ✅ **Cách 1: Xuất image và gửi file `.tar`**

1. **Xuất image**:

   ```bash
   docker save -o my_model_image.tar my_model_image:latest
   ```
2. **Gửi file `my_model_image.tar`** qua USB, Google Drive, scp, v.v.
3. **Trên máy bạn mày, import image**:

   ```bash
   docker load -i my_model_image.tar
   ```
4. **Chạy container**:

   ```bash
   docker run -p 8000:8000 my_model_image:latest
   ```

---

### ✅ **Cách 2: Đẩy image lên Docker Hub hoặc registry riêng**

1. **Tag image**:

   ```bash
   docker tag my_model_image yourdockerhubusername/my_model_image:latest
   ```
2. **Push lên Docker Hub**:

   ```bash
   docker push yourdockerhubusername/my_model_image:latest
   ```
3. **Trên máy chủ**, bạn mày chỉ cần:

   ```bash
   docker pull yourdockerhubusername/my_model_image:latest
   docker run -p 8000:8000 yourdockerhubusername/my_model_image:latest
   ```

---

### ✅ Ưu điểm khi dùng Docker image:

* Không cần cài lại môi trường Python, dependency, CUDA, Torch,...
* Tái sử dụng, dễ triển khai, chạy được ngay nếu máy chủ có Docker.

---

👉 Nếu mày muốn tao gợi ý Dockerfile mẫu cho model AI (FastAPI, PyTorch, HuggingFace chẳng hạn), chỉ cần nói là tao làm cho luôn nha!
