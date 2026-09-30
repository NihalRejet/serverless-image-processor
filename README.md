# Serverless Event-Driven Image Processing Pipeline

An automated, serverless infrastructure deployed on AWS using Terraform. This project demonstrates event-driven architecture by automatically processing, compressing, and converting image uploads into multiple optimized web formats in real-time.

## 🏗️ Architecture
*(Add your architecture.png diagram here)*

## 🚀 Features
* **Event-Driven Execution:** Configured S3 event notifications to trigger AWS Lambda instantly upon file upload.
* **Automated Media Optimization:** Utilizes the Python `Pillow` library to automatically generate 5 distinct variants of the original image:
  * WebP format
  * PNG format
  * High-compression JPEG
  * Low-compression JPEG
  * 200x200 Thumbnail
* **Infrastructure as Code (IaC):** 100% of the AWS infrastructure (Buckets, Lambda, IAM, CloudWatch) is provisioned, managed, and version-controlled using **Terraform**.
* **Continuous Integration (CI):** Integrated GitHub Actions workflow to automatically run `terraform fmt` and `terraform validate` on every push to the main branch.
* **Least-Privilege Security & Encryption:** Implemented strict IAM roles scoping Lambda access only to the required source and destination buckets, and enforced **AES-256 Server-Side Encryption (SSE-S3)** on all stored objects.

## 🛠️ Tech Stack
* **Cloud Provider:** Amazon Web Services (AWS)
* **Compute:** AWS Lambda (Python 3.12)
* **Storage:** Amazon S3
* **Observability:** Amazon CloudWatch
* **Infrastructure as Code:** Terraform
* **CI/CD:** GitHub Actions

## 🧠 Technical Challenges Overcome

**C-Extension Compatibility in Serverless Runtimes:**
During deployment, the Lambda function initially failed because the local Python `Pillow` library contained C-extensions compiled for a different architecture than Lambda's Amazon Linux runtime. I resolved this by bypassing Docker and leveraging WSL to strictly target pre-compiled `manylinux2014_x86_64` Python 3.12 binaries via `pip`. I then packaged this as an independent **AWS Lambda Layer**, allowing the core function code to remain lightweight while ensuring perfect runtime compatibility.

<img width="2880" height="1800" alt="Screenshot 2026-09-30 054433" src="https://github.com/user-attachments/assets/49520bfd-6fd8-4ac7-8bb9-0ac1ed37934f" />


**1. S3 Destination Bucket Output**
The Lambda function successfully catches the S3 upload event and generates the 5 formatted variants in the destination bucket.
![S3 Output](Screenshot 2026-09-30 054433.jpg)

<img width="2880" height="1800" alt="Screenshot 2026-09-30 054715" src="https://github.com/user-attachments/assets/5f3a471a-6f98-45e2-aa82-9b88e216089c" />


**2. CloudWatch Execution Logs**
System logs confirming the Lambda invocation, the generation of all variants, and the successful upload of the processed images in ~2175 ms.
![CloudWatch Logs](Screenshot 2026-09-30 054715.jpg)

## 💻 Local Setup & Deployment

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/](https://github.com/)<YourUsername>/serverless-image-processor.git
   cd serverless-image-processor
