---
layout: default
title: Trang chủ
---

# 📘 Bộ Tài liệu Khái niệm VinaLib - Web3 & Blockchain

Chào mừng đến với kho tri thức nền tảng của dự án **VinaLib** - Nền tảng cho thuê sách phi tập trung.

Bộ tài liệu này được biên soạn để cung cấp kiến thức từ cơ bản đến nâng cao về các công nghệ cốt lõi: Blockchain, Smart Contracts, IPFS, Chainlink và IoT.

> [!NOTE]
> **Deployment:** VinaLib là **DApp trên NDAChain** (hạ tầng DID quốc gia) để tối ưu hóa chi phí (< $0.01/tx), tốc độ (< 2s finality). Hiện test trên AVAX Fuji (temporary). Xem [VinaLib Deployment Strategy](./VinaLib-Deployment-Strategy.md).

---

## 🔬 DSR Framework (Design Science Research)

Dự án VinaLib được phát triển theo **phương pháp luận khoa học thiết kế (DSR)**, đảm bảo tính nghiêm ngặt trong nghiên cứu và phát triển giải pháp:

| Tài liệu DSR | Nội dung | Dành cho |
|--------------|----------|----------|
| **[DSR Framework](./DSR_Framework.md)** | Tổng quan về methodology, quy trình nghiên cứu | Researchers, Academic reviewers |
| **[Problem Statement](./Problem_Statement.md)** | Xác định vấn đề: Tranh chấp tài sản, xử lý thủ công, quản lý dòng đời | Product Managers, Stakeholders |
| **[Solution Objectives](./Solution_Objectives.md)** | Mục tiêu cụ thể của DApp với metrics đo lường | All roles |
| **[Evaluation Results](./Evaluation_Results.md)** | Kết quả đánh giá, KPIs, test plans | Researchers, QA teams |

> [!NOTE]
> **Dành cho Researchers & Stakeholders:** Nếu bạn muốn hiểu **tại sao** và **như thế nào** VinaLib giải quyết vấn đề thực tế, hãy bắt đầu từ **DSR Framework** ở trên trước khi đi vào chi tiết kỹ thuật.

---

## 🚀 Bắt đầu từ đâu?

Nếu đây là lần đầu tiên bạn tiếp cận, hãy bắt đầu theo lộ trình sau:

1.  **[START_HERE: Lý thuyết Nền tảng](./START_HERE_Ly_thuyet_nen_tang.md)** 🌟
    *   *Dành cho:* Tất cả mọi người (Newbies, PM, Dev).
    *   *Nội dung:* Giải thích toàn bộ khái niệm bằng ngôn ngữ tự nhiên, ví dụ đời thường, không code.

2.  **[0. Kiến trúc Tổng quan](./0-Kien-truc-Tong-quan.md)**
    *   *Dành cho:* Dev, Architect.
    *   *Nội dung:* Sơ đồ hệ thống, luồng dữ liệu, thuật ngữ kỹ thuật.

3.  **[Lưu đồ trực quan hóa](https://wane-bs.github.io/so-do/)**
    *   *Nội dung:* Sơ đồ trực quan hóa cấu trúc và luồng dữ liệu.

---

## 📚 Tài liệu Chuyên sâu (Technical Deep-Dive)

Sau khi đã nắm vững nền tảng, hãy đi sâu vào từng module:

| # | Module | Nội dung chính |
|---|--------|----------------|
| **1** | **[Hệ thống Lưu trữ (IPFS)](./1-IPFS.md)** | Content Addressing, CID, Pinning, Cách VinaLib lưu ảnh bìa sách. |
| **2** | **[Smart Contracts - Cơ bản](./2-Smart-Contracts-Co-ban.md)** | Solidity, EVM, Gas, Workflow chính của BookAsset & BookRental. |
| **3** | **[Smart Contracts - Nâng cao](./3-Smart-Contracts-Nang-cao.md)** | NDAChain với PoA, Design Patterns (Ownable), Token Standards (ERC-721, SBT). |
| **4** | **[Chainlink Oracle](./4-Chainlink.md)** | Kết nối dữ liệu off-chain, VRF (Randomness), Automation, Functions. |
| **5** | **[IoT & Thực tế](./5-IoT.md)** | Kết nối khóa thông minh, MQTT, Bảo mật thiết bị vật lý. |

---

## 🏗️  Chiến lược Triển khai

| Tài liệu | Nội dung | Dành cho |
|----------|----------|----------|
| **[VinaLib Deployment Strategy](./VinaLib-Deployment-Strategy.md)** 🆕 | Roadmap to NDAChain: PoA consensus, AVAX temporary testnet, NDAChain target platform. | Developers, DevOps, Architects |

---

## 🤝  Đóng góp

- Tài liệu được viết bằng Markdown.
- Mọi đóng góp xin vui lòng tạo Pull Request hoặc Issue trên GitHub.
- Cập nhật lần cuối: **02/2026**
