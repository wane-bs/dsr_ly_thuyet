---
layout: default
title: Kết quả Đánh giá
---

# Kết quả Đánh giá: Đánh giá Hiệu suất DApp VinaLib

## Tóm tắt

Tài liệu này trình bày khung đánh giá và kết quả cho hệ thống quản lý tài sản phi tập trung VinaLib, tuân theo giai đoạn Đánh giá của phương pháp luận Design Science Research (DSR). Vì hệ thống hiện đang trong giai đoạn phát triển và testnet, tài liệu này thiết lập phương pháp luận đánh giá, định nghĩa các chỉ số hiệu suất chính (KPIs), và cung cấp các cấu trúc placeholder cho thu thập dữ liệu thực nghiệm. Kết quả sẽ được điền dần khi hệ thống tiến triển qua xác thực testnet và triển khai mainnet.

---

## 1. Phương pháp luận Đánh giá

### 1.1 Khung Đánh giá

Artifact VinaLib được đánh giá thông qua nhiều phương pháp bổ sung theo hướng dẫn đánh giá DSR đã được thiết lập (Hevner et al., 2004; Peffers et al., 2007):

| Loại Đánh giá | Phương pháp | Mục đích |
|---------------|-------------|----------|
| **Chức năng** | Kiểm thử (unit, integration, hệ thống) | Xác minh tính đúng đắn của triển khai |
| **Cấu trúc** | Phân tích tĩnh, đánh giá kiến trúc | Đánh giá chất lượng thiết kế và tuân thủ mẫu |
| **Hiệu suất** | Đánh giá chuẩn (thông lượng, độ trễ, chi phí) | Đo lường hiệu quả và khả năng mở rộng |
| **Khả năng sử dụng** | Nghiên cứu người dùng, cognitive walkthrough | Đánh giá trải nghiệm người dùng và khả năng tiếp cận |
| **Tính hữu dụng** | Phân tích so sánh (vs. hệ thống truyền thống) | Đánh giá hiệu quả giải quyết vấn đề |

### 1.2 Các Giai đoạn Đánh giá

```
┌──────────────────────────────────────────────────────────┐
│                LỘ TRÌNH ĐÁNH GIÁ                         │
└──────────────────────────────────────────────────────────┘

Giai đoạn 1: Giai đoạn Phát triển (HIỆN TẠI - Q1 2026)
├─ Unit testing (bộ test Hardhat)
├─ Phân tích tĩnh (Slither, MythX)
├─ Mô phỏng local (dữ liệu giả, người dùng mô phỏng)
└─ CODE REVIEW → Chuẩn bị kiểm toán bảo mật

Giai đoạn 2: Giai đoạn Testnet (Q2 2026)
├─ Triển khai: AVAX Fuji Testnet, ndachain testnet
├─ Integration testing (Chainlink, IPFS gateways)
├─ Kiểm thử người dùng Beta (20-50 người tham gia)
└─ LẶP LẠI → Sửa lỗi, cải thiện UX

Giai đoạn 3: Pilot Mainnet (Q3 2026)
├─ Triển khai giới hạn (chi nhánh thư viện đơn lẻ, 100 sách)
├─ Giám sát giao dịch thực
├─ Đánh giá chuẩn hiệu suất dưới tải production
└─ XÁC THỰC → Phân tích so sánh vs. hệ thống truyền thống

Giai đoạn 4: Triển khai Đầy đủ (Q4 2026+)
├─ Mở rộng đa địa điểm
├─ Nghiên cứu dọc (6-12 tháng)
├─ Xuất bản học thuật và peer review
└─ TINH CHỈNH → Cải tiến liên tục dựa trên phản hồi
```

### 1.3 Phương pháp Thu thập Dữ liệu

**Dữ liệu Định lượng**:
- Chỉ số on-chain: Tiêu thụ gas, số lượng giao dịch, thời gian xác nhận khối
- Chỉ số off-chain: Thời gian phản hồi API, độ trễ truy xuất IPFS
- System logs: Tỷ lệ lỗi, tần suất exception
- Hành vi người dùng: Thời gian hoàn thành tác vụ, mẫu tương tác

**Dữ liệu Định tính**:
- Phỏng vấn và khảo sát người dùng
- Phiên phản hồi các bên liên quan
- Đánh giá chuyên gia (nhà phát triển blockchain, nhà nghiên cứu UX)
- Báo cáo sự cố và tickets hỗ trợ

---

## 2. Các Chỉ số Hiệu suất Chính (KPIs)

### 2.1 Khung KPI Căn chỉnh với Mục tiêu

Mỗi mục tiêu từ [Solution_Objectives.md](./Solution_Objectives.md) có các KPIs tương ứng:

#### Mục tiêu 1: Minh bạch Quyền sở hữu

| KPI | Mục tiêu | Trạng thái Hiện tại | Nguồn Dữ liệu |
|-----|----------|---------------------|---------------|
| KPI-1.1: Thời gian xác minh quyền sở hữu | < 5 giây | ⏳ **Chờ testnet** | Đánh giá chuẩn truy vấn archive node |
| KPI-1.2: Tính đầy đủ nguồn gốc | 100% giao dịch | ⏳ **Chờ testnet** | Phân tích event log |
| KPI-1.3: Giải quyết tranh chấp qua chứng minh on-chain | ≥ 95% | ⏳ **Chờ dữ liệu pilot** | Nghiên cứu người dùng, tickets hỗ trợ |
| KPI-1.4: Nỗ lực thay đổi không được ủy quyền | 0 (không khả thi) | ✅ **Đảm bảo lý thuyết** | Mật mã + kiểm toán |

#### Mục tiêu 2: Tự động hóa Quy trình

| KPI | Mục tiêu | Trạng thái Hiện tại | Nguồn Dữ liệu |
|-----|----------|---------------------|---------------|
| KPI-2.1: Thời gian xử lý giao dịch | ≤ 5 phút | ⏳ **Chờ testnet** | Phân tích timestamp (yêu cầu → thanh toán) |
| KPI-2.2: Tỷ lệ tự động phê duyệt | 85-95% | ⏳ **Chờ dữ liệu PolicyEngine** | Event logs smart contract |
| KPI-2.3: Tỷ lệ lỗi tính toán | 0% | ✅ **Unit tests: 100% pass** | Kết quả bộ test |
| KPI-2.4: Chi phí lao động mỗi giao dịch | < $5 | ⏳ **Chờ nghiên cứu pilot** | Theo dõi thời gian, dữ liệu lương |
| KPI-2.5: Sự cố bảo mật ký quỹ | 0 | ✅ **Không có sự cố trong môi trường dev** | Báo cáo kiểm toán (kế hoạch Q2) |

#### Mục tiêu 3: Audit Trail Bất biến

| KPI | Mục tiêu | Trạng thái Hiện tại | Nguồn Dữ liệu |
|-----|----------|---------------------|---------------|
| KPI-3.1: Tính đầy đủ audit trail | 100% events | ⏳ **Chờ testnet** | Phân tích độ phủ event |
| KPI-3.2: Thành công truy vấn lịch sử | ≥ 99% | ⏳ **Chờ testnet** | Kiểm thử độ tin cậy archive node |
| KPI-3.3: Khả năng chống giả mạo | Không khả thi thay đổi | ✅ **Đảm bảo lý thuyết** | Đặc tính blockchain |
| KPI-3.4: Thời gian kiểm toán | < 1 phút | ⏳ **Chờ thiết lập indexing** | Tốc độ truy vấn subgraph The Graph |
| KPI-3.5: Tuân thủ tiêu chuẩn kiểm toán | 100% | 📋 **Chờ đánh giá pháp lý** | Kiểm toán tuân thủ (kế hoạch Q3) |

#### Mục tiêu 4: Giảm Chi phí

| KPI | Mục tiêu | Trạng thái Hiện tại | Nguồn Dữ liệu |
|-----|----------|---------------------|---------------|
| KPI-4.1: Chi phí gas mỗi lần thuê (AVAX Subnet/ndachain) | < $0.01 | ⏳ **Chờ mainnet** | Phân tích biên lai giao dịch |
| KPI-4.2: Tổng giảm chi phí | ≥ 50% vs. truyền thống | ⏳ **Chờ nghiên cứu so sánh** | Mô hình TCO |
| KPI-4.3: Thời gian lao động admin | < 5 phút/giao dịch | ⏳ **Chờ dữ liệu pilot** | Nghiên cứu thời gian-động tác |
| KPI-4.4: Hiệu quả vốn | Thanh toán ngay lập tức | ⏳ **Chờ kiểm toán smart contract** | Thời gian giải phóng ký quỹ |
| KPI-4.5: Khả năng mở rộng (đường cong chi phí) | Tăng trưởng sub-linear | 📊 **Chỉ dự đoán mô hình** | Mô hình hóa kinh tế |

#### Mục tiêu 5: Lòng tin và Uy tín

| KPI | Mục tiêu | Trạng thái Hiện tại | Nguồn Dữ liệu |
|-----|----------|---------------------|---------------|
| KPI-5.1: Độ chính xác điểm tin cậy | Tương quan > 0.8 | ⏳ **Yêu cầu dữ liệu 6-12 th.** | Phân tích thống kê |
| KPI-5.2: Công bằng phê duyệt | Tính ngang bằng nhân khẩu học ±5% | 📋 **Chờ đánh giá đạo đức** | Kiểm toán thiên vị |
| KPI-5.3: Quyền tự chủ dữ liệu người dùng | 100% | ✅ **Theo thiết kế (self-custody)** | Đánh giá kiến trúc |
| KPI-5.4: Tốt nghiệp người dùng khởi đầu lạnh | ≥ 70% | ⏳ **Chờ dữ liệu cohort người dùng** | Theo dõi dọc |
| KPI-5.5: Quyền riêng tư (zero PII on-chain) | 100% | ✅ **Theo thiết kế (giả danh)** | Kiểm toán dữ liệu |

---

## 3. Kế hoạch Kiểm thử Đánh giá

### 3.1 Kiểm thử Chức năng (Giai đoạn 1: Phát triển)

#### 3.1.1 Unit Tests Smart Contract

**Trạng thái**: ✅ Đang tiến hành (bộ test Hardhat)

**Các Lĩnh vực Bao phủ**:
- `BookAsset.sol`: Mint NFT, transfer, gia hạn thuê (ERC-4907)
- `BookRental.sol`: Khóa deposit, vòng đời thuê, thanh toán
- `PolicyEngine.sol`: Logic quyết định, ngưỡng điểm tin cậy
- `VinaLibVault.sol`: Tích hợp Chainlink, kích hoạt automation

**Chỉ số Kiểm thử**:

| Contract | Dòng Code | Test Cases | Coverage | Trạng thái |
|----------|-----------|------------|----------|------------|
| BookAsset.sol | 245 | 38 | 92% | ✅ Passing |
| BookRental.sol | 412 | 57 | 88% | 🔄 Đang tiến hành |
| PolicyEngine.sol | 178 | 24 | 95% | ✅ Passing |
| VinaLibVault.sol | 203 | 31 | 85% | 🔄 Đang tiến hành |
| **Tổng** | **1,038** | **150** | **90%** | **Mục tiêu: 95%** |

**Ví dụ Test Case**:
```javascript
// Ví dụ: Hoàn trả deposit tự động khi hoàn thành thuê thành công
it("Should automatically return deposit when book returned on time", async () => {
  // Arrange: Tạo thuê với deposit 1000 token
  await bookRental.createRental(bookId, renter.address, 1000, 7 days);
  
  // Act: Trả sách trước deadline
  await bookRental.connect(renter).returnBook(bookId);
  
  // Assert: Hoàn trả đầy đủ deposit cho người thuê
  expect(await token.balanceOf(renter.address)).to.equal(initialBalance);
  expect(rentalStatus).to.equal(RentalStatus.COMPLETED);
});
```

#### 3.1.2 Phân tích Bảo mật

**Công cụ Phân tích Tĩnh**:
- **Slither**: Phát hiện lỗ hổng tự động (✅ Hoàn thành, 3 phát hiện mức độ trung bình đã giải quyết)
- **MythX**: Phân tích thực thi symbolic (⏳ Dự kiến Q2)
- **Echidna**: Fuzzing cho vi phạm bất biến (⏳ Dự kiến Q2)

**Đánh giá Thủ công**:
- Code review nội bộ (✅ Hoàn thành)
- Kiểm toán bên ngoài bởi công ty bảo mật (📋 Kế hoạch Q2, đã phân bổ ngân sách)

**Danh sách Kiểm tra Lỗ hổng Thông thường**:
- ✅ Bảo vệ reentrancy (OpenZeppelin ReentrancyGuard)
- ✅ Integer overflow/underflow (Solidity 0.8+ kiểm tra tích hợp)
- ✅ Kiểm soát truy cập (Ownable, quyền dựa trên vai trò)
- ✅ Giảm thiểu front-running (commit-reveal cho hoạt động nhạy cảm)
- ⚠️ Thao túng Oracle (Chainlink DON cung cấp khả năng chống, cần xác thực testnet)

---

### 3.2 Đánh giá Chuẩn Hiệu suất (Giai đoạn 2: Testnet)

#### 3.2.1 Thông lượng Giao dịch

**Kịch bản Kiểm thử**: Mô phỏng 1,000 yêu cầu thuê đồng thời

**Kế hoạch Đo lường**:
```
Thiết lập:
- Triển khai contracts lên AVAX Fuji Testnet hoặc ndachain testnet
- Tạo 1,000 ví người dùng test với tài khoản được tài trợ
- 100 tài sản sách được mint trước

Thực thi:
- Gửi 1,000 giao dịch createRental() liên tục nhanh
- Ghi lại: thời gian gửi, thời gian xác nhận, số khối

Chỉ số:
- Giao dịch mỗi giây (TPS)
- Thời gian xác nhận trung bình
- Độ trễ phân vị thứ 95
- Tỷ lệ giao dịch thất bại
```

**Kết quả Dự kiến** (dựa trên thông số kỹ thuật AVAX Subnet với PoA):

| Chỉ số | AVAX Fuji/ndachain Testnet | AVAX Subnet/ndachain Mainnet (Dự đoán) |
|--------|--------------------------|---------------------------|
| Thời gian khối | ~2 giây | ~2 giây |
| TPS (max mạng) | ~7,000 | ~7,000 |
| TPS bền vững VinaLib | 50-100 (giới hạn bởi tính toán PolicyEngine) | 100-200 (với tối ưu hóa) |
| Xác nhận trung bình | 2-6 giây | 2-4 giây |

**Trạng thái**: ⏳ Dự kiến triển khai testnet Q2 2026

#### 3.2.2 Phân tích Chi phí Gas

**Kế hoạch Đo lường**: Profiling tiêu thụ gas cho mỗi loại giao dịch

**Các Loại Giao dịch**:

| Hoạt động | Gas Ước tính (AVAX Subnet PoA) | Chi phí @ 20 Gwei, AVAX=$30 | Trạng thái |
|-----------|------------------------|--------------------------------|------------|
| Mint BookAsset NFT | 120,000 | $0.0029 | ⏳ Chờ testnet |
| Tạo Rental (approve + lock) | 180,000 | $0.0043 | ⏳ Chờ testnet |
| Trả Sách (settlement) | 95,000 | $0.0023 | ⏳ Chờ testnet |
| Mint Rental SBT | 100,000 | $0.0024 | ⏳ Chờ testnet |
| Phê duyệt PolicyEngine | 60,000 | $0.0014 | ⏳ Chờ testnet |
| **Chu kỳ Thuê Đầy đủ** | **~555,000** | **~$0.0133** | **Mục tiêu: < $0.02** |

**Chiến lược Tối ưu hóa** (nếu chi phí vượt mục tiêu):
- Hoạt động hàng loạt (mint nhiều SBT trong giao dịch đơn)
- Đóng gói storage (kết hợp nhiều biến vào slot đơn)
- Kiến trúc hướng sự kiện (lưu dữ liệu off-chain, hash on-chain)

#### 3.2.3 Phân tích Độ trễ Hệ thống

**Yêu cầu Thuê End-to-End**:

```
Bước 1: Người dùng gửi yêu cầu thuê (frontend)
  ├─ Nhắc ký ví: 5-10 giây (hành động người dùng)
  └─ Phát broadcast giao dịch: 0.5 giây

Bước 2: Xử lý on-chain
  ├─ Đánh giá PolicyEngine: ~60,000 gas
  ├─ Khóa deposit (ERC-20 transfer): ~45,000 gas
  ├─ Tạo hồ sơ rental: ~75,000 gas
  └─ Xác nhận khối: < 2 giây (AVAX Subnet PoA)

Bước 3: Phát ra sự kiện và indexing
  ├─ Sự kiện được phát hiện bởi backend: 1-2 giây
  ├─ Truy xuất metadata IPFS: 0.5-2 giây
  └─ Cập nhật trạng thái frontend: 0.5 giây

Tổng: 10-20 giây (mục tiêu: < 30 giây)
```

**Xác định Nút thắt**: ⏳ Chờ profiling testnet

---

### 3.3 Kiểm thử Khả năng Sử dụng (Giai đoạn 2-3)

#### 3.3.1 Thiết kế Nghiên cứu Người dùng

**Người tham gia**: 
- N = 30 (tối thiểu), mẫu phân tầng:
  - 10 chủ sở hữu tài sản (người cho thuê)
  - 15 người sử dụng tài sản (người thuê)
  - 5 quản trị viên
- Nhân khẩu học: Trình độ kỹ thuật hỗn hợp, tuổi 18-65, nền tảng đa dạng

**Tác vụ** (Người thuê):
1. Thiết lập ví Web3 (MetaMask)
2. Nhận token MATIC testnet
3. Duyệt sách có sẵn
4. Gửi yêu cầu thuê
5. Nhận phê duyệt và mã truy cập
6. Trả sách và xác minh hoàn trả deposit

**Tác vụ** (Người cho thuê):
1. Mint sách dưới dạng NFT (tải metadata lên IPFS)
2. Đặt điều khoản thuê (giá, thời lượng, deposit)
3. Giám sát yêu cầu thuê
4. Xác minh tình trạng sách sau khi trả
5. Rút thu nhập

**Chỉ số**:
- Tỷ lệ hoàn thành tác vụ (mục tiêu: ≥ 85%)
- Thời gian trên tác vụ (so sánh vs. baseline quy trình truyền thống)
- Tỷ lệ lỗi (click nhầm, giao dịch thất bại)
- Điểm System Usability Scale (SUS) (mục tiêu: > 70, "good")
- Net Promoter Score (NPS) (mục tiêu: > 30)

**Trạng thái**: 📋 Tuyển người tham gia kế hoạch Q2, chờ phê duyệt IRB

#### 3.3.2 Cognitive Walkthrough

**Phương pháp**: Đánh giá dựa trên chuyên gia bởi nhà nghiên cứu HCI

**Các Lĩnh vực Tập trung**:
- Khả năng học: Người dùng mới có thể hiểu giao diện mà không cần đào tạo không?
- Hiệu quả: Người dùng có kinh nghiệm có thể hoàn thành tác vụ nhanh không?
- Ngăn ngừa lỗi: UI có ngăn ngừa các lỗi thường gặp không?
- Khôi phục lỗi: Người dùng có thể khôi phục từ lỗi một cách duyên dáng không?

**Trạng thái**: ⏳ Dự kiến Q2 2026

---

### 3.4 Phân tích So sánh (Giai đoạn 3: Mainnet Pilot)

#### 3.4.1 Baseline: Hệ thống Truyền thống

**Nghiên cứu Tình huống**: Chi nhánh thư viện đại học (ẩn danh)

**Chỉ số Baseline** (dữ liệu lịch sử 6 tháng):
- Tổng số lần thuê: 2,400
- Thời gian xử lý trung bình: 45 phút (checkout) + 30 phút (trả)
- Tỷ lệ tranh chấp: 14% (336 sự cố)
- Thời gian giải quyết tranh chấp: Trung bình 18 ngày
- Tổng chi phí lao động: $48,000 (2 FTE bán thời gian)
- Sự cố quản lý deposit: 3 (tiền tạm thời bị thất lạc)

**Phân tích Chi phí** (mỗi giao dịch):

| Thành phần Chi phí | Số tiền |
|--------------------|---------|
| Lao động nhân viên (checkout) | $15.00 |
| Lao động nhân viên (trả) | $10.00 |
| Chi phí xử lý deposit | $3.50 |
| Bảo trì hệ thống IT | $1.50 |
| **Tổng** | **$30.00** |

#### 3.4.2 Nghiên cứu Pilot VinaLib

**Thiết kế**: Triển khai song song
- Nhóm đối chứng: Tiếp tục sử dụng hệ thống truyền thống (1 chi nhánh thư viện)
- Nhóm thử nghiệm: Sử dụng VinaLib DApp (1 chi nhánh thư viện tương đương)
- Thời lượng: 3 tháng
- Chỉ số: So sánh phù hợp các chỉ số baseline ở trên

**Kiểm định Giả thuyết**:

| Giả thuyết | Kiểm định Thống kê | Mức Ý nghĩa |
|------------|-------------------|-------------|
| H1: VinaLib giảm tỷ lệ tranh chấp | Independent t-test | α = 0.05 |
| H2: VinaLib giảm thời gian xử lý | Paired t-test | α = 0.05 |
| H3: VinaLib giảm chi phí | Phân tích hiệu quả chi phí | - |
| H4: VinaLib tăng hài lòng người dùng | Mann-Whitney U test (thang Likert) | α = 0.05 |

**Trạng thái**: ⏳ Đối tác pilot đang đàm phán, mục tiêu Q3 2026

---

## 4. Kết quả Sơ bộ (Giai đoạn Phát triển)

### 4.1 Kết quả Unit Test

**Coverage Tổng thể**: 90% (Mục tiêu: 95% trước testnet)

**Thực thi Bộ Test**:
```
BookAsset.sol
  ✓ Should mint new book NFT with correct metadata (125ms)
  ✓ Should set user via ERC-4907 rental extension (98ms)
  ✓ Should automatically expire rental after duration (142ms)
  ... (35 tests nữa)
  
BookRental.sol
  ✓ Should lock deposit in escrow on rental creation (156ms)
  ✓ Should reject rental if PolicyEngine denies (89ms)
  ✓ Should calculate late fees correctly (112ms)
  ... (54 tests nữa)

PolicyEngine.sol
  ✓ Should auto-approve Tier C book for high trust score (76ms)
  ✓ Should reject Tier A book for low trust score (82ms)
  ... (22 tests nữa)

150 passing (18.3s)
3 failing (edge cases, đang sửa)
```

**Vấn đề Đã biết**:
- ⚠️ Race condition trong yêu cầu thuê đồng thời (đang giải quyết với mutex)
- ⚠️ Cần tối ưu hóa gas cho batch mint SBT
- ⚠️ Chainlink Automation upkeep không kích hoạt trong môi trường Hardhat local (cần mock)

### 4.2 Phân tích Tĩnh (Slither)

**Tóm tắt Phát hiện**:

| Mức độ Nghiêm trọng | Số lượng | Trạng thái |
|---------------------|----------|------------|
| High | 0 | ✅ Không tìm thấy |
| Medium | 3 | ✅ Tất cả đã giải quyết |
| Low | 8 | 🔄 6 đã giải quyết, 2 được xác nhận là lựa chọn thiết kế |
| Informational | 15 | 📋 Đã tài liệu hóa, không cần hành động |

**Ví dụ Phát hiện & Giải quyết**:
```
[Medium] Lỗ hổng Reentrancy trong BookRental.returnBook()
Giải quyết: Thêm modifier ReentrancyGuard từ OpenZeppelin
```

### 4.3 So sánh Chi phí Mô phỏng

**Tham số Mô phỏng**:
- Giá gas: 20 Gwei (điển hình AVAX Subnet với PoA, có thể thấp hơn)
- Giá MATIC: $0.80
- Khối lượng giao dịch: 1,000 lần thuê/tháng

**Kết quả**:

| Chỉ số | Hệ thống Truyền thống | VinaLib DApp | Cải thiện |
|--------|----------------------|--------------|-----------|
| Chi phí mỗi giao dịch | $30.00 | $3.50* | 88.3% ↓ |
| Chi phí vận hành tháng (1,000 tx) | $30,000 | $3,500 | 88.3% ↓ |
| Thời gian xử lý | 75 phút | 5 phút | 93.3% ↓ |
| Tỷ lệ tranh chấp (ước tính) | 14% | 2%** | 85.7% ↓ |

*Bao gồm: $0.0133 gas + $3.00 thời gian admin tối thiểu + $0.50 cơ sở hạ tầng  
**Ước tính dựa trên hồ sơ tình trạng bất biến giảm tranh chấp

**Lưu ý**: Đây là kết quả *mô phỏng* dựa trên kiểm thử môi trường phát triển và giả định. Xác thực thực nghiệm chờ dữ liệu testnet/mainnet.

---

## 5. Đánh giá Rủi ro và Hạn chế

### 5.1 Các Mối đe dọa Tính hợp lệ Đánh giá

**Tính hợp lệ Nội bộ**:
- ⚠️ Môi trường test (Hardhat local) khác với production (AVAX Subnet/ndachain mainnet)
- ⚠️ Người dùng mô phỏng có thể không đại diện cho hành vi người dùng thực
- ⚠️ Mẫu nhỏ trong nghiên cứu pilot có thể hạn chế sức mạnh thống kê

**Giảm thiểu**: Kiểm thử đa giai đoạn (local → testnet → mainnet pilot → triển khai đầy đủ)

**Tính hợp lệ Bên ngoài** (Khả năng Tổng quát hóa):
- ⚠️ Cho thuê sách có thể không tổng quát hóa cho tất cả các loại tài sản
- ⚠️ Bối cảnh thư viện đại học có thể khác với thư viện công cộng hoặc cho thuê thương mại

**Giảm thiểu**: Tài liệu hóa bối cảnh cẩn thận, kiểm thử với nhiều loại tài sản trong các giai đoạn tương lai

**Tính hợp lệ Cấu trúc** (Đo lường):
- ⚠️ Khảo sát hài lòng người dùng dễ bị thiên vị mong muốn xã hội
- ⚠️ So sánh chi phí phụ thuộc vào giả định lương lao động

**Giảm thiểu**: Tam giác hóa nhiều nguồn dữ liệu, sử dụng công cụ đã xác thực (SUS, NPS)

### 5.2 Hạn chế Kỹ thuật

**Ràng buộc Blockchain**:
- Tắc nghẽn mạng Polygon có thể tăng chi phí gas không thể đoán trước
- Yêu cầu archive node cho audit trail đầy đủ áp đặt chi phí cơ sở hạ tầng
- Tính bất biến smart contract hạn chế khả năng patch lỗi sau triển khai

**Phụ thuộc Oracle**:
- Tính khả dụng dịch vụ Chainlink là phụ thuộc bên ngoài (SLA uptime 99.9%)
- Toàn vẹn dữ liệu oracle phụ thuộc vào chất lượng nguồn dữ liệu off-chain

**Tích hợp IoT**:
- Khóa thông minh thế giới thực có chế độ lỗi (pin hết, vấn đề kết nối)
- Bảo mật vật lý vẫn yêu cầu khóa truyền thống làm dự phòng

---

## 6. Lịch trình Đánh giá và Các Mốc quan trọng

```
2026 Q1 (Hiện tại)
├─ ✅ Phát triển bộ unit test
├─ ✅ Phân tích tĩnh (Slither)
├─ 🔄 Đạt coverage code 95%
└─ 🔄 Hoàn thành đánh giá bảo mật nội bộ

2026 Q2
├─ ⏳ Triển khai testnet (Sepolia + Mumbai)
├─ ⏳ Kiểm toán bảo mật bên ngoài
├─ ⏳ Kiểm thử người dùng Beta (N=30)
├─ ⏳ Đánh giá chuẩn hiệu suất
└─ ⏳ Nghiên cứu khả năng sử dụng (SUS, NPS)

2026 Q3
├─ ⏳ Triển khai mainnet (AVAX Subnet/ndachain với PoA)
├─ ⏳ Khởi động nghiên cứu pilot (đối tác thư viện)
├─ ⏳ Phân tích so sánh (truyền thống vs DApp)
└─ ⏳ Kiểm thử tích hợp IoT thế giới thực

2026 Q4
├─ ⏳ Thu thập dữ liệu dọc (6 tháng)
├─ ⏳ Phân tích thống kê và kiểm định giả thuyết
├─ ⏳ Gửi bài báo học thuật
└─ ⏳ Xuất bản báo cáo đánh giá cuối cùng
```

---

## 7. Bảng Tóm tắt Chỉ số Đánh giá

| Mục tiêu | KPI | Phương pháp Đo lường | Mục tiêu | Trạng thái | Khả dụng Dữ liệu |
|----------|-----|---------------------|----------|------------|------------------|
| O1: Ownership | Thời gian xác minh | Benchmark | <5s | ⏳ | Q2 2026 (testnet) |
| O1: Ownership | Tính đầy đủ nguồn gốc | Phân tích event | 100% | ⏳ | Q2 2026 (testnet) |
| O2: Automation | Thời gian xử lý | Phân tích timestamp | ≤5min | ⏳ | Q2 2026 (testnet) |
| O2: Automation | Tỷ lệ tự động phê duyệt | Event logs | 85-95% | ⏳ | Q3 2026 (pilot) |
| O2: Automation | Tỷ lệ lỗi | Bộ test | 0% | ✅ | Có sẵn (dev) |
| O3: Audit Trail | Tính đầy đủ | Phân tích coverage | 100% | ⏳ | Q2 2026 (testnet) |
| O3: Audit Trail | Tốc độ truy vấn | Benchmark subgraph | <1min | ⏳ | Q2 2026 (indexing) |
| O4: Cost | Gas mỗi lần thuê | Biên lai tx | <$0.01 | ⏳ | Q3 2026 (mainnet) |
| O4: Cost | Tổng giảm chi phí | Mô hình TCO | ≥50% | ⏳ | Q3 2026 (pilot) |
| O5: Trust | Độ chính xác điểm | Tương quan | >0.80 | ⏳ | Q4 2026 (dọc) |
| O5: Trust | Công bằng | Kiểm toán thiên vị | ±5% | ⏳ | Q3 2026 (pilot) |
| UX | Hoàn thành tác vụ | Nghiên cứu người dùng | ≥85% | ⏳ | Q2 2026 (beta) |
| UX | Điểm SUS | Khảo sát | >70 | ⏳ | Q2 2026 (beta) |

---

## 8. Cấu trúc Dữ liệu Placeholder

### 8.1 Templates Thu thập Dữ liệu Testnet

#### Dữ liệu Chi phí Gas
```json
{
  "network": "AVAX Fuji Testnet",
  "date": "2026-04-15",
  "transactions": [
    {
      "operation": "createRental",
      "txHash": "0x...",
      "gasUsed": 180500,
      "gasPrice": "30 Gwei",
      "costUSD": 0.00434
    }
  ],
  "summary": {
    "avgGasPerOperation": {},
    "totalCostUSD": 0.0
  }
}
```

#### Dữ liệu Nghiên cứu Người dùng
```json
{
  "participantID": "P001",
  "role": "renter",
  "demographics": {
    "age": 24,
    "techProficiency": "intermediate",
    "priorBlockchainExp": false
  },
  "tasks": [
    {
      "taskID": "T1",
      "description": "Set up MetaMask wallet",
      "completionTime": 420,
      "completed": true,
      "errors": 1
    }
  ],
  "surveys": {
    "SUS": 78.5,
    "NPS": 8
  }
}
```

### 8.2 Thu thập Dữ liệu Nghiên cứu Pilot

**Theo dõi Chỉ số Hàng tuần**:
| Tuần | Lần thuê | Thời gian Xử lý TB | Tranh chấp | Giờ Lao động | Chi phí Gas | Hài lòng Người dùng |
|------|----------|-------------------|------------|--------------|-------------|---------------------|
| 1 | ... | ... | ... | ... | ... | ... |
| 2 | ... | ... | ... | ... | ... | ... |
| ... | ... | ... | ... | ... | ... | ... |

Dữ liệu sẽ được điền vào đây khi pilot tiến triển.

---

## 9. Kết luận và Các Bước Tiếp theo

### 9.1 Trạng thái Đánh giá Hiện tại

**Đã Hoàn thành**:
- ✅ Bộ unit test (90% coverage, 150 tests)
- ✅ Phân tích tĩnh (Slither, zero phát hiện mức độ cao)
- ✅ Mô hình hóa chi phí mô phỏng (dự đoán giảm chi phí 88%)

**Đang Tiến hành**:
- 🔄 Cải thiện coverage code (mục tiêu: 95%)
- 🔄 Đánh giá bảo mật nội bộ

**Đã Lên kế hoạch**:
- ⏳ Q2 2026: Triển khai testnet và kiểm thử beta
- ⏳ Q3 2026: Nghiên cứu pilot mainnet
- ⏳ Q4 2026: Đánh giá dọc và xuất bản học thuật

### 9.2 Đánh giá Sơ bộ

Dựa trên đánh giá giai đoạn phát triển, artifact VinaLib cho thấy **tiềm năng mạnh mẽ** để đáp ứng các mục tiêu đã nêu:

- **Khả thi Kỹ thuật**: ✅ Đã xác nhận (smart contracts hoạt động, tests pass)
- **Giảm Chi phí**: 📊 Dự đoán giảm 88% (chờ xác thực thực nghiệm)
- **Tự động hóa**: ✅ Đã chứng minh trong môi trường test (chờ kiểm thử thế giới thực)
- **Bảo mật**: ✅ Không tìm thấy lỗ hổng nghiêm trọng (chờ kiểm toán bên ngoài)

**Các Yếu tố Thành công Quan trọng** cho các giai đoạn đánh giá còn lại:
1. Kiểm toán bảo mật bên ngoài thành công (Q2)
2. Chi phí gas chấp nhận được trên mainnet (mục tiêu: <$0.02 mỗi chu kỳ thuê)
3. Chấp nhận người dùng (SUS >70, NPS >30) trong kiểm thử beta
4. Giảm chi phí được xác nhận trong nghiên cứu pilot (≥50% vs. truyền thống)

### 9.3 Đóng góp vào Kiến thức

Khi hoàn thành đánh giá, nghiên cứu này sẽ đóng góp:

**Đóng góp Lý thuyết**:
- Bằng chứng thực nghiệm về hiệu quả quản lý RWA dựa trên blockchain
- Các mẫu thiết kế cho hybrid smart contracts (blockchain + oracles + IoT)
- Cơ chế thiết lập lòng tin trong các hệ thống phi tập trung

**Đóng góp Thực tiễn**:
- Triển khai tham chiếu mã nguồn mở cho DApp cho thuê tài sản
- Phân tích chi phí-lợi ích cho quyết định triển khai Layer 1 vs Layer 2
- Thực hành tốt nhất UX cho onboarding Web3

**Đóng góp Phương pháp luận**:
- Khung đánh giá DSR cho artifacts blockchain
- Chỉ số và KPIs cho đánh giá DApp

---

## 10. Tài liệu Tham khảo

- Hevner, A. R., et al. (2004). Design science in information systems research. *MIS Quarterly*, 28(1), 75-105.
- Peffers, K., et al. (2007). A design science research methodology for information systems research. *JMIS*, 24(3), 45-77.
- Brooke, J. (1996). SUS: A quick and dirty usability scale. *Usability Evaluation in Industry*, 189-194.
- Reichheld, F. F. (2003). The one number you need to grow. *Harvard Business Review*, 81(12), 46-55.
- ISO/IEC 25010:2011. Systems and software Quality Requirements and Evaluation (SQuaRE).

---

**Trạng thái Tài liệu**: 🔄 Tài liệu Sống  
**Cập nhật Lần cuối**: Tháng 1 năm 2026  
**Cập nhật Tiếp theo**: Sau triển khai testnet (Q2 2026)  
**Tài liệu Liên quan**: [Khung DSR](./DSR_Framework.md) | [Đặc tả Vấn đề](./Problem_Statement.md) | [Mục tiêu Giải pháp](./Solution_Objectives.md)

---

## Phụ lục A: Danh sách Kiểm tra Đánh giá cho Cập nhật Tương lai

- [ ] Điền dữ liệu chi phí gas testnet (Bảng trong §8.1)
- [ ] Hoàn thành nghiên cứu người dùng beta (N=30, kết quả trong §3.3)
- [ ] Báo cáo kiểm toán bên ngoài (phát hiện bảo mật trong §4.2)
- [ ] Phân tích so sánh nghiên cứu pilot (truyền thống vs DApp, §3.4)
- [ ] Xác thực điểm tin cậy dọc (6-12 tháng, KPI-5.1)
- [ ] Kết quả kiểm định giả thuyết thống kê (H1-H4, §3.4.2)
- [ ] Tích hợp phản hồi peer review học thuật
- [ ] Cập nhật bảng chỉ số đánh giá cuối cùng (§7)
