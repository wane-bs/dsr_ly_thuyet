---
layout: default
title: IPFS
---

# IPFS - InterPlanetary File System
## Tài liệu Giới thiệu Khái niệm và Ứng dụng

> 📍 **Vị trí trong bộ tài liệu**: 1/5 - Nền tảng lưu trữ  
> ⬅️ [Kiến trúc Tổng quan](./0-Kien-truc-Tong-quan.md) | ➡️ [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md)

### 📎 Tài liệu Liên quan
| Liên kết đến | Mối quan hệ |
|-------------|-------------|
| [Smart Contracts (Cơ bản)](./2-Smart-Contracts-Co-ban.md) | `tokenURI()` trỏ đến CID trên IPFS |
| [Chainlink](./4-Chainlink.md) | Functions có thể upload metadata lên IPFS |

---

## 🌉 Từ Lý thuyết sang Thực hành

Nếu bạn vừa đọc xong file [START_HERE_Ly_thuyet_nen_tang.md](./START_HERE_Ly_thuyet_nen_tang.md), bạn đã hiểu **IPFS** (InterPlanetary File System) là gì: một hệ thống lưu trữ phi tập trung sử dụng **Content Addressing** (định danh theo nội dung) thay vì Location Addressing.

File này sẽ đi sâu vào **chi tiết kỹ thuật**: cấu trúc CIDv0 vs CIDv1, DHT (Distributed Hash Table), Merkle DAG, và quan trọng nhất - **cách VinaLib implement IPFS trong thực tế**.

Bạn sẽ thấy code examples, workflows cụ thể, và phân tích implementation. Đừng lo nếu chưa hiểu hết ngay - mục tiêu là  làm quen với kiến trúc thực tế.

---

## 💡 Tại sao VinaLib chọn IPFS?

**Câu hỏi:** Tại sao không dùng AWS S3, Google Cloud Storage, hay lưu trực tiếp trên Blockchain?

**Trả lời:**

| Giải pháp | Vấn đề | Tại sao không phù hợp |
|-----------|--------|---------------------|
| **Lưu trên Blockchain** | Chi phí gas cực cao | Upload 1 ảnh 100KB tốn ~$50 (Ethereum mainnet), không khả thi |
| **AWS S3 / Google Cloud** | Tập trung hóa | Mâu thuẫn với triết lý phi tập trung của Web3, có thể bị censorship |
| **IPFS** | ✅ Phi tập trung, ✅ Content addressing (immutable), ✅ Chi phí thấp | **Lựa chọn tối ưu** |

**Quyết định thiết kế:**
- **Hiện tại:** Sử dụng IPFS Simulator (local) cho development - nhanh, miễn phí, dễ debug
- **Tương lai:** Migrate sang Pinata hoặc Web3.Storage cho production - đảm bảo availability toàn cầu
- **Smart Contract:** Chỉ lưu CID (46 bytes) thay vì raw data → Tiết kiệm 60% gas cost

---

## 📚 Mục lục

1. [Giới thiệu IPFS](#1-giới-thiệu-ipfs)
2. [Content Addressing vs Location Addressing](#2-content-addressing-vs-location-addressing)
3. [CID - Content Identifier](#3-cid---content-identifier)
4. [Cơ chế phân giải dữ liệu (Content Routing)](#4-cơ-chế-phân-giải-dữ-liệu)
5. [Distributed Hash Table (DHT)](#5-distributed-hash-table-dht)
6. [Merkle DAG](#6-merkle-dag)
7. [IPFS Gateway](#7-ipfs-gateway)
8. [Quy trình thực thi tại máy đầu cuối](#8-quy-trình-thực-thi-tại-máy-đầu-cuối)
9. [Phân tích Implementation trong VinaLib](#9-phân-tích-implementation-trong-vinalib)
10. [Performance & Best Practices](#10-performance--best-practices)

---

## 1. Giới thiệu IPFS

### 👤 Góc nhìn Người dùng

**IPFS là gì?**  
Tưởng tượng IPFS như một "thư viện toàn cầu" mà không ai làm chủ. Khi bạn tải một file lên IPFS, nó được chia nhỏ và lưu trữ trên nhiều máy tính khác nhau trên thế giới. Bạn không cần phải thuê server hay lo lắng về việc dịch vụ ngừng hoạt động.

**Ưu điểm cho người dùng:**
- ✅ **Không mất dữ liệu**: Miễn là còn ai đó lưu file, bạn vẫn truy cập được
- ✅ **Tải nhanh hơn**: Tải từ nhiều nguồn gần bạn nhất thay vì 1 server xa xôi
- ✅ **Không kiểm duyệt**: Không ai có quyền xóa nội dung của bạn
- ✅ **Tiết kiệm chi phí**: Không cần trả tiền hosting hàng tháng

**Nhược điểm cần lưu ý:**
- ⚠️ **Public by default**: Ai cũng có thể xem nội dung nếu biết mã CID
- ⚠️ **Cần internet**: Không thể truy cập offline (trừ khi bạn đã tải trước)
- ⚠️ **Tốc độ phụ thuộc peers**: Ít người chia sẻ = tải chậm hơn

### ⚙️ Góc nhìn Hệ thống

**IPFS Protocol Stack:**
```
┌─────────────────────────────────────┐
│   Applications (DApps, Pinata...)   │
├─────────────────────────────────────┤
│   IPFS Core (Content Routing, ...)  │
├─────────────────────────────────────┤
│   IPLD (Data Model)                 │
├─────────────────────────────────────┤
│   libp2p (P2P Networking)           │
│   - Bitswap, DHT, PubSub            │
└─────────────────────────────────────┘
```


---

## 2. Content Addressing vs Location Addressing

### 👤 Góc nhìn Người dùng

**Location Addressing (Web truyền thống - HTTP/HTTPS):**
```
https://example.com/images/cat.jpg
       ↑                    ↑
    Địa chỉ server      Tên file
```
- Bạn hỏi: "Hãy cho tôi file `cat.jpg` từ server `example.com`"
- **Vấn đề**: Nếu server `example.com` sập hoặc xóa file → Bạn mất quyền truy cập
- **Vấn đề 2**: Nếu ai đó thay đổi nội dung file → Bạn không biết được

**Content Addressing (IPFS):**
```
ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf3...
        ↑
    "Vân tay" của nội dung
```
- Bạn hỏi: "Hãy cho tôi file có vân tay là `bafy...`"
- Mạng lưới tìm **bất kỳ ai** đang lưu file đó
- **Ưu điểm**: Nội dung không thể bị thay đổi mà CID không đổi theo

### ⚙️ Góc nhìn Hệ thống

**So sánh chi tiết:**

| Tiêu chí | Location-Based (HTTP) | Content-Based (IPFS) |
|----------|----------------------|---------------------|
| **Định danh** | URL (server + path) | CID (cryptographic hash) |
| **Tìm kiếm** | DNS → IP address | DHT → Peer IDs |
| **Persistence** | Phụ thuộc server | Phân tán (distributed) |
| **Verification** | Không có (trust server) | Cryptographic (hash match) |
| **Caching** | HTTP Cache-Control | Automatic (content-based) |
| **Deduplication** | Không tự động | Tự động (same content = same CID) |
| **Versioning** | Manual (filenames) | Automatic (different content = different CID) |

**Khái niệm quan trọng:**

Trong HTTP, bạn tin tưởng server trả đúng nội dung. Trong IPFS, bạn **tự kiểm tra** bằng cách hash lại dữ liệu nhận được và so sánh với CID. Nếu khớp → dữ liệu chính xác. Nếu không khớp → lỗi corruption.

---

## 3. CID - Content Identifier

### 👤 Góc nhìn Người dùng

**CID là gì?**  
CID giống như "mã vạch duy nhất" cho nội dung. Cùng một nội dung → cùng một CID, bất kể ai upload, upload từ đâu, upload khi nào.

**Ví dụ thực tế:**
```
Bạn chụp một bức ảnh và upload lên IPFS
→ CID: bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oczg6uqhz

Bạn bè bạn upload cùng bức ảnh đó
→ CID: bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oczg6uqhz
      (GIỐNG NHAU!)

Bạn chỉnh sửa 1 pixel trong ảnh và upload lại
→ CID: bafkreiabcd... (KHÁC HOÀN TOÀN!)
```

### ⚙️ Góc nhìn Hệ thống

**Cấu trúc CID:**

```
CID Example: bafybeigdyrzt5sfp7udm7hu76uh7y26nf3efuylqabf3oczg6uqhzieaazm

┌─────────────────────────────────────────────────────────────┐
│  CIDv1 Structure (Self-Describing Format)                   │
└─────────────────────────────────────────────────────────────┘

┌──────┬──────────┬───────────────────────────────────────┐
│ bafy │   bei    │  gdyrzt5sfp7udm7hu76uh7y26nf3...     │
└──────┴──────────┴───────────────────────────────────────┘
   ↓        ↓                        ↓
[Base32] [Codec]            [Multihash Content]


Breakdown:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Multibase Prefix: 'b' = base32
   - Cho biết encoding scheme
   - base32 = URL-safe, case-insensitive

2. CID Version: 0x01 (embedded)
   - CIDv0: Qdag-pb + SHA-256 only
   - CIDv1: Flexible, future-proof

3. Multicodec: 'dag-pb' (0x70)
   - Định dạng dữ liệu: dag-pb, raw, dag-cbor, etc.

4. Multihash: SHA-256 hash
   ┌─────────┬────────┬────────────────────────┐
   │ 0x12    │  0x20  │  actual hash digest    │
   └─────────┴────────┴────────────────────────┘
      ↓         ↓              ↓
   SHA-256   32 bytes    hash của content
```

**CID Generation Process (Tổng quan):**

```
Step 1: Raw Content (File gốc)
Step 2: Chunking (chia nhỏ nếu file lớn)
Step 3: Hash mỗi block bằng SHA-256
Step 4: Build Multihash (hash function + digest)
Step 5: Prepend Multicodec (loại dữ liệu)
Step 6: Prepend CID Version (v0 hoặc v1)
Step 7: Base32 Encode (mã hóa an toàn)
Step 8: Prepend Multibase ('b' prefix)
→ Final CID: bafybeigdyrzt...
```

**Phân loại phiên bản CID (CID Versions):**

Hiện tại trên thế giới IPFS sử dụng 2 phiên bản CID chính:

**1. CIDv0 (Legacy):**
- **Đặc điểm**: Luôn bắt đầu bằng `Qm...`.
- **Cấu trúc**: Base58btc(multihash(content)).
- **Hạn chế**: Cố định với thuật toán SHA-256 và định dạng dag-pb. Không hỗ trợ các chuẩn dữ liệu mới.
- **Vấn đề URL**: Do dùng Base58 (có cả chữ hoa và thường), CIDv0 không an toàn khi sử dụng làm subdomain (vì DNS không phân biệt hoa thường).

**2. CIDv1 (Modern):**
- **Đặc điểm**: Thường bắt đầu bằng `b...` (nếu dùng Base32).
- **Cấu trúc**: Multibase(Version + Multicodec + Multihash).
- **Ưu điểm**:
    - **Self-describing**: Tự mô tả chính nó (biết được dùng codec gì, hash gì ngay từ đầu).
    - **Future-proof**: Dễ dàng mở rộng cho các thuật toán mới.
    - **URL-safe**: Mặc định dùng Base32 (chỉ chữ thường), hoàn toàn tương thích với DNS và URL (ví dụ: `https://bafy...ipfs.dweb.link`).

**Bảng so sánh CIDv0 vs CIDv1:**

| Feature | CIDv0 | CIDv1 |
|---------|-------|-------|
| **Format** | Qm... (Base58) | bafy... (Base32) |
| **Start with** | `Qm` | `b` (thường gặp nhất) |
| **Codec** | dag-pb only | Flexible (Raw, CBOR, JSON...) |
| **Hash** | SHA-256 only | Multiple (SHA-2, Blake3, Keccak...) |
| **Self-describing** | ❌ No | ✅ Yes |
| **URL-safe** | ⚠️ Not guaranteed | ✅ Yes |

> **Lưu ý**: Có thể chuyển đổi từ CIDv0 sang CIDv1 (không làm thay đổi nội dung file), nhưng không phải lúc nào cũng chuyển ngược lại được nếu CIDv1 dùng codec không phải dag-pb.
> **IPFS default hiện tại**: CIDv1 + Base32 cho URLs (URL-safe, DNS-compatible)

---

## 4. Cơ chế phân giải dữ liệu (Content Routing)

### 👤 Góc nhìn Người dùng

Khi bạn click vào một link IPFS (ví dụ: xem ảnh bìa sách), điều gì xảy ra?

```
Bạn → "Tôi muốn xem CID bafybei...!"
      ↓
IPFS Node của bạn → "Để tôi hỏi mạng lưới ai đang có..."
      ↓
Mạng IPFS → "Node A, B, C đang lưu file này!"
      ↓
IPFS Node → Kết nối với A, B, C và tải các mảnh
      ↓
Ghép các mảnh lại → Hiển thị ảnh cho bạn
```

### ⚙️ Góc nhìn Hệ thống

**Các giai đoạn:**

1. **DHT Query**: Tìm kiếm các node đang lưu trữ CID thông qua Distributed Hash Table
2. **Peer Discovery**: Nhận danh sách provider peers
3. **Block Exchange**: Download các blocks song song từ nhiều peers (Bitswap protocol)
4. **Verification**: Kiểm tra hash của từng block
5. **Reassembly**: Ghép các blocks lại theo cấu trúc DAG

---

## 5. Distributed Hash Table (DHT)

### 👤 Góc nhìn Người dùng

**DHT là gì?**  
Tưởng tượng DHT như một "danh bạ điện thoại phân tán". Thay vì có một quyển danh bạ duy nhất, mỗi người giữ một phần danh bạ. Khi bạn cần tìm số điện thoại (CID), bạn hỏi láng giềng, họ chỉ bạn đến người có thông tin đó.

### ⚙️ Góc nhìn Hệ thống

**Kademlia DHT trong IPFS:**

IPFS sử dụng **Kademlia DHT** với đặc điểm:

1. **XOR Distance Metric**: Đo khoảng cách giữa 2 node IDs bằng phép XOR
2. **K-buckets**: Mỗi node duy trì routing table với các bucket chứa peer gần nhất
3. **Logarithmic Lookup**: Tìm kiếm trong O(log N) hops

**Lookup Process:**
```
Target CID: bafybei...

Step 1: Hash CID → DHT Key (256-bit)
Step 2: Tìm α (=3) closest peers trong routing table
Step 3: Gửi FIND_NODE(key) đến 3 peers đó
Step 4: Nhận về danh sách closer peers
Step 5: Lặp lại cho đến khi tìm được providers
Step 6: Return danh sách peers đang lưu trữ CID
```

**Provider Records:**

Khi một node lưu trữ content, nó announce lên DHT: "Tôi có CID này!". Thông tin này được lưu trong DHT để các node khác tìm thấy.

---

## 6. Merkle DAG

### 👤 Góc nhìn Người dùng

**Merkle DAG là gì?**  
Tưởng tượng bạn có một quyển sách dày. Thay vì lưu cả quyển làm một khối, IPFS chia nó thành các trang. Mỗi trang có mã riêng, và có một "mục lục" chính chứa tất cả mã của các trang. Nếu bạn sửa một trang, chỉ mã của trang đó thay đổi.

### ⚙️ Góc nhìn Hệ thống

**Merkle DAG Structure:**

DAG = Directed Acyclic Graph (Đồ thị có hướng không chu trình)

```
Example: Large Video File (10 MB)

                    ┌──────────────────┐
                    │   Root Block     │ ← CID của toàn bộ file
                    │  (Metadata +     │   (Đây là CID mà user nhận)
                    │   Links)         │
                    └─────────┬────────┘
                              │
              ┌───────────────┼───────────────┐
              ▼               ▼               ▼
         ┌─────────┐     ┌─────────┐     ┌─────────┐
         │ Chunk 1 │     │ Chunk 2 │     │ Chunk 3 │
         │ 256 KB  │     │ 256 KB  │     │ 256 KB  │
         │ CID_1   │     │ CID_2   │     │ CID_3   │
         └─────────┘     └─────────┘     └─────────┘
```

**UnixFS**: IPFS sử dụng UnixFS để map files/directories lên DAG structure

**Chunking Strategies:**

1. **Fixed-Size Chunking** (default): 256 KB chunks, đơn giản nhưng deduplication kém
2. **Content-Defined Chunking (CDC)**: Rabin fingerprinting, deduplication tốt hơn

---

## 7. IPFS Gateway

### 👤 Góc nhìn Người dùng

**Gateway là gì?**  
Gateway như một "phiên dịch viên". Trình duyệt của bạn chỉ "nói" được ngôn ngữ HTTP/HTTPS, còn IPFS "nói" ngôn ngữ riêng (`ipfs://`). Gateway dịch giữa hai bên để bạn có thể xem nội dung IPFS trên trình duyệt thông thường.

**Các loại Gateway:**

```
1. Public Gateway (Miễn phí):
   https://ipfs.io/ipfs/bafybei...
   https://dweb.link/ipfs/bafybei...
   
2. Private Gateway (Pinata, Infura):
   https://gateway.pinata.cloud/ipfs/bafybei...
   https://ipfs.infura.io:5001/ipfs/bafybei...
   
3. Local Gateway (Your own node):
   http://localhost:8080/ipfs/bafybei...
```

**Gateway Request Flow:**

```
Step 1: DNS Resolution (gateway domain → IP)
Step 2: HTTPS Connection (TLS handshake)
Step 3: Gateway parses CID from URL
Step 4: Check cache
Step 5: If miss, fetch from IPFS network
Step 6: Stream data back to browser
```

---

## 8. Quy trình thực thi tại máy đầu cuối (Rendering)

### 👤 Góc nhìn Người dùng

Sau khi tải xong, trình duyệt của bạn:
1. **Kiểm tra tính toàn vẹn**: IPFS tự động kiểm tra file không bị hỏng
2. **Ghép các mảnh**: Nếu file lớn, ghép các mảnh lại theo đúng thứ tự
3. **Hiển thị**: Giống như xem file thông thường

### ⚙️ Góc nhìn Hệ thống

**Verification Process:**

Mỗi block khi download được verify bằng cách:
1. Hash block data bằng SHA-256
2. So sánh hash với expected hash trong CID
3. Nếu khớp → Accept, nếu không → Reject và retry

**Reassembly:**

```
Root CID: bafybeiXYZ

1. Fetch Root Block (chứa links đến các chunks)
2. Traverse Links (DFS hoặc BFS)
3. Concatenate chunks theo thứ tự
4. Return final data
```

**Rendering by Content Type:**

| Content Type | Rendering Process |
|-------------|-------------------|
| **Image** (JPEG, PNG) | Decode via Canvas API / `<img>` tag |
| **Video** (MP4, WebM) | Stream via `<video>` tag + Media Source Extensions |
| **PDF** | PDF.js or native viewer |
| **JSON/Text** | Parse and display |

---

## 9. Phân tích Implementation trong VinaLib

### 📋 Tổng quan

Hệ thống VinaLib sử dụng **IPFS Simulator** - một implementation giả lập IPFS trên local filesystem để phục vụ cho giai đoạn phát triển và testing ban đầu. Đây là giải pháp tạm thời trước khi migrate sang IPFS thật (Pinata, Web3.Storage, hoặc self-hosted node).


### 🔧 Components

#### 1. **IPFS Simulator (`/IPFS/ipfs_simulator.js`)**

**Chức năng:**
- Generate CIDv1 chuẩn IPFS (Base32, SHA-256)
- Lưu trữ files vào local filesystem (thay vì IPFS network)
- Hỗ trợ 3 loại folder: `images/`, `metadata/`, `storage/`

**Quy trình tạo CID:**
```
Input: Buffer content
      ↓
SHA-256 Hash
      ↓
Build Multihash (0x12 + 0x20 + hash)
      ↓
Prepend Codec (0x55 = Raw)
      ↓
Prepend Version (0x01 = CIDv1)
      ↓
Base32 Encode
      ↓
Prepend 'b' (multibase prefix)
      ↓
Output: CIDv1 (e.g., bafybei...)
```

**API Methods:**
- `add(content, folderName)`: Thêm content và trả về CID
- `get(cid)`: Lấy content theo CID
- `generateCIDv1(content)`: Tạo CID từ content

#### 2. **IPFS Module (`/backend/src/modules/IPFS/controller.js`)**

**Endpoint:**
```
GET /ipfs/:cid
```

**Chức năng:**
- Nhận CID từ URL parameter
- Gọi `ipfs_simulator.get(cid)`
- Trả file về client với `Content-Type: image/jpeg`

**Error Handling:**
- 404: CID not found
- 500: Server error

#### 3. **Smart Contract (`BookAsset.sol`)**

**IPFS Storage:**
```solidity
mapping(uint256 => string) public tokenCIDs;
```

**Functions sử dụng CID:**

**a) safeMint:**
```
function safeMint(address to, string memory cid)
```
- Lưu CID vào `tokenCIDs[tokenId]`
- CID là hash của book cover image

**b) tokenURI:**
```
function tokenURI(uint256 tokenId) returns (string)
```
- Returns: `https://ipfs.io/ipfs/` + `tokenCIDs[tokenId]`
- Tạo metadata URI theo chuẩn ERC-721

**c) _baseURI:**
```
function _baseURI() returns (string)
```
- Returns: `"https://ipfs.io/ipfs/"`
- Base URI cho tất cả tokens

### 📊 Data Flow

#### Upload Flow:

```
1. User selects book cover image
      ↓
2. Frontend sends image to backend API
      ↓
3. Backend:
   - Receives image buffer
   - Calls ipfs_simulator.add(buffer, 'images')
   - Gets CID back
      ↓
4. Backend calls BookAsset.safeMint(owner, CID)
      ↓
5. Smart Contract:
   - Mints NFT
   - Stores CID in tokenCIDs mapping
   - Emits BookStatusChanged event
      ↓
6. Return success to frontend
```

#### Display Flow:

```
1. Frontend needs to display book cover
      ↓
2. Calls smart contract: tokenCIDs[tokenId]
      ↓
3. Gets CID back
      ↓
4. Constructs URL: http://localhost:3000/ipfs/{CID}
      ↓
5. IPFS Module controller:
   - Gets CID from URL
   - Calls ipfs_simulator.get(CID)
   - Returns file buffer
      ↓
6. Browser displays image
```

### 🎯 Ưu điểm và Hạn chế

#### ✅ Ưu điểm của IPFS Simulator:

1. **Development Speed**: Không cần setup IPFS node hoặc external service
2. **Offline Development**: Không cần internet
3. **Deterministic**: CID generation giống IPFS thật
4. **Cost**: Miễn phí, không tốn gas cho storage
5. **Testing**: Dễ dàng test và debug

#### ⚠️ Hạn chế:

1. **Không phân tán**: Files chỉ lưu local, không có redundancy
2. **Không có DHT**: Không hỗ trợ content routing
3. **Không persistent**: Mất data nếu server restart (nếu không backup)
4. **Single point of failure**: Phụ thuộc vào 1 server
5. **Không có pinning service**: Không đảm bảo availability

### 🚀 Migration Path (Future)

Để chuyển sang IPFS production, cần:

**Phase 1: Thêm Pinning Service**
- Tích hợp Pinata SDK hoặc Web3.Storage
- Upload files lên service khi add()
- Vẫn giữ local copy cho development

**Phase 2: Gateway Integration**
- Update `_baseURI()` trong smart contract
- Point đến dedicated gateway (e.g., `gateway.pinata.cloud`)
- Implement fallback gateways

**Phase 3: Smart Contract Migration** (nếu cần)
- Deploy contract mới với updated baseURI
- Migrate existing CIDs
- Ensure backward compatibility

### 💡 Best Practices hiện tại

1. **CID Storage**: ✅ Lưu CID trong smart contract, không lưu raw data
2. **Folder Organization**: ✅ Phân loại images/metadata/storage
3. **Error Handling**: ⚠️ Cần thêm retry logic và fallback
4. **Caching**: ⚠️ Frontend nên cache CID → URL mapping
5. **Validation**: ⚠️ Cần validate CID format trước khi query

### 📈 Metrics

**Gas Cost Comparison:**

| Storage Type | Cost per Book |
|-------------|---------------|
| On-chain image (100KB) | ~50,000 gas |
| **CID only (46 bytes)** | **~20,000 gas** |
| **Savings** | **60% cheaper** |

**Storage Locations:**

```
Local IPFS Simulator:
├── images/          (Book covers)
│   └── bafybei...   (CID-named files)
├── metadata/        (Book metadata JSON)
│   └── bafybei...
└── storage/         (Other files)
    └── bafybei...

Smart Contract:
BookAsset.tokenCIDs[tokenId] → "bafybei..."
```

---

## 10. Performance & Best Practices

### 👤 Góc nhìn Người dùng

**Tips để trải nghiệm tốt nhất:**
- Sử dụng các website đã "pin" (giữ) nội dung → Tải nhanh hơn
- Nếu tải chậm, thử đổi gateway khác
- Nội dung phổ biến (nhiều người xem) thường tải nhanh hơn

### ⚙️ Góc nhìn Hệ thống

### Best Practices

#### 1. **Pinning Strategy**

- ❌ BAD: Upload rồi quên → CID sẽ mất sau thời gian
- ✅ GOOD: Pin ngay sau khi upload → Node giữ file
- ✅ BETTER: Dùng Pinning Service (Pinata, Web3.Storage)

**Pinning Services Comparison:**

| Service | Free Tier | Pricing | Features |
|---------|-----------|---------|----------|
| **Pinata** | 1 GB | $20/mo (100GB) | Dedicated gateway, Submarine keys |
| **Web3.Storage** | 1 TB Free | Free (funded by Protocol Labs) | Simple API, generous quota |
| **Infura** | 5 GB | $50/mo (100GB) | Also provides RPC nodes |
| **Filebase** | 5 GB | $5.99/mo (1TB) | S3-compatible API |

#### 2. **Gateway Fallback**

Implement multiple gateway fallback để tăng reliability:

**Concept:**
```
Primary Gateway: gateway.pinata.cloud
  ↓ (timeout/error)
Fallback 1: ipfs.io
  ↓ (timeout/error)
Fallback 2: dweb.link
  ↓ (timeout/error)
Fallback 3: cloudflare-ipfs.com
```

#### 3. **CDN Integration**

Sử dụng Cloudflare IPFS Gateway có CDN:
- Faster global delivery
- Automatic caching
- DDoS protection

#### 4. **Image Optimization**

Pre-process images trước khi upload:
- Resize to appropriate dimensions
- Compress (JPEG quality 80-85%)
- Convert to modern formats (WebP, AVIF)

#### 5. **Monitoring và Health Checks**

Regular health check cho pinned content:
- Verify accessibility mỗi 24h
- Monitor gateway response times
- Alert nếu content không accessible

#### 6. **Error Handling**

Comprehensive error handling:
- Timeout errors → Try different gateway
- Not found errors → Check pin status
- Corruption errors → Re-upload and re-pin

---

## Phụ lục: Công cụ và Tài nguyên

### Tools

- **IPFS Desktop**: GUI application để quản lý IPFS node
- **IPFS Companion**: Browser extension (Chrome/Firefox)
- **js-ipfs**: JavaScript implementation
- **go-ipfs (kubo)**: Official Go implementation

### Useful Commands

```bash
# Initialize IPFS node
ipfs init

# Start daemon
ipfs daemon

# Add file
ipfs add myfile.jpg

# Cat (view) file
ipfs cat <CID>

# Pin file
ipfs pin add <CID>

# List pins
ipfs pin ls
```

### Resources

- **Official Docs**: https://docs.ipfs.tech
- **IPFS Blog**: https://blog.ipfs.tech
- **Awesome IPFS**: https://github.com/ipfs/awesome-ipfs
- **IPFS Specs**: https://github.com/ipfs/specs

---

## Tổng kết

IPFS mang lại paradigm shift trong cách chúng ta nghĩ về dữ liệu trên internet:

**Từ Location-based → Content-based**  
**Từ Centralized → Distributed**  
**Từ Mutable → Immutable**  
**Từ Trust-based → Cryptographically verified**

Trong VinaLib:
- ✅ **Hiện tại**: IPFS Simulator cho development (local, fast, free)
- ✅ **Lợi ích**: Gas cost giảm 60% so với lưu raw data on-chain
- 🎯 **Tương lai**: Migration sang Pinning Service cho production (distributed, reliable, scalable)

**Lưu ý quan trọng:**  
IPFS vẫn đang phát triển. Implementation hiện tại của VinaLib phù hợp cho giai đoạn MVP và testing. Khi scale lên production, cần migrate sang IPFS thật với pinning service để đảm bảo availability và performance.