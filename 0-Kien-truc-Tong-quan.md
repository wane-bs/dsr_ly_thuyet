---
layout: default
title: Kiến trúc Tổng quan
---

# Kiến trúc Tổng quan - Bộ Khái niệm VinaLib
## Hướng dẫn Đọc và Mối quan hệ giữa các Khái niệm

> [!NOTE]
> **Bạn mới bắt đầu?** Vui lòng đọc [START_HERE_Ly_thuyet_nen_tang.md](./START_HERE_Ly_thuyet_nen_tang.md) trước để hiểu các khái niệm cơ bản bằng ngôn ngữ tự nhiên.
>
> **File này dành cho:** Technical deep-dive vào kiến trúc VinaLib, sơ đồ kỹ thuật, glossary đầy đủ, và roadmap implementation.

---

## 🔬 DSR Context (Design Science Research)

> [!IMPORTANT]
> **Vị trí trong DSR Methodology:** Tài liệu này là phần **Design & Development** (Thiết kế và Phát triển) trong quy trình DSR.
>
> - Để hiểu **vấn đề** được giải quyết → xem [Problem Statement](./Problem_Statement.md)
> - Để hiểu **mục tiêu** của giải pháp → xem [Solution Objectives](./Solution_Objectives.md)  
> - Để hiểu **phương pháp luận** DSR → xem [DSR Framework](./DSR_Framework.md)
> - Để xem **kết quả đánh giá** → xem [Evaluation Results](./Evaluation_Results.md)

**Mapping từ Vấn đề đến Thiết kế:**

```
Problem Domain              →  Solution Component (Tài liệu này)
────────────────────────────────────────────────────────────────
Tranh chấp tài sản          →  BookAsset NFT (ERC-721/4907)
Xử lý thủ công              →  Smart Contracts (Automation)
Quản lý dòng đời opacity    →  Blockchain Events + IPFS
Escrow security risks       →  Smart Contract Escrow
Trust deficits              →  PolicyEngine + Chainlink
```

---

## 📚 Về Bộ Tài liệu này

Bộ tài liệu **"Bộ Khái niệm"** giải thích các công nghệ nền tảng mà hệ thống **VinaLib** (nền tảng cho thuê sách phi tập trung) sử dụng. Mỗi tài liệu tập trung vào một lĩnh vực công nghệ cụ thể, đồng thời phân tích cách nó được áp dụng trong project VinaLib.

---

## 🗂️ Danh sách Tài liệu

| # | Tài liệu | Chủ đề chính | Độ khó |
|---|----------|--------------|--------|
| 0 | **Kiến trúc Tổng quan** (file này) | Overview, Glossary, Roadmap | ⭐ |
| 1 | [IPFS](./1-IPFS.md) | Lưu trữ phân tán, CID, Gateway | ⭐⭐ |
| 2 | [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md) | Hợp đồng thông minh, EVM, Gas, Workflow | ⭐⭐⭐ |
| 3 | [Smart Contracts (Nâng cao)](./3-Smart-Contracts-Nang-cao.md) | Patterns, Standards, NFT, SBT | ⭐⭐⭐⭐ |
| 4 | [Chainlink](./4-Chainlink.md) | Oracle Network, VRF, Automation, Functions | ⭐⭐⭐⭐ |
| 5 | [IoT](./5-IoT.md) | Smart Lock, Tuya, Physical Access | ⭐⭐⭐ |

---

## 🔗 Sơ đồ Kiến trúc Tổng quan

```
                     ┌───────────────────┐
                     │   👤 Người dùng   │
                     │  Web/Mobile App   │
                     └─────────▲─────────┘
                               │ Web3
                               ▼
┌─────────────────────────────────────────────────────────┐
│                ⛓️ BLOCKCHAIN LAYER                      │
│                                                         │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────┐  │
│  │ BookAsset    │    │ BookRental   │    │ Policy   │  │
│  │ (ERC-721)    │    │ (Logic)      │    │ Engine   │  │
│  └──────┬───────┘    └──────┬───────┘    └────┬─────┘  │
│         │                   │                 │        │
│         └────────┬──────────┴─────────────────┘        │
│                  │ VinaLibVault                        │
└──────────────────┼──────────────────────────────────────┘
                   │
         ┌─────────┼──────────────────────┐
         │         │ Request/Response     │ 
         ▼         ▼                      ▼
┌──────────────┐ ┌──────────────┐ ┌──────────────┐
│ 🔮 ORACLE    │ │ 💾 STORAGE   │ │ 🔐 PHYSICAL  │
│ (Chainlink)  │ │ (IPFS)       │ │ (IoT)        │
│              │ │              │ │              │
│ - Data Feeds │ │ - Metadata   │ │ - Smart Lock │
│ - Automation │ │ - Book Covers│ │ - Logs       │
│ - Functions  │ │              │ │              │
└──────────────┘ └──────────────┘ └──────────────┘
```

---

## 📖 Thứ tự Đọc Khuyến nghị

### Lộ trình cho Người mới (Beginner Path)
```
1-IPFS.md          → Hiểu cách lưu trữ dữ liệu
       ↓
2-Smart-Contracts-  → Hiểu logic business on-chain
   Co-ban.md
       ↓
5-IoT.md           → Hiểu tương tác vật lý
```

### Lộ trình Đầy đủ (Full Path)
```
1-IPFS.md                    → Storage foundation
       ↓
2. Smart Contracts (Cơ bản)   → Core logic
       ↓
3. Smart Contracts (Nâng cao) → Patterns & Standards
       ↓
4-Chainlink.md               → Oracle integration
       ↓
5-IoT.md                     → Physical world bridge
```

### Lộ trình theo Vai trò

| Vai trò | Tài liệu ưu tiên |
|---------|------------------|
| **Frontend Dev** | 1 → 2 → 5 |
| **Smart Contract Dev** | 2 → 3 → 4 |
| **Full-stack Dev** | 1 → 2 → 3 → 4 → 5 |
| **Product Manager** | 0 (file này) → Đọc phần "Góc nhìn Người dùng" trong các file khác |

---

## 🔄 Luồng Dữ liệu End-to-End

### Quy trình Thuê Sách

```
User (👤)        Smart Contract (⛓️)      Chainlink (🔮)        IPFS (💾)          IoT (🔐)
   │                    │                    │                    │                    │
   │ 1. Request Rental  │                    │                    │                    │
   │───────────────────>│                    │                    │                    │
   │                    │ 2. Policy Check    │                    │                    │
   │                    │ (Trust/Tier)       │                    │                    │
   │                    │                    │                    │                    │
   │                    │ 3. Verify Request  │                    │                    │
   │                    │───────────────────>│                    │                    │
   │                    │                    │                    │                    │
   │                    │ 4. Verify Result   │                    │                    │
   │                    │<───────────────────│                    │                    │
   │                    │                    │                    │                    │
   │                    │ 5. Approve/Reject  │                    │                    │
   │                    │                    │                    │                    │
   │                    │ 6. Get Metadata    │                    │                    │
   │                    │────────────────────────────────────────>│                    │
   │                    │                    │                    │                    │
   │                    │ 7. Return Data     │                    │                    │
   │                    │<────────────────────────────────────────│                    │
   │                    │                    │                    │                    │
   │                    │ 8. Emit Event      │                    │                    │
   │                    │─────────────────────────────────────────────────────────────>│
   │                    │                    │                    │   9. Trigger Lock  │
   │                    │                    │                    │                    │
   │ 10. Receive Code   │                    │                    │                    │
   │<──────────────────────────────────────────────────────────────────────────────────│
   │                    │                    │                    │                    │
   │ 11. Enter Code     │                    │                    │                    │
   │──────────────────────────────────────────────────────────────────────────────────>│
   │                    │                    │                    │                    │
   │                    │ 12. Log Access     │                    │                    │
   │<────────────────────────────────────────┼─────────────────────────────────────────│
   │                    │                    │                    │                    │
```

---

## 📊 Ma trận Liên kết Giữa các Khái niệm

| Khái niệm | IPFS | Smart Contracts | Chainlink | IoT |
|-----------|------|-----------------|-----------|-----|
| **IPFS** | - | Lưu CID trong contract | - | - |
| **Smart Contracts** | tokenURI() trỏ đến IPFS | - | Gọi oracle để verify | Emit events trigger IoT |
| **Chainlink** | Functions có thể upload IPFS | Callback fulfillRequest() | - | Functions lấy IoT data |
| **IoT** | - | Nhận events từ contract | Logs được hash on-chain | - |

---

## 📝 Bảng Thuật ngữ (Glossary)

### Thuật ngữ Chung

| Thuật ngữ | Định nghĩa | Tài liệu liên quan |
|-----------|------------|-------------------|
| **Blockchain** | Sổ cái phân tán, bất biến, lưu trữ các giao dịch theo blocks | [2. Smart Contracts](./2-Smart-Contracts-Co-ban.md) |
| **Decentralized** | Phi tập trung - không có đơn vị trung tâm kiểm soát | Tất cả |
| **On-chain** | Dữ liệu/logic được lưu/thực thi trực tiếp trên blockchain | [2. Smart Contracts](./2-Smart-Contracts-Co-ban.md) |
| **Off-chain** | Dữ liệu/logic nằm ngoài blockchain (servers, APIs, IPFS) | [4. Chainlink](./4-Chainlink.md) |
| **Gas** | Đơn vị đo lường chi phí tính toán trên blockchain | [2. Smart Contracts](./2-Smart-Contracts-Co-ban.md) |
| **Wallet** | Ví điện tử chứa private key để ký giao dịch (MetaMask, Coinbase) | [2. Smart Contracts](./2-Smart-Contracts-Co-ban.md) |

### Thuật ngữ IPFS

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **CID** | Content Identifier - "vân tay" duy nhất của nội dung dựa trên hash |
| **CIDv0** | Phiên bản cũ, bắt đầu bằng `Qm...`, chỉ hỗ trợ SHA-256 |
| **CIDv1** | Phiên bản mới, bắt đầu bằng `b...`, tự mô tả (self-describing) |
| **Gateway** | Cổng trung gian cho phép truy cập IPFS qua HTTP thông thường |
| **Pinning** | Giữ nội dung trên IPFS node để không bị garbage collected |
| **DHT** | Distributed Hash Table - bảng băm phân tán để tìm peers |

### Thuật ngữ Smart Contracts

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **EVM** | Ethereum Virtual Machine - máy ảo thực thi bytecode contracts |
| **Solidity** | Ngôn ngữ lập trình phổ biến nhất để viết smart contracts |
| **ABI** | Application Binary Interface - định nghĩa cách gọi functions |
| **Event** | Log được emit từ contract để frontend/backend lắng nghe |
| **Modifier** | Decorator thêm điều kiện trước khi function thực thi |

### Thuật ngữ ERC Standards

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **ERC-20** | Chuẩn token fungible (có thể thay thế lẫn nhau) như USDT, LINK |
| **ERC-721** | Chuẩn NFT - token không thể thay thế, mỗi token là duy nhất |
| **ERC-4907** | Mở rộng ERC-721, cho phép tách quyền sở hữu và quyền sử dụng (rentable NFT) |
| **SBT** | Soulbound Token - NFT không thể chuyển nhượng, dùng làm chứng chỉ |

### Thuật ngữ Chainlink

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **Oracle** | Cầu nối đưa dữ liệu off-chain lên blockchain một cách đáng tin cậy |
| **DON** | Decentralized Oracle Network - mạng lưới oracles độc lập |
| **Data Feeds** | Nguồn dữ liệu pre-aggregated (giá tiền, thời tiết...) |
| **VRF** | Verifiable Random Function - số ngẫu nhiên có thể chứng minh |
| **Automation** | Keepers tự động trigger smart contract functions |
| **Functions** | Thực thi JavaScript code off-chain, trả kết quả về contract |
| **OCR** | Off-Chain Reporting - gom nhiều oracle responses thành 1 transaction |

### Thuật ngữ IoT

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **IoT** | Internet of Things - mạng lưới thiết bị vật lý kết nối internet |
| **MQTT** | Message Queuing Telemetry Transport - protocol nhẹ cho IoT |
| **Smart Lock** | Khóa điện tử có thể điều khiển từ xa qua app/API |
| **Tuya** | Platform IoT cho manufacturers, cung cấp cloud + SDK |
| **Edge Computing** | Xử lý dữ liệu tại thiết bị (local) thay vì gửi lên cloud |

### Thuật ngữ VinaLib-specific

| Thuật ngữ | Định nghĩa |
|-----------|------------|
| **BookAsset** | Smart contract ERC-721 + ERC-4907 đại diện cho sách vật lý |
| **BookRental** | Smart contract quản lý logic thuê sách, escrow deposits |
| **PolicyEngine** | Smart contract tự động đánh giá và approve/reject rentals |
| **VinaLibVault** | Smart contract tích hợp Chainlink Functions + Automation |
| **RentalAgreementSBT** | SBT chứng nhận user đã ký hợp đồng thuê |
| **SuChinToken** | ERC-20 token dùng để thanh toán trong hệ thống |
| **Trust Score** | Điểm tín nhiệm của user, ảnh hưởng đến khả năng auto-approve |
| **Book Tier** | Phân loại sách (A=quý hiếm, B=thông thường, C=phổ biến) |

---

## 🏗️ Kiến trúc Smart Contracts

```
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│  SuChinToken │       │  BookRental  │       │ BookAsset    │
│  (ERC-20)    │       │  (Logic Core)│       │ (ERC-721)    │
└──────┬───────┘       └──────┬───────┘       └──────┬───────┘
       │ Payment              │ Queries              │
       └─────────────────────>│<─────────────────────┘
                              │
               Requests       │ Mints/Burns
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ▼                   ▼                   ▼
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│ PolicyEngine │       │ RentalAgrmt  │       │ VinaLibVault │
│ (Auto-Approve│       │ SBT          │       │ (Chainlink)  │
└──────────────┘       └──────────────┘       └──────┬───────┘
                                                     │
                                                     │ Triggers
                                                     ▲
                                                     │
                                              (Automation)
```

---

## 🚀 Roadmap Tích hợp

| Phase | Công nghệ | Status | Mô tả |
|-------|-----------|--------|-------|
| **Phase 1** | IPFS Simulator | ✅ Done | Local storage giả lập IPFS cho dev |
| **Phase 1** | Smart Contracts | ✅ Done | BookAsset, BookRental, PolicyEngine trên Hardhat |
| **Phase 1** | IoT Mock | ✅ Done | Mock server giả lập Tuya API |
| **Phase 2** | Chainlink Functions | 🔄 In Progress | Integration cơ bản trong VinaLibVault |
| **Phase 2** | Chainlink Automation | 🔄 In Progress | Placeholder checkUpkeep/performUpkeep |
| **Phase 3** | Real IPFS | 📋 Planned | Migrate sang Pinata/Web3.Storage |
| **Phase 3** | Real Tuya | 📋 Planned | Connect với Tuya Cloud API thật |
| **Phase 4** | Testnet Deploy | 📋 Planned | AVAX Fuji Testnet hoặc ndachain testnet |
| **Phase 5** | Mainnet Deploy | 📋 Planned | AVAX Subnet hoặc ndachain với PoA consensus |

---

## 📎 Tài liệu Liên quan

- [CHÚ_THÍCH_SƠ_ĐỒ.md](../CHÚ_THÍCH_SƠ_ĐỒ.md) - Giải thích chi tiết các sơ đồ kiến trúc
- [DIAGRAMS.md](../DIAGRAMS.md) - Tổng hợp tất cả Mermaid diagrams
- [README.md](../README.md) - Tổng quan dự án VinaLib

---

*Cập nhật lần cuối: 2026-01-31*
