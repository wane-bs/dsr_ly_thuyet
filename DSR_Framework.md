---
layout: default
title: DSR Framework
---

# Khung Phương Pháp Luận Design Science Research cho VinaLib

## Tóm tắt

Tài liệu này trình bày việc áp dụng phương pháp luận Design Science Research (DSR) vào quá trình phát triển và đánh giá VinaLib, một ứng dụng phi tập trung dựa trên blockchain cho quản lý vòng đời tài sản số. Khung DSR cung cấp phương pháp khoa học nghiêm ngặt để hiểu miền vấn đề, xác định mục tiêu giải pháp, thiết kế và triển khai artifact, và đánh giá có hệ thống hiệu quả của nó.

---

## 1. Giới thiệu về Design Science Research

### 1.1 Định nghĩa và Phạm vi

Design Science Research (DSR) là một mô hình nghiên cứu nhằm mở rộng ranh giới khả năng của con người và tổ chức bằng cách tạo ra các artifact mới và sáng tạo (Hevner et al., 2004). Khác với khoa học hành vi (behavioral science) tìm cách phát triển và xác minh các lý thuyết giải thích hoặc dự đoán các hiện tượng tổ chức và con người, design science tìm cách tạo ra những đổi mới định nghĩa các ý tưởng, thực tiễn, khả năng kỹ thuật và sản phẩm thông qua đó việc phân tích, thiết kế, triển khai và sử dụng hệ thống thông tin có thể được thực hiện một cách hiệu quả và hiệu suất (Denning, 1997; Tsichritzis, 1998).

### 1.2 Các Thành phần Khung DSR

Phương pháp luận DSR bao gồm sáu hoạt động thiết yếu (Peffers et al., 2007):

1. **Xác định Vấn đề và Động lực**: Định nghĩa vấn đề nghiên cứu cụ thể và biện minh giá trị của giải pháp
2. **Xác định Mục tiêu của Giải pháp**: Suy luận các mục tiêu giải pháp từ đặc tả vấn đề
3. **Thiết kế và Phát triển**: Tạo ra artifact (construct, model, method, hoặc instantiation)
4. **Minh chứng**: Chứng minh việc sử dụng artifact để giải quyết các trường hợp của vấn đề
5. **Đánh giá**: Quan sát và đo lường mức độ artifact hỗ trợ giải pháp cho vấn đề
6. **Truyền thông**: Truyền đạt vấn đề, artifact, tính hữu dụng và tính mới đến các nhà nghiên cứu và thực hành

### 1.3 Tính Liên quan đến VinaLib

VinaLib đại diện cho một artifact design science—cụ thể là một **instantiation** (thể hiện) của công nghệ blockchain, smart contracts, lưu trữ phi tập trung (IPFS), mạng oracle (Chainlink), và tích hợp IoT để giải quyết các khoảng trống đã xác định trong hệ thống quản lý tài sản truyền thống.

---

## 2. Áp dụng DSR cho VinaLib

### 2.1 Bối cảnh Nghiên cứu

**Miền**: Quản lý Tài sản Số, Ứng dụng Blockchain, Hệ thống Phi tập trung

**Miền Vấn đề**: Các hệ thống quản lý tài sản truyền thống (cụ thể là cho thuê sách thư viện, nhưng có thể tổng quát hóa cho các loại tài sản rộng hơn) gặp phải những hạn chế cơ bản về tính minh bạch, tự động hóa, thiết lập lòng tin và khả năng truy vết vòng đời.

**Loại Artifact**: Instantiation phần mềm (DApp) + Kiến trúc kỹ thuật

### 2.2 Quy trình DSR trong VinaLib

```
┌─────────────────────────────────────────────────────────────┐
│                  QUY TRÌNH PHƯƠNG PHÁP LUẬN DSR             │
└─────────────────────────────────────────────────────────────┘

1. XÁC ĐỊNH VẤN ĐỀ
   ├─ Thách thức giải quyết tranh chấp tài sản
   ├─ Sự kém hiệu quả của xử lý thủ công
   └─ Tính mờ đục trong quản lý vòng đời tài sản
          │
          ▼
2. MỤC TIÊU GIẢI PHÁP
   ├─ Tính minh bạch quyền sở hữu qua blockchain
   ├─ Tự động hóa quy trình qua smart contracts
   ├─ Audit trail bất biến
   └─ Giảm chi phí và rủi ro
          │
          ▼
3. THIẾT KẾ & PHÁT TRIỂN
   ├─ Tầng Blockchain (Smart Contracts)
   ├─ Tầng Lưu trữ (IPFS)
   ├─ Tầng Oracle (Chainlink)
   └─ Tích hợp Vật lý (IoT)
          │
          ▼
4. MINH CHỨNG
   ├─ Triển khai nguyên mẫu (Hardhat local)
   ├─ Triển khai testnet (Sepolia, Mumbai)
   └─ Các kịch bản use case
          │
          ▼
5. ĐÁNH GIÁ
   ├─ Các chỉ số định lượng (TPS, chi phí, độ trễ)
   ├─ Đánh giá định tính (khả năng sử dụng, lòng tin)
   └─ Phân tích so sánh (truyền thống vs DApp)
          │
          ▼
6. TRUYỀN THÔNG
   ├─ Tài liệu kỹ thuật (repository này)
   ├─ Xuất bản học thuật (đã lên kế hoạch)
   └─ Mã nguồn mở
```

### 2.3 Mô tả Artifact

**Tên Artifact**: Hệ thống Quản lý Tài sản Phi tập trung VinaLib

**Các Thành phần Artifact**:
- **Smart Contracts**: BookAsset (ERC-721/ERC-4907), BookRental, PolicyEngine, VinaLibVault, RentalAgreementSBT
- **Cơ sở hạ tầng Lưu trữ**: IPFS cho metadata và media
- **Tích hợp Oracle**: Chainlink Functions, Automation, VRF
- **Giao diện Vật lý**: Khóa thông minh IoT và cảm biến
- **Nền kinh tế Token**: SuChinToken (ERC-20) cho thanh toán

**Stack Công nghệ**:
- Blockchain: AVAX Subnet hoặc ndachain với PoA consensus
- Ngôn ngữ Smart Contract: Solidity
- Framework Phát triển: Hardhat
- Frontend: React/Web3.js
- Lưu trữ: IPFS (Pinata/Web3.Storage)
- Oracle: Chainlink DON

---

## 3. Cấu trúc Tài liệu

### 3.1 Tài liệu Căn chỉnh theo DSR

| Tài liệu | Giai đoạn DSR | Mục đích |
|----------|---------------|----------|
| [Đặc tả Vấn đề](./Problem_Statement.md) | 1. Xác định Vấn đề | Trình bày các thách thức trong quản lý tài sản truyền thống |
| [Mục tiêu Giải pháp](./Solution_Objectives.md) | 2. Định nghĩa Mục tiêu | Xác định các mục tiêu đo lường được cho DApp |
| [Kiến trúc Tổng quan](./0.%20Kiến%20trúc%20Tổng%20quan.md) | 3. Thiết kế & Phát triển | Kiến trúc hệ thống và thiết kế kỹ thuật |
| [IPFS](./1.%20IPFS.md) | 3. Thiết kế & Phát triển | Thiết kế tầng lưu trữ |
| [Smart Contracts (Cơ bản)](./2.%20Smart%20Contracts%20(Cơ%20bản).md) | 3. Thiết kế & Phát triển | Logic contract cốt lõi |
| [Smart Contracts (Nâng cao)](./3.%20Smart%20Contracts%20(Nâng%20cao).md) | 3. Thiết kế & Phát triển | Các mẫu và tiêu chuẩn nâng cao |
| [Chainlink](./4.%20Chainlink.md) | 3. Thiết kế & Phát triển | Thiết kế tích hợp oracle |
| [IoT](./5.%20IoT.md) | 3. Thiết kế & Phát triển | Tích hợp thế giới vật lý |
| [Kết quả Đánh giá](./Evaluation_Results.md) | 5. Đánh giá | Đánh giá và xác thực thực nghiệm |

### 3.2 Lộ trình Đọc theo Vai trò

#### Cho Nhà nghiên cứu / Người đánh giá Học thuật
```
1. DSR_Framework.md (tài liệu này) → 
2. Problem_Statement.md → 
3. Solution_Objectives.md → 
4. 0-Kien-truc-Tong-quan.md → 
5. Evaluation_Results.md
```

#### Cho Quản lý Sản phẩm / Các bên Liên quan
```
1. Problem_Statement.md → 
2. Solution_Objectives.md → 
3. START_HERE_Ly_thuyet_nen_tang.md (giải thích bằng ngôn ngữ tự nhiên) → 
4. Evaluation_Results.md
```

#### Cho Người triển khai Kỹ thuật / Nhà phát triển
```
1. Solution_Objectives.md (hiểu mục tiêu) → 
2. 0-Kien-truc-Tong-quan.md → 
3. Tài liệu chuyên sâu kỹ thuật (1-5) → 
4. Các repository code
```

---

## 4. Tính Nghiêm ngặt và Tính Liên quan của DSR

### 4.1 Tính Nghiêm ngặt

**Nền tảng Lý thuyết**: Artifact được dựa trên các nghiên cứu đã được thiết lập về:
- Công nghệ Blockchain và đồng thuận phân tán (Nakamoto, 2008; Wood, 2014)
- Bảo mật smart contract và các mẫu thiết kế (Atzei et al., 2017)
- Hệ thống lưu trữ phi tập trung (Benet, 2014 - IPFS)
- Giải pháp cho vấn đề oracle (Breidenbach et al., 2021 - Chainlink)

**Tính Nghiêm ngặt Phương pháp luận**: 
- Phân tích vấn đề có hệ thống
- Thiết kế và tinh chỉnh lặp đi lặp lại
- Khung đánh giá chính thức với các chỉ số định lượng và định tính
- Peer review và kiểm toán code (đã lên kế hoạch)

### 4.2 Tính Liên quan

**Tác động Thực tiễn**:
- Giải quyết các sự kém hiệu quả thực tế trong quản lý tài sản
- Có thể tổng quát hóa ra ngoài cho thuê sách cho nhiều loại tài sản khác
- Cung cấp giải pháp hiệu quả về chi phí (triển khai Layer 2)
- Tích hợp cơ chế lòng tin người dùng (PolicyEngine, điểm uy tín)

**Đóng góp cho Cơ sở Tri thức**:
- Chứng minh tích hợp thực tế blockchain + IPFS + oracles + IoT
- Cung cấp triển khai tham chiếu mã nguồn mở
- Tài liệu hóa các mẫu thiết kế cho hybrid smart contracts
- Thiết lập khung đánh giá cho hiệu quả DApp

---

## 5. Các Câu hỏi Nghiên cứu

Dự án DSR này giải quyết các câu hỏi nghiên cứu sau:

**RQ1**: Công nghệ blockchain có thể cải thiện tính minh bạch và lòng tin trong các hệ thống quản lý tài sản như thế nào so với các phương pháp tập trung truyền thống?

**RQ2**: Smart contracts có thể tự động hóa các quy trình ra quyết định (phê duyệt, ký quỹ, thanh toán) ở mức độ nào trong khi vẫn duy trì tính bảo mật và công bằng?

**RQ3**: Các đánh đổi về mặt kỹ thuật và kinh tế khi triển khai DApp quản lý tài sản trên mạng blockchain Layer 1 so với Layer 2 là gì?

**RQ4**: Mạng oracle và tích hợp IoT có thể thu hẹp khoảng cách giữa logic on-chain và trạng thái tài sản trong thế giới thực như thế nào?

**RQ5**: Các rào cản áp dụng cho người dùng cuối khi chuyển từ hệ thống truyền thống sang quản lý tài sản dựa trên blockchain là gì?

Các câu hỏi này được khám phá thông qua thiết kế, triển khai và đánh giá artifact VinaLib.

---

## 6. Các Loại Đóng góp

Theo Gregor và Hevner (2013), các đóng góp DSR có thể được phân loại thành ba nhóm:

### Cấp độ 1: Phát minh (Artifact Mới)
VinaLib đóng góp một **kiến trúc tích hợp mới lạ** kết hợp:
- Tiêu chuẩn cho thuê ERC-4907 (tách quyền sở hữu và quyền sử dụng)
- Soulbound Token (SBT) cho chứng nhận thỏa thuận thuê
- Chainlink Functions cho tính toán off-chain + xác minh on-chain
- Khóa thông minh IoT được điều khiển bởi các sự kiện blockchain

### Cấp độ 2: Cải tiến (Nâng cao Artifacts Hiện có)
- Cải tiến so với hệ thống ký quỹ truyền thống thông qua tự động hóa smart contract
- Nâng cao NFT tĩnh với khả năng cho thuê động
- Tối ưu hóa chi phí gas thông qua triển khai Layer 2

### Cấp độ 3: Exaptation (Áp dụng vào Bối cảnh Mới)
- Áp dụng tokenization tài sản blockchain vào miền quản lý thư viện
- Tổng quát hóa mẫu cho các use case quản lý tài sản rộng hơn

---

## 7. Chiến lược Xác thực

Artifact sẽ được xác thực thông qua nhiều phương pháp:

### 7.1 Kiểm thử Chức năng
- Unit tests cho smart contracts (bộ test Hardhat)
- Integration testing trên testnets (AVAX Fuji, ndachain testnet)
- Kiểm toán bảo mật (phân tích tĩnh + đánh giá thủ công)

### 7.2 Đánh giá Hiệu suất
- Đo lường thông lượng và độ trễ giao dịch
- Phân tích chi phí gas (Layer 1 vs Layer 2)
- Kiểm thử khả năng mở rộng dưới tải

### 7.3 Nghiên cứu Người dùng
- Kiểm thử khả năng sử dụng với người dùng đại diện (người cho thuê, người thuê, quản trị)
- Khảo sát nhận thức về lòng tin
- Đánh giá đường cong học tập cho việc sử dụng ví Web3

### 7.4 Phân tích So sánh
- So sánh Trước/Sau (quy trình thủ công vs DApp tự động)
- Phân tích chi phí-lợi ích
- Đánh giá rủi ro (bảo mật, vận hành, tài chính)

---

## 8. Hạn chế và Phạm vi

### 8.1 Các Hạn chế Hiện tại

1. **Triển khai Thế giới Thực Hạn chế**: Hiện đang ở giai đoạn phát triển/testnet; chưa có dữ liệu production mainnet
2. **Độ Trưởng thành Tích hợp IoT**: Tích hợp khóa thông minh sử dụng API giả lập; tích hợp thiết bị Tuya thực đang chờ xử lý
3. **Cơ sở Người dùng**: Không có dữ liệu áp dụng quy mô lớn; đánh giá dựa trên các kịch bản mô phỏng
4. **Cân nhắc Quy định**: Khung pháp lý cho tài sản được tokenize khác nhau theo khu vực pháp lý; không được đề cập trong phạm vi hiện tại

### 8.2 Hướng Nghiên cứu Tương lai

- Khả năng tương tác cross-chain cho chuyển giao tài sản
- Cơ chế bảo vệ quyền riêng tư (zero-knowledge proofs cho dữ liệu nhạy cảm)
- Cơ chế quản trị (DAO cho các quyết định nền tảng)
- Mô hình hóa kinh tế của các khuyến khích token

---

## 9. Tài liệu Tham khảo

- Atzei, N., Bartoletti, M., & Cimoli, T. (2017). A survey of attacks on Ethereum smart contracts (SoK). *Principles of Security and Trust*, 164-186.
- Benet, J. (2014). IPFS - Content Addressed, Versioned, P2P File System. *arXiv preprint arXiv:1407.3561*.
- Breidenbach, L., Cachin, C., Chan, B., Coventry, A., Ellis, S., et al. (2021). Chainlink 2.0: Next Steps in the Evolution of Decentralized Oracle Networks. Chainlink Labs.
- Denning, P. J. (1997). A new social contract for research. *Communications of the ACM*, 40(2), 132-134.
- Gregor, S., & Hevner, A. R. (2013). Positioning and presenting design science research for maximum impact. *MIS Quarterly*, 37(2), 337-355.
- Hevner, A. R., March, S. T., Park, J., & Ram, S. (2004). Design science in information systems research. *MIS Quarterly*, 28(1), 75-105.
- Nakamoto, S. (2008). Bitcoin: A peer-to-peer electronic cash system. *Decentralized Business Review*, 21260.
- Peffers, K., Tuunanen, T., Rothenberger, M. A., & Chatterjee, S. (2007). A design science research methodology for information systems research. *Journal of Management Information Systems*, 24(3), 45-77.
- Tsichritzis, D. (1998). The dynamics of innovation. In *Beyond Calculation* (pp. 259-265). Springer, New York, NY.
- Wood, G. (2014). Ethereum: A secure decentralised generalised transaction ledger. *Ethereum Project Yellow Paper*, 151(2014), 1-32.

---

*Cập nhật lần cuối: Tháng 1 năm 2026*  
*Phiên bản: 1.0*  
*Trạng thái: Tài liệu Sống - Sẽ được cập nhật khi dự án tiến triển qua các giai đoạn DSR*
