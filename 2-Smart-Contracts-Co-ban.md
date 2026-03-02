---
layout: default
title: Smart Contracts (Cơ bản)
---

# Smart Contracts Cơ bản: Nguyên lý và Ứng dụng trong VinaLib
## Tài liệu Giới thiệu Khái niệm và Ứng dụng

> 📍 **Vị trí trong bộ tài liệu**: 2/5 - Logic cốt lõi (Cơ bản)  
> ⬅️ [IPFS](./1-IPFS.md) | ➡️ [Smart Contracts (Nâng cao)](./3-Smart-Contracts-Nang-cao.md)

### 📎 Tài liệu Liên quan
| Liên kết đến | Mối quan hệ |
|-------------|-------------|
| [IPFS](./1-IPFS.md) | Lưu CID (ảnh bìa, metadata) trong contract storage |
| [Smart Contracts (Nâng cao)](./3-Smart-Contracts-Nang-cao.md) | Patterns nâng cao: Ownable, Pausable, ERC Standards, SBT |
| [Chainlink](./4-Chainlink.md) | PolicyEngine sẽ dùng Oracle để verify external data |
| [IoT](./5-IoT.md) | Events từ contract trigger IoT unlock |

### 📌 Phạm vi Tài liệu này
Tài liệu này bao gồm các **khái niệm cơ bản** về Smart Contracts:
- ✅ Smart Contract là gì, cách hoạt động
- ✅ Solidity và EVM
- ✅ Gas Mechanism
- ✅ State Management
- ✅ Security Considerations
- ✅ **BookAsset, BookRental, PolicyEngine** - workflow chi tiết

👉 Để tìm hiểu về **Design Patterns, ERC Standards, NFT, SBT**, xem [Smart Contracts (Nâng cao)](./3-Smart-Contracts-Nang-cao.md)

---

## 🌉 Từ Lý thuyết sang Thực hành

Trong file lý thuyết, bạn đã biết **Smart Contract** (Hợp đồng Thông minh) là những đoạn mã tự động thực thi khi điều kiện được đáp ứng - giống như máy bán hàng tự động.

File này sẽ chuyển từ ẩn dụ sang **thực tế kỹ thuật**: Solidity code, EVM execution, gas optimization, và quan trọng nhất - **phân tích chi tiết 3 smart contracts chính của VinaLib**: BookAsset (NFT đại diện sách), BookRental (logic thuê), và PolicyEngine (tự động duyệt).

Đừng lo nếu thấy code - mỗi đoạn đều có giải thích. Mục tiêu là hiểu **workflow**, không nhất thiết phải nhớ từng dòng code.

---

## 💡 Tại sao VinaLib chọn Solidity và EVM?

**Câu hỏi:** Có nhiều blockchain platforms (Ethereum, Solana, Polkadot, Avalanche...) và nhiều ngôn ngữ (Solidity, Rust, Move...). Tại sao chọn Solidity?

**Trả lời:**

| Yếu tố | Lựa chọn VinaLib | Lý do |
|--------|-----------------|-------|
| **Platform** | **NDAChain** (Testnet: AVAX Fuji) | Tương thích 100% EVM, dev tools phong phú (Hardhat, Remix), cộng đồng lớn. |
| **Ngôn ngữ** | Solidity | Tài liệu phong phú, library (OpenZeppelin) đã kiểm định |
| **EVM** | Deterministic execution | Đảm bảo kết quả giống nhau trên mọi node |
| **Consensus** | Proof of Authority (PoA) | Nhanh, hiệu quả, phù hợp cho trusted validators |

**Quyết định thiết kế:**
- **Development:** Hardhat (local Ethereum network) - test nhanh, không tốn tiền
- **Testnet:** AVAX Fuji Testnet hoặc ndachain testnet - test với điều kiện gần thực tế
- **Mainnet:** NDAChain (hạ tầng DID quốc gia) - Sử dụng PoA consensus cho tốc độ và hiệu quả, gas rẻ

**Tại sao chọn NDAChain với PoA?**
- ✅ **EVM-compatible**: Code Solidity hiện tại chạy ngay không cần sửa
- ✅ **PoA Consensus**: Nhanh hơn PoS/PoW, tốn ít năng lượng, phù hợp cho mạng riêng
- ✅ **Subnet flexibility**: Cho phép tuỳ biến governance và validator set
- ✅ **Gas rẻ**: Chi phí thấp cho người dùng cuối
- ✅ **Throughput cao**: Xử lý nhiều giao dịch song song

**Alternatives không chọn:**
- ❌ Solana (Rust): Nhanh nhưng ecosystem Web3 (Wallet, NFT marketplace) chưa mature
- ❌ Aptos (Move): Quá mới, ít thư viện hỗ trợ
- ❌ Polygon PoS: Tốt nhưng không linh hoạt bằng subnet riêng
- ✅ NDAChain: Cân bằng tối ưu giữa tốc độ, chi phí, và bảo mật DID

---

## 📚 Mục lục

1. [Giới thiệu Smart Contracts](#1-giới-thiệu-smart-contracts)
2. [Smart Contracts vs Traditional Contracts](#2-smart-contracts-vs-traditional-contracts)
3. [Cơ chế hoạt động](#3-cơ-chế-hoạt-động)
4. [Solidity và EVM](#4-solidity-và-evm)
5. [Deployment và Execution](#5-deployment-và-execution)
6. [Gas Mechanism](#6-gas-mechanism)
7. [State Management](#7-state-management)
8. [Security Considerations](#8-security-considerations)
9. [Phân tích Implementation trong VinaLib](#9-phân-tích-implementation-trong-vinalib)
10. [Best Practices và Design Patterns](#10-best-practices-và-design-patterns)

---

## 1. Giới thiệu Smart Contracts

### 👤 Góc nhìn Người dùng

**Smart Contract là gì?**  
Tưởng tượng Smart Contract như một "máy bán hàng tự động" trên blockchain. Bạn bỏ tiền vào (gửi transaction), máy tự động kiểm tra điều kiện, và thực hiện hành động mà không cần người trung gian. Ví dụ: khi bạn thuê sách, hợp đồng tự động giữ tiền cọc và sẽ trả lại khi bạn trả sách.

**Ưu điểm cho người dùng:**
- ✅ **Tự động hóa**: Không cần chờ admin duyệt thủ công
- ✅ **Minh bạch**: Ai cũng có thể xem code và logic của hợp đồng
- ✅ **Không thể thay đổi**: Sau khi deploy, logic không thể bị sửa đổi
- ✅ **Không cần tin tưởng**: Blockchain đảm bảo thực thi đúng như đã lập trình
- ✅ **Tiết kiệm thời gian**: Giao dịch xử lý nhanh (15 giây - vài phút)

**Nhược điểm cần lưu ý:**
- ⚠️ **Tốn phí Gas**: Mỗi thao tác phải trả phí
- ⚠️ **Không thể sửa nếu có bug**: Code bug = mất tiền vĩnh viễn
- ⚠️ **Phức tạp**: Khó hiểu đối với người dùng thông thường
- ⚠️ **Phụ thuộc mạng**: Network congestion → phí cao, chậm

### ⚙️ Góc nhìn Hệ thống

**Smart Contract Ecosystem:**
```
┌─────────────────────────────────────────┐
│   DApps (Web3 Frontend)                 │
├─────────────────────────────────────────┤
│   Web3 Libraries (ethers.js, web3.js)   │
├─────────────────────────────────────────┤
│   JSON-RPC Interface                    │
├─────────────────────────────────────────┤
│   Ethereum Node (Geth, Hardhat)         │
├─────────────────────────────────────────┤
│   EVM (Ethereum Virtual Machine)        │
│   - Executes Bytecode                   │
│   - Manages State                       │
└─────────────────────────────────────────┘
```

## 2. Smart Contracts vs Traditional Contracts

### 👤 Góc nhìn Người dùng

**Traditional Contract (Hợp đồng truyền thống):**
```
Bạn và chủ nhà ký hợp đồng thuê nhà
        ↓
Hợp đồng viết trên giấy
        ↓
Khi tranh chấp → Tòa án giải quyết
        ↓
Mất thời gian: 3-6 tháng, chi phí luật sư cao
```

**Smart Contract:**
```
Bạn và chủ nhà ký "hợp đồng số"
        ↓
Hợp đồng = Code trên blockchain
        ↓
Tranh chấp → Code tự động xử lý theo logic đã lập trình
        ↓
Kết quả: 15 giây, không cần luật sư
```

**Ví dụ cụ thể: Thuê sách**

**Traditional:**
1. Đặt cọc 100K → Chủ nhà giữ tiền
2. Trả sách muộn → Chủ nhà tự ý giữ cọc
3. Bạn phản đối → Phải gọi điện, chat, tranh luận
4. Không đồng ý → Kiện tụng

**Smart Contract:**
1. Gửi 100K vào smart contract (escrow)
2. Trả sách đúng hạn → Contract tự động trả 100K
3. Trả muộn → Contract tự động trừ 10K/ngày
4. Không tranh cãi, minh bạch 100%

### ⚙️ Góc nhìn Hệ thống

**So sánh chi tiết:**

| Tiêu chí | Traditional Contract | Smart Contract |
|----------|---------------------|---------------|
| **Môi trường** | Giấy tờ, luật pháp | Blockchain, code |
| **Thực thi** | Tòa án, cơ quan pháp luật | Tự động bởi EVM |
| **Thời gian** | Tháng, năm | Giây, phút |
| **Chi phí** | Luật sư, tòa án ($$$) | Gas fees ($) |
| **Minh bạch** | Riêng tư, chỉ 2 bên biết | Public, ai cũng thấy |
| **Bất biến** | Có thể sửa đổi/hủy bỏ | Không thể thay đổi (immutable) |
| **Tin tưởng** | Cần tin tưởng bên thứ 3 | Trustless (không cần tin tưởng) |
| **Ngôn ngữ** | Tiếng nói tự nhiên (mơ hồ) | Code (rõ ràng, chính xác) |

**Khái niệm quan trọng:**

Smart Contracts loại bỏ **middleman** (trung gian). Thay vì tin tưởng con người, bạn tin tưởng **toán học** và **cryptography**.

---

## 3. Cơ chế hoạt động

### 👤 Góc nhìn Người dùng

**Quy trình sử dụng Smart Contract:**

```
Step 1: Bạn kết nối ví (MetaMask, Coinbase Wallet)
        ↓
Step 2: DApp hiển thị form (ví dụ: thuê sách)
        ↓
Step 3: Bạn điền thông tin và nhấn "Thuê"
        ↓
Step 4: Ví popup → Xác nhận transaction (+ gas fee)
        ↓
Step 5: Bạn nhấn "Confirm"
        ↓
Step 6: Chờ 15-30 giây
        ↓
Step 7: Nhận thông báo "Thuê thành công!"
```

### ⚙️ Góc nhìn Hệ thống

**Các giai đoạn:**

1. **Transaction Creation**: DApp tạo transaction object với function call data
2. **Signing**: User ký transaction bằng private key
3. **Broadcasting**: Node nhận và broadcast TX đến network
4. **Validation**: Nodes validate signature, nonce, balance
5. **Execution**: EVM execute bytecode
6. **State Update**: Contract state được cập nhật
7. **Confirmation**: Transaction được ghi vào block

---

## 4. Solidity và EVM

### 👤 Góc nhìn Người dùng

**Solidity là gì?**  
Solidity là ngôn ngữ lập trình để viết Smart Contracts. Giống như JavaScript là ngôn ngữ cho website, Solidity là ngôn ngữ cho blockchain.

**EVM là gì?**  
EVM (Ethereum Virtual Machine) như một "máy tính ảo" chạy 24/7 trên hàng nghìn server khắp thế giới. Nó đọc code Solidity và thực hiện chính xác những gì code yêu cầu.

## 5. Deployment và Execution

### 👤 Góc nhìn Người dùng

**Deployment (Triển khai) là gì?**  
Giống như upload một app lên App Store, nhưng thay vì Apple server, bạn upload lên blockchain. Sau khi deploy, contract có địa chỉ cố định (như URL) và ai cũng có thể tương tác.

**Ví dụ:**
```
Contract Address: 0x1234567890abcdef...
→ Giống như: https://example.com
```

### ⚙️ Góc nhìn Hệ thống

**Deployment Process:**

**Execution Flow:**

```
User calls: contract.rentBook(123)
        ↓
1. DApp encodes function call:
   functionSelector = keccak256("rentBook(uint256)").slice(0,4)
   → 0x12345678
   params = ABI.encode(123)
   → 0x000000000000000000000000000000000000000000000000000000000000007b
   
   calldata = 0x12345678 + 0x000000...7b
        ↓
2. Create Transaction:
   {
     to: 0x<contract_address>,
     data: 0x12345678000000...7b,
     value: 100000,
     gas: 300000
   }
        ↓
3. EVM receives TX:
   - Load contract bytecode from storage
   - Jump to function selector 0x12345678
   - Execute opcode sequence
   - Update state
   - Emit events
        ↓
4. Return receipt to user
```

---

## 6. Gas Mechanism

### 👤 Góc nhìn Người dùng

**Gas là gì?**  
Gas như "xăng" để chạy smart contract. Càng phức tạp thì tốn càng nhiều gas. Bạn phải trả gas fee cho miners (thợ mỏ) để họ thực thi code của bạn.

**Ví dụ:**
```
Gửi ETH đơn giản:     21,000 gas   (~$1)
Swap token trên Uniswap: 150,000 gas (~$5)
Mint NFT:               100,000 gas  (~$3)
Deploy contract:        2,000,000 gas (~$50)
```

**Tại sao phải trả gas?**
- Chống spam: Nếu free → ai cũng spam contract → network tê liệt
- Trả công miners: Họ dùng điện + máy tính để xử lý transaction
- Ưu tiên: Gas cao hơn → TX xử lý nhanh hơn

### ⚙️ Góc nhìn Hệ thống

**Gas Calculation:**

```
Total Fee = Gas Used × Gas Price

Example:
Gas Used: 50,000 gas (thực tế tiêu thụ)
Gas Price: 20 gwei (1 gwei = 0.000000001 ETH)
→ Total Fee = 50,000 × 20 gwei = 1,000,000 gwei = 0.001 ETH

If ETH = $2000:
→ Total Fee = $2
```

**Gas Limit vs Gas Used:**

```
Set Gas Limit: 100,000 gas (maximum sẵn sàng trả)
Actual Gas Used: 50,000 gas (thực tế dùng)
→ Refund: 50,000 gas

If Gas Used > Gas Limit:
→ Transaction FAILED (out of gas)
→ Mất hết gas đã set
→ State rollback (không có thay đổi)
```


**Gas Optimization Strategies:**

| Tối ưu hóa | Gas Savings | Example |
|-----------|-------------|---------|
| `uint256` thay vì `uint8` | ~200 gas | Pack variables |
| `calldata` thay vì `memory` | ~300 gas/param | Function params |
| `delete` thay vì `= 0` | Refund 15,000 gas | Clear storage |
| Batch operations | 50-80% savings | Process nhiều items cùng lúc |
| `immutable` variables | 2,100 gas/read | Constants |

---

## 7. State Management

### 👤 Góc nhìn Người dùng

**State là gì?**  
State là "bộ nhớ" của smart contract. Ví dụ: state lưu ai đang thuê sách, số dư của mỗi người, danh sách sách available. State tồn tại vĩnh viễn trên blockchain.

**Ví dụ:**
```
State trước khi thuê sách:
bookRentals[123] = null (không ai thuê)
userBalance[You] = 0

Sau khi thuê:
bookRentals[123] = "Your Address" (bạn đang thuê)
userBalance[You] = 100000 (cọc 100K)
```

### ⚙️ Góc nhìn Hệ thống

**Storage Access Costs:**

```
Operation          | Cold Access | Warm Access | Notes
-------------------|-------------|-------------|-------
SLOAD (read)       | 2,100 gas   | 100 gas     | First vs subsequent
SSTORE (write)     | 20,000 gas  | 2,900 gas   | Zero → Non-zero
SSTORE (update)    | 5,000 gas   | 100 gas     | Non-zero → Non-zero
SSTORE (delete)    | Refund      | Refund      | Non-zero → Zero
```

**Memory vs Storage:**

```
┌───────────────────────────────────────────────┐
│              Memory (Temporary)                │
│  - Wiped after function execution             │
│  - Cheap: 3 gas per 32 bytes                  │
│  - Use for: Temporary calculations            │
└───────────────────────────────────────────────┘

┌───────────────────────────────────────────────┐
│              Storage (Persistent)              │
│  - Permanent on blockchain                    │
│  - Expensive: 20,000 gas per 32 bytes         │
│  - Use for: Contract state (balances, data)   │
└───────────────────────────────────────────────┘

┌───────────────────────────────────────────────┐
│              Calldata (Read-only)              │
│  - Function parameters                        │
│  - Cheapest: No copy, read directly           │
│  - Use for: Large input data                  │
└───────────────────────────────────────────────┘
```

---

## 8. Security Considerations

### 👤 Góc nhìn Người dùng

**Rủi ro khi dùng Smart Contracts:**

1. **Rug Pull**: Developer có thể rút toàn bộ tiền
   - ✅ Cách phòng tránh: Kiểm tra contract có audit không, đọc code
   
2. **Phishing**: Website giả mạo yêu cầu bạn approve token
   - ✅ Cách phòng tránh: Kiểm tra URL kỹ, dùng hardware wallet

3. **Approval Risk**: Bạn approve unlimited token cho contract
   - ✅ Cách phòng tránh: Approve đúng số lượng cần thiết


## 9. Phân tích Implementation trong VinaLib

### 📋 Tổng quan

Hệ thống VinaLib sử dụng **3 Smart Contracts chính** để quản lý toàn bộ quy trình cho thuê sách, từ tạo NFT đại diện cho sách vật lý, đến quản lý tiền cọc và tự động đánh giá chính sách cho thuê. Tất cả được deploy trên Hardhat local network cho môi trường development.

### 🔧 Components

#### 1. **BookAsset.sol (ERC-721 NFT)**

**Vai trò trong hệ thống:**
Contract này biến mỗi cuốn sách vật lý thành một NFT (Non-Fungible Token) độc nhất trên blockchain. Mỗi NFT có token ID riêng và chứa metadata như tên sách, tác giả, và CID (Content Identifier) trỏ tới ảnh bìa sách trên IPFS.

**Chức năng chính:**
- **Tạo NFT**: Khi lender đăng ký sách mới, Admin mint một NFT mới đại diện cho sách đó
- **Lưu trữ thông tin**: Metadata như title, author, ISBN, condition được lưu on-chain hoặc link tới IPFS
- **Theo dõi ownership**: Blockchain tự động ghi lại ai là chủ sở hữu hiện tại của cuốn sách
- **Transfer history**: Mọi lần chuyển giao quyền sở hữu đều được ghi lại vĩnh viễn

**Tại sao dùng NFT?**
Vì mỗi cuốn sách là duy nhất (có thể có tình trạng khác nhau, chủ sở hữu khác nhau), nên dùng NFT (non-fungible) thay vì ERC-20 token (fungible). Bạn không thể đổi sách ID #1 lấy sách ID #2 theo tỷ lệ 1:1 như với tiền tệ.

#### 2. **BookRental.sol (Rental Logic)**

**Vai trò trong hệ thống:**
Contract này quản lý toàn bộ lifecycle của một lần cho thuê, từ khi user đặt cọc cho đến khi trả sách và nhận lại tiền.

**Chức năng chính:**
- **Quản lý trạng thái cho thuê**: Mỗi rental có các trạng thái: PENDING (chờ duyệt) → ACTIVE (đang thuê) → RETURNED (đã trả) → COMPLETED (hoàn tất)
- **Escrow deposits**: Giữ tiền cọc an toàn trong contract. Không Admin hay Owner nào có thể rút tiền trước khi rental hoàn tất
- **Tính toán phí trễ hạn**: Tự động tính phí nếu user trả sách muộn (ví dụ: 10,000 wei/ngày)
- **Refund logic**: Sau khi xác nhận trả sách, contract tự động tính: Refund = Deposit - LateFee, rồi chuyển về cho renter

**Workflow chi tiết:**

**Bước 1 - Tạo Rental:**
- User chọn sách và gửi transaction kèm tiền cọc (ví dụ: 0.01 ETH)
- Contract tạo rental record mới với status = PENDING
- Tiền cọc được lock trong contract (escrow)
- Contract emit event `RentalCreated` để frontend biết và hiển thị

**Bước 2 - Approval:**
- Contract gọi PolicyEngine để đánh giá request
- PolicyEngine xem xét trust score của user, book tier, và deposit amount
- Trả về quyết định: AUTO (tự động duyệt) / MANUAL (cần admin review) / REJECT (từ chối)

**Bước 3 - Active:**
- Nếu AUTO hoặc admin approve: Status chuyển thành ACTIVE
- User được phép đến lấy sách vật lý
- Thời gian bắt đầu được ghi lại (startTime = block.timestamp)

**Bước 4 - Return:**
- User mang sách đến trả
- Admin kiểm tra tình trạng vật lý và gọi function `returnBook(rentalId)`
- Contract tính số ngày thuê: `daysRented = (block.timestamp - startTime) / 86400`
- Nếu `daysRented > agreedDuration`: Tính late fee
- Chuyển tiền refund về cho user
- Status → RETURNED hoặc COMPLETED

**Bước 5 - Trust Score Update:**
- Nếu trả đúng hạn và sách không hư: Tăng trust score
- Nếu trả muộn hoặc sách hư hại: Giảm trust score hoặc giữ nguyên
- Trust score ảnh hưởng đến lần thuê sau (higher score = auto-approve dễ hơn)

#### 3. **PolicyEngine.sol (Auto-Approval Logic)**

**Vai trò trong hệ thống:**
Contract này tự động hóa quyết định có nên approve hay reject một rental request, giảm thiểu công việc thủ công cho admin.

**Logic đánh giá:**

**Input factors:**
- **User Trust Score**: Điểm tín nhiệm của người thuê (0-100)
- **Book Tier**: Độ quý của sách (A = rare/expensive, B = standard, C = common)
- **Deposit Amount**: Số tiền cọc user đặt
- **Rental Duration**: Thời gian thuê (ngày)

**Decision Tree:**

**Tier A books (quý hiếm):**
- Trust Score ≥ 80 + Deposit ≥ bookValue × 1.5 → AUTO
- Trust Score 50-79 + Deposit ≥ bookValue × 2.0 → MANUAL
- Trust Score < 50 hoặc Deposit không đủ → REJECT

**Tier B books (thông thường):**
- Trust Score ≥ 60 + Deposit ≥ bookValue → AUTO
- Trust Score 40-59 + Deposit ≥ bookValue × 1.5 → MANUAL
- Else → REJECT

**Tier C books (phổ biến):**
- Trust Score ≥ 40 + Deposit ≥ bookValue × 0.5 → AUTO
- Else → MANUAL

**Ví dụ cụ thể:**
```
User A: Trust Score = 85, thuê sách Tier A giá $50, đặt cọc $100
→ PolicyEngine: AUTO (vì 85 ≥ 80 && $100 ≥ $50 × 1.5)

User B: Trust Score = 65, thuê sách Tier A giá $50, đặt cọc $80
→ PolicyEngine: MANUAL (vì 65 < 80, cần admin review)

User C: Trust Score = 30, thuê sách Tier C giá $10, đặt cọc $5
→ PolicyEngine: MANUAL (vì trust score thấp)
```

### 📊 Complete Data Flow

**Toàn bộ quy trình từ đầu đến cuối:**

**Phase 1: Initiation (User bắt đầu)**
1. User mở DApp, browse sách có sẵn
2. Click nút "Thuê sách" cho Book ID #123
3. Chọn duration: 7 ngày
4. Frontend tính tiền cọc cần thiết: bookPrice × tierMultiplier = 0.01 ETH
5. User xác nhận transaction trong wallet (MetaMask popup)

**Phase 2: On-Chain Execution**
6. Transaction được broadcast lên Hardhat network
7. BookRental contract nhận call: `createRental(bookId: 123, duration: 7 days)`
8. Contract lock 0.01 ETH vào escrow
9. Tạo rental record:
   - rentalId: auto-increment ID
   - renter: msg.sender (địa chỉ user)
   - bookId: 123
   - deposit: 0.01 ETH
   - startTime: chưa set (chờ approve)
   - duration: 7 days
   - status: PENDING

**Phase 3: Policy Evaluation**
10. Contract gọi PolicyEngine: `decideApproval(userAddress, bookId, deposit)`
11. PolicyEngine query user's trust score from off-chain database (qua backend API)
12. Query book tier from BookAsset contract
13. Chạy decision tree logic
14. Return: AUTO / MANUAL / REJECT

**Phase 4: Approval & Activation**
15a. Nếu AUTO:
    - BookRental tự động gọi `_approveRental(rentalId)`
    - Set status = ACTIVE
    - Set startTime = block.timestamp
    - Emit event: `RentalApproved(rentalId, AUTO)`

15b. Nếu MANUAL:
    - Emit event: `RentalPendingReview(rentalId)`
    - Backend notify admin
    - Admin review qua admin dashboard
    - Admin gọi `approveRental(rentalId)` hoặc `rejectRental(rentalId)`

15c. Nếu REJECT:
    - Refund 0.01 ETH về cho user
    - Delete rental record
    - Emit event: `RentalRejected(rentalId, reason)`

**Phase 5: Physical Handover (Off-chain)**
16. User đến library với rental ID
17. Staff verify rental status = ACTIVE
18. Mở IoT smart lock (tích hợp Tuya)
19. User lấy sách

**Phase 6: Return Process**
20. User mang sách đến trả (7 ngày sau)
21. Admin kiểm tra tình trạng sách
22. Admin gọi: `returnBook(rentalId, isDamaged: false)`
23. Contract tính:
    - actualDuration = (now - startTime) / 86400 = 7 days
    - lateFee = 0 (vì đúng hạn)
    - refund = deposit - lateFee = 0.01 ETH
24. Transfer 0.01 ETH từ contract về user
25. Set status = RETURNED
26. Emit event: `BookReturned(rentalId, refund: 0.01 ETH)`

**Phase 7: Trust Score Update**
27. Backend listener bắt event `BookReturned`
28. Call PolicyEngine: `recordSuccessfulReturn(userAddress)`
29. Increase trust score: 65 → 70
30. Lưu history vào database để tracking

### 🎯 Ưu điểm và Hạn chế

#### ✅ Ưu điểm hiện tại:

1. **Minh bạch tuyệt đối**: 
   - Mọi logic rental đều public trên blockchain
   - User có thể verify contract code trước khi sử dụng
   - Không ai có thể manipulate rules sau khi deploy

2. **Tự động hóa thông minh**:
   - PolicyEngine giảm 70-80% workload cho admin
   - Chỉ cần review manually các case biên
   - Giải phóng admin focus vào customer service

3. **Immutability**:
   - Logic không thể bị thay đổi bởi admin hay hacker
   - User tin tưởng vào code, không phải con người
   - Tránh được "rule changes" đột ngột

4. **Escrow an toàn**:
   - Tiền cọc lock trong smart contract, không ai withdraw được trước hạn
   - Không risk "admin cầm tiền bỏ trốn"
   - Math-based refund calculation (không thể tranh cãi)

5. **Event-driven architecture**:
   - Frontend realtime updates qua event subscriptions
   - Backend có thể trigger workflows dựa trên events
   - Easy integration với monitoring systems

#### ⚠️ Hạn chế cần cải thiện:

1. **Gas costs**:
   - Hiện tại: Hardhat local (free gas)
   - Mainnet: Mỗi transaction tốn $5-50 (quá đắt cho rental nhỏ)
   - Solution: Deploy lên Layer 2 như Polygon (gas < $0.01)

2. **Immutability challenges**:
   - Nếu tìm thấy bug trong logic → Phải redeploy contract mới
   - Redeploy = mất hết data cũ, user phải migrate
   - Solution: Implement upgradeable proxy pattern (OpenZeppelin)

3. **Scalability**:
   - Hardhat: 1 máy local, không có load
   - Mainnet: 15 TPS, congestion khi busy
   - Solution: L2 scaling solutions

4. **Admin dependency**:
   - Vẫn cần admin confirm physical return
   - Không thể fully autonomous vì sách là vật lý
   - Solution: IoT sensors + Chainlink Oracles để auto-verify return

5. **Oracle problem**:
   - Trust score hiện lưu off-chain (database)
   - Contract phải trust backend API
   - Solution: Migrate trust score lên Chainlink DON (decentralized)

### 🚀 Migration Path (Roadmap)

**Phase 1: Testnet Deployment (Q2 2024)**
- Deploy contracts lên AVAX Fuji Testnet hoặc ndachain testnet
- Test với real gas costs và network latency
- Cấu hình PoA validator set cho môi trường test
- Invite 50 beta testers để stress test
- Fix bugs trước khi lên mainnet

**Phase 2: Subnet/ndachain Setup (Q3 2024)**
- Migrate lên NDAChain Mainnet khi platform mở public access
- Cấu hình PoA consensus với trusted validators
- Gas cost tối ưu hóa (mục tiêu < $0.01 per transaction)
- Update frontend RPC endpoints
- Implement monitoring và alert systems

**Phase 3: Decentralized Oracle (Q4 2024)**
- Tích hợp Chainlink Automation thay thế admin manual approve
- Chainlink Functions để calculate trust score on-chain
- IoT integration qua Chainlink External Adapters
- Giảm admin dependency xuống 20%

**Phase 4: DAO Governance (2025)**
- Transfer ownership từ EOA sang DAO multi-sig
- Community vote on policy changes (book tiers, fee structure)
- Decentralized dispute resolution mechanism
- Fully autonomous system

### 💡 Best Practices đang áp dụng

**✅ Đã implement:**
1. **Access Control**: Sử dụng OpenZeppelin's Ownable/AccessControl cho role management
2. **Reentrancy Guard**: Protect withdrawal functions với ReentrancyGuard modifier
3. **Comprehensive Events**: Emit events cho mọi state changes để frontend/backend track được
4. **Input Validation**: require() checks đầy đủ trước khi process bất kỳ logic nào
5. **Checks-Effects-Interactions Pattern**: Update state trước khi gọi external calls

**⚠️ Cần cải thiện:**
1. **Gas Optimization**: Storage packing chưa tối ưu (có thể giảm 20-30% gas)
2. **Upgradeability**: Chưa implement TransparentUpgradeableProxy pattern
3. **Formal Verification**: Chưa có formal proofs cho critical functions
4. **Circuit Breaker**: Chưa có mechanism để pause contracts trong emergency
5. **Rate Limiting**: Chưa có protection chống spam transactions



---

## 10. Best Practices và Design Patterns

### 👤 Góc nhìn Người dùng

**Làm sao biết một Smart Contract an toàn?**

1. ✅ **Audit Report**: Có audit từ CertiK, OpenZeppelin, Quantstamp
2. ✅ **Open Source**: Code public trên Etherscan, GitHub
3. ✅ **Timelock**: Admin không thể thay đổi ngay lập tức (có delay 24-48h)
4. ✅ **Multi-sig**: Cần nhiều chữ ký để thực hiện thay đổi quan trọng
5. ✅ **Bug Bounty**: Có chương trình thưởng cho người tìm bug

## Phụ lục: Công cụ và Tài nguyên

### Development Tools

- **Hardhat**: Development environment, testing, deployment
- **Remix IDE**: Online Solidity IDE với debugger
- **OpenZeppelin Contracts**: Secure, audited contract libraries
- **Etherscan**: Block explorer và contract verification
- **Tenderly**: Advanced debugging và monitoring

### Useful Commands

```bash
# Initialize Hardhat project
npx hardhat init

# Compile contracts
npx hardhat compile

# Run tests
npx hardhat test

# Deploy to local network
npx hardhat run scripts/deploy.js --network localhost

# Start local node
npx hardhat node

# Verify contract on Etherscan
npx hardhat verify --network mainnet <CONTRACT_ADDRESS> <CONSTRUCTOR_ARGS>
```

### Audit Resources

- **Slither**: Static analysis tool by Trail of Bits
- **Mythril**: Security analysis tool
- **Manticore**: Symbolic execution tool
- **CertiK**: Professional audit service
- **OpenZeppelin Defender**: Security monitoring

---

## Tổng kết

Smart Contracts mang lại sự thay đổi căn bản trong cách thực thi thỏa thuận:

**Từ Centralized → Decentralized**  
**Từ Trust-based → Cryptographically enforced**  
**Từ Manual → Automated**  
**Từ Mutable → Immutable**

Trong VinaLib:
- ✅ **Hiện tại**: 3 contracts (BookAsset, BookRental, PolicyEngine) trên Hardhat local
- ✅ **Lợi ích**: Tự động hóa rental flow, escrow an toàn, policy engine transparent
- 🎯 **Tương lai**: Deploy lên NDAChain Mainnet, integrate Chainlink Oracle, implement DAO governance

**Lưu ý quan trọng:**  
Smart Contracts **không thể sửa** sau khi deploy. Testing và audit kỹ lưỡng là **bắt buộc** trước khi lên mainnet. Bugs trong contract = mất tiền vĩnh viễn.
