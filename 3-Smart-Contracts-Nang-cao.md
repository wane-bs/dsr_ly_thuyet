---
layout: default
title: Smart Contracts (Nâng cao)
---

# Smart Contracts Nâng cao: Patterns & Standards
## Tài liệu Chuyên sâu về Ownable, Pausable, ERC Standards, NFT, SBT, và OOP trong Solidity

> 📍 **Vị trí trong bộ tài liệu**: 3/5 - Logic cốt lõi (Nâng cao)  
> ⬅️ [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md) | ➡️ [Chainlink](./4-Chainlink.md)

### 📎 Tài liệu Liên quan
| Liên kết đến | Mối quan hệ |
|-------------|-------------|
| [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md) | Khái niệm nền tảng: EVM, Gas, Workflow |
| [Chainlink](./4-Chainlink.md) | VinaLibVault kế thừa FunctionsClient, AutomationCompatible |
| [IPFS](./1-IPFS.md) | NFT metadata được lưu trên IPFS |

### 📌 Phạm vi Tài liệu này
Tài liệu này bao gồm các **design patterns và standards nâng cao**:
- ✅ **Ownable Pattern** - Access control
- ✅ **Pausable Pattern** - Emergency stop
- ✅ **ERC Standards** - ERC-20, ERC-721, ERC-1155, ERC-4907
- ✅ **NFT** - Non-Fungible Tokens
- ✅ **SBT** - Soulbound Tokens
- ✅ **Inheritance** - OOP trong Solidity
- ✅ **RentalAgreementSBT, SuChinToken, VinaLibVault** - contracts nâng cao

👉 Để tìm hiểu về **khái niệm cơ bản, workflow thuê sách**, xem [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md)

---

## 🌉 Từ Cơ bản sang Nâng cao

Trong file trước, bạn đã hiểu **cách viết một Smart Contract** và workflow cụ thể của BookAsset, BookRental, PolicyEngine.

File này sẽ nâng level: thay vì viết mọi thứ từ đầu, bạn sẽ học cách **sử dụng các tiêu chuẩn (Standards) và mẫu thiết kế (Patterns) đã được kiểm định** từ OpenZeppelin. Điều này giống như xây nhà: thay vì tự đúc gạch, bạn dùng gạch chất lượng cao đã được kiểm định an toàn.

Bạn sẽ thấy: ERC-721 (NFT standard), ERC-4907 (Rentable NFT), Ownable (access control), Pausable (emergency stop), và SBT (non-transferable credentials).

---

## 💡 Tại sao chọn OpenZeppelin và các Patterns này?

**Câu hỏi:** Tại sao không tự viết access control, token standards từ đầu? Tại sao phải follow các patterns?

**Trả lời:**

| Vấn đề | Tự viết từ đầu | Dùng OpenZeppelin + Patterns |
|--------|----------------|------------------------------|
| **Security** | ⚠️ Dễ mắc lỗi (reentrancy, overflow...) | ✅ Đã audit bởi hàng nghìn developers |
| **Development time** | ❌ Mất hàng tuần | ✅ Chỉ mất vài giờ (import và inherit) |
| **Interoperability** | ❌ Wallet/Marketplace không hiểu custom format | ✅ Tất cả tools nhận ra ERC standards |
| **Upgradability** | ❌ Khó maintain | ✅ Community support, bug fixes liên tục |

**Quyết định thiết kế VinaLib:**
- **ERC-721**: Sách là NFT duy nhất → Dùng chuẩn OpenZeppelin ERC721
- **ERC-4907**: Cần cho thuê NFT → Extend với Rentable interface
- **Ownable**: Admin cần quyền pause/unpause → Inherit Ownable pattern
- **SBT (Soulbound)**: Rental agreement không thể bán → Custom implementation non-transferable

**Lợi ích cụ thể:**
- Tiết kiệm ~2000 dòng code (và vô số bugs tiềm ẩn)
- MetaMask tự động nhận diện BookAsset NFT
- OpenSea có thể list sách (nếu muốn)
- Audit dễ dàng hơn (auditor đã quen với OpenZeppelin)

---

## 📚 Mục lục

1. [Layer 1 vs Layer 2 Blockchains](#0-layer-1-vs-layer-2-blockchains)
2. [Ownable Pattern](#1-ownable-pattern)
3. [Pausable Pattern](#2-pausable-pattern)
4. [ERC Standards](#3-erc-standards)
5. [NFT (Non-Fungible Token)](#4-nft-non-fungible-token)
6. [Soulbound Token (SBT)](#5-soulbound-token-sbt)
7. [Inheritance trong Solidity](#6-inheritance-trong-solidity)
8. [Dependency Management](#7-dependency-management)
9. [Phân tích Implementation trong VinaLib](#8-phân-tích-implementation-trong-vinalib)

---

## 0. Layer 1 vs Layer 2 Blockchains

### 👤 Góc nhìn Người dùng

**Layer 1 (L1) và Layer 2 (L2) là gì?**

Hãy tưởng tượng hệ thống giao thông:
- **Layer 1** = Đường cao tốc chính (Ethereum mainnet) - An toàn nhất, nhưng đông đúc và phí đường cao
- **Layer 2** = Đường vòng song song - Nhanh hơn, rẻ hơn, nhưng vẫn kết nối với đường chính

```
Ví dụ thực tế:
Bạn muốn gửi tiền cho bạn bè:

Layer 1 (Ethereum):
💰 Chi phí: $5-50 per transaction (tùy congestion)
⏱️ Thời gian: 12-15 giây/block
✅ Bảo mật: Cao nhất (secured by full Ethereum network)

Layer 2 (Polygon):
💰 Chi phí: $0.001-0.01 per transaction (rẻ hơn 1000x)
⏱️ Thời gian: 2 giây/block
✅ Bảo mật: Cao (periodically anchored to Ethereum)
```

### ⚙️ Góc nhìn Hệ thống

**Layer 1 (L1) Blockchains:**

Layer 1 là blockchain cơ sở, tự thực thi consensus và xử lý toàn bộ transactions on-chain.

| L1 Chain | Consensus | TPS | Avg Gas Cost | Smart Contract |
|----------|-----------|-----|--------------|----------------|
| **Ethereum** | Proof of Stake | ~15-30 | $1-50 | ✅ Solidity (EVM) |
| **Bitcoin** | Proof of Work | ~7 | $0.5-5 | ❌ Limited scripting |
| **Binance Smart Chain (BSC)** | Proof of Staked Authority | ~60-100 | $0.2-1 | ✅ Solidity (EVM) |
| **Avalanche** | Avalanche Consensus | ~4500 | $0.01-0.5 | ✅ Solidity (EVM) |
| **Solana** | Proof of History + PoS | ~3000-5000 | $0.00025 | ✅ Rust (SVM) |
| **NDAChain (PoA)** | Proof of Authority (tuỳ biến) | ~5000+ | < $0.01 | ✅ Solidity (EVM) |

**Đặc điểm Layer 1:**
- ✅ **Fully Decentralized**: Mọi transaction được validate bởi toàn bộ network
- ✅ **Maximum Security**: Không phụ thuộc vào layer khác
- ❌ **Scalability Trilemma**: Khó đạt được cả 3: Decentralization, Security, Scalability
- ❌ **High Gas Fees**: Càng nhiều người dùng → gas càng đắt

**Layer 2 (L2) Solutions:**

Layer 2 xử lý transactions off-chain hoặc parallel, sau đó gửi "proof" lên Layer 1 để finalize.

**Các loại L2:**

**1. Sidechains** (Polygon PoS)
```
Architecture:
┌─────────────────────────────────────┐
│  Ethereum L1 (Settlement Layer)     │ ← Checkpoints mỗi 30 phút
└──────────────┬──────────────────────┘
               │ Bridge
┌──────────────▼──────────────────────┐
│  Polygon PoS (Independent Chain)    │ ← Consensus riêng (100+ validators)
│  - Fast blocks (2s)                 │
│  - Low fees ($0.001)                │
│  - EVM compatible                   │
└─────────────────────────────────────┘
```

**2. Rollups** (Optimism, Arbitrum, zkSync)
```
Optimistic Rollups (Optimism, Arbitrum):
- Assume transactions valid by default
- 7-day challenge period for fraud proofs
- Gas: ~5-10x cheaper than L1

zkRollups (zkSync, StarkNet):
- Use zero-knowledge proofs (zk-SNARKs)
- Instant finality (no challenge period)
- Gas: ~50-100x cheaper than L1
```

**So sánh Layer 2:**

| L2 Solution | Type | TPS | Finality | Gas Cost (vs L1) | EVM Compatible |
|-------------|------|-----|----------|------------------|----------------|
| **Polygon PoS** | Sidechain | ~7000 | ~2s (soft), 30min (hard) | 1000x cheaper | ✅ 100% |
| **Optimism** | Optimistic Rollup | ~2000 | 7 days | 5-10x cheaper | ✅ 100% |
| **Arbitrum** | Optimistic Rollup | ~4000 | 7 days | 5-10x cheaper | ✅ 100% |
| **zkSync Era** | zkRollup | ~2000 | Minutes | 50-100x cheaper | ⚠️ ~95% (some differences) |
| **StarkNet** | zkRollup | ~3000 | Minutes | 50-100x cheaper | ❌ Cairo (different VM) |

---

### 💡 Tại sao VinaLib chọn NDAChain với PoA?

> [!NOTE]
> **Quyết định triển khai:** VinaLib là **DApp trên NDAChain** - hạ tầng DID quốc gia sử dụng **Proof of Authority (PoA)** để tối ưu hóa chi phí, tốc độ, và bảo mật.
>
> Xem chi tiết: [VinaLib Deployment Strategy](./VinaLib-Deployment-Strategy.md)

**Quyết định chiến lược:**

| Yếu tố | Ethereum L1 | NDAChain (PoA) |
|--------|-------------|----------------------------|
| **Gas Cost** | $5-50/tx | < $0.01/tx |
| **Tốc độ** | 15s/block | < 2s/block (PoA) |
| **UX** | ❌ User phải đợi lâu, trả phí cao | ✅ Nhanh, chi phí gần như 0 |
| **Customization** | ❌ Không thể tuỳ biến | ✅ Tuỳ biến governance, consensus |
| **Consensus** | PoS | PoA (nhanh, hiệu quả) |

**Kịch bản thực tế trong VinaLib:**

```
Workflow thuê sách trên Ethereum L1:
1. User approve ERC-20 token → Gas: $10
2. Create rental request → Gas: $15
3. Policy check + escrow deposit → Gas: $20
4. Mint RentalAgreementSBT → Gas: $12
5. Emit event cho IoT → Gas: $8
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: $65 chỉ để THUÊ MỘT CUỐN SÁCH! ❌

Workflow thuê sách trê NDAChain với PoA:
1. User approve → Gas: < $0.001
2. Create rental → Gas: < $0.001
3. Policy + escrow → Gas: < $0.002
4. Mint SBT → Gas: < $0.001
5. Emit event → Gas: < $0.001
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Total: < $0.006 cho toàn bộ flow! ✅
```

**Trade-offs được chấp nhận:**

| Aspect | L1 Ethereum | NDAChain (PoA) | VinaLib Accept? |
|--------|-------------|----------------------------|-----------------|
| **Decentralization** | ~500k validators | 5-20 trusted validators | ✅ Yes (PoA phù hợp cho trusted consortium) |
| **Security** | Maximum | High (validators có reputation stake) | ✅ Yes (sách không phải DeFi triệu $) |
| **Permissionless** | Ai cũng validate được | Chỉ authorized validators | ✅ Yes (trade-off cho tốc độ & cost) |
| **Finality** | 12-15s probabilistic | < 2s deterministic | ✅ Yes (finality nhanh hơn, rõ ràng hơn) |
| **Flexibility** | Cố định | Hoàn toàn tuỳ biến | ✅ Yes (điều chỉnh theo nhu cầu VinaLib) |

**Lộ trình Deploy của VinaLib:**

```
Phase 1: Development
├─ Hardhat Local Network (EVM simulation)
└─ Gas: FREE (local testing)

Phase 2: Testnet
├─ AVAX Fuji Testnet - Testing subnet behavior
├─ hoặc ndachain testnet - Testing custom chain
└─ Gas: FREE (testnet faucets)

Phase 3: Production (Target)
├─ NDAChain Mainnet (Target Platform)
├─ Reasons:
│  ✅ EVM 100% compatible (code Solidity chạy ngay)
│  ✅ PoA consensus: Fast, efficient, low cost
│  ✅ Tuỳ biến governance: Control validator set
│  ✅ Gas ~1000x+ rẻ hơn Ethereum
│  ✅ Subnet isolation: Không bị ảnh hưởng traffic khác
│  ✅ ndachain option: Blockchain riêng 100%, kiểm soát hoàn toàn
└─ Validator Setup: 5-10 trusted nodes (libraries, partners, VinaLib team)
```

**Tích hợp và Onboarding:**

**VinaLib không cần Bridge vì:**
- Toàn bộ ecosystem (BookAsset, SuChinToken, rentals) đều trên subnet/ndachain
- User nhận AVAX hoặc native token để trả gas  
- Onboarding đơn giản: User connect wallet và nhận gas token từ faucet hoặc buy on CEX

---

### 📊 So sánh Chi phí Thực tế

**Deployment Cost (VinaLib Full Stack):**

| Contract | Ethereum L1 | NDAChain (PoA) | Savings |
|----------|-------------|----------------------------|---------|
| BookAsset (ERC-721) | $150 | < $0.10 | 99.9%+ |
| BookRental | $200 | < $0.10 | 99.9%+ |
| PolicyEngine | $180 | < $0.10 | 99.9%+ |
| RentalAgreementSBT | $120 | < $0.08 | 99.9%+ |
| SuChinToken (ERC-20) | $100 | < $0.05 | 99.9%+ |
| VinaLibVault (Chainlink) | $250 | < $0.15 | 99.9%+ |
| **TOTAL** | **$1000** | **< $0.58** | **99.94%+** |

**Ongoing Operations (per month, 1000 users):**

| Operation | Frequency | L1 Cost | Subnet/ndachain Cost |
|-----------|-----------|---------|-----------------------|
| Mint BookAsset NFT | 100/month | $1200 | < $1 |
| Create Rental | 500/month | $7500 | < $3 |
| Return Book | 500/month | $5000 | < $3 |
| Mint SBT | 500/month | $6000 | < $2.5 |
| **TOTAL** | - | **$19,700/month** | **< $9.5/month** |

**Kết luận:** Triển khai trên NDAChain với PoA consensus cho phép VinaLib hoạt động hoàn toàn sustainable với chi phí gần như bằng 0, đồng thời tận dụng hạ tầng DID quốc gia để xác thực người dùng.

---

## 1. Ownable Pattern

### 👤 Góc nhìn Người dùng

**Ownable là gì?**  
Ownable là pattern phổ biến nhất để quản lý quyền Admin trong smart contract. Giống như "chìa khóa chủ" của một căn nhà - chỉ chủ nhà (owner) mới có quyền thực hiện các thao tác quan trọng như thay đổi cấu hình, pause contract, hay mint token.

**Ví dụ thực tế:**
```
Smart Contract = Cửa hàng online

❌ Không có Ownable:
   Bất kỳ ai cũng có thể:
   - Thay đổi giá sản phẩm
   - Rút tiền từ vault
   - Pause toàn bộ hệ thống
   → CHAOS!

✅ Với Ownable:
   Chỉ Owner (chủ cửa hàng) mới có thể:
   - Cập nhật giá
   - Withdraw funds
   - Thêm/xóa sản phẩm
   → SECURE & CONTROLLED
```

**Security Considerations:**

```
⚠️ Risks:
1. Single Point of Failure: Nếu owner mất private key → Không thể quản lý contract
2. Centralization: Owner có quyền tuyệt đối (không decentralized)
3. Malicious Owner: Owner có thể rug pull (rút tiền, thay đổi logic)

✅ Mitigations:
1. Multi-sig Wallet: Dùng Gnosis Safe làm owner (cần 3/5 signatures)
2. Timelock: Thay đổi quan trọng phải đợi 24-48h (cho user time to exit)
3. Ownable2Step: Yêu cầu newOwner accept transfer (tránh typo địa chỉ)
4. Role-based Access Control: Dùng AccessControl thay vì Ownable đơn giản
```

**Ownable vs AccessControl:**

| Feature | Ownable | AccessControl |
|---------|---------|---------------|
| **Complexity** | Simple (1 owner) | Advanced (multiple roles) |
| **Flexibility** | Low | High |
| **Gas Cost** | Lower | Higher |
| **Use Case** | Small projects, single admin | Large projects, team management |
| **Example Roles** | owner | ADMIN, MINTER, PAUSER, UPGRADER |

---

## 2. Pausable Pattern

### 👤 Góc nhìn Người dùng

**Pausable là gì?**  
Pausable giống "nút dừng khẩn cấp" (emergency stop button). Khi phát hiện lỗi hoặc bị tấn công, admin có thể tạm dừng toàn bộ hoặc một phần chức năng của contract để bảo vệ người dùng.

**Ví dụ thực tế:**
```
Tình huống: Phát hiện lỗ hổng security trong contract

Không có Pausable:
❌ Hacker exploit liên tục
❌ Mất tiền không ngừng
❌ Phải đợi deploy contract mới (mất vài giờ)

Có Pausable:
✅ Admin gọi pause() ngay lập tức
✅ Tất cả transfers/mints bị block
✅ Hacker không thể exploit thêm
✅ Team có thời gian fix bug và deploy v2
✅ Sau đó unpause() hoặc migrate
```

## 3. ERC Standards

### 👤 Góc nhìn Người dùng

**ERC là gì?**  
ERC (Ethereum Request for Comments) là các tiêu chuẩn kỹ thuật cho smart contracts trên Ethereum. Giống như USB Type-C là chuẩn chung cho cáp sạc - bất kỳ thiết bị nào cũng có thể cắm được.

**Tại sao cần standards?**

```
Không có standards:
❌ Mỗi token có function names khác nhau:
   TokenA: sendTokens()
   TokenB: transferCoins()
   TokenC: move()
   → Wallet/Exchange phải code riêng cho từng token → Impossible!

Có standards (ERC-20):
✅ Tất cả đều dùng: transfer()
   → MetaMask, Uniswap, Binance... support tất cả ERC-20
   → Plug & Play ecosystem
```

### ⚙️ Góc nhìn Hệ thống

**Major ERC Standards:**

#### ERC-20: Fungible Tokens

```solidity
interface IERC20 {
    // Total supply
    function totalSupply() external view returns (uint256);
    
    // Balance queries
    function balanceOf(address account) external view returns (uint256);
    
    // Transfer
    function transfer(address to, uint256 amount) external returns (bool);
    
    // Allowance mechanism
    function allowance(address owner, address spender) 
        external view returns (uint256);
    function approve(address spender, uint256 amount) 
        external returns (bool);
    function transferFrom(address from, address to, uint256 amount) 
        external returns (bool);
    
    // Events
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
```

**Use Cases:** Cryptocurrencies, stablecoins, governance tokens, utility tokens

**Examples:** USDT, LINK, UNI, AAVE

#### ERC-721: Non-Fungible Tokens (NFTs)

```solidity
interface IERC721 {
    // Balance & ownership
    function balanceOf(address owner) external view returns (uint256);
    function ownerOf(uint256 tokenId) external view returns (address);
    
    // Transfer ownership
    function transferFrom(address from, address to, uint256 tokenId) external;
    function safeTransferFrom(address from, address to, uint256 tokenId) external;
    function safeTransferFrom(
        address from, 
        address to, 
        uint256 tokenId, 
        bytes calldata data
    ) external;
    
    // Approval
    function approve(address to, uint256 tokenId) external;
    function getApproved(uint256 tokenId) external view returns (address);
    function setApprovalForAll(address operator, bool approved) external;
    function isApprovedForAll(address owner, address operator) 
        external view returns (bool);
    
    // Events
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
}
```

**Key Difference from ERC-20:**
```
ERC-20: Fungible (1 USDT = 1 USDT)
ERC-721: Non-Fungible (NFT #1 ≠ NFT #2)
```

**Use Cases:** Art NFTs, gaming items, real estate, digital identity

#### ERC-1155: Multi-Token Standard

```solidity
interface IERC1155 {
    // Can represent both fungible and non-fungible in one contract
    function balanceOf(address account, uint256 id) 
        external view returns (uint256);
    function balanceOfBatch(address[] calldata accounts, uint256[] calldata ids) 
        external view returns (uint256[] memory);
    
    // Batch transfers (gas efficient)
    function safeTransferFrom(
        address from,
        address to,
        uint256 id,
        uint256 amount,
        bytes calldata data
    ) external;
    
    function safeBatchTransferFrom(
        address from,
        address to,
        uint256[] calldata ids,
        uint256[] calldata amounts,
        bytes calldata data
    ) external;
}
```

**Use Cases:** Gaming (multiple item types), fractional NFTs

**Example:** Enjin gaming assets

#### ERC-4907: Rentable NFTs

```solidity
interface IERC4907 {
    // Separate owner and user
    event UpdateUser(
        uint256 indexed tokenId, 
        address indexed user, 
        uint64 expires
    );
    
    function setUser(uint256 tokenId, address user, uint64 expires) external;
    function userOf(uint256 tokenId) external view returns (address);
    function userExpires(uint256 tokenId) external view returns (uint256);
}
```

**Innovation:** Allows NFT rental without transferring ownership

**Use Cases:** 
- Virtual land rental (Metaverse)
- Gaming item rental
- **VinaLib book rental** ← Current project!

**Comparison Table:**

| Standard | Type | Use Case | Example |
|----------|------|----------|---------|
| **ERC-20** | Fungible | Currencies, tokens | USDT, LINK |
| **ERC-721** | Non-Fungible | Unique items | CryptoPunks, Bored Apes |
| **ERC-1155** | Multi-token | Gaming, collections | Enjin, Sandbox |
| **ERC-4907** | Rentable NFT | Rentals, subscriptions | Metaverse land, books |
| **ERC-777** | Advanced Fungible | Hooks, operators | (Less adopted) |
| **ERC-2981** | NFT Royalty | Creator fees | Marketplaces |

---

## 4. NFT (Non-Fungible Token)

### 👤 Góc nhìn Người dùng

**NFT là gì?**  
NFT là "chứng chỉ sở hữu kỹ thuật số" duy nhất, không thể thay thế bằng cái khác. Giống như bạn có chứng nhận quyền sở hữu nhà - không có 2 căn nhà nào giống hệt nhau.

**Properties:**
```
🎯 Unique: Mỗi NFT có ID duy nhất
🔒 Ownership: Blockchain ghi nhận ai sở hữu
💸 Tradable: Có thể bán/mua trên marketplace
📜 Metadata: Chứa thông tin (ảnh, tên, thuộc tính)
```

### ⚙️ Góc nhìn Hệ thống

**NFT Architecture:**

```
┌──────────────────────────────────────────────┐
│        Smart Contract (On-Chain)             │
│                                              │
│  Token ID: 123                               │
│  Owner: 0xABC...                             │
│  Metadata URI: ipfs://bafybei...             │
└──────────────┬───────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│        Metadata (Off-Chain - IPFS)           │
│                                              │
│  {                                           │
│    "name": "Cool NFT #123",                  │
│    "description": "An awesome NFT",          │
│    "image": "ipfs://bafybeiabc.../image.png",│
│    "attributes": [                           │
│      {"trait_type": "Rarity", "value": "Rare"}│
│    ]                                         │
│  }                                           │
└──────────────┬───────────────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────┐
│        Image/Media (IPFS)                    │
│        [Actual PNG/MP4 file]                 │
└──────────────────────────────────────────────┘
```

**Implementation Example:**

```solidity
contract MyNFT is ERC721 {
    uint256 private _tokenIdCounter;
    mapping(uint256 => string) private _tokenURIs;

    constructor() ERC721("MyNFT", "MNFT") {}

    function mint(address to, string memory metadataURI) public {
        uint256 tokenId = _tokenIdCounter++;
        _safeMint(to, tokenId);
        _tokenURIs[tokenId] = metadataURI;
    }

    function tokenURI(uint256 tokenId) 
        public 
        view 
        override 
        returns (string memory) 
    {
        require(_ownerOf(tokenId) != address(0), "Token doesn't exist");
        return _tokenURIs[tokenId];
    }
}
```

---

## 5. Soulbound Token (SBT)

### 👤 Góc nhìn Người dùng

**SBT là gì?**  
Soulbound Token là NFT "liên kết linh hồn" - không thể chuyển nhượng cho người khác. Giống như bằng đại học, chứng chỉ, hồ sơ y tế - chỉ thuộc về bạn, không thể bán/tặng.

**Use Cases:**

```
📜 Credentials:
   - Bằng cấp (university degree)
   - Chứng chỉ chuyên môn (AWS certification)
   - Giấy phép hành nghề (medical license)

🎮 Achievements:
   - In-game achievements (World of Warcraft)
   - Platform badges (GitHub contributions)
   - Event attendance (POAPs)

💳 Identity:
   - Digital ID
   - Credit score
   - Medical records

🤝 Reputation:
   - DAO voting history
   - DeFi protocol usage
   - **Rental agreements** ← VinaLib!
```

**Advantages:**

```
✅ Cannot be sold/transferred → True proof of achievement
✅ Non-transferable → Cannot fake credentials
✅ Permanent record → Trustworthy reputation system
✅ Privacy-preserving → Can prove without revealing details (ZK proofs)
```
## 6. Inheritance trong Solidity

### 👤 Góc nhìn Người dùng

**Inheritance là gì?**  
Giống như con thừa hưởng đặc điểm từ cha mẹ. Smart contract "con" kế thừa tất cả functions và variables từ contract "cha", rồi có thể thêm/sửa đổi.

**Ví dụ thực tế:**
```
Contract Cha: Animal
- eat()
- sleep()

Contract Con: Dog extends Animal
- eat()      ← Inherited
- sleep()    ← Inherited
- bark()     ← New function
```

## 7. Dependency Management

### 👤 Góc nhìn Người dùng

**Dependencies là gì?**  
Giống như khi bạn nấu ăn cần nguyên liệu từ siêu thị. Smart contracts cũng "nhập" code từ các thư viện khác (như OpenZeppelin) thay vì tự viết từ đầu.

**Lợi ích:**
```
✅ Save Time: Dùng code đã có thay vì viết lại
✅ Security: OpenZeppelin đã audit kỹ
✅ Standards: Tuân thủ ERC standards
✅ Updates: Dễ dàng nâng cấp khi có phiên bản mới
```

## 8. Phân tích Implementation trong VinaLib

### 📋 Tổng quan

Hệ thống VinaLib sử dụng tất cả các patterns và standards đã đề cập để xây dựng một nền tảng cho thuê sách phi tập trung với các tính năng quản trị, bảo mật và chuẩn hóa cao.

### 🔧 Smart Contracts Implementation

#### 1. **BookAsset.sol** - NFT Sách có thể cho thuê

**Cấu trúc Kế thừa:**

Contract `BookAsset` kế thừa từ ba contract cha:
- **ERC4907**: Cung cấp khả năng cho thuê NFT (tách biệt quyền sở hữu và quyền sử dụng)
- **Ownable**: Quản lý quyền admin (chỉ owner mới thực hiện được một số thao tác quan trọng)
- **Pausable**: Cho phép tạm dừng khẩn cấp các chức năng chuyển nhượng

**Vai trò của Ownable:**

Contract sử dụng pattern Ownable để đảm bảo chỉ Admin mới có quyền:
- **Tạo sách mới (mint)**: Khi lender muốn đăng sách lên hệ thống, Admin phải xác nhận và tạo NFT đại diện cho sách đó
- **Xác minh tình trạng sách**: Trước khi cho phép niêm yết hoặc cho thuê, Admin phải kiểm tra tình trạng vật lý của sách và đánh dấu là "Verified"
- **Xác nhận trả sách**: Sau khi renter trả sách, Admin kiểm tra xem sách có bị hư hại không và cập nhật trạng thái tương ứng

> **Lý do thiết kế**: Vì sách là tài sản vật lý, cần có người (Admin) xác nhận tình trạng thực tế trước khi cho phép giao dịch. Điều này đảm bảo chất lượng sách và bảo vệ người thuê.

**Vai trò của Pausable:**

Contract có hai function `pause()` và `unpause()` cho phép Admin:
- **Tạm dừng khẩn cấp**: Nếu phát hiện lỗi trong logic cho thuê hoặc bị tấn công, Admin có thể gọi `pause()` để ngay lập tức chặn mọi transfers và rentals
- **Khôi phục hoạt động**: Sau khi fix lỗi, Admin gọi `unpause()` để hệ thống hoạt động trở lại

Tất cả các thao tác chuyển nhượng NFT đều được kiểm tra modifier `whenNotPaused`, nghĩa là khi contract bị pause, không ai có thể transfer/rent sách.

**Vai trò của ERC-4907:**

Contract kế thừa standard ERC-4907 để hỗ trợ cho thuê NFT:
- **setUser()**: Gán quyền sử dụng tạm thời cho người thuê với thời hạn cụ thể (expires)
- **userOf()**: Kiểm tra ai đang là người thuê hiện tại (trả về address(0) nếu đã hết hạn)
- **userExpires()**: Xem thời hạn thuê còn bao lâu

Điều quan trọng là owner vẫn giữ quyền sở hữu NFT, chỉ "cho mượn" quyền sử dụng trong khoảng thời gian nhất định.

**Quản lý Trạng thái Sách:**

Contract định nghĩa 4 trạng thái cho mỗi cuốn sách:
1. **PendingVerification**: Sách mới được mint, đang chờ Admin kiểm tra
2. **Verified**: Admin đã xác nhận sách đạt chuẩn, sẵn sàng cho thuê
3. **Rented**: Sách đang được thuê bởi ai đó
4. **Returned**: (Không dùng trực tiếp) - Sau khi trả, sách sẽ chuyển về Verified hoặc PendingVerification tùy tình trạng

Luồng trạng thái:
```
Mint → PendingVerification → [Admin verify] → Verified → [Rent out] → Rented → [Return] → Verified (nếu không hư) hoặc PendingVerification (nếu hư hại)
```

#### 2. **RentalAgreementSBT.sol** - Chứng nhận Hợp đồng Thuê

**Cấu trúc Kế thừa:**

Contract `RentalAgreementSBT` kế thừa từ:
- **ERC721**: Standard NFT cơ bản
- **Ownable**: Chỉ Admin mới có quyền mint SBT mới

**Cơ chế Soulbound (Liên kết linh hồn):**

Contract override function `_update()` để chặn mọi thao tác chuyển nhượng:
- **Cho phép mint**: Khi tạo SBT mới (from = address(0))
- **Cho phép burn**: Khi xóa SBT (to = address(0))  
- **Chặn transfer**: Nếu cả from và to đều khác address(0), nghĩa là đang cố transfer giữa 2 địa chỉ → Revert với lỗi "Token is soulbound"

> **Lý do thiết kế**: Rental Agreement SBT là bằng chứng rằng user đã ký hợp đồng thuê sách. Đây là proof of agreement cá nhân, không phải tài sản có thể mua bán, nên phải là soulbound (không thể chuyển nhượng).

**Lưu trữ Hash điều khoản hợp đồng:**

Contract sử dụng mapping để lưu hash của điều khoản hợp đồng:
- Mỗi SBT có một `termsHash` (kiểu bytes32) được lưu khi mint
- Hash này được tính từ nội dung văn bản hợp đồng đầy đủ
- Nếu sau này có tranh chấp, có thể verify rằng điều khoản không bị sửa đổi bằng cách so sánh hash

> **Ứng dụng**: Khi user thuê sách, họ nhận được một SBT chứa hash của hợp đồng. Nếu có vấn đề pháp lý, có thể lấy văn bản hợp đồng gốc, hash lại và so sánh với hash trên blockchain để chứng minh tính toàn vẹn.

#### 3. **SuChinToken.sol** - Token Thanh toán ERC-20

**Cấu trúc Kế thừa:**

Contract `SuChinToken` kế thừa từ:
- **ERC20**: Standard token fungible của Ethereum
- **Ownable**: Chỉ Admin mới có quyền mint token mới

**Tuân thủ Standard ERC-20:**

Contract tự động có sẵn các function chuẩn từ ERC20:
- **transfer()**: Chuyển token cho người khác
- **balanceOf()**: Kiểm tra số dư
- **approve()**: Cho phép địa chỉ khác tiêu token thay mình
- **transferFrom()**: Rút token từ địa chỉ đã approve

Constructor khởi tạo token với tên "SuChin Token", symbol "SUC", và mint 1 triệu token ban đầu cho deployer.

**Khả năng Mint thêm:**

Contract có function `mint()` với modifier `onlyOwner`, cho phép Admin:
- Tạo thêm token mới khi cần (ví dụ: user nạp tiền fiat, Admin mint tương ứng số SUC token)
- Điều này phục vụ cho cơ chế on-ramp (chuyển tiền thật thành token)

> **Sử dụng hiện tại**: Trong môi trường test/dev, SUC token được dùng để mô phỏng thanh toán. Tương lai, khi tích hợp payment gateway, user nạp tiền thật → Admin mint SUC → User dùng SUC để trả phí thuê sách.

#### 4. **VinaLibVault.sol** - Contract Điều phối Trung tâm

**Cấu trúc Kế thừa Phức tạp:**

Contract `VinaLibVault` kế thừa từ ba contract/interface:
- **FunctionsClient** (Chainlink): Cho phép gọi Chainlink Functions để thực thi JavaScript off-chain
- **AutomationCompatibleInterface** (Chainlink): Tương thích với Chainlink Keepers để tự động hóa các tác vụ (timeout liquidation)
- **ReentrancyGuard** (OpenZeppelin): Ngăn chặn tấn công Reentrancy (Re-entrant calls)
- **Ownable**: Quản lý quyền admin

**Tối ưu Storage (Slot-Packing Optimization):**

Một trong những cải tiến kỹ thuật quan trọng của VinaLibVault ở phiên bản V3.1 là tối ưu hóa không gian lưu trữ (storage slots) để tiết kiệm phí Gas, kỹ thuật này gọi là **Slot-packing**.

Trong Solidity, mỗi storage slot có kích thước 32 bytes (256 bits). Việc đọc/ghi vào storage cực kỳ tốn gas. VinaLibVault tối ưu cấu trúc `EvidencePack` bằng cách đóng gói nhiều trường nhỏ vào chung một slot 32 bytes:

```solidity
struct EvidencePack {
    bytes32 termsHash;     // Slot 1 (32 bytes)
    bytes32 deliveryHash;  // Slot 2 (32 bytes)
    
    // Slot 3 (Tổng: 23 bytes, nằm gọn trong 1 slot 32 bytes)
    address renter;        // 20 bytes
    uint16 version;        // 2 bytes
    RentalStatus status;   // 1 byte (enum)
    
    // Slot 4 (Tổng: 16 bytes, nằm gọn trong 1 slot)
    uint64 timestamp;      // 8 bytes
    uint64 expires;        // 8 bytes
    
    string pspRef;         // Slot 5 (dynamic string, con trỏ 32 bytes)
}
```
*Lợi ích:* Kỹ thuật này giúp tiết kiệm khoảng ~20,000 đến 40,000 Gas mỗi khi bắt đầu một hợp đồng thuê (`createRental`) hoặc kết thúc vòng đời của thuê sách.

**Quản lý Trạng thái Thuê (DApp V3.1 State Machine):**

VinaLibVault sử dụng Enum `RentalStatus` để quản lý chặt chẽ vòng đời 1 hợp đồng thuê sách độc lập với trạng thái tổng của NFT BookAsset:
1. `Active`: Hợp đồng thuê đang chạy.
2. `ReturnRequested`: Người thuê đã yêu cầu trả sách, chờ Admin xác nhận.
3. `Concluded`: Hợp đồng đã hoàn tất (Thành công, Hủy trước thời điểm bắt đầu, bị thanh lý do vi phạm/quá hạn timeout).

**Ma Trận Phân Quyền (Access Control Matrix) thực tế:**

| Cấp Quyền | Hành động được phép (Functions) |
|-----------|---------------------------------|
| **Admin (`owner`)** | `setConfig()`, `sendRequest()`, `setContracts()`, `confirmReturn()`, `cancelListing()`, `claimCollateral()` (Xử lý vi phạm) |
| **Renter (`msg.sender` / `user`)** | `createRental()` (Được uỷ quyền hoặc trực tiếp), `requestReturn()` |
| **Chainlink Keepers** | `performUpkeep()` (Tự động thu hồi quyền `setUser` khi hết hạn) |

**Gas Estimation (Dự tính phí giao dịch):**
- Hàm `createRental()`: ~150,000 - 180,000 Gas (Do chi phí ghi mới mapping Storage).
- Hàm `confirmReturn()`: ~100,000 - 120,000 Gas.
- Hàm `requestReturn()`: ~45,000 - 60,000 Gas.
- Tự động duy trì hệ thống (`performUpkeep`): ~80,000 Gas + phí nền tảng Chainlink LINK.

**Vai trò của Ownable và Chainlink Automation:**

Contract sử dụng Ownable để bảo vệ các chức năng quan trọng:
- **Cấu hình Chainlink**: Function `setConfig()` chỉ owner mới gọi được, dùng để thiết lập subscriptionId, donId, gasLimit cho Chainlink
- **Tạo rental**: Function `createRental()` chỉ owner (backend service) mới có quyền khởi tạo quy trình thuê sách
- **Gửi request**: Function `sendRequest()` để gửi JavaScript code tới Chainlink Functions cũng bị bảo vệ bởi `onlyOwner`

> **Thiết kế**: End-users không trực tiếp gọi các function trên contract này. Thay vào đó, backend service (do admin sở hữu) đóng vai trò trung gian điều phối toàn bộ quy trình thuê sách, từ verify policy đến gọi Chainlink và tạo rental on-chain. Điều này cho phép kiểm soát tốt hơn và tích hợp business logic phức tạp.

### 📊 Dependency Analysis

**package.json:**

```json
{
  "dependencies": {
    "@openzeppelin/contracts": "^5.0.0",
    "@chainlink/contracts": "^1.3.0"
  }
}
```

**Import Map:**

```
BookAsset.sol
├── ./ERC4907.sol (local)
├── @openzeppelin/contracts/access/Ownable.sol
└── @openzeppelin/contracts/utils/Pausable.sol

RentalAgreementSBT.sol
├── @openzeppelin/contracts/token/ERC721/ERC721.sol
└── @openzeppelin/contracts/access/Ownable.sol

VinaLibVault.sol
├── @chainlink/contracts/src/v0.8/functions/v1_0_0/FunctionsClient.sol
├── @chainlink/contracts/src/v0.8/functions/v1_0_0/libraries/FunctionsRequest.sol
├── @chainlink/contracts/src/v0.8/automation/interfaces/AutomationCompatibleInterface.sol
└── @openzeppelin/contracts/access/Ownable.sol
```

### 🎯 Design Patterns Summary

| Pattern | Contract | Purpose |
|---------|----------|---------|
| **Ownable** | BookAsset, RentalAgreementSBT, SuChinToken, VinaLibVault | Admin control (verification, minting, config) |
| **Pausable** | BookAsset | Emergency stop for transfers |
| **ERC-721** | BookAsset, RentalAgreementSBT | NFT standard compliance |
| **ERC-20** | SuChinToken | Fungible token standard |
| **ERC-4907** | BookAsset | Rentable NFT (separate owner/user) |
| **Soulbound** | RentalAgreementSBT | Non-transferable proof of agreement |
| **Multiple Inheritance** | VinaLibVault, BookAsset | Combine functionalities from multiple patterns |

### 💡 Key Insights

**1. Separation of Concerns:**
```
BookAsset = Physical book representation (NFT)
RentalAgreementSBT = Legal proof of rental contract (SBT)
SuChinToken = Payment medium (ERC-20)
VinaLibVault = Orchestration + external integrations (Chainlink)

→ Modular design, easy to upgrade individual components
```

**2. Security Layers:**
```
Layer 1: Ownable → Only admin can verify/mint
Layer 2: Pausable → Emergency stop mechanism
Layer 3: Soulbound → Prevent fraudulent transfer of agreements
Layer 4: ERC standards → Battle-tested, audited code
```

**3. Upgradability Consideration:**
```
Current: Non-upgradable contracts (immutable logic)
Future: Consider Transparent Proxy pattern if need upgrades
Trade-off: Simplicity vs Flexibility
```

---

## 9. Cổng Thanh Quan & Kiểm Định Bảo Mật (Security Audit)

Trong thiết kế DApp Web3 đặc biệt tại giai đoạn dự án VinaLib 3.1, kiểm định an toàn (Security Audit) là một khái niệm không thể thiếu. Vì Smart Contract khi đã xuất xưởng lên Chain là **Immutable** (Bất biến), mọi kẽ hở đều có thể dẫn đến hậu quả nghiêm trọng về cả tiền tài lẫn quyền sở hữu.

### 🛡️ Vai Trò Của Kiểm Định Bảo Mật (Audit)
Việc Audit (Giám định thuật toán hợp đồng) được thực hiện với 3 mục tiêu cốt lõi:
1. **Kiểm tra tính an toàn (Vuln/Exploits):** Phát hiện Reentrancy (lỗi tấn công vòng lặp ẩn), tràn số nguyên (Overflow/Underflow), lỗi quyền truy cập (Access Control Flaws), thiếu kiểm tra Logic ở đầu vào (Input Validation Flaws).
2. **Kiểm tra tính tối ưu (Gas Optimization):** Phát hiện các điểm cần tối ưu hóa Code (sử dụng calldata thay cho memory, slot-packing, vòng lặp không kiểm soát – DOS gas limit).
3. **Tính nhất quán Business Logic (Business Inconsistency):** Kiểm tra xem mã được viết có "làm đúng như đặc tả yêu cầu" không. (Ví dụ: Hàm trả sách phải xóa bỏ hoàn toàn quyền được gán cho người dùng trước đó).

### 🛠️ Các Công Cụ Kiểm Định Thông Dụng
Dự án VinaLib sử dụng các công cụ mạnh mẽ trong quy trình Audit (điển hình qua kịch bản tự động `Master_KiemDinhBaoMat`):

*   **Slither:** Một framework phân tích tĩnh (Static Analysis) phổ biến nhất đối với Solidity. Giúp quét nhanh các mẫu mã độc (pattern-matching) để định danh các rủi ro căn bản và khuyến nghị Gas.
*   **Mythril:** Công cụ thực thi ký hiệu (Symbolic Execution). Thay vì "đọc chay", thuật toán đi sâu vào mọi nhánh rẽ Logic độc lập bên trong máy ảo EVM để chứng minh liệu Hacker có đường đi để vi phạm các điều kiện (Require) và tấn công hay không.
*   **Echidna / Foundry Fuzzing:** Kiểm thử Fuzz. Bơm một lượng lớn dữ liệu ngẫu nhiên hoặc có chủ đích trong không gian số nhằm đánh sập Smart Contract nếu xảy ra lỗi ngoại lệ chưa lường trước.

### 🔒 Ví dụ triển khai Security trên VinaLib
Ở VinaLib V3.1, các giải pháp Security Code được nhúng sâu vào các Contract cốt lõi:

*   **Chống Reentrancy:** `VinaLibVault` sử dụng `ReentrancyGuard` của OpenZeppelin với từ khoá `nonReentrant` tại các hàm xử lý vòng đời tài sản (Như `createRental()`, `confirmReturn()`). Từ khoá này kết hợp mô hình Checks-Effects-Interactions đảm bảo khi hàm thứ hai chưa kết thúc thì hàm ban đầu không được tái kích hoạt.
*   **AccessControl khắt khe:** Biến cố `claimCollateral()` (tịch thu cọc) hoặc `cancelListing()` chỉ duy nhất Admin (Lessor) được quyền kích hoạt, chống lợi dụng từ người dùng với dã tâm lấy sách mà được đền bù tiền cọc vô lý.  

---

## Phụ lục: Công cụ và Tài nguyên

### Development Tools

**1. Smart Contract Libraries:**
```
- OpenZeppelin Contracts: https://docs.openzeppelin.com/contracts
- Chainlink Contracts: https://docs.chain.link
- Solmate (Gas-optimized): https://github.com/transmissions11/solmate
```

**2. Development Frameworks:**
```
- Hardhat: https://hardhat.org
- Foundry: https://book.getfoundry.sh
- Remix IDE: https://remix.ethereum.org
```

**3. Testing & Security Tools:**
```
- Hardhat Tests (Mocha + Chai)
- Foundry Tests (Solidity-based)
- Tenderly (Debugging & Simulation)
- Slither (Static analysis): https://github.com/crytic/slither
- Mythril (Symbolic execution)
- Echidna (Fuzzing)
```

### Resources

- **EIP Repository**: https://eips.ethereum.org
- **Solidity Docs**: https://docs.soliditylang.org
- **OpenZeppelin Forum**: https://forum.openzeppelin.com
- **Ethernaut (Security Game)**: https://ethernaut.openzeppelin.com

---

## Tổng kết

Smart contract design patterns và standards tạo nền tảng cho hệ sinh thái blockchain. Đối với phiên bản dự án **VinaLib V3.1**, các hợp đồng không chỉ dừng ở bước khởi tạo tiêu chuẩn mà đã tối ưu triệt để:

**🔹 Core Patterns:**
- **Ownable**: Access control cho admin functions.
- **Pausable**: Emergency stop mechanism.
- **Inheritance & Dependency**: Kế thừa Chainlink (Automation, Functions) và OpenZeppelin (ERC/Security Guards).
- **Trạng thái đa cực (Multi-state):** Quản lý qua Enum độc lập ở cả Asset gốc và Vault (EvidencePack).

**🔹 Standards & Optimizations:**
- **ERC-20 & ERC-721**: Cho thanh toán nền và định hình tài sản chính (Asset SBT).
- **ERC-4907**: Rentable NFTs phân tách rõ Quyền Sở Hữu / Quyền Sử Dụng.
- **SBT**: Chứng chỉ Rental không thể chuyển nhượng.
- **Slot-Packing:** Ép chuỗi dữ liệu EvidencePack xuống kích thước Slot để giảm thiểu Gas tới tối đa.
- **Security Audit Guards:** Chặn Re-entrancy, Phân quyền thao tác chặt chẽ thông qua Static Analysis nội tại.

**🎯 Best Practices:**
1. Use established standards (don't reinvent the wheel).
2. Inherit from audited libraries (OpenZeppelin / Contracts Security).
3. Combine patterns carefully (test inheritance hierarchy).
4. Áp dụng Slot-packing để tối đa hoá hiệu suất sử dụng Data (Gas Economics).
5. Implement access control khắt khe đối với Logic quan trọng.
6. Luôn triển khai Audit và Security Checklist trước khi Release (Slither/Mythril).
